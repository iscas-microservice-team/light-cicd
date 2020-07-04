# light-cicd
yaml、java、nodejs为三个二级文件夹，分别对应的内容如下。

## yaml

1. 了解pod和pod 控制器（job、DamonSet、rc和rs之间的控制关系）
2. 构造git/gitLab/github的pod及其job的yaml，yaml能够表达项目地址，用户名，密码，文档路径等参数，一个yaml实现代码拉取的功能；
3. 构造mvn/nodejs的pod及其job的yaml，yaml表达编译环境，源码位置，打包命令、测试参数等内容，一个yaml实现源码编译的过程；
4. 构造docker build进程描述的yaml，可以基于yaml和命令启动一个docker build过程，该进程要能封装build命令（简化），根据3中的编译输出、dockerfile位置构造新的容器镜像；
5. 构造clair及其job的yaml，能够启动一个容器对指定镜像进行安全扫描；
6. 构造业务应用的发布yaml,能根据这个yaml完成发布功能（包含微服务编排，资源使用等大量信息）；

## java

用于控制1-6过程的可靠性、稳定性、可视化和自定义分支（如跳过步骤5）等；
所有请求k8s的接口都使用as组自定义的client接口（与汤婷沟通请求资源的方式，很简单）。
1. 自定义符合k8s资源规范的CICD请求参数，也是一个yaml，首先定义yaml的格式；
2. 在1中表达1-6中所有需要的参数；
3. 在1中表达1-6中的跳转关系；
4. 在1中表达1-6的存储路径等可以自动生成或事先规划的参数；
5. CICD执行引擎：根据1转化为1-6个yaml，分别执行，根据每个job是否（completed状态，可以通过client查询）执行完毕，控制pod及job的执行时机（通过java代码实现apply，delete或update）；
6. CICD可视化及异常控制：通过注解等解耦方式和5分离，根据5的执行结果，收集每个pod的状态数据、日志等信息，封装运行时中间结果或异常信息；
7. 中间结果进行再次封装，通过websocket模式，支持前端的准确刷新；

## Vue

RACE基于vue-element-admin已经实现一般，java和yaml完成后，可以安排其他前端人员介入，完成对接工作。

1. 接口沟通学习；
2. 绘制前端CICD界面；
3. 前台传入CICD yaml，能触发后台CICD过程；
4. 逐渐完善中间过程的展示，如成功、失败、当前阶段、日志信息等。
