```py
import pyotp
from urllib.parse import urlparse, parse_qs

totp_uri = "otpauth://totp/har%3annie.a%4017x.onmicrosoft.com?secret=28392382example&issuer=Microsoft"

totp_parsed_uri = urlparse(totp_uri)

totp_parsed_uri_query_secret = parse_qs(totp_parsed_uri.query).get('secret')[0]

totp_secret = pyotp.TOTP(totp_parsed_uri_query_secret)

print(totp_secret.now())
```