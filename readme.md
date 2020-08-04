# GD Share
## demo
[https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2](https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2)

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
