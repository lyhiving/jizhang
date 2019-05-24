### 版本区分
1. 本版本为PHP+MYSQL版本
2. PHP+SQLite版本见：https://github.com/chenstor/jizhangSqlite

### 特别说明
1. 目前版本，其中一个函数文件content.php源码没有开放，后续处理好了再考虑开放，所以目前这个文件是加密状态。
2. 将文件上传到服务器时，若使用的客户端是FlashFXP，请务必设置上传模式为：二进制，否则会导致安装之后界面出现白屏。
3. 若服务器禁用获取磁盘大小的函数，可能导致安装不顺，直接网址上加参数跳过即可。

### 程序名称
PHP+MYSQL多用户记账程序

### 安装说明
1. 将程序放到指定目录，可以根目录，可以二级目录
2. 运行/install/，或者直接输入域名都可以自动判断，未安装的会进入安装界面
3. 一路next（什么协议那些没有弄）
4. 输入数据库地址、端口、数据库名、账号、密码等参数（安装测试数据的功能屏蔽掉）
5. 网站名称就是安装之后的系统名称，可以在安装之后在data/config.php 里面修改，其他内容不建议修改
6. 注意，默认会记录安装时的域名，这个域名唯一的用处就是找回密码的邮件，如果安装之后没有换过域名，不用理会，否则需要在data/config.php 里面修改
7. 找回密码是通过发邮件找回，需要配置SMTP，配置见：inc/smtp_config.php（包括找回密码的邮件模板，也是在这个文件修改）
8. 其他文件，不建议修改，除非你看得懂

### 作者
1. 程序的第一版是@zhengyong100 URL：https://github.com/zhengyong100/ji
2. 我在其版本的基础上做了二次开发
3. 问题反馈通过提交issues或者到博客https://itlu.org/articles/2550.html 反馈，谢谢！

### 功能介绍
1. 登录界面　功能是没变，全部改成Ajax请求，做了一系列的安全措施。　另外界面模仿了WP的后台登录界面进行调整，所以很容易看到WP的影子。因为使用JQ模拟form提交，之前还不能支持键盘提交，昨天给加了回车键提交，实在是不能再爽。
2. 记账页　将收入和支出合并使用Tab显示，默认是支出。另外就是支出记录之后还是默认为支出，收入记录之后还是默认为收入。优化页面内容输出，使用统一的SQL进行输出，页面就执行foreach直接将内容展示，实在是不能再爽。代码量少了很多。编辑页面使用Bootstrap弹出的窗口进行修改，弹出时将列表的数据通过json格式传递到弹出层，减少查询数据库。只有保存的时候才update，因为在列表的时候已经将数据查询出来，没必要编辑的时候还要再查询。以前是写法是编辑的时候还要根据ID查询一次，根本不考虑数据库的查询优化。
3. 近期统计　使用一个查询语句，传入开始时间和结束时间进行查询结果。代码上是简洁，但实际上执行了16次查询，感觉这里需要再优化。至于剩余的金额，就是收入-支持进行页面计算，不进行数据查询了。
4. 年度统计　感觉年度统计是优化得最好的。每个分类只需要一次查询，后续的数据全部是根据分类的查询结果进行页面的JS计算，不会再查数据库，做这个功能的时候，做过几个版本，目前这个版本算是比较满意的。
5. 导入导出　基本上就是页面样式的改动，功能没做大调整。不过代码还是优化了，对于不符合条件的数据，直接跳过，最后再弹出提示，成功多少条，失败多少条。这个功能之前是没有的。
6. 查询修改　这个页面改得比较多，支持多条件查询，翻页，弹出层编辑数据等一系列优化。
7. 用户编辑　优化界面，阉割掉删除用户数据的功能，暂时不想开放。
8. 安全方面　登录上做了安全过滤、错误次数限制、记账金额校验、密码长度校验等，引入安全过滤函数。使用统一的get和post过滤一系列安全函数进行过滤。登录密码使用加盐算法，注册时候生成一次盐，改动密码又生成一次盐，只要不是长期不改密码的，理论上密码的安全系数是比较高的。
9. 系统安装　可以自定义账户密码。
