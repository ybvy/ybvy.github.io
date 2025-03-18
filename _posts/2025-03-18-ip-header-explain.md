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
## version
* Vị trí: Octec 0 (4 Bit đầu)
* Chức năng: Chỉ định phiên bản của IP packet.

| Giá trị | Phiên bản | Mô tả |
|---------|----------|--------|
| 4 | IPv4 | Phiên bản IP phổ biến nhất hiện nay |
| 6 | IPv6 | Phiên bản IP mới, cải thiện về không gian địa chỉ và bảo mật |
| 5 | ST (Internet Stream Protocol, RFC 1819) | Một giao thức thử nghiệm, không được sử dụng rộng rãi |
| 7-9 | Để dành | Dự trữ cho các mục đích tương lai |
| 10-15 | Không hợp lệ | Không được sử dụng chính thức |
{:.inner-borders}
---
## IHL (Internet Header Length)
* Vị trí: Octec 0 (Bit 4 -> Bit 7)
* Chức năng: Xác định kích thước của phần Header

Vì ở đây là 4 bit nên max value là 15(1111) nhưng min value lại là 5(0101)
### Công thức tính kích thước Header
{% highlight bash %}
length(header) = IHL * 4
min(length(header)) = 5 * 4 = 20(bytes) // Header IPv4 chuẩn
max(length(header)) = 15 * 4 = 60(bytes) 
{% endhighlight %}

Nếu ở IHL=5 thì chỉ chứa Header IPv4 chuẩn, không có phần Option (giải thích ở dưới). Vì nó chỉ có 20 byte, bắt đầu từ byte 0 đến byte 19, quan sát hình ở trên bạn sẽ thấy rằng phần Option nó xuất hiện từ byte 20.