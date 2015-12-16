---
layout: post
title:  "DNSSEC update"
date:   2015-12-17 1:36:42
categories: blog
---

Cập nhật một số thông tin triển khai DNSSEC của VNNIC:

VNNIC đã cập nhật lộ trình triển khai DNSSEC cho zone .vn, hy vọng đến cuối năm 2016 sẽ sign xong toàn zone .vn, hiện tại thông tin chủ yếu xoay quanh [đào tạo](http://www.vnnic.vn/sites/default/files/VNNIC-ChuongTrinhDaoTaoDNSSEC-Nangcao.pdf) là chính:

Hiện tại việc triển khai dnssec đã dễ dàng hơn trước rất nhiều, phần lớn nhờ nỗ lực của các công ty Internet lớn như Google hay Cloudflare.

Google domain và Cloudflare tích hợp dễ dàng khi triển khai dnnsec, ví dụ [http://dnsviz.net/d/vdchuyen.com/dnssec](http://dnsviz.net/d/vdchuyen.com/dnssec). Với Cloudflare chỉ cần enable DNSSEC thì Cloudflare sẽ tự generate DS records, Algorithm index, Token và hash. Sau đó cập nhật các thông tin này lên registrar là Google domain control panel thì domain sẽ được enable DNSSEC.
