模块加载时机:
1. 作为启动文件加载 - Module.runMain
2. 通过require加载 - Module.prototype.require

node加载模块步骤：

　　1） 路径分析 （如判断是不是核心模块、是绝对路径还是相对路径等）

　　2） 文件定位 （文件扩展名分析， 目录和包处理等细节）

　　3） 编译执行

node加载模块步骤：

原生模块加载顺序

　　1） 缓存

　　2） 本地原生模块

文件模块加载顺序

　　1） 缓存

　　2） 如果是绝对路径， 则直接按路径读取并编译

　　3） 如果是“/”则直接从/node_modules目录查找

　　4） 如果是相对路径， 则生成如下查询规则,

    ‘/home/myapp/mydir/node_module‘,
    ‘/home/myapp/node_module‘
    ‘/home/node_module‘,
    ‘/node_module‘


　　5） 从上述数组中取出第一个目录作为查找对象， 如果存在结束查找

　　6） 然后依次尝试添加.js、.json、.node后缀继续查找， 如果存在则结束

　　7） 尝试将require参数作为一个包查找， 读取目录下的package.json文件， 取得main参数指定的文件

　　8） 根据指定的文件未找到， 如果没有，执行第6步

　　9） 如果main参数不存在或者第8步未找到， 则查找该目录下index文件， 如果没有， 执行第6步

　　10） 如果依然没有找到， 则开始取出数组第二条路径， 然后执行5-7步。 直到数组中最后一个值

　　11） 如果还没找到， 抛出异常