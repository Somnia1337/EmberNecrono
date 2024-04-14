一个简陋的 email 用户代理，遵循「计算机网络」课程项目 \#2 的要求，用 Rust 实现。

项目地址(GitHub)：https://github.com/Somnia1337/EchoUnityArchivist

## 展示

### 流程

登录：

![[Login.png]]

写信：

![[Compose.png]]

![[Sent.png|350]]

收信：

![[Fetch.png]]

登出 & 关闭：

![[Logout.png]]

### 细节

英语支持：

![[Multi Lang EN.png]]

3 种行首标志：

`>`：会话

![[Session.png|250]]

`✓`：成功

![[Succeed.png|300]]

`!`：出错

![[Fail.png]]

邮箱格式检查：

![[Email Check.png|300]]

邮件分割线：

![[Horizontal.png|300]]

发送二次确认：

![[Reconfirmation.png|150]]

## 实现

使用的第三方 crate：

```toml
[dependencies]
imap = "2.4.1"
lettre = "0.11.6"
native-tls = "0.2.11"
```

得益于 Rust 强大的类型系统，实现此 CLI 应用的体验非常流畅、舒适，重构让代码更加优雅、可扩展，也让我对 Rust 有了更深的理解。

优雅地处理错误：

```rust
// Logout from IMAP server
println!("{}{}...", prompts.eua_logging_out, user.imap_domain);
match imap_cli.logout() {
	Ok(_) => println!("{}", prompts.eua_logout_succeed),
	Err(e) => println!("{}{:?}", prompts.eua_logout_fail, e),
}
```

轻松地持续读取并验证用户输入：

```rust
pub fn read_selection(prompt_read: &str, prompt_invalid: &str, lo: usize, hi: usize) -> usize {
    loop {
        match read_input(prompt_read).trim().parse().ok() {
            Some(x) if x >= lo && x <= hi => return x,
            _ => println!("{}{}.", prompt_invalid, hi),
        }
    }
}
```

`lettre` 库让发送逻辑变得简洁明了：

```rust
pub fn compose_and_send(
	&self,
	smtp_cli: &SmtpTransport,
	prompts: &Prompts,
) -> Result<Option<String>, Box<dyn Error>> {
	// ...
	
	// Build the message
	let email = Message::builder()
		.from(self.email.clone().parse().unwrap())
		.to(to.clone())
		.subject(read_input(prompts.compose_subject))
		.header(ContentType::TEXT_PLAIN)
		.body(read_body(&prompts))
		.unwrap();
	
	// ...
	
	// Reconfirm
	if !read_reconfirmation(&prompts) {
		return Ok(None);
	}
	
	// Send the message
	println!("{}", prompts.send_sending);
	match smtp_cli.send(&email) {
		Ok(_) => Ok(Some(to.to_string())),
		Err(e) => Err(Box::new(e)),
	}
}
```

## 最后

500 余行的小项目（去掉“不那么优雅”的多语言实现就 300 余行），没有使用任何 Rust 高级特性（如 `unsafe` 黑魔法），就实现了一个相对健壮的 CLI 应用。

回看写了一年多的 Java，突然感觉不香了（一定是 skill issue🤣）。

BTW，昨天打完蓝桥杯了，今年 Java A 组题难度颇低，本周内写一写题解😉