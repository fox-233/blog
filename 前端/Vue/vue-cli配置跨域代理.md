现在使用vue大多使用了前后端分离模式，因此游览器经常显示跨域失败的信息，现在跨域的方式很多种，主要分两大类，ajax跨域，dom跨域，具体的方法就不例举啦。

vue-cli作为一个强大的脚手架，内置了一个简单的配置型跨域方式
<img src="https://raw.githubusercontent.com/869288142/photo/master/proxy.png">
找到目录下的config文件下，index.js中dev配置对象中的proxyTable属性，这里是一个对象

    下面对这个对象属性进行解析：
<img src="https://raw.githubusercontent.com/869288142/photo/master/proxyTableObj.png">

```javascript
 proxyTable: {
      '/api':{            //这里的key就是axios的baseURL
        target: 'http://127.0.0.1',    //访问域名
        changeOrigin: true,            //开启跨域
        pathRewrite:{  // 路径重写，
            '^/api': 'api/index.php'  // 替换target中的请求地址
        }
      }
    }

```
也就是设置axios的baseURL可以只设置为'/api'，然后在proxyTable里面定义匹配这个路径的代理配置对象，设置target为访问的域名，设置重写为访问域名的后缀，即域名后的地址，然后开启changeOrigin属性即可。

注意：配置好后由于这个文件不在src下，不会触发构建，每次修改需要重新npm run dev 来使用新的配置，此时成功会看到命令行输出代理服务器配置信息