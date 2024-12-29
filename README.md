## **Classic ASP Two-Factor Authentication**

This project is made for Classic ASP developers who need a ready-to-use library to implement two-factor authentication compatible with Google Authenticator, Microsoft Authenticator and other services using the time-based one-time password (TOTP; specified in RFC 6238) and HMAC-based one-time password (HOTP; specified in RFC 4226).
The entire library consists of just two files:

1. QRCodeLib.asp generate the necessary qrcode to scan with authentication apps (Vbscript)
2. Verify2FA.asp check and validate the TOTP code. It can be changed to check HOTP code (Jscript Ecmascript3 compatible)

They are a mod and refactory of the excellent work done by Brian Turek, Allan Jiang and Yasunori Ikeda.

## How to use

### QRCodeLib example (vbscript):
```
Const FORE_COLOR = "#000000"
Const BACK_COLOR = "#ffffff"
Const SCALE = 10

dim path: path = "yourpath"

'create QrCode image
Dim OAuthPath
OAuthPath = "otpauth://totp/yoursite:youruser?secret=yoursecret&issuer=yoursite"
Dim sbls: Set sbls = CreateSymbols(ECR_M, 40, False)
sbls.AppendText OAuthPath
Dim sbl: Set sbl = sbls.Item(0)
sbl.SaveAs2 path, SCALE, True, False, FORE_COLOR, BACK_COLOR 

'publishing image
Response.ContentType = "image/png"
Set adoStream = Server.CreateObject("ADODB.Stream") 
adoStream.Open
adoStream.Type = 1
FPath = Server.MapPath("path")
adoStream.LoadFromFile FPath
Response.BinaryWrite adoStream.Read 
adoStream.Close
Set adoStream = Nothing 
```
### Verify2FA example (Jscript):
```
var totp = new Totp(30, 6);
var secret = "ABCDEFGHIJKLMNOP"; // your secret CHAR(16)
var otp = totp.getOtp(secret); 
if (otp == Request.Form("TOTP")) {
	Response.Redirect("success.asp");
} else {
    Response.Redirect("failure.asp");
}
```
