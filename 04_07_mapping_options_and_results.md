The `Option` and `Result` types also have plenty of methods that use callables.

```rust
enum SendEmailError{
    AddressNotFound(UserId),
    RenderError(String),
    MailSendError(SmtpErr),
}

fn get_address(user_id: UserId)->Option<EmailAddress> {...}
fn render_email(user_id: UserId)->Result<Html, String> {...}
fn send_email(destination: EmailAddress, message_content: Html)->Result<(), SmtpErr> {...}

fn do_email(user_id: UserId)->Result<(), SendEmailError>{
    let rendered: Html = render_email(user_id)
        .map_err(|msg| SendEmailError::RenderError(msg))?;
    let address: EmailAddress = get_address(user_id)
        .ok_or_else(|| SendEmailError::AddressNotFound(user_id))?;
    send_email(address, rendered)
        .map_err(|smtp_err| SendEmailError::MailSendError(smtp_err))
}
```