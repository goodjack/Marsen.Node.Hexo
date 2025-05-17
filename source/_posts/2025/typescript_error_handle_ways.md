---
title: " [學習筆記] TypeScript 不同寫法的除錯難易比較"
date: 2025/05/17 13:23:41
tags:
  - 學習筆記
---

## 前情提要

最近在整理一個專案的 API 路由權限設定，  
對同樣的資料結構、不同的寫法，對於型別檢查和除錯有一點小小心得，稍微作個記錄。

## 情境說明

假設我有一個 API 權限設定如下，每個路由對應一組角色才能訪問：

```ts
{
  endpoint: "/admin",
  roles: ["admin", "superuser"]
}
```

然後我有一個函式 registerPermissions 要吃這個設定。

### 薦議寫法

先給最近建立的寫法，先定義型別，然後變數也套型別
好處是能具體的抓到錯誤，壞處是要多考慮一個型別(命名、擺放位置都會是一個要考量的點)

```ts
type PermissionConfig = {
  routes: {
    endpoint: string;
    roles: string[];
  }[];
};

const permissionConfig: PermissionConfig = {
  routes: [
    {
      endpoint: "/admin",
      roles: ["admin", "superuser"],
    },
    {
      endpoint: "/profile",
      roles: "user", // ❌ 馬上紅字報錯
    },
  ],
};

const registerPermissions = (config: PermissionConfig) => {
  // 註冊權限邏輯
};

registerPermissions(permissionConfig);
```

### 先定義變數，但沒套型別

最不推薦的寫法，錯誤一大坨，難以閱讀，但是寫起來最順手最快，不用考慮命名與檔案擺放的位置，  
現在的 AI 時代，也許錯誤可以理解的更快。但是我還是不建議，畢竟習慣這種寫法後，  
容易錯上加錯，單一的錯誤 AI 還好理解，錯中錯 AI 可能就會昏頭了  

```ts
const permissionConfig = {
  routes: [
    {
      endpoint: "/admin",
      roles: ["admin", "superuser"],
    },
    {
      endpoint: "/profile",
      roles: "user", // 🧨 這裡故意寫錯，應該是 array
    },
  ],
};


const registerPermissions = (config: {
  routes: {
    endpoint: string;
    roles: string[];
  }[];
}) => {
  // 註冊權限邏輯
};


registerPermissions(permissionConfig);
```

這種寫法會噴出一堆錯誤

### 寫法三：直接 inline 傳入函式

如果沒有共用必要的參數，我一般都蠻推薦 inline 的，  
當然有一些前提，ex: 方法的複雜度要夠低，讓人容易理解
而且也可以省去命名的思考與型別檔案擺放位置的判斷。

```ts
const registerPermissions = (config: {
  routes: {
    endpoint: string;
    roles: string[];
  }[];
}) => {
  // 註冊權限邏輯
};

registerPermissions({
  routes: [
    {
      endpoint: "/admin",
      roles: ["admin", "superuser"],
    },
    {
      endpoint: "/profile",
      roles: "user", // ❌ 馬上紅字報錯
    },
  ],
});
```

## 小結：推薦寫法

最推薦第一種，但實務上我可能會更多的選擇第三種寫法。
要有上下文，請團隊作好自已的判斷。

(fin)
