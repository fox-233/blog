响应基于 set 和 get(Object.defineProperty)

    类型：

        单向绑定
        双向绑定

    简单例子(基于Object.defineProperty)

```html
<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div>
        <form action="" class="form">
            <div>
                <label for="name">name</label>
                <input type="text" name="name" id="name">
            </div>
            <div>
                <label for="age">age</label>
                <input type="text" name="age" id="age">
            </div>
        </form>
    </div>

</body>
<script>
    // 实现Model到View的单向绑定
    //Model
    let person = {
        data: {
            name: '',
            age: '',
        }
    };
    //ViewModel
    function binding(obj) {
        for (let key in obj.data) {
            Object.defineProperty(obj, key, {
                get() {
                    console.log("getter");
                    return obj.data.key;
                },
                set(value) {
                    console.log("setter: " + value);
                    let dom = document.querySelector(`#${key}`);
                    dom.value = value;
                    console.log("dom.value: " + dom.value);
                    obj.data.key = value;
                    console.log("obj.data.key: " + obj.data.key);
                },
            });
        }
    }

    binding(person);

    // 实现view到model的单向绑定
    //View
    //获取表单对象
    // let form = document.querySelector(".form");

    // //监听form的input事件
    // form.addEventListener('input', (e) => {
    //     let value = e.target.value;
    //     let name = e.target.getAttribute('name');
    //     person[name] = value;
    // });

</script>

</html>
 ``` 
    分析:
        1.js部分划分为数据(model)、关联视图方法viewModel

        2.只需要使用binding方法，此方法就会遍历观测对象的所有key，并为key定义属性拦截器(Object.defineProperty)，在数据set、get时定义额外的处理

        3.特别地，在set的时候，不仅仅更新的观测对象的的值，还更新了DOM元素的值，当然这里是做了简单处理，用以了解响应设计基础部分,详细部分的话，有兴趣可以去看看官方源码设计

        4.大家可能注意到有一个部分被我注释了，那个就是那个addEventListener这部分，这里实现了view到model的单向更新

总结: **vue的响应式核心是基于ES5特性Object.defineProperty生成的set、get**  

    Vue对数组和对象的观测处理：

        非根级变量响应方法：

            1.数组(vue对这些数组方法进行了代理，调用时做了更新视图的处理),至于为什么，下面马上解释
            
            检测边界：
                1.增加或者修改

                新增：vm.items[indexOfItem]=newValue
                这种情况其实就是在为对象添加一个新key，数组本身也是Array类的一个实例

                修改：vm.items[indexOfItem]=newValue按照数组本身是一个对象应该会触发更新，但是vue设计源码没有把数组对象的key进行set和get转换

                2.修改长度

                vm.items.length = newLength
                同样这样没有set和get没法触发视图更新

            另辟蹊径：

                通过对原生方法进行改写来触发视图更新
            
                1.变异方法(改变原数组)

                    push()
                    pop()
                    shift()
                    unshift()
                    splice()
                    sort()
                    reverse()

                2.非变异方法(返回新数组)，vue在更新视图对这种覆盖原数组的情况作了优化处理，无需担心渲染效率

                    filter()
                    contact()
                    slice()


            检测边界的新处理方法：

                1.增加元素

                    1.1 按顺序插入

                        push(value)
                        unshift(value)

                    1.2 按指定key插入

                        Vue.$set(arr,index,value)
   

                2.删除元素

                    splice(index,count)    

                3.修改元素

                    splice(index,1,value)


            2.对象(遍历data赋予set和get)
                
                 Vue.$set(target, key, value) 
                 
                 注意事项:被设置变量不能存在(源码发现key存在就会跳过此key的set和get转换)

                增加单个属性

                Vue.$set(target, key , value)
                
                增加多个属性
                
                Object.assign({},target,source)
