---
title: " [實作筆記] 怎麼建立一個網站？(一) - 購買網域到設定DNS"
date: 2016/08/21 22:12:53
tags:
  - 實作筆記
---

## 前置作業

1. 準備好你的 google 帳號。
2. 可以連到美國的 VPN。[google domain beta](https://atom.io/packages/atom-beautify) 台灣尚未開放
3. 準備一張信用卡，狠狠的刷下去(挑對 domain 其實很便宜啦)
4. 你的網站，什麼語言都可以，靜態的網頁也可以。  
   \*這裡我事先準備好了兩個網站,  
   分別是用[github page](https://pages.github.com/) 與 [nodejs](https://nodejs.org) 的 [express](http://expressjs.com/),實作有機會再作記錄。

## 設定 domain

1. Github page 所建立的網站,會提供一組 domain 給你  
   ex:`myDomain.github.io`

2. OpenShift 建立的網站,一樣會提供一組 domain 給你  
   ex:`myDomain.rhcloud.com`

3. google domain 本身有提供 Name servers , 但是由於 type`A` 的 Domain,  
    必須指定公用 ip(家中有裝 HiNet ADSL 可以申請一組);但實際上我的兩個網站,並不需要我準備實體 IP,  
    只需要使用 type `CName` 將我的 subdomain 指向原本服務的 domain 即可。  
    ex:

   - blog.myDomain.me → myDomain.github.io
   - www.myDomain.me → myDomain.rhcloud.com

4. github page 要注意的事情，

你需要在你的 github page 的 repo root 加入一個名叫`CName`的檔案,
檔案的內容很簡單, 只需要你的 domain 即可  
ex:
blog.myDomain.me  
 myDomain.me

## Bare CName

大多數的時候, `CName` 的設定就夠了;不過對於我來說,  
我會希望可以直接使用我的頂級 domain,畢竟這樣網址可以更短一些,  
`myDomain.me` 比 `www.myDomain.me` 更有感覺。

這裡受限於 google 的 Name servers , 頂級 domain 必為 Type`A`須綁定 ip  
這裡我們可以使用一個免費的服務[cloudflare](https://www.cloudflare.com/),來達成目的

1. 註冊[cloudflare](https://www.cloudflare.com/)
2. 登入後[add site](https://www.cloudflare.com/a/add-site),輸入你註冊的 domain
   ex:`myDomain`
3. [cloudflare](https://www.cloudflare.com/) 會提供你至少兩組 Name servers  
   ex:  
   `carter.ns.cloudflare.com`  
   `tina.ns.cloudflare.com`
4. 請先登入[google domain beta](https://atom.io/packages/atom-beautify)設定 Name servers 到[cloudflare](https://www.cloudflare.com/)
5. 請依以下步驟設定

   - \* → myDomain.me
   - blog.myDomain.me → myDomain.github.io
   - www.myDomain.me → myDomain.rhcloud.com

6. 等待約數分鐘就 ok 啦

## 結語

前前後後查資料弄了一個禮拜，但是實際上設定大概 1~2 小時就搞定了。  
對 domain 跟 ip 的相對關係與實務結合後更有體會了。

歡迎指教討論。

## 系列文章

- [怎麼建立一個網站？(一)](https://blog.marsen.me/2016/08/21/2016/setting_DNS_with_google/)
- [怎麼建立一個網站？(二)](https://blog.marsen.me/2016/08/28/2016/how_to_use_github_page/)
- [怎麼建立一個網站？(三)](https://blog.marsen.me/2016/09/04/2016/http2_by_cloudflare/)
- ~~[怎麼建立一個網站？(四)](https://blog.marsen.me/2020/10/22/2020/google_domain_forward_mail/)~~
- [怎麼建立一個網站？(四)](https://blog.marsen.me/2025/08/27/2025/brevo_smtp/)
- [怎麼建立一個網站？(五)](https://blog.marsen.me/2021/04/06/2021/create_404/)

(fin)
