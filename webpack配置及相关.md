### 前端工程化

- 模块化（资源，css，js）

- 组件化（复用）

- 规范化（编码、接口、文档规范化，目录结构的划分、git分支管理）

- 自动化（构建、部署、测试）

  #####  webpack（具体解决方案）

   解决兼容性问题，性能优化

  ![image-20211221195652460](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211221195652460.png)

  

  - 在项目中配置webpack

    - webpack.config.js配置文件中

      ```js
      module.exports={
      	mode:'development' //mode用来指定构建模式，development(开发，不会压缩mian.js,但打包速度会缩短)、production(生产,生成的main.js会被压缩，体积变小，但打包时间变长)
      }
      ```

    - pakage.json

      ```js
      "scripts":{
      	"dev":"webpack" //可以通过 npm run xx(dev) 来运行
      }
      ```

  - webpack默认约定

    > 默认打包入口文件为 src->index.js
    >
    > 默认输出文件路径为dist->main.js

    **修改**

    ```js
    const path = require('path') //导入path
    
    module.exports={
        //指定处理的文件
    	entry:path.join(__dirname/*当前文件所在目录*/,'./src/xxx.js')
        //后面拼接指定文件
        
        //指定生成的文件
        output:{
        	path:path.join(__dirname,'dist')//生成路径
        	filename:'xxx.js' //生成文件名
    	}
    }
    ```

  - webpack插件

    - webpack-dev-server(修改代码自动实时打包构建 `会将生成的js文件存放到内存而不是磁盘，不在dist中，引用<script src="/xxx.js"></script>`)

      `npm install webpack-dev-server@3.11.2 -D  `

      - 配置

        ```js
        "scripts":{
        	"dev":"webpack serve"
        }
        ```

        

      

      



