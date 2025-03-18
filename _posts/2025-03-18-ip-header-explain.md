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

# Câc thành phần của IP Header
## Version
* Vị trí: Octec 0 - 4 Bit đầu 
* Chức năng: Chỉ định phiên bản của `IP packet`.
| Giá trị | Phiên bản | Mô tả |
|---------|----------|--------|
| 4 | IPv4 | Phiên bản IP phổ biến nhất hiện nay |
| 6 | IPv6 | Phiên bản IP mới, cải thiện về không gian địa chỉ và bảo mật |
| 5 | ST (Internet Stream Protocol, RFC 1819) | Một giao thức thử nghiệm, không được sử dụng rộng rãi |
| 7-9 | Để dành | Dự trữ cho các mục đích tương lai |
| 10-15 | Không hợp lệ | Không được sử dụng chính thức |
