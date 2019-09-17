### 环境：
主机名为 A、B、C 的3台相同CPU架构的linux服务器，且3台服务器可通过主机名互相访问
其中
A上安装： golang编译环境，ansible工具，harbor镜像仓库服务（开机启动），均使用最新稳定版；
B、C上安装： docker-ce-18.06.3（开机启动）。

### 目标：
1. 在A上使用golang编写代码：实现一个web服务，在访问 http://[主机名]/self/ 时返回 "Hello [主机名] [版本号]!"；
2. 在A服务器上使用ansible-playbook命令实现：
   - 2.1 将golang代码编译成二进制文件并打包成为镜像，同时修改tag号递增加1，从v1开始；
   - 2.2 将新的镜像上传到harbor保存；
   - 2.3 判断成功上传镜像后，在B、C上启动镜像（如果之前有启动上个版本镜像，则覆盖启动新的镜像）；
   2.4 B和C上的golang容器开机启动并在docker kill后还能自动再次起来新的容器。
3. 在A上使用web浏览器或者curl访问： 
   - 3.1 http://B/self/ 返回 "Hello B v1!" 带有主机名和镜像版本号的字符串；
   - 3.2 http://C/self/ 返回 "Hello B v1!" 带有主机名和镜像版本号的字符串。
   
### 备注：
1. 只用一条ansible-playbook命令实现代码编译，镜像打包上传，应用发布和更新；
2. 虽然B和C返回不同的字符串，但是注意：B和C的容器启动都使用同一个镜像，只是环境变量不同；
3. golang代码逻辑可自由发挥，Dockerfile文件名、ansible-playbook文件名、镜像名称、容器名称可自定义。
