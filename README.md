# axios
axios的 优势： 
1. axios 是基于promise 封装的ajax库
2. 可以设置请求响应拦截 axios.interceptors.request.use    axios.interceptors.response.use
3. 可以设置取消请求
4. 可以防御攻击



json-server 模拟本地服务器进行测试

1.  安装命令：  npm install -g json-server

2.  根目录下新建  db.json 文件

{
    "posts": [
      { "id": 1, "title": "json-server", "author": "typicode" }
    ],
    "comments": [
      { "id": 1, "body": "some comment", "postId": 1 }
    ],
    "profile": { "name": "typicode" }
}

3.   启动json-server 命令  ：  json-server --watch db.json  

    运行后可以看到终端中出现 http://localhost:3000 访问即可


axios的使用：  1.用过npm 下载到工程中  2. 使用 BootCDN 通过script标签引入

使用 ： 
    axios.defaults.baseURL = "http://localhost:3000"   // 配置默认信息  axios.defaults
    // get请求
    axios({
        url:"/posts",
        method:"GET ,         // 默认是 GET 请求
        params:{              // params 用于 get 请求携带参数
            id:1
        }
    }).then(res => {
        console.log(res.data)
    })


    // post 请求
    axios({
        url:"/posts",
        methods:"POST",          // 指定 请求方法为 post
        data:{"title":"cht"}     // data 用于 post 请求携带参数  
    }).then(res=> {
        console.log(res.data)
    })


    // 语法糖 

    axios.get("/post" , {params:{id:1}}).then(res=> {
        console.log(res.data);
    })

    axios.post("/post" , { "title":"cht" }).then(res=> {
        console.log(res.data);
    })


    注意：1. 默认method 是 get请求
          2. get 请求 使用 params 
          3. post 请求使用 data 


    // interceptors 拦截器
    // 请求拦截
    axios.interceptors.request.use(congig => {
        console.log("This is request resolved");
        return config;
    },err=> {
        return Promise(rejecte(err));
    })

    // 响应拦截
    axios.interceptors.response.use(response => {
        console.log("This is response resolved");
        return response;
    },err=> {
        return Promise(rejecte(err))
    })



    //取消请求
    let cancel;  // 用于保存 cancelToken 构造器中的 取消函数 c  便于全局使用

    axios({
        url:"/post",
        cancelToken: new axios.cancelToken((c)=> {
            cancel = c;
        })
    }).then(res=> {
        cancel = null ;   // 请求成功后，释放cancel
        console.log(res.data);
    } , err => {
        cancel = null ;  // 请求失败后，也释放cancel
        console.log(err);
    })

    function cancelRequest() {
        if( typeof cancel === 'function') {
            cancel("强制取消请求");
        }else {
            console.log("cancel不是一个函数，请先发送请求！");
        }
    }
