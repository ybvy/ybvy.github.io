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

---
## TOS(Type of Service)
TOS (Type of Service) là một trường 8 bit trong IPv4 Header, được sử dụng để xác định ưu tiên và chất lượng dịch vụ của gói tin IP.
* Vị trí: Octec 1 (Bit 8 -> Bit 15)
* Chức năng: Được dùng để kiểm soát độ ưu tiên, độ trễ, thông lượng và độ tin cậy của gói tin.
* Bao gồm 2 thành phần chính: DSCP và ECN
### DSCP (Differentiated Services Code Point)
DSCP (6 bit) là một phần của trường TOS, được sử dụng để phân loại mức độ ưu tiên của gói tin trong mạng.

* Vị trí: 6 bit đầu tiên của byte thứ 2 trong IPv4 header.
* Chức năng: Giúp các bộ định tuyến (router) ưu tiên xử lý một số gói tin nhất định (ví dụ: thoại, video, dữ liệu quan trọng).

| Tên                     | Giá trị DSCP (Decimal) | Binary  | Ứng dụng |
|-------------------------|----------------------|---------|------------|
| Default                 | 0                    | 000000  | Không ưu tiên |
| CS0 (Default)           | 0                    | 000000  | Lưu lượng bình thường |
| CS1 (Low-Priority Data) | 8                    | 001000  | Dữ liệu ít quan trọng |
| AF11                    | 10                   | 001010  | Assured Forwarding (Thấp) |
| AF21                    | 18                   | 010010  | Assured Forwarding (Trung bình) |
| AF31                    | 26                   | 011010  | Assured Forwarding (Cao) |
| AF41                    | 34                   | 100010  | Assured Forwarding (Rất cao) |
| EF (Expedited Forwarding) | 46                | 101110  | Lưu lượng quan trọng (VoIP, Video) |
| CS5 (Video)             | 40                   | 101000  | Streaming Video |
| CS6 (Network Control)   | 48                   | 110000  | Gói tin điều khiển mạng (OSPF, BGP) |
| CS7 (Reserved)          | 56                   | 111000  | Dự trữ |
{:.inner-borders}

<!-- <script src="https://giscus.app/client.js"
        data-repo="ybvy/ybvy.github.io"
        data-repo-id="R_kgDONiHcVw"
        data-category="Announcements"
        data-category-id="DIC_kwDONiHcV84ClolG"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="vi"
        crossorigin="anonymous"
        async>
</script> -->