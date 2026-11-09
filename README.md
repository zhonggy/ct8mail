# ct8 自助开邮箱

在 ct8 上搭一个网页，不用登录 DevilWEB 面板就能自助创建邮箱账号，并附带一个站内收件箱。

## 功能

- **自助开邮箱**：填邮箱前缀 + 密码（可手动输入，也可一键随机生成），多域名下拉选择，一键创建
- **统一登录**：首页用邮箱 + 密码登录（IMAP 当场验证）。管理员邮箱进开邮箱页，普通邮箱直接进收件箱；管理员密码不写在网站上
- **站内收件箱**：左右分栏（左列表、右内容），合并全部已收文件夹按时间倒序，垃圾邮件带“垃圾邮件”标签；已发送、草稿、已删除不显示
- **HTML 邮件渲染**：HTML 正文在沙盒 iframe 里渲染（禁用脚本），可切换纯文本版；附件可下载
- **创建历史**：开过的邮箱自动记到服务器（`hist_data.php`，不进仓库），支持复制邮箱/密码、一键进收件箱、跳官方 webmail、删除，以及每条记录的**标签**（输完点别处自动保存）
- **防重复提交**：开邮箱成功后跳转回本页（PRG），刷新不会再提交一遍
- **官方 webmail 备用入口**：`poczta.ct8.pl`

## 部署

1. 在 DevilWEB 面板里建一个 PHP 类型的网站（WWW Websites），拿到网站根目录（`public_html`）
2. 把 `app.php`、`inbox.php`、`index.php`、`bg-lowpoly-light.webp` 上传到 `public_html`
3. 在 `public_html` 下新建 `config.php`，只写一行（把密码换成你自己的面板密码）：
   ```php
   <?php define('PANEL_PASS', '你的面板密码');
   ```
4. 打开 `app.php` 顶部的配置区，按自己的信息填写：
   - `PANEL_BASE`：`https://panel.ct8.pl`（不用改）
   - `PANEL_USER`：ct8 面板用户名
   - `MAIL_DOMAINS`：ct8 账号里开通了邮件功能的域名，例如 `['your-domain.ct8.pl']`
   - `ADMIN_EMAIL`：管理员邮箱（登录后进开邮箱页）
5. `inbox.php` 顶部的 `IMAP_HOST` 是 `s1.ct8.pl`（不用改）
6. 访问你的网站域名，用管理员邮箱登录即可使用

> 每个域名需要在面板里至少创建一个邮箱账号（如 `postmaster@你的域名`），该域名的邮件功能才会被激活，之后本页面就能为它开邮箱了。

ct8 的面板是新版 DevilWEB 2.2，开邮箱走 JSON API（`POST /api/v3/auth` 登录拿 Cookie 会话，`POST /api/v3/mail/mailbox` 创建），不是旧版的表单页面。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `app.php` | 开邮箱页 + 统一登录 + 创建历史 API |
| `inbox.php` | 站内收件箱（裸 socket 直连 IMAP） |
| `index.php` | 首页入口（引用 `app.php`） |
| `bg-lowpoly-light.webp` | 页面背景图 |
| `config.php` | 面板密码（自己建，**不要进仓库**，已在 `.gitignore` 里） |
| `hist_data.php` | 创建历史数据（服务端自动生成，**不要进仓库**） |
