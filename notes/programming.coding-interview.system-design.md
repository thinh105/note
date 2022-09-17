---
id: fbxmpswvy84s18z94k681g8
title: System Design
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 10.8326
longitude: 106.6581
altitude: 0
title_imported: System Design
updated_imported: '2022-05-08T11:29:38.000Z'
created_imported: '2022-02-08T16:30:05.000Z'
---

[TOC]


# [What was the most useful thing you’ve read about system design?](https://www.teamblind.com/post/What-was-the-most-useful-thing-youve-read-about-system-design-ccPEkEWM)


# https://leerob.io/blog/style-guides-component-libraries-design-systems

# [Book: System Design Interview – An Insider's Guide: Volume 2](https://www.amazon.com/System-Design-Interview-Insiders-Guide/dp/1736049119)

> I wish there are more books like this. Besides this book, I recommend the following:
> 
> - system design primer github by donnemartin
>     
> - eng blogs: uber, airbnb, discord, facebook, netflix, etc.
>     
> - some important papers: scaling memcache at facebook, google’s 3 papers in big data,
>     
> - design data-intensive applications by Martin Kleppmann
>     
> - system design interview by alex xu, volume 1. This is more basic but it’s still a good read.
>     
> - youtube channels: system design interview channel, uber engineering, infoq, and hussein nasser.
>     
> - find mock interview partners. This can be really helpful
>     

# [Prep for system design](https://phong.medium.com/how-i-get-an-offer-from-facebook-eb3a1ce65939#:~:text=of%20Image%3A%20internet%29-,System%20design,-This%20is%20my)

![15e2306ae59dee05c6b947253434595d.png](/assets/15e2306ae59dee05c6b947253434595d-xaxu9w0f2dml.png)

Thật sự thì trước giờ đây là điểm yếu nhất của mình.

Một phần là do trước giờ không chú ý tới interview dạng này lẫn trong quá trình làm cũng không gặp phải vấn đề scaling gì nhiều vì toàn dev app cho internal là chính.

Trong quá trình phỏng vấn trước giờ, đôi lúc cũng có cty hỏi về system design.

Lần nào cũng ngớ người ra mặc dù có đọc một vài bài về system design rồi. Nhiều lúc nghĩ system design interview có vẻ quá sức đối với mình.

Cho nên lần này quyết tâm phải có cách tiếp cận khác vì Facebook cũng như các cty top tier khác khá là serious về mảng này.

Thậm chí ở một số level có hẳn 2 system design interviews.
Và đây là cách mình đã làm:

- Search và đọc các post/comment trên blind (hoăc forum tương tự như leetcode) về system design. Đặc biệt là về system design interview cho facebook. Mình ưng nhất là post này  https://www.teamblind.com/post/My-Approach-to-System-Design-V4SJARdx
- [tryexponent](https://www.tryexponent.com/refer/gvkyk)  — Thấy rất hữu dụng, rất là sát với system design interview của facebook. Đặc biệt là cái series mock interview. Các bạn sẽ học được cách dẫn dắt một system design interview thế nào và các kiến thức về system design nói chung. Các bạn lưu ý là trong cái interview này, inteviewer expects các bạn phải dẫn dắt toàn bộ trong buổi phỏng vấn. Các bạn để interviewer chủ động hỏi càng nhiều thì khả năng tạch càng cao.
- Xem các video về system design trên youtube, các bạn nên làm việc này sau khi đã làm 2 bước trên để lựa xem video nào phù hợp nhất cho interview, vì khá nhiều nội dung sida.

Sau tất cả, mình đã hiểu cách design tầm vài system phổ biến như facebook, instagram, messenger, parking lot, ...
Mình đúc kết các bước trong một system design interview như sau (mình để tiếng anh vì thật ra mình thấy ko nên dịch ra lắm vì khá technical):

- Taking the problem into specific features - Định nghĩa rõ ràng các tính năng cụ thể
- Estimate the non-functional requirements - Ước lượng các yêu cầu phi tính năng.
- Design the APIs, from this point, go all the way to the backend as drawing the system components. - Thiết kế API, bắt đầu vẽ các system component từ đây cho xuống tới backend
- Should mention about “interface” for API-Client Layer (GraphQL, Restful) - Đề cập về các chuẩn API như GraphQL, Restful, ... pros and cons và nêu ra lý do tại sao lựa chọn nó.
- Design the Entities, Data Models => Database design - Thiết kế database
- Discuss choices about the database (SQL, NoSQL) - Thảo luận về các loại database và chọn loại phù hợp, nêu lý do.
- Early verify the design by test all the features from API to the database with the simple case. - Test sớm các tính năng từ api xuống datase với các use case đơn giản.
- Then the fun part: scaling, go from top to bottom again to identify bottlenecks and add sufficient scaling ability to the design (LB, Cache, Message Queue, Replication,…) - Phần này quan trọng, tìm các bottlenecks và tìm cách scale chúng ra.
- Handle special cases - Xử lý các trường hợp đặc biệt, edge cases
- Walkthrough all the feature from top to bottom again with all cases and failures - Test các tính năng lần nữa, lần này với toàn bộ trường hợp cũng như sự cố.

Với cách này mình đạt điểm E5 trong system design interview. Trong toàn bộ quá trình, interviewer không hỏi mình gì nhiều ngoại trừ special case. Cũng không ngờ phần mình yếu nhất lại làm tốt nhất

Một số tài liệu về system design mình cũng dùng tới:
- https://github.com/donnemartin/system-design-primer
- Grokking the System Design Interview ([10% Discount Referral Link](https://educative.io/signup?referralCode=phonglk-3jMEKXw4R4A))
- [System Design Interview — An insider’s guide, Second Edition](https://amzn.to/3cpGRne)