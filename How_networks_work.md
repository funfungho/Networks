<!-- 《网络是怎样连接的》笔记 -->
# 1. 浏览器生成消息
1. **域名**：服务器的名称，如 `www.google.com`
1. ***URL***: uniform resource locator, 统一资源定位符。
    - `http`, `ftp`, `file`, `mailto` 等表示访问数据源的机制、方法，即协议。
        - ***FTP***: file transfer protocol
        - ***HTTP***: hypertext transfer protocol
<!--     
    1. 用 HTTP 服务器访问 Web 服务器
        - `http://user:password@www.glasscom.com:80/dir/file1.html` 
        - 协议 （+ 用户名） （+ 密码） + Web 服务器域名 （+ 端口号） + 文件路径名
    1. 用 FTP 协议下载、上传文件
        - `ftp://user:password@ftp.glasscom.com:21/dir/file1.html`
        -  协议 （+ 用户名） （+ 密码） + FTP 服务器域名 （+ 端口号） + 文件路径名
    1. 读取本地文件
        - `file://localhost/c:/path/file1.zip`
        - 协议 （+ 计算机名） + 文件路径名
    1. 发送电子邮件
        - `mailto:tone@glasscom.com`
        - 协议 + 邮箱地址
    1. 阅读新闻组文章
        - `news:comp.protocols.tcp-ip`
        - 协议 + 新闻组名
     -->
2. URI: uniform resource identifier, 统一资源标识符。
3. 路由器：一种对包进行转发的设备。
4. 集线器：一种对包进行转发的设备，分为中继式和交换式。
3. DNS 服务器, 域名, Socket 库, 解析器   
- World Wide Web 是最早开发的浏览器兼 HTML 编辑器的名称。
## 生成 HTTP 请求
### 浏览器解析 URL
- URL 中文件路径名为 `/` 表示根目录。
- 服务器不允许文件名和目录名设置成相同。
- 对于每个目录，服务器会设置好默认访问的文件名，通常为 `index.html` 或 `default.html`, 因此要访问的文件名省略时也能正常访问默认设置的文件。
    - `https://sb.io` 和 `https://sb.io/` 会访问到根目录下的默认文件。
    - `https://sb.io/dir/` 会访问到 `/dir/` 目录下的默认文件。
    - `https://sb.io/dir` 的情况，若服务器上存在名为 `dir` 的文件，则访问到该文件；否则，若存在 `/dir/` 目录，则访问到该目录的默认文件。
- 浏览器解析完 URL 后，使用 HTTP 协议来访问 Web 服务器。
### HTTP 协议
- HTTP 请求包含“对什么”和“进行什么操作”。
    - “对什么”部分称为 URI, 
    - “进行什么操作”部分称为方法或 HTTP 谓词。
- 请求消息
    1. 请求行 - `<方法><空格><URI><空格><HTTP 版本>`
    2. 消息头 - 附加信息，每行包含一个头字段（`<字段名>: <字段值>`）。
        - 头字段分通用头，请求头，响应头。
    3. 空行
    3. 消息体 - 客户端向服务器发送的数据，如 POST 方法发送的表单数据。
        - 使用 GET 方法时消息体为空，无需数据，服务器通过识别 GET 方法和 URI 即可操作。
- 响应消息
    1. 状态行 - `<HTTP 版本><空格><状态码><空格><响应短语>`
        - （状态码） `1xx` - 告知请求的处理进度和情况
        - `2xx` - 成功
        - `3xx` - 需要进一步操作
        - `4xx` - 客户端错误
        - `5xx` - 服务器错误 
    2. 消息头
    3. 空行
    4. 消息体 - 服务器向客户端发送的数据。
        - 消息体的内容作为二进制数据处理，通过消息头中的 `Content-Type` 字段来定义（MIME 类型）。
- 每条请求消息中只能有一个 URI, 所以每次只能获取一个文件。
- 浏览器遇到图片相关的标签时，会在屏幕上预留显示图片的空间，然后发送请求消息请求相应图片。
- 生成 HTTP 消息后，浏览器委托操作系统将消息发送给 Web 服务器。
## 向 DNS 服务器查询 Web 服务器的 IP 地址
### IP 地址
- **子网**是通过集线器连接起来的计算机群组。拖过集线器连接起来的所有设备都属于同一个子网
- 子网通过路由器连接成网络。
- IP 地址是一串 32 比特的数字，8 比特（1 字节）为一组分为 4 组。
- IP 地址分为**网络号**和**主机号**。
    - 网络号分配给整个子网。
    - 主机号分配给子网中的网络设备（硬件）。
- 网络号和主机号加起来总共 32 比特，但两部分的具体结构不固定，组建网络时用户可以自行决定分配关系。
- 通过**子网掩码**来确定 IP 地址的内部结构。
    - 子网掩码也是一串 32 比特的数字， 1 的部分表示网络号，0 的部分表示主机号。
        - `10.1.2.3/255.255.255.0` 和 `10.1.2.3/24` 均表示网络号为 `10.1.2`, 主机号为 `3`.
    - 主机号部分全为 0 表示整个子网而非子网中的某台设备；全为 1 表示向子网上的所有设备发送包，即对整个子网广播。
## DNS 服务器接力
## 浏览器委托协议栈发送消息