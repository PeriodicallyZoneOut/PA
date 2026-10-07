# Phân tích ba ý tưởng đồ án: Maintenance Management, BriefCheck và LoopGuard

**Người chuẩn bị:** Nguyễn Minh Tú — 23120101  
**Ngày:** 06/10/2026  
**Mục đích:** So sánh ý tưởng, vai trò LLM theo yêu cầu PA#1 và khả năng phát triển thành đồ án môn Phát triển ứng dụng web nâng cao.

## 1. Cơ sở đánh giá

Tài liệu được đối chiếu:

- [Rubric PA#1 — Proposal and planning](<PA%231 rubric — Proposal and planning.pdf>).
- [Maintenance Request Management](tu-maintainance-req-management-proposal-and-planning.md).
- [BriefCheck](briefcheck-proposal.pdf).
- [LoopGuard](LoopGuard-Project-Proposal.pdf).

Phân tích dưới đây phân biệt **nội dung đã có trong proposal** và **đề xuất cần bổ sung khi triển khai**. Các nhận định về khả năng làm đồ án là đánh giá kỹ thuật, không phải xác nhận của giảng viên. Chưa có đầy đủ rubric các mốc sau hoặc lịch checkpoint chính thức để kết luận chắc chắn về điểm cuối kỳ.

### PA#1 yêu cầu gì ở ý tưởng và LLM?

| Tiêu chí | Điểm | Điều cần thể hiện |
|---|---:|---|
| Vấn đề và người dùng | 20 | Người dùng cụ thể, hoàn cảnh cụ thể, một vấn đề rõ ràng và cách họ đang xử lý hiện nay |
| Tính năng LLM và hậu quả khi sai | 25 | Một tính năng cụ thể; ai bị ảnh hưởng, mức thiệt hại, khả năng khắc phục và cách phát hiện lỗi |
| Phạm vi | 15 | Nêu rõ làm gì và không làm gì trong học kỳ; nhất quán với kế hoạch |
| Kế hoạch và trách nhiệm | 20 | Sáu checkpoint có công việc, người chịu trách nhiệm được nêu tên và ngày phù hợp lịch môn học |
| Rủi ro | 10 | Hai rủi ro có thể làm dự án thất bại, kèm biện pháp bắt đầu được trong tuần này |
| Công nghệ | 10 | Lý do chọn từng công nghệ gắn với dự án, gồm nhà cung cấp mô hình và chi phí |

**LLM không bắt buộc phải là toàn bộ sản phẩm hoặc giải quyết vấn đề lớn nhất của sản phẩm.** Rubric yêu cầu một tính năng cụ thể, có ích và có hậu quả khi sai; không quy định tỷ lệ chức năng phải dùng AI. Một bước hỗ trợ phân loại yêu cầu vẫn phù hợp nếu có người dùng, quyết định được hỗ trợ và cách đánh giá rõ ràng.

Rubric cũng nói tham vọng không được chấm điểm: sản phẩm nhỏ với tính năng LLM rõ ràng có lợi hơn sản phẩm lớn nhưng vai trò LLM mơ hồ. Vì vậy, số lượng màn hình hoặc mức độ nghiêm trọng của hậu quả không tự động làm proposal tốt hơn.

## 2. Maintenance Request Management — Quản lý yêu cầu bảo trì

### 2.1. Ý tưởng và vấn đề

Ứng dụng cho một tòa nhà, phục vụ ba vai trò: cư dân, quản lý và nhân viên kỹ thuật. Cư dân gửi yêu cầu sửa chữa; quản lý duyệt và phân công; kỹ thuật viên cập nhật công việc; hệ thống lưu bình luận, hình ảnh và lịch sử trạng thái.

**Luồng chính:** Gửi yêu cầu → quản lý xem xét → phân công → thực hiện/cập nhật → hoàn tất; có quy trình xin mở lại yêu cầu.

Giả thuyết cần kiểm chứng là việc tiếp nhận qua điện thoại, Zalo và bảng tính khiến người quản lý khó theo dõi trách nhiệm, còn cư dân phải hỏi lại tiến độ. Các persona trong proposal là minh họa, chưa phải kết quả phỏng vấn.

### 2.2. Tính năng LLM theo PA#1

**Đầu vào:** Mô tả tự do bằng tiếng Việt hoặc tiếng Anh.  
**Đầu ra:** Gợi ý nhóm sự cố, mức ưu tiên, nhóm kỹ thuật phụ trách và giải thích ngắn.  
**Người sử dụng:** Quản lý tiếp nhận yêu cầu.  
**Quyền quyết định:** Quản lý xác nhận hoặc sửa gợi ý rồi chọn kỹ thuật viên; LLM không tự phân công hoặc thay đổi trạng thái.

Đây là một tính năng thống nhất: **hỗ trợ phân loại ban đầu**, dù có nhiều trường đầu ra. Vai trò của LLM là diễn giải mô tả không theo mẫu, không phải thực hiện các thao tác CRUD.

| Khi LLM sai | Ai chịu ảnh hưởng và hậu quả | Phát hiện và kiểm soát |
|---|---|---|
| Gợi ý sai nhóm kỹ thuật | Cư dân chờ lâu hơn; nhân viên mất thời gian chuyển việc | So sánh với dữ liệu có nhãn; ghi nhận quản lý sửa gợi ý và lý do |
| Đánh giá ưu tiên quá cao | Quản lý phân bổ nguồn lực chưa hợp lý | Theo dõi bất đồng và tỷ lệ nâng ưu tiên sai |
| Đánh giá quá thấp một báo cáo nguy hiểm | Có thể trì hoãn phản ứng; một số thiệt hại không thể khắc phục | Đánh giá riêng ca nguy hiểm, cảnh báo độc lập với LLM và quy trình liên hệ khẩn cấp |

Ví dụ “Strong gas smell in kitchen” cho thấy giới hạn quan trọng: việc quản lý có thể sửa gợi ý **không đủ để bảo đảm xử lý khẩn cấp kịp thời**. Proposal đã đặt cảnh báo và hướng dẫn liên hệ khẩn cấp độc lập, đồng thời không cho LLM gỡ cờ nguy hiểm. Luật từ khóa cũng có thể bỏ sót; không phát hiện từ khóa không có nghĩa là an toàn.

Proposal dự kiến 120 báo cáo có nhãn, tách dữ liệu phát triển và kiểm tra; các mục tiêu độ chính xác và giảm thời gian duyệt là mục tiêu tương lai, chưa phải kết quả đã đạt.

### 2.3. Điểm mạnh

- Nghiệp vụ dễ giải thích và dễ trình diễn bằng một quy trình xuyên suốt.
- Có nhiều vấn đề web thực chất: phân quyền theo bản ghi, quản lý tệp riêng tư, chuyển trạng thái, giao việc và lịch sử thay đổi.
- Phần mềm vẫn tiếp nhận và xử lý thủ công khi API AI lỗi hoặc hết ngân sách.
- Dễ xác định ranh giới giữa gợi ý của AI và quyết định chịu trách nhiệm của con người.
- Có thể đánh giá lợi ích bằng thời gian duyệt, độ đúng của gợi ý và số lần người quản lý chỉnh sửa.

### 2.4. Điểm yếu và việc cần bổ sung

- Phạm vi dễ phình to nếu thêm thanh toán, kho vật tư, nhiều tòa nhà hoặc điều phối nhà thầu.
- Cần người hiểu nghiệp vụ để kiểm chứng nhãn ưu tiên; dữ liệu giả lập không thay thế được hoàn toàn tình huống thực tế.
- Nếu chỉ làm các màn hình thêm/sửa/xóa, đồ án sẽ chưa thể hiện rõ chiều sâu kỹ thuật.
- Cần đặc tả xử lý khi hai quản lý cùng phân công, quyền truy cập sau khi chuyển người phụ trách, và tính nhất quán giữa trạng thái với lịch sử.
- Cần phân biệt phản hồi sửa gợi ý với nhãn chuẩn: một lần quản lý override chưa tự chứng minh rằng mô hình sai.

### 2.5. Mức phù hợp cho học phần

**Phù hợp nhất để thể hiện năng lực xây dựng ứng dụng web đầy đủ.** Chiều sâu nên đến từ phân quyền phía máy chủ, giao dịch dữ liệu, quy tắc nghiệp vụ, kiểm thử và xử lý lỗi tích hợp. Không cần thêm microservices hay nhiều công nghệ để làm dự án trông “nâng cao”.

Phạm vi hợp lý: một tòa nhà, ba vai trò, một quy trình bảo trì, một tính năng LLM có thể tắt để quay về xử lý thủ công.

## 3. BriefCheck — Checklist bài tập có dẫn chứng

### 3.1. Ý tưởng và vấn đề

Ứng dụng giúp sinh viên chuẩn bị nộp bài không bỏ sót yêu cầu nằm rải rác trong đề, rubric, bảng và ghi chú.

**Luồng chính:** Tải đề và rubric → trích xuất checklist → xem từng mục cùng trích dẫn và số trang → sửa/xác nhận → lưu và xuất Markdown.

Người dùng mục tiêu trong proposal là sinh viên CSC13114 chuẩn bị nộp PA#1. Cách hiện tại là đọc lại tài liệu và tự ghi chú. Proposal dự kiến quan sát ba bạn cùng lớp để kiểm chứng nhu cầu.

### 3.2. Tính năng LLM theo PA#1

**Đầu vào:** Một đề và một rubric tiếng Anh, dạng PDF có văn bản.  
**Đầu ra:** Các yêu cầu có thể thực hiện, điều kiện áp dụng, trích dẫn nguồn và số trang; đánh dấu điểm mơ hồ.  
**Người sử dụng:** Sinh viên kiểm tra bài trước khi nộp.  
**Quyền quyết định:** Sinh viên chỉnh sửa và đánh dấu đã kiểm tra; ứng dụng không tự nộp bài hay chấm điểm.

LLM đóng vai trò trung tâm vì phải diễn giải yêu cầu với nhiều cách viết và điều kiện. Ví dụ, rubric PA#1 phải tạo được mục yêu cầu có `SELF_ASSESSMENT_REPORT.md` trong tệp nộp.

| Khi LLM sai | Ai chịu ảnh hưởng và hậu quả | Phát hiện và kiểm soát |
|---|---|---|
| Bỏ sót một tệp bắt buộc | Sinh viên có thể bị trừ điểm | Đối chiếu checklist chuẩn do người đọc tài liệu lập |
| Hiểu sai điều kiện cá nhân/nhóm | Sinh viên chuẩn bị sai cách nộp | Đánh giá riêng các yêu cầu có điều kiện; hiển thị đoạn gốc |
| Bịa thêm yêu cầu hoặc dẫn chứng | Sinh viên làm thêm việc không cần thiết | Kiểm tra trích dẫn có tồn tại và đo số mục không được nguồn hỗ trợ |

Theo rubric đang có, thiếu báo cáo tự đánh giá bị trừ 10 điểm ở mốc PA#1. Đây là hậu quả cụ thể; trước hạn có thể sửa, sau hạn phụ thuộc chính sách nộp lại. Tuy nhiên, **trích dẫn hợp lệ không chứng minh checklist đầy đủ**, và câu trích dẫn tồn tại cũng chưa chứng minh cách diễn giải điều kiện là đúng.

### 3.3. Điểm mạnh

- Vấn đề và đối tượng gần với nhóm phát triển; thuận tiện tìm người thử và tài liệu được phép sử dụng.
- Tính năng LLM rõ, gắn trực tiếp với giá trị của sản phẩm và hậu quả khi sai.
- Phạm vi tương đối gọn: PDF văn bản tiếng Anh, chỉnh checklist, lưu và xuất; không OCR, không LMS, không viết bài hộ.
- Có hướng đánh giá cụ thể bằng yêu cầu bị bỏ sót, mục không có căn cứ, điều kiện sai và thời gian người dùng sửa.
- Phù hợp để làm hoàn chỉnh từ đầu đến cuối trong một học kỳ.

### 3.4. Điểm yếu và việc cần bổ sung

- Bảng và bố cục PDF có thể làm mất liên hệ giữa điều kiện và yêu cầu trước cả bước gọi LLM.
- Nếu người dùng vẫn phải đọc lại toàn bộ tài liệu để sửa nhiều lỗi, lợi ích tiết kiệm thời gian có thể không còn.
- Bản triển khai chỉ tải tệp rồi hiển thị câu trả lời AI sẽ có chiều sâu web hạn chế.
- Bộ mười cặp tài liệu dự kiến còn nhỏ; cần tách tài liệu dùng chỉnh prompt khỏi tài liệu kiểm tra cuối cùng, không xem kết quả trên vài rubric là khả năng tổng quát.
- Cần phân biệt phiên bản checklist do AI tạo và phiên bản người dùng sửa, tránh ghi đè chỉnh sửa khi chạy lại.

### 3.5. Mức phù hợp cho học phần

**Phù hợp cho cá nhân hoặc nhóm muốn một sản phẩm nhỏ nhưng hoàn thiện và kiểm chứng được.** Chiều sâu kỹ thuật có thể đến từ xử lý tài liệu, truy xuất dẫn chứng, trạng thái tác vụ, lưu chỉnh sửa, bảo vệ tài liệu và kiểm thử cả luồng tải lên–xuất kết quả.

Những bổ sung này là hướng triển khai, không phải yêu cầu bắt buộc của rubric PA#1. Không cần thêm cộng tác hay tích hợp LMS chỉ để tăng số tính năng.

## 4. LoopGuard — Phát hiện vòng lặp trong workflow tự động hóa

### 4.1. Ý tưởng và vấn đề

Ứng dụng phân tích các workflow tự động hóa để tìm đường kích hoạt vòng tròn trước khi chúng được đưa vào sử dụng.

**Ví dụ:** Workflow A cập nhật bản ghi → kích hoạt B → B gọi webhook → kích hoạt lại A.

**Luồng trong proposal:** Tải các tệp JSON workflow → loại metadata không cần thiết → LLM phân tích liên kết → hiển thị đường vòng lặp, mức độ và giải thích.

Người dùng là đội vận hành, DevOps hoặc người xây dựng tự động hóa ít kinh nghiệm lập trình. Proposal đề cập n8n/Zapier; khả năng lấy đủ cấu hình cần thiết từ từng nền tảng vẫn cần kiểm chứng.

### 4.2. Tính năng LLM theo PA#1

**Đầu vào:** JSON của nhiều workflow cùng thông tin trigger, action và điều kiện còn lại sau tiền xử lý.  
**Đầu ra:** Liên kết nghi ngờ tạo vòng lặp, mức độ và giải thích.  
**Người sử dụng:** Người thiết kế hoặc duyệt workflow trước triển khai.

LLM được kỳ vọng suy luận các liên hệ gián tiếp giữa các hệ thống. Đây là một tính năng có hậu quả rõ khi sai, nhưng proposal cần xác định cụ thể bằng chứng nào cho phép suy ra từng liên kết.

| Khi LLM sai | Ai chịu ảnh hưởng và hậu quả | Phát hiện và kiểm soát cần có |
|---|---|---|
| Báo vòng lặp không tồn tại hoặc có điều kiện dừng | Người dùng mất thời gian điều tra, có thể trì hoãn triển khai | Đo precision; có ca an toàn dùng chung dịch vụ và ca chu trình có điểm dừng |
| Bỏ sót vòng kích hoạt lặp vô hạn | Có thể phát sinh nhiều API call, bản ghi trùng và gián đoạn dịch vụ | Đo recall, đặc biệt với ca nghiêm trọng; kiểm tra đường phụ thuộc có nhãn |

Proposal mô tả hậu quả có thể lên tới hàng nghìn USD, nhưng chưa có giả định về giá mỗi lần gọi, tần suất lặp hay thời gian phát hiện để chứng minh con số. Nên đưa một kịch bản tính cụ thể thay vì coi đó là thiệt hại chắc chắn.

Proposal cũng nói cảnh báo sai có thể “chặn triển khai”, trong khi phần xây dựng mới mô tả công cụ tải tệp và dashboard. Cần làm rõ đó là quyết định của người dùng hay tích hợp chặn tự động. Với học kỳ này, công cụ tư vấn để người dùng duyệt có phạm vi dễ kiểm soát hơn.

### 4.3. Điểm mạnh

- Có bài toán kỹ thuật rõ và hậu quả thực tế khi phân tích sai.
- Có cơ hội thể hiện mô hình hóa đồ thị, trực quan hóa, tiền xử lý và quản lý tác vụ phân tích.
- Đã đề cập dữ liệu đối chứng gồm 40 workflow an toàn/có vòng lặp và kiểm thử hồi quy.
- Có thể phát triển thành công cụ cho lập trình viên với kết quả phân tích truy nguyên được.

### 4.4. Điểm yếu và việc cần bổ sung

- **Thiếu thông tin đầu vào:** JSON có thể không chứa cấu hình webhook bên ngoài, workflow chưa tải lên hoặc hành vi hệ thống thứ ba. LLM không thể xác nhận đáng tin cậy điều không có trong dữ liệu.
- **Chu trình không đồng nghĩa chạy vô hạn:** Điều kiện, thay đổi trạng thái và giới hạn số lần chạy có thể làm workflow dừng.
- **Cần giải thích vì sao dùng LLM:** Khi đồ thị phụ thuộc đã xác định, thuật toán đồ thị có thể tìm chu trình. Giá trị LLM cần tập trung vào việc diễn giải quan hệ chưa được biểu diễn trực tiếp.
- **Mục tiêu >90% precision chưa đủ:** Ít cảnh báo sai vẫn có thể đi cùng nhiều vòng nguy hiểm bị bỏ sót. Cần recall và phân tích false negative.
- **Nguy cơ học theo bộ kiểm tra:** Nếu chỉnh prompt liên tục trên cùng 40 workflow, cần một tập kiểm tra độc lập để tránh đánh giá quá lạc quan.
- Trong bản PDF hiện tại, mục ngoài phạm vi và lựa chọn công nghệ chưa có nội dung; tên thành viên, repository và ngày checkpoint cần được xác nhận trước khi dùng làm cam kết.

### 4.5. Hướng triển khai phù hợp hơn

Đề xuất thu hẹp về **một nền tảng và một tập loại node/trigger được hỗ trợ**, sau đó tách các bước:

1. Parser lấy các liên kết tường minh bằng mã thông thường.
2. LLM đề xuất liên kết chưa chắc chắn và chỉ ra trường dữ liệu làm bằng chứng.
3. Thuật toán đồ thị tìm chu trình trên tập liên kết đó.
4. Giao diện phân biệt chu trình xác định, liên kết suy đoán và dữ liệu còn thiếu.
5. Người dùng kiểm tra điều kiện dừng trước khi kết luận nguy cơ chạy lặp vô hạn.

**Phù hợp với nhóm thích công cụ phân tích và có dữ liệu thực tế từ sớm.** Rủi ro là phần suy luận chiếm gần hết học kỳ, khiến phần ứng dụng web và trải nghiệm người dùng chưa hoàn chỉnh.

## 5. So sánh khả năng phát triển thành đồ án

Các mức dưới đây là nhận định tương đối theo phạm vi proposal hiện có, không phải điểm chấm.

| Khía cạnh | Maintenance Management | BriefCheck | LoopGuard |
|---|---|---|---|
| Vai trò LLM | Hỗ trợ một bước phân loại | Tạo checklist là giá trị trung tâm | Suy luận quan hệ là giá trị trung tâm |
| Chiều sâu web nổi bật | Phân quyền, nghiệp vụ, dữ liệu đồng thời, lịch sử, tệp | Xử lý tài liệu, dẫn chứng, lưu chỉnh sửa, tác vụ | Pipeline phân tích, đồ thị, kết quả phân tích |
| Khả năng tìm người dùng thử | Cần tiếp cận cư dân/quản lý | Thuận lợi trong lớp | Cần người có workflow thực tế |
| Khó khăn kiểm chứng | Nhãn ưu tiên và tình huống nguy hiểm | Tính đầy đủ và đúng điều kiện | Đủ dữ liệu để suy ra liên kết và điều kiện dừng |
| Nguy cơ vượt phạm vi | Nhiều nghiệp vụ quản lý tòa nhà | Thêm OCR, nhiều ngôn ngữ, LMS | Nhiều nền tảng và quá nhiều loại tích hợp |
| Khi LLM không dùng được | Vẫn xử lý yêu cầu thủ công | Người dùng có thể tự lập checklist, nhưng giá trị tự động giảm | Có thể phân tích liên kết tường minh nếu có parser; chưa đủ chức năng suy luận dự kiến |
| Mức khả thi tương đối | Tốt nếu giữ phạm vi một tòa nhà | Tốt nhất để làm nhỏ và hoàn chỉnh | Bất định lớn nhất |

### Những việc còn thiếu để hoàn thiện PA#1

| Dự án | Việc ưu tiên |
|---|---|
| Maintenance Management | Điền repository thật; khớp sáu ngày với lịch môn; kiểm chứng giả thuyết nghiệp vụ; xác nhận phạm vi và năng lực làm cá nhân |
| BriefCheck | Thay placeholder thành viên và repository; xác nhận lịch môn; có quyền sử dụng tài liệu đánh giá; định nghĩa yêu cầu quan trọng và cách tách tập kiểm tra |
| LoopGuard | Hoàn thiện ngoài phạm vi và công nghệ/chi phí; xác nhận người dùng cụ thể, tên/URL/ngày; kiểm chứng dữ liệu xuất; làm rõ cảnh báo hay chặn triển khai; bổ sung đo bỏ sót |

## 6. Kết luận và đề xuất lựa chọn

**Nếu mục tiêu là thể hiện cân bằng các kỹ năng của một ứng dụng web nâng cao, nên chọn Maintenance Request Management.** Dự án có nhiều tình huống nghiệp vụ và dữ liệu để chứng minh năng lực backend, frontend, phân quyền, kiểm thử và tích hợp AI. Cần ưu tiên tính đúng của luồng xử lý hơn mở rộng danh sách tính năng.

**Nếu mục tiêu là sản phẩm gọn, dễ tiếp cận người dùng và đánh giá kỹ trong học kỳ, nên chọn BriefCheck.** Tính năng LLM rõ và phù hợp PA#1; chất lượng đồ án phụ thuộc vào việc làm tốt dẫn chứng, chỉnh sửa và xử lý lỗi, không chỉ hiển thị đầu ra mô hình.

**Nếu nhóm ưu tiên bài toán phân tích kỹ thuật và chấp nhận rủi ro cao hơn, có thể chọn LoopGuard sau khi thử nghiệm tính khả thi.** Trước khi cam kết, cần chứng minh rằng dữ liệu xuất từ một nền tảng cung cấp đủ thông tin cho các trường hợp được hỗ trợ.

Không thể kết luận dự án nào chắc chắn đạt điểm cao nhất chỉ từ ý tưởng. Cả ba đều có thể đáp ứng PA#1 nếu làm rõ tính năng LLM và hậu quả khi sai; chất lượng đồ án sau đó phụ thuộc vào phạm vi thực tế, bằng chứng đánh giá và mức hoàn thiện của ứng dụng.
