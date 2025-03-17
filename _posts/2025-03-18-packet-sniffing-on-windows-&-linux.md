---
author: "ybvy"
layout: post
title:  "Packet sniffing on Windows and Linux"
date: 2025-03-18
comments: true
description: Packet sniffing trên Windows và Linux
categories: introduce
tags: 
- security
---

# Giới thiệu
Packet sniffing là quá trình bắt và phân tích gói tin truyền qua mạng. Kỹ thuật này được sử dụng để giám sát lưu lượng mạng, phân tích lỗi, kiểm tra bảo mật hoặc thậm chí thực hiện các cuộc tấn công mạng. Trên Windows và Linux, việc triển khai packet sniffing có một số điểm khác biệt do cách xử lý raw sockets của từng hệ điều hành.

# Raw Sockets là gì?
Raw sockets là loại socket cho phép truy cập trực tiếp vào gói tin ở mức thấp, bao gồm cả phần tiêu đề (header) và dữ liệu (payload). Điều này giúp lập trình viên có thể bắt, phân tích, và thậm chí tạo các gói tin tùy chỉnh.

## Ứng dụng
* Giám sát và phân tích mạng (tương tự Wireshark)
* Phát hiện và khai thác lỗ hổng bảo mật
* Tạo công cụ tấn công mạng như ARP Spoofing, Man-in-the-Middle (MITM)
* Xây dựng tường lửa hoặc hệ thống phát hiện xâm nhập (IDS/IPS)

# Packet Sniffing trên Windows
Trên Windows, raw sockets bị giới hạn, đặc biệt từ Windows 10 trở lên, do Microsoft vô hiệu hóa khả năng gửi gói tin tùy chỉnh nhằm ngăn chặn các cuộc tấn công mạng.

## Các bước thực hiện
* Tạo raw socket với giao thức IPPROTO_IP
* Bật chế độ promiscuous mode bằng IOCTL (SIO_RCVALL)
* Bắt gói tin và phân tích dữ liệu
* Tắt promiscuous mode sau khi hoàn tất

## Ví dụ mã Python
>Python
{% highlight python linenos %}
import socket
import os

HOST = '192.168.1.203'

def main():
    if os.name == 'nt':
        socket_protocol = socket.IPPROTO_IP
    else:
        socket_protocol = socket.IPPROTO_ICMP

    sniffer = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket_protocol)
    sniffer.bind((HOST, 0))
    sniffer.setsockopt(socket.IPPROTO_IP, socket.IP_HDRINCL, 1)
    
    if os.name == 'nt':
        sniffer.ioctl(socket.SIO_RCVALL, socket.RCVALL_ON)
    
    print(sniffer.recvfrom(65565))
    
    if os.name == 'nt':
        sniffer.ioctl(socket.SIO_RCVALL, socket.RCVALL_OFF)

if __name__ == '__main__':
    main()
{% endhighlight %}

# Packet Sniffing trên Linux
Linux cung cấp nhiều quyền kiểm soát hơn đối với raw sockets so với Windows, giúp dễ dàng thực hiện packet sniffing.

## Các bước thực hiện
* Tạo raw socket với giao thức IPPROTO_ICMP
* Bind socket vào giao diện mạng
* Nhận và phân tích gói tin

## Ví dụ mã Python
>Python
{% highlight python linenos %}
import socket

sniffer = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_ICMP)
sniffer.bind(('0.0.0.0', 0))
print(sniffer.recvfrom(65535))
{% endhighlight %}