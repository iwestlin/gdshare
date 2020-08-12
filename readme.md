# GD Share
## demo
[https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2](https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2)

## 介绍
这是一款受goindex启发而产生的项目，适合部署于cloudflare worker，相比原版有以下特性：

- 全盘搜索（包括个人盘和所有有权限的团队盘，可点击搜索结果中的链接跳转到对应的google drive官方网址）
- 分页浏览（可自定义每页文件数，每页可根据文件名和大小排序）
- 更美观的UI（致谢 ant design）
- 防爬虫，对于所有目录和文件，只有管理员才有读取和下载的权限（原版goindex可以通过在目录下防止 .password 来给目录设置读取密码，但无法限制单个文件的下载）
- 可以生成下载直链，方便第三方下载工具下载，对于流媒体文件，可以用potplayer等播放器直接打开进行播放（可以自定义有效期）
- 可以生成带有提取码的分享链接，方便分享给他人浏览和下载（同样支持自定义有效期）

## changelog
### 2020-08-12
- 添加批量获取直链功能（可自行选择，也可一键获取当前目录下所有直接子文件的直链）
- 添加唤起外部播放器功能（支持IINA/PotPlayer/VLC/nPlayer/MXPlayer）
- 添加指定范围搜素功能（可在目录列表页进行搜索，由于Google API的限制，只能搜索当前目录的直接子文件，无法搜索到递归子目录的内容。若当前目录为团队盘根目录，则支持整盘搜索）
- 「显示路径」按钮可正确识别团队盘名称（之前会返回"Drive"）
- 后端搜索接口添加失败重试机制

## tips
- 本工具亦可当作goindex使用，只需将目录ID添加到 `https://your.website.com/ls/` 后即可。
比如浏览团队盘内容可以直接访问 `https://your.website.com/ls/你的团队盘ID`
浏览个人盘根目录可以直接访问 `https://your.website.com/ls/root`

## 搭建方法
打开[template.js](./template.js)，根据提示修改变量：
```javascript
const CONFIG = {
    PASSKEY: "this is your passkey", // 管理员网页登录密钥，请自行修改，尽量复杂
    HASHKEY: "this is your hash key", // 用于校验生成的下载链接和分享链接，请自行修改，尽量复杂。修改后之前生成的下载和分享链接都会失效
    RETRY_LIMIT: 5, // 有时调用 google drive api 读取目录时会报错，这里设置最多允许重试的次数
    PAGESIZE: 100, // 读取列表的单页对象数，官方限制最大 1000
    AUTH: {
        client_id: "insert_your_client_id", // 这三项是你的google帐号个人授权信息，和goindex相同
        client_secret: "insert_your_client_secret", // 同上必填
        refresh_token: "insert_your_refresh_token", // 同上必填
        expires: 0,
        access_token: "" // 可不填
    }
}
```
变量设置完成后，将 `template.js` 整体复制到 cloudflare worker 中（具体步骤可参考[https://www.jiyiblog.com/archives/031279.html](https://www.jiyiblog.com/archives/031279.html)），完成。
