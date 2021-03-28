# 對稱加密與非對稱加密
## CIA
- 機密性(Confidentiality)：確保訊息只有被授權者才能取得
- 完整性(Integrity)：偵測訊息是否遭受竄改
- 身分認證(Authentication)：傳送方與接受方需驗證識別
- 不可否認性(Non-Reputation)：提供訊息傳送方與接受方的交易證明


## DES/Triple DES、AES
```
傳送方與接收方的加解密皆使用同一把密鑰，
所以只要雙方都擁有這把鑰匙，
當傳送方傳資料時，
使用這把鑰匙加密，
接收方收到訊息後，
再用同一把鑰匙解密，
就能解開訊息了
```

## RSA

```
每個使用者都擁有一對金鑰：
公開金鑰(Public key)及私密金鑰(Private key)，
公開金鑰能被廣泛的發佈與流傳，
而私密金鑰則必須被妥善的保存；
當訊息由其中一把金鑰加密後，
就必須用另一把金鑰解密，
加解密的鑰匙要是完整一對(pair)的，
所以可以是公鑰加密私鑰解密，
也可以是私鑰加密公鑰解密。
但無法公鑰自己加密，公鑰自己解密；
或私鑰自己加密然後私鑰自己解密。
```

```
運作原理是傳送方與接收方在傳送之前，
先把彼此的公鑰傳給對方，
當傳送方要傳送時，
就用接收方的公鑰將訊息加密，
接收方收到加密訊息後，
再用自己的密鑰解開，
這樣即使有心人拿到公鑰，
只要沒拿到接收方的私鑰，
也還是無法解密訊息。
```

- 加密
- 簽章
    - 資訊驗證
    - 不可否認性

- 範例
    - 加密
        - https://www.devglan.com/online-tools/rsa-encryption-decryption
    - 簽章
        - https://8gwifi.org/rsasignverifyfunctions.jsp

# 公開金鑰基礎建設 (Public Key Infrastructure) (PKI)

- A certificate authority (CA) : 一個 憑證授權單位 (CA)，用來儲存、發行、簽署數位憑證
- A registration authority : 一個 註冊機構 (RA)，用來驗證申請憑證的實體身分 (個人、法人、應用程式、...)
- A central directory : 一個 中央目錄，用來儲存金鑰與檢索金鑰的地方
- A certificate management system : 一套 憑證管理系統，用來管理像是存取憑證、傳遞已簽發憑證的系統
- A certificate policy : 一套 憑證管理政策，用來規範維護 PKI 基礎架構下所有的管理程序，確保憑證與金鑰的安全性


# CSR (Certificate Signing Request) / PKCS #10
- 作為業務需要發名片時，需要跟公司申請名片，此時需要填寫申請表
- 員工申請名片時提供的資料 (個人資訊與 public key)
- 副檔名
    - *.csr

# Certificate Authority (CA)

- 公司
- 發名片 (憑證)
    - 透過任職公司所獨有的 private key 頒發 (掛保證蓋印章)
    - 名片內容除了發名片的人的資訊還同時包含 public key

## 舉例假設
- 人跟人的溝通只能透過 public key 才能對話翻譯 (外人拿 public key, 自己拿 private key)
- 每個人都有一個唯一的一組 public key 與 private key
- 到職的時候要拿 public key 去跟公司申請名片

## 身分認證與中繼憑證
- 怎麼確定發給我名片的人真的是員工
- 透過發名片的公司的 public key 確認，問公司說這個真的是你們員工嗎?
- 怎麼知道這間公司不是假人頭公司？可以再問政府說真的有這間公司嗎？
- 怎麼知道這個政府單位不是假的政府單位？再往更高階層的認證單位詢問


# 憑證編碼格式

## DER (Distinguished Encoding Rules)
- 檔案內容為二進位格式
- 副檔名
    - *.der
    - *.cer (常用於Windows作業系統)
    - *.crt (這種副檔名比較不好猜格式，建議少用)

## PEM (Privacy Enhanced Mail)
- 檔案內容為文字格式
- 以 -----BEGIN ***----- 開頭，以 -----END ***----- 結尾
- 副檔名
    - *.pem
    - *.cert (常用於Linux作業系統)
    - *.crt (這種副檔名比較不好猜格式，建議少用)

> CRT 與 CER 或 CERT 都是 Certficiate (憑證) 這個單字的縮寫

# 憑證檔案格式(內容)

## P7B / PKCS#7
- 檔只會包含憑證與中繼憑證，不會包含私密金鑰。
- 副檔名
    - *.p7b
    - *.p7c
    - *.keystore (在 Java 開發環境下常用這個副檔名)

## PKCS#8
- 檔主要包含私密金鑰，且這份私密金鑰必須設定密碼！
- 副檔名
    - *.key
    - *.pkcs8.key

## PFX / PKCS#12 (predecessor of PKCS#12)
### PFX
- 在 Windows 平台下，IIS 改用 PFX 檔案格式，將憑證與金鑰結合在一個檔案裡，並加入一些擴充屬性(Metadata)。
- 副檔名
    - *.pfx
### PKCS#12 
- 後來 PKCS 推出一份 PKCS#12 規格 (二進位格式)，可以在一個檔案中同時包含憑證、中繼憑證、私密金鑰，取代 PFX 成為標準格式。
- 副檔名
    - .p12
    - *.pkcs12

# Reference
- https://medium.com/@RiverChan/%E5%9F%BA%E7%A4%8E%E5%AF%86%E7%A2%BC%E5%AD%B8-%E5%B0%8D%E7%A8%B1%E5%BC%8F%E8%88%87%E9%9D%9E%E5%B0%8D%E7%A8%B1%E5%BC%8F%E5%8A%A0%E5%AF%86%E6%8A%80%E8%A1%93-de25fd5fa537

- https://blog.miniasp.com/post/2018/04/21/PKI-Digital-Certificate-Format-Convertion-Notes

- https://progressbar.tw/posts/98
