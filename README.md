# How to prepare TLS certificates for use with the Splunk platform
## Certification Authority (CA)

Creo la chiave privata per la CA:
```sh
$ openssl genrsa -aes256 -out myCertAuthPrivateKey.key 2048
```

Creo il Certificate Signing Request (CSR) per la CA:
```sh
$ openssl req -new -key myCertAuthPrivateKey.key -out myCertAuthCertificate.csr
```

Creo il certificato self-signed per la CA:
```sh
$ openssl x509 -req -in myCertAuthCertificate.csr -sha512 -signkey myCertAuthPrivateKey.key -CAcreateserial -out myCertAuthCertificate.pem -days 1095

```

## Server Certificate
Creo la chiave privata per il server:
```sh
$ openssl genrsa -aes256 -out myServerPrivateKey.key 2048
```

Creo il Certificate Signing Request (CSR) per il server:
```sh
$ openssl req -new -key myServerPrivateKey.key -out myServerCertificate.csr
```

Creo il certificato per il server firmandolo con la CA:
```sh
$ openssl x509 -req -in myServerCertificate.csr -SHA256 -CA myCertAuthCertificate.pem -CAkey myCertAuthPrivateKey.key -CAcreateserial -out myServerCertificate.pem -days 1095
```

Creo la certificate chain incollando in rigorosa sequenza:
1. myServerCertificate.pem
2. myServerPrivateKey.key
3. myCertAuthCertificate.pem

Il file definitivo, es. myServerCertificateCombined.pem avrà questo aspetto:
```
-----BEGIN CERTIFICATE-----
MIIDTzCCAjcCFApCRqGXIY1uiYuIeZLHORsb4NdiMA0GCSqGSIb3DQEBCwUAMGMx
CzAJBgNVBAYTAklUMQ0wCwYDVQQIDARSb21lMQ0wCwYDVQQHDARSb21lMRIwEAYD
VQQKDAlQaXBwbyBMdGQxDDAKBgNVBAsMA1NlYzEUMBIGA1UEAwwLY2EucGlwcG8u
aXQwHhcNMjMwMjIyMTAzMDM5WhcNMjYwMjIxMTAzMDM5WjBlMQswCQYDVQQGEwJJ
VDENMAsGA1UECAwEUm9tZTENMAsGA1UEBwwEUm9tZTESMBAGA1UECgwJUGlwcG8g
THRkMQwwCgYDVQQLDANTZWMxFjAUBgNVBAMMDTE5Mi4xNjguMS4yMTYwggEiMA0G
CSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCyv/ce2Zl4Hr5sYInq2Tu5xIJbeY3c
KIGXqqhs6BdOHxwsHUqIONbzVVwpr33i7t0izKvaJ2De6FTNqdrnD49UKAMppdCm
Dso0NrOwKuZjJis+dZryfroeb0ruDdKuhS8krwXZRI9G1aOUPe3APmUpWBIek14F
t0R6StP5hp43d6wPBgKn/AReiyVQE9IkCqen9f5ulN43Sw222vU+9teXkx9WY7Y1
ZEcp4H+KSrOPy6nMknVB979LFnRsb1DnfrKDN55/OTYtLG01LoAVuuKWLGUyg0t6
fyzkyC4zTDMw8JcnL1NZOUJs1fRWkHUWfdIXPQdKgFLLVrSdkooWZYotAgMBAAEw
DQYJKoZIhvcNAQELBQADggEBAGlAyIXooxKNmbwWl8SzHGIi7Lrh58CxJfMXDQ3k
FLmfayi3lmo6466bLQyFOhwDqz0W40CBnAdG7cjYv3a67yjZEgzb/8p8Z8O7Z4NL
1X15JStf9QX6nlnylLti2dgjaPi0bcBDCSUDvx0tzL94KC3pWcfs26mFsAYft6Sh
GeVcPJ23sAjRe2F4QE+XAEPYhTrL86/fnEJpafrAdLbAGc5l57KEzDVqyGchvz9m
6RRrDoiu9ppHDlVvOBhBe5fzDXnqB/nmzPA/m3lwZStCS4oUZSygfSzfDP+gXW4T
x7xCZF0bWYy93AFkDMurf6VVzzsiyPQKfv9V3duqprfWhTE=
-----END CERTIFICATE-----
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-256-CBC,A113E832B45E48D603D229933C63422A

oD+DNugz2w9apumuTKAfKi8cy/87rqmkq4BWI8J3wQUwwidyuRH2xweuRfIS39Ib
BkYp3A7L8yq2WoPCYfI11NA+LZalBlMu4U3FisdRWkySOz0KW3aTlj9T2Qltw18j
UZlA/sRTzNSSgAzQvORBuHMJVkxrz0MOMrp0PeltvdXU9CangHa3Dl/0pvvRaMQ0
OLryvQfYw37ywGcSRX+MOUDjXqjp3u/C4RFDYUKdOwsuce/IJUc2X4FbIyEteujg
Y6BtZYOAy1XUpU0zkDPiYr4L8EM823FkQjUtTS1tgkqeG3/9Ga5CjxEYBntLBK7v
5K+11VFtjRCsfMvPb2c9dqLtQsucX1Bx+ckdvjmoxwGHtIEb69mlkrkhj7XgsBt9
fx0OgyqiTKKZhUejr94NKCj+YbQcFH+UPzNqUdcMYwj81HYKmFDvOCrInfVbKmGm
NntmGAeGgnC2MjSKBeiigsLdwlxcRbelUffZrxrJJ0xd1yIDhSLba8z9kLmYlZNb
t/IhZ3fQOw8d4TUhIEGFMSuQXXaMdK+zUFjT+8ZFHz5hmKnpCNPxhWbQC64kO/6B
tRn8w6zjZkJWHhKptuZZuLGrZlSdYKRn7hGifqd5b9ipswBRCuHNyBsg2ZRsT692
tDhG79iba4PqTAzWPV/IGK0xC8JAn8DoMT2TcHX+wgcTgLVJWoE9JsS83ApGvtu0
iQQWBsEc31j74NTAlPBL7aivXKtMAVJHi3e5N4ppwJ1LYozApskK2pDWYnC9DC3/
Fd7lNsEikTyI9y0xWOCQmcjWmYW2pS40yOR8iII19iTDcOrKH28uskR+4MmwiydR
EJlBO3KyLjTOyOjrjduFnaKA/AfR0Gy3Q/lRNowZkZE4GkmSNwSg+/qI8NpMVv14
qoxuoqLL+IpSUCntl1KImxYZVsWjaepBrJ38k3thggb62ykhgNjitTBjq50yyp7r
UuOISkjIh3+RbcO+abKVluGd2viOaVnr2+STh2Et5TNijvG+7hVLlfllGdP6Dcex
AfJWJ6w+nB7MPEKLfxdNge/tRv8dMGMhbH5ZrI5PM/s9l9Nq8lFdpAdR10W39dhJ
skQxATNgCnVBzJjngGISynD1DgKGBoCdehsA3Xf3ZHaDxD9ifH14vjOAGKkzzQhl
ruBn2VM9RQOSalfqAWnE0kRXVsCPkRY5w0xgy3JkPXok46g0UhGnkgLYrUlo6WG1
UH6fPNmzN85UG0b/lJ45Bb1dJOn6sMJbFVs+Cq/6JoC+lNWx3Rbr6+x0zQFEbp2k
FIOZQSZvWsegh99eQyIToYOACmKyPdJpqvWuVOdRyklGQX5nKzfTh+Tp89A28mVz
Czw+DdzHHjC2QEBxRivdWbzCUCFJGhZwBtrlYTbPq0jKEMsFH8+pNulFIY9mMSfS
oECEw4HOC42WIVTXkCEycW6Tob7C/TLK06IAjovOrp3MZRQIer8WFimFVYBHLuUP
c0/pOrtl31k6szN0CuLeSKhWp6q5BWQC5VzyPS7S9R7uXc0HapJdRENocQqBED3N
MCYJzFxXsHzY91S/Ax7CNQyonezrN0iBn2Xnp+PqVRLJ4+g3ekcldmSFxgkfhBt4
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIDTTCCAjUCFEdxdVXxkb5pyo1bDsNrhl6piNc0MA0GCSqGSIb3DQEBDQUAMGMx
CzAJBgNVBAYTAklUMQ0wCwYDVQQIDARSb21lMQ0wCwYDVQQHDARSb21lMRIwEAYD
VQQKDAlQaXBwbyBMdGQxDDAKBgNVBAsMA1NlYzEUMBIGA1UEAwwLY2EucGlwcG8u
aXQwHhcNMjMwMjIyMTAyNjU2WhcNMjYwMjIxMTAyNjU2WjBjMQswCQYDVQQGEwJJ
VDENMAsGA1UECAwEUm9tZTENMAsGA1UEBwwEUm9tZTESMBAGA1UECgwJUGlwcG8g
THRkMQwwCgYDVQQLDANTZWMxFDASBgNVBAMMC2NhLnBpcHBvLml0MIIBIjANBgkq
hkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA0Gstl72iPqteNKjQ+FbfeYuAc9bINffy
uSJae1A9jZcTlQRrE6rmNrRIi/wyHfskKsDlD3dljp+swkxf6+YTqV0XGp2V+t/W
T3I2G0vZcQudC3fqQ+Ith1rtp8MvjqD6BLX6YK7C5M6jHQLLHODhu+CtglVShGTu
FkolMiR96CJoy/OWnRazyA8nkRH9aIqJcP3c4J/05WcomXIUdtWE9F5V79HSA5rz
u7WjJp5bEc8QGbbtV/WboydEIybdtzR7QzVE0x8sicvXVq2myZ7gZzZS6PbDu9gz
NDBdmuxZzB2Y25ynmVNVtzNiSPmXo9xpO4vT6u9VedzgrMw+X8Wx7QIDAQABMA0G
CSqGSIb3DQEBDQUAA4IBAQCQvgicSTsmZ+xLXMwisvimN7mplEJQ7wvfIvESKqZu
Kc8jKvoqDoP1t/YeJN7b/OI3YBG2WcB8gXqXt9oKxHG3coiGWgVj+1HNVMg2A5EF
omaEBAsmDPXZvs17u5o0tqwIO8gg+Z8jazGXqdDdDxpc7H/LbEouEVv7k6OKGZSh
YOATlIt93AIDclZKvG8JxzpAnsDqpShHfvw1sH8hHGK89C5puC9jsPHLceuszTKN
V9iU+f3+CUGlIsr0bTDlPSQ5yzlw5+/xc3fynrgvSCvOMxq08rQCpZDEbQlIYk/q
4J8Ir82U4D6sjy98DoWH/kGBRFKBFdoCNUwhUYwkNpFW
-----END CERTIFICATE-----
```


