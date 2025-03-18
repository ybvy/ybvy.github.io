---
author: "ybvy"
layout: post
title:  "IP Header Explain"
date: 2025-03-18
comments: true
description: Giải thích về IP Header
categories: introduce
tags: 
- security
- python
---

# Giới thiệu
IP Header là phần tiêu đề của 1 gói tin IP(Internet Protocol) khi truyền đi trong mạng hay internet. Nó gồm nhiểu trường khác nhau, chứa các thông tin quan trọng để định tuyến và truyền dữ liệu qua mạng. Mỗi gói tin IP bao gồm 2 phần chính:
* IP Header
* Data

![IP Header](/assets/images/posts/2025-03-18-ip-header-explain/ip_header.png)

# Các thành phần của IP Header
## Version
* Vị trí: Octec 0 - 4 Bit đầu 
* Chức năng: Chỉ định phiên bản của `IP packet`.

|First Header | Second Header|
|------------ | -------------|
|Content from cell 1 | Content from cell 2|
|Content in the first column | Content in the second column|
