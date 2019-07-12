---
title: 解决一个微信小程序登录问题
date: 2019-07-12 14:05:17
tags: [微信小程序]
---

### 问题描述

对于已经授权过的用户，index页面每次进入时会先显示授权页，之后自动切换到个人中心

### 期望

已经授权的就不要进入点击按钮授权页

### 解决

- 原来的情况是

    ```wxml
    <!--index.wxml-->
    <block wx:if="{{!authed}}">
        授权页面
    </block>
    <block wx:else>
        个人中心
    </block>
    ```

    ```javascript
    // index.js
    this.data = {
        authed: false;
    }
    ```

- 修改后

    ```wxml
    <!--index.wxml-->
    <block wx:if="{{unauthed}}">
        授权页面
    </block>
    <block wx:else>
        个人中心
    </block>
    ```

    ```javascript
    // index.js
    this.data = {
    }

    onLoad: function() {
        if (wx.getStorageSync('authed')) {
            this.postAuthed()
        } else {
            this.setData({unauthed: true})
        }
    }
    ```

### 分析

这个问题原来折腾了很久也没弄好。因为之前一直都是默认设置page的authed为false，这样不等js调用，页面渲染出来的总会显示授权页面。如今改了思路，默认是authed，这样显示的默认就会是个人中心页面，只有检查到没有auth，才显式地将`unauthed`置为true，这时才去授权页。
