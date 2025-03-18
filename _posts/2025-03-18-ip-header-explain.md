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
* Payload

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

---

### DSCP (Differentiated Services Code Point)
DSCP (6 bit) là một phần của trường TOS, được sử dụng để phân loại mức độ ưu tiên của gói tin trong mạng.
* Vị trí: 6 bit đầu tiên của byte thứ 2 trong IPv4 header.
* Chức năng: Giúp các bộ định tuyến (router) ưu tiên xử lý một số gói tin nhất định (ví dụ: thoại, video, dữ liệu quan trọng).

| Tên                     |  Decimal | Binary  | Ứng dụng |
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

---

###  ECN (Explicit Congestion Notification) 
ECN là một cơ chế giúp phát hiện và kiểm soát tắc nghẽn mạng mà không cần drop (hủy bỏ) gói tin.
* Vị trí: ECN là 2 bit cuối cùng của byte DSCP/ECN trong IP Header.
* Ứng dụng: Hỗ trợ kiểm soát tắc nghẽn trong TCP/IP mà không mất gói tin.

| ECN Value | Binary | Ý nghĩa |
|-----------|--------|---------|
| 00        | 00     | **Not-ECT (Non-ECN Capable Transport)** – Không hỗ trợ ECN |
| 01        | 01     | **ECT(1) (ECN-Capable Transport 1)** – Hỗ trợ ECN |
| 10        | 10     | **ECT(0) (ECN-Capable Transport 0)** – Hỗ trợ ECN |
| 11        | 11     | **CE (Congestion Experienced)** – Xác nhận tắc nghẽn |

---

## Total Length
Total Length là một trường 16-bit trong IP Header.
* Vị trí: Thuộc byte 2 và 3 (Bit 16 -> Bit 31).
* Úng dụng: Biểu thị tổng kích thước của toàn bộ gói tin IP, bao gồm Header và Payload (dữ liệu).
* Giá trị tối thiểu: 20 bytes (khi chỉ có IP Header, không có dữ liệu).
* Giá trị tối đa: 65,535 bytes (giới hạn bởi kích thước 16-bit).

### Công thức tính kích thước Total Length
{% highlight bash %}
Total Length=IP Header Length+Payload Length
{% endhighlight %}

Nếu Total Length > MTU (Maximum Transmission Unit), gói tin sẽ bị chia nhỏ (fragmentation)

| Kích thước (Byte)  | Ý nghĩa |
|-------------------|----------------------------------|
| 20 - 1,500       | Kích thước phổ biến trong mạng Ethernet (MTU = 1500 bytes). |
| 1,500 - 65,535   | Yêu cầu phân mảnh do vượt quá MTU. |
| > 65,535         | Không hợp lệ trong IPv4. |
{:.inner-borders}

---

## Identification
Identification (ID) là một trường 16-bit trong IP Header.
* Vị trí: Thuộc byte 4 và 5.
* Úng dụng: Dùng để xác định từng gói tin IP riêng lẻ. Khi một gói tin bị phân mảnh, tất cả các mảnh sẽ có cùng ID để bộ thu có thể ghép lại đúng thứ tự.

---

## Flags (3) + Fragment Offset (13)
Flags + Fragment Offset là một trường 16-bit trong IP Header.
* Vị trí: Nằm ở byte 6 và 7.

### Flag
* Vị trí: 3 bit đầu của byte 6.
* Ứng dụng: Flags giúp xác định xem gói tin có bị phân mảnh hay không.

| Bit  | Tên                  | Ý nghĩa |
|------|----------------------|------------------------------------------------|
| Bit 0 | Reserved            | Luôn bằng **0** (Không sử dụng). |
| Bit 1 | DF (Don't Fragment) | **1** = Không phân mảnh gói tin.<br>**0** = Cho phép phân mảnh. |
| Bit 2 | MF (More Fragments) | **1** = Còn nhiều mảnh phía sau.<br>**0** = Đây là mảnh cuối cùng. |
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