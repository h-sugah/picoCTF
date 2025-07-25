# john_pollard
Author: Samuel S  

### URL
https://play.picoctf.org/practice/challenge/6  

### Description
Sometimes RSA certificates are breakable  

<br>
<br>

## 解答の過程
「certificates」をクリックすると、「cert」ファイルをダウンロードすることができます。  
certファイルは、テキスト形式のファイルであるため、まずはエディターで内容を確認します。  
PEM形式の証明書のようです。  

```
-----BEGIN CERTIFICATE-----
MIIB6zCB1AICMDkwDQYJKoZIhvcNAQECBQAwEjEQMA4GA1UEAxMHUGljb0NURjAe
Fw0xOTA3MDgwNzIxMThaFw0xOTA2MjYxNzM0MzhaMGcxEDAOBgNVBAsTB1BpY29D
VEYxEDAOBgNVBAoTB1BpY29DVEYxEDAOBgNVBAcTB1BpY29DVEYxEDAOBgNVBAgT
B1BpY29DVEYxCzAJBgNVBAYTAlVTMRAwDgYDVQQDEwdQaWNvQ1RGMCIwDQYJKoZI
hvcNAQEBBQADEQAwDgIHEaTUUhKxfwIDAQABMA0GCSqGSIb3DQEBAgUAA4IBAQAH
al1hMsGeBb3rd/Oq+7uDguueopOvDC864hrpdGubgtjv/hrIsph7FtxM2B4rkkyA
eIV708y31HIplCLruxFdspqvfGvLsCynkYfsY70i6I/dOA6l4Qq/NdmkPDx7edqO
T/zK4jhnRafebqJucXFH8Ak+G6ASNRWhKfFZJTWj5CoyTMIutLU9lDiTXng3rDU1
BhXg04ei1jvAf0UrtpeOA6jUyeCLaKDFRbrOm35xI79r28yO8ng1UAzTRclvkORt
b8LMxw7e+vdIntBGqf7T25PLn/MycGPPvNXyIsTzvvY/MXXJHnAqpI5DlqwzbRHz
q16/S1WLvzg4PsElmv1f
-----END CERTIFICATE-----
```

まず、opensslコマンドを使い、X.509フォーマットにして内容を確認します。  

```
> openssl x509 -in cert -text -noout
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number: 12345 (0x3039)
        Signature Algorithm: md2WithRSAEncryption
        Issuer: CN=PicoCTF
        Validity
            Not Before: Jul  8 07:21:18 2019 GMT
            Not After : Jun 26 17:34:38 2019 GMT
        Subject: OU=PicoCTF, O=PicoCTF, L=PicoCTF, ST=PicoCTF, C=US, CN=PicoCTF
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (53 bit)
                Modulus: 4966306421059967 (0x11a4d45212b17f)
                Exponent: 65537 (0x10001)
    Signature Algorithm: md2WithRSAEncryption
    Signature Value:
        07:6a:5d:61:32:c1:9e:05:bd:eb:77:f3:aa:fb:bb:83:82:eb:
        9e:a2:93:af:0c:2f:3a:e2:1a:e9:74:6b:9b:82:d8:ef:fe:1a:
        c8:b2:98:7b:16:dc:4c:d8:1e:2b:92:4c:80:78:85:7b:d3:cc:
        b7:d4:72:29:94:22:eb:bb:11:5d:b2:9a:af:7c:6b:cb:b0:2c:
        a7:91:87:ec:63:bd:22:e8:8f:dd:38:0e:a5:e1:0a:bf:35:d9:
        a4:3c:3c:7b:79:da:8e:4f:fc:ca:e2:38:67:45:a7:de:6e:a2:
        6e:71:71:47:f0:09:3e:1b:a0:12:35:15:a1:29:f1:59:25:35:
        a3:e4:2a:32:4c:c2:2e:b4:b5:3d:94:38:93:5e:78:37:ac:35:
        35:06:15:e0:d3:87:a2:d6:3b:c0:7f:45:2b:b6:97:8e:03:a8:
        d4:c9:e0:8b:68:a0:c5:45:ba:ce:9b:7e:71:23:bf:6b:db:cc:
        8e:f2:78:35:50:0c:d3:45:c9:6f:90:e4:6d:6f:c2:cc:c7:0e:
        de:fa:f7:48:9e:d0:46:a9:fe:d3:db:93:cb:9f:f3:32:70:63:
        cf:bc:d5:f2:22:c4:f3:be:f6:3f:31:75:c9:1e:70:2a:a4:8e:
        43:96:ac:33:6d:11:f3:ab:5e:bf:4b:55:8b:bf:38:38:3e:c1:
        25:9a:fd:5f
```

Public Key Infoのサブジェクトをみると、通常のRSA公開鍵より短いbit数（53bit）であり、Modulusが4966306421059967、Exponentが65537であることが確認できます。  

Hint1を参照すると、「The flag is in the format picoCTF{p,q}」と書かれています。  
Modulusはnで現わされるので、RSA暗号で説明される、n = p * qの関係からpとqを求めれば良いことになりそうです。  

nからpとqを求めるために、Pythonのsympyモジュールを使って素数を導き出すコードを作成しました。  
コードは、以下のようになります。  

```
from sympy import factorint

n = 4966306421059967  // X.509フォーマットで得られたModulusをセットする。

factors = factorint(n)

print(factors)
```

上記のコードを実行すると、以下の結果が出力されます。  

```
{73176001: 1, 67867967: 1}
```

73176001, 67867967をそれぞれp, qとして、指定されたフラグフォーマットにします。  

<br>
<br>
<br>
<br>

## フラグ
> picoCTF{73176001,67867967}  

<br>
<br>

## この問題は
素数に関して学ぶ問題だと思われます。  
なお、タイトルのJohn Pollardさんは、数学者とのことです。  

参考：[John Pollard (mathematician)](https://en.wikipedia.org/wiki/John_Pollard_(mathematician))  
