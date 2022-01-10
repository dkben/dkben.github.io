---
layout: post
title:  "Vue JS 執行 API 取值設定後， 顯示 [__ob__: Observer]"
subtitle: "雖然有透過 API 取到值，後端的返回值看起來沒有問題"
date:   2021-03-01 09:00:00 +0800
categories: VueJs
meta_description: "Vue JS 執行 API 取值設定後， 顯示 [__ob__: Observer]，雖然有透過 API 取到值，後端的返回值看起來沒有問題"
tags:
- "VueJs"
---

Vue JS 執行 API 取值並設定到變數後， console顯示 \[\_\_ob\_\_: Observer\]

雖然有透過 API 取到值，後端的返回值看起來沒有問題

但是郤無法讓值在畫面上的 select option 成功渲染

![](/images/medium/1__2MKKMCOxaoGj__Rar__8jEPQ.png)

不管是在 VueJS 中的 created, mount 中執行，都只會取得 Observer 觀察者陣列

網路上的討論說明是，這是代表 VueJS 在觀察值，所以出現 Observer 觀察者名稱

應該是 Axios 預設是異步處理，所以在賦值時，API 還沒有成功返回值，我的理解是這樣

最後找到解決方法

在 created 方法的調用，改用 async，然後執行 API 時使用 await，讓它使用同步方式取值設定值

```javascript
async created () {  
  let res = await this.getBase();  
  this.customers = JSON.parse(res.data.customers);  
  this.employees = JSON.parse(res.data.employees);
},
```

取值的 API

```javascript
methods: {  
  getBase() {  
    return axios.get('/admin/purchase/api-get-purchase-base')  
  }  
}
```


渲染的部份

```vue
<div class_="col-sm-6">  
  <div class_="form-group">  
    <label>客戶</label>  
    <select name="" id="" class="form-control" v-model="selectCustomer">  
      <option value="0">請選擇...</option>  
      <option :value="customer.id" v-for="customer in customers">{{ customer.name }}</option>  
    </select>  
  </div>  
</div>
```