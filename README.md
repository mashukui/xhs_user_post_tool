# xhs_user_post_tool
> 马哥原创：爬小红书博主软件，xhs_user_post_tool，界面软件工具，可以把小红薯指定博主的已发布帖子的数据及图片采集下来。

> 本软件工具仅限于学术交流使用，严格遵循相关法律法规，符合平台内容合法合规性，禁止用于任何商业用途！

# 一、背景分析
## 1.1 开发背景与功能介绍

我是[@马哥python说](https://github.com/mashukui)，一枚10年+程序猿，现全职独立开发。

曾经和很多同学聊过，他们希望有一个工具，可以把小红书指定博主（对标账号）的主页已发布帖子的数据全部采集下来，然后做数据分析使用。为了满足这类需求，我特意用python开发了这款工具：**xhs_user_post_tool**

软件运行界面：

![软件运行界面](https://files.mdnice.com/user/32110/7b0de3af-008e-4784-aee2-ecbebbf82997.png)

采集结果-笔记详情数据：

![采集结果-笔记详情数据](https://files.mdnice.com/user/32110/664d9035-6389-48c6-829f-174bb887bee9.png)
 
笔记图片一级文件夹：

![笔记图片一级文件夹](https://files.mdnice.com/user/32110/e8dd040f-a175-49e5-abe9-7e75e49d5a2d.png)

笔记图片二级文件夹：

<img width="1440" height="697" alt="image" src="https://github.com/user-attachments/assets/ff7592c9-4fa2-4929-ab80-cd4b65455c75" />

 
图片文件的存储规则：
1. 以博主昵称命名一级文件夹
2. 一级下面是二级文件夹，二级文件夹以笔记id命名
3. 二级文件夹里的图片命名为01.jpg，02.jpg，以此类推
4. 如果笔记是视频类，文件夹中只保存一张图片，即视频封面图

以上。

## 1.2 软件说明

重要说明，请详读：
1. Windows系统、Mac系统均可直接运行，无需配置python环境
2. 软件通过接口协议爬取，并非通过模拟浏览器等RPA类工具，稳定性较高！
3. 软件运行完成后，会在当前文件夹（即，软件所在文件夹）生成csv结果文件
4. 爬取过程中，每爬一条，存一次csv。并非爬完最后一次性保存！防止因异常中断导致丢失前面的数据（每条请求间隔2s，可自由配置）
5. 爬取过程中，有log文件详细记录运行过程，方便回溯
6. 采集结果有18个字段，含：作者昵称,作者id,作者链接,页码,笔记标题,笔记id,笔记链接,笔记链接_长,头图链接,笔记类型,点赞数,收藏数,评论数,转发数,笔记正文,发布时间,修改时间,IP属地。

# 二、主要技术

## 2.1 模块介绍
软件全部模块采用python语言开发，主要分工如下：
- tkinter：GUI软件界面
- requests：爬虫请求
- json：解析响应数据
- time：间隔等待，防止反爬
- csv：保存csv结果
- logging：日志记录

出于版权考虑，暂不公开完整源码，仅向用户提供软件使用权。

## 2.2 部分源码
软件界面：
```python
# 创建主窗口
root = tk.Tk()
root.title('爬小红书博主软件v6 | 马哥python说')
# 设置窗口大小
root.minsize(width=850, height=660)
```
爬虫请求：
```python
# 发送请求
r = requests.get(url, headers=h2, params=params)
# 接收响应数据
json_data = r.json()
```
保存数据：
```python
# 保存到csv文件
with open(self.result_file, 'a+', encoding='utf_8_sig', newline='') as f:
	writer = csv.writer(f)
	writer.writerow(data_row)
self.tk_show('csv已保存：' + self.result_file)
```
日志记录：
```python
def get_logger(self):
	self.logger = logging.getLogger(__name__)
	# 日志格式
	formatter = '[%(asctime)s-%(filename)s][%(funcName)s-%(lineno)d]--%(message)s'
	# 日志级别
	self.logger.setLevel(logging.DEBUG)
	# 控制台日志
	sh = logging.StreamHandler()
	log_formatter = logging.Formatter(formatter, datefmt='%Y-%m-%d %H:%M:%S')
	# info日志文件名
	info_file_name = time.strftime("%Y-%m-%d") + '.log'
	# 将其保存到特定目录
	case_dir = r'./logs/'
	info_handler = TimedRotatingFileHandler(filename=case_dir + info_file_name,
											when='MIDNIGHT',
											interval=1,
											backupCount=7,
											encoding='utf-8')
	self.logger.addHandler(sh)
	sh.setFormatter(log_formatter)
	self.logger.addHandler(info_handler)
	info_handler.setFormatter(log_formatter)
	return self.logger
```
日志文件截图：
<img width="1104" height="896" alt="image" src="https://github.com/user-attachments/assets/c703df54-04dd-4d3a-9a36-f94309e409cb" />
 
以上。

# 三、演示视频
软件使用过程演示视频：[【软件演示】爬小红书博主软件v6，批量采集主页笔记](https://mp.weixin.qq.com/s/Ia1ns-E_d13kNBzJbBLkdw)

# 四、付费说明
## 4.1 卡密说明
付费如下：
```python
日卡：使用期限1天，29元。日卡仅能购买一次。适合试用等临时需求
月卡：使用期限1个月，149元。月卡可多次购买。适合短期采集需求
季卡：使用期限3个月，399元。季卡可多次购买。适合中期采集需求
年卡：使用期限1年，799元。年卡可多次购买。适合长期采集需求
```
付费方式：
<img width="2144" height="412" alt="收款码v4" src="https://github.com/user-attachments/assets/88457ab9-52cf-4089-a72d-226c26ebee2d" />

付费后，加我微信（493882434）自动掉落登录卡密。

## 4.2 一机一码
软件采用一机一码机制，一个卡密只能在一台电脑运行、不可多电脑运行。
## 4.3 软件多开
一台电脑仅允许运行一个软件，不支持软件多开。

## 4.4 软件维护
软件由本人独立原创开发，长期维护更新，提供稳定运行。

# 五、软件获取
公众号"**老男孩的平凡之路**"，后台回复"**爬小红书博主**"获取最新版软件安装包。
<img width="1938" height="364" alt="二维码-公众号放底部v2" src="https://github.com/user-attachments/assets/8181d46d-a579-41ee-b659-fc18f6a58fa4" />
