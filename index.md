![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-26d14973086efef8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
七牛云储存是thinkphp储存类型可选项之一，怎么使用呢，好了不卖关子了，下面就详细介绍使用方法
首先是注册一个七牛账户
![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-31874145a85c82bc?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后登陆
![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-257ce79d9f25f4ef?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后选择添加资源，选择对象存储
![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-51b1d0bd389bd14a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-1ca85b996176067c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后填写空间名称（即bucket，后面会用到这个名称），访问控制为公开（选择公开访问是为了操作方便，当然你也可以选择私有，不过请求资源时候需要授权），确认创建。
进入刚才创建的资源，记录下域名
![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-4f74fc66d9052339?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在个人面板中选择密钥管理这里能获得你的AccessKey和SecreKey 。准备工作做好了，下面就是具体的配置和代码实现了。
在配置文件config.php中做如下配置
 ```
'config_qiniu' => array(

'accessKey' => '', //这里填七牛AK
'secretKey' => '', //这里填七牛SK
'domain' => '',//这里是域名
'bucket' => 'sangaolamu'//这里是七牛中的“空间”
),
'config' => array(
'maxSize' => 5*1024*1024,
'rootPath' => './Uploads/',
'savePath' => '',
'saveName' => array('uniqid',''),
'exts' => array('jpg', 'gif', 'png', 'jpeg'),
'autoSub' => true,
'subName' => array('date','Ymd'),
),

//然后在需要调用上传的地方将原来上传到本地代码片段修改为以下代码

$config = C('config');
$config_qiniu = C('config_qiniu');
$upload = new ThinkUpload($config,'Qiniu',$config_qiniu);
$info = $upload->upload();![Thinkphp之七牛云储存](http://upload-images.jianshu.io/upload_images/2825702-adf11ef0707f8407?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
$info 即为上传后返回的信息，其中url是文件的地址，形如[url] => http://ob9pbn9dt.bkt.clouddn.com/20160802_57a05d764e1f4.jpg，将该字段保存，后面访问时候就访问这个地址。至此文件上传到七牛云储存完毕，后面我会给出如何进行删除及其他操作的示例。
