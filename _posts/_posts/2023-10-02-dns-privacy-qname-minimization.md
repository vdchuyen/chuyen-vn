Lâu quá không bàn gì về kỹ thuật, hôm nay chắc bàn về một thứ mà ít người *biết* kẻo lụt nghề :P

Vào cỡ 2015, 2016 [Stephane Bortzmeyer](https://dblp.org/pid/186/8836.html) ở APNIC Pháp soạn thảo một vài bản ghi nhớ trong đó có RFC7816 nhằm mục đích tăng cường tính bí mật của hệ thống phân giải tên miền mạng toàn cầu. 

Ý tưởng đơn giản ban đầu là hạn chế thông tin không cần thiết khi đi hỏi các server các cấp khác nhau. Ví dụ thay vì một người dùng tên Chim dùng mạng X ten gì đấy truy cập một trang thien.dia.com, nếu trong ngữ cảnh bình thường thì 100% nhà mạng X ten ấy sẽ có dữ liệu truy cập của user đấy. Tuy nhiên nếu phiên bản phần mềm phân giải của người dùng Chim có hỗ trợ tính năng *qname minimization* thì lúc ấy các server dns sẽ chỉ thấy _.thien.dia.com, _.dia.com và _.com tương ứng với từng cấp mà không thể thấy hết người dùng Chim truy cập địa chỉ đó, cụ thể hơn thì firefox có một bài viết khá [trực quan](https://hacks.mozilla.org/2018/05/a-cartoon-intro-to-dns-over-https/)

Tuy nhiều phần mềm phân giải tên miền đã thêm tính năng này vào tuy nhiên có nhiều bất cập, ví dụ nếu bạn quản lý server DNS có traffic khá lớn thì sẽ thấy một lượng lớn request NXDOMAIN bắt đầu từ underscore (_) gửi đến, do phần xử lý [lẫn lộn](https://ns1.com/blog/why-qname-minimization-can-lead-to-increased-nxdomain-responses) giữa record NS và A.

Stephane Bortzmeyer có cập nhật tính năng này lên thành một phiên bản khác RFC9156 để nhằm thay thế cho RFC7816 và mở rộng QTYPE thay vì NS như trước thì có thể dùng bất kỳ loại record nào và một thuật toán mới và tăng tỉ lệ cache, số lượng query do dùng ít dữ liệu hơn tuy nhiên tỉ lệ fail vẫn có thể cao hơn.

Tóm lại ngoài các tính năng như DNSSEC, DOH, DOT để tăng cường bảo mật, bí mật của hệ thống DNS thì bên cạnh đó DNS còn nhiều vấn đề, ý tưởng để mở rộng, chủ yếu cũng vì một mục đích đảm bảo an toàn cho nền tảng Internet chung.

Bên cạnh đó, mong các anh chị, các bạn đang giữ các hệ thống DNS chung và riêng luôn cập nhật, xử lý các vấn đề chung để hệ thống của mình được nhanh và tối ưu hơn.
