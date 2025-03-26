---
author: "waibui"
layout: post
title:  "Psdir - My web path scan tool"
date: 2025-03-27
comments: true
description: Công cụ quét đường dẫn website dựa trên wordlist của tôi.
categories: introduce
toc: true
tags: 
- security
- python
- web
---

# Giới thiệu
Đây là security tool đầu tay của tay mơ waibui, code tuy lỏ nhưng chạy đủ nhanh, option vừa đủ phục vụ cho nhu cầu pentest của tui.
Thực ra có rất nhiều tool mạnh mẽ và nhiều chức năng hơn, nhưng dùng cái do mình tự tạo ra cảm thấy thỏa mãn, custom tùy ý, không sợ cái mẹ chi hết...
Tui tạo cái này do nhu cầu của tui, phân tích riết cũng mệt, code cho vui nhà vui cửa...

# Cấu trúc
{% highlight bash %}
psdir
.
├── controllers
│   ├── controller.py
│   ├── __init__.py
│   └── scan.py
├── core
│   ├── dependencies.py
│   ├── __init__.py
│   └── setting.py
├── models
│   ├── __init__.py
│   └── option.py
├── parse
│   ├── __init__.py
│   └── parse_cmd.py
├── statics
│   ├── common.txt
│   ├── user-agent.txt
│   └── wordlist.txt
├── views
│   ├── banner.py
│   ├── file_logging.py
│   ├── __init__.py
│   └── logging.py
├── __init__.py
├── LICENSE
├── psdir.py
├── README.md
├── requirements.txt
└── setup.py
{% endhighlight %}

<script src="https://giscus.app/client.js"
        data-repo="waibui/waibui.github.io"
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
</script>