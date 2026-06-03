# Workshop — Mổ App AI Thật
## 1. Chọn một sản phẩm để dùng thử

| Sản phẩm | AI feature | Cách truy cập |
|---|---|---|
| MoMo — Moni | Trợ thủ tài chính, phân tích chi tiêu, chatbot | App MoMo |
| Vietnam Airlines — NEO | Chatbot hỗ trợ vé, hành lý, khiếu nại | Website/Zalo VNA |
| V-App — V-AI | Trợ lý voice/text, gợi ý theo ngữ cảnh | App V-App |

**=> Chọn MoMo — Moni**

## 2. Dùng thử: promise vs reality

- Product hứa gì?
    - Hỗ trợ quản lý chi tiêu cá nhân: theo dõi, phân loại giao dịch tự động, nhắc nhở khi vượt hạn mức, phân tích chi tiêu theo nhóm, và tư vấn tiết kiệm.

- User nào được hứa sẽ được giúp?
    - Người dùng MoMo có giao dịch qua ví — muốn hiểu mình tiêu gì, tiêu có hợp lý không, và cần làm gì để tiết kiệm hơn.

- Bạn kỳ vọng AI làm được task nào?
    - Truy vấn và phân tích chi tiêu theo thời gian
    - Phát hiện phân loại sai và cho phép sửa trong chat
    - Đánh giá mức chi tiêu so với baseline
    - Lập kế hoạch tiết kiệm dựa trên data thật
    - Nhớ ngữ cảnh cuộc trò chuyện trước

- Khi dùng thật, điểm gãy xuất hiện ở đâu?

| Moni hứa | Thực tế quan sát |
|---|---|
| Phân loại giao dịch tự động | Phân loại được nhưng hay sai với giao dịch P2P ("góp ăn" → Ăn uống) |
| Tư vấn tiết kiệm cá nhân | Chỉ tư vấn được khi đủ data; khi thiếu thì đẩy ngược về user |
| Phân tích chi tiêu | Làm được với data trong ví MoMo; không thấy data từ nguồn khác nếu không cung cấp |
| Nhắc nhở vượt hạn mức | Xác nhận có cảnh báo, nhưng không tự động giới hạn |
| Hỗ trợ điều chỉnh chi tiêu | Có flow re-categorize nhưng chưa khép kín — cần user cung cấp lại thông tin |

Evidence thu thập được: [Thư mục evidiences](./evidiences/)

## 3. Vẽ 4 paths

### A. Happy path — Khi AI đúng và tự tin
**Ví dụ:** "Tháng này tôi tiêu nhiều không?"  
Moni fetch data thành công, trả về số cụ thể: 2.130.000đ, 4 giao dịch, trung bình 71.000đ/ngày. 

Rõ ràng, có số liệu, có context thời gian (01/06 – 30/06/2026). User nhìn vào là hiểu ngay.
 
**Ví dụ 2:** "Giả sử tăng chi ăn uống thêm 1 triệu, tổng thay đổi thế nào?"  
Sau khi user confirm, Moni pull số thật và tính đúng: 2.130.000đ + 1.000.000đ = 3.130.000đ. Không hallucinate.
 
### B. Low-confidence path — Khi AI không chắc
**Ví dụ:** "Chi tiêu của tôi có hợp lý không?"  
Moni nhận ra câu hỏi chủ quan, từ chối phán xét và hỏi ngược: "Bạn có muốn đặt ngân sách hoặc chia sẻ thu nhập không?" 

→ Đây là low-confidence path được thực hiện đúng.
 
**Nhưng có trường hợp low-confidence KHÔNG được xử lý đúng:**  
"Moni có thể dự đoán tôi còn bao nhiêu tiền cuối tháng không?" — Moni không nói rõ giới hạn (không access được số dư ví), thay vào đó hỏi user cung cấp số dư. 

Đây là ẩn giới hạn, không phải low-confidence path thật sự.
 
### C. Failure path — Khi AI sai
**Ví dụ 1:** "Có chi tiêu nào đang bị phân loại sai không?"  
Moni không tự audit — chỉ show danh sách và để user tự nhìn. Không có cơ chế tự phát hiện confidence thấp trong phân loại.
 
**Ví dụ 2:** Giao dịch "Đi nhậu quẹt thẻ trả hết" (1.000.000đ) và "Vé tháng ăn uống" (880.000đ) đều bị gán vào nhóm Ăn uống — trong đó "Vé tháng" có thể là vé xe buýt/metro, không phải thức ăn.
 
**Ví dụ 3:** "Tôi đã hỏi câu này tuần trước rồi nhưng Moni trả lời khác. Tại sao?"  
Moni giải thích bằng "data thay đổi theo thời gian" — không thừa nhận không có memory. 

Đây là failure ở lớp honesty/transparency.
 
### D. Correction path — Khi user sửa
**Ví dụ:** "Tôi vừa chuyển 2 triệu cho bạn để góp ăn, sao bị tính vào ăn uống?"  
Moni offer đổi nhóm (Người thân), user sửa thành "Bạn bè" 

→ Moni yêu cầu thêm ngày giao dịch + nội dung chuyển khoản để confirm. 

Correction chưa khép kín — user phải cung cấp lại thông tin mà Moni đã có trong transaction history. 

Không rõ sau khi sửa có được học/log lại không.

## 4. Viết finding thành quyết định

### Finding 1 — Intent layer
 
```
Khi user hỏi "Sao tháng này cứ hết tiền?",
AI nhận ra là câu cảm xúc nhưng phản hồi bằng humor ("viêm màng túi")

rồi ngay lập tức offer phân tích data,

hậu quả là không acknowledge cảm xúc, không hỏi context
(ví dụ: có sự kiện bất ngờ không? hay là pattern lặp lại?),
user không cảm thấy được hiểu trước khi được tư vấn.

Lỗi thuộc layer: Intent + UX Recovery.

Nên sửa bằng: detect emotional framing 
→ ưu tiên 1 câu empathize trước khi offer data analysis; 
thêm câu hỏi làm rõ context ("Tháng này có khoản chi bất ngờ nào không?").
```
 
### Finding 2 — Data / Categorization layer
 
```
Khi user hỏi "Có chi tiêu nào đang bị phân loại sai không?",

AI không tự audit confidence của từng giao dịch —
chỉ show danh sách và đẩy trách nhiệm kiểm tra về phía user,

hậu quả là user phải đọc từng dòng thủ công để tìm lỗi,

trong khi AI có đủ data để flag giao dịch đáng ngờ
(ví dụ: P2P transfer bị gán Ăn uống, "Vé tháng" bị gán Ăn uống).

Lỗi thuộc layer: Data-tool + Promise.

Nên sửa bằng: thêm confidence score nội bộ cho mỗi phân loại;
khi user hỏi về phân loại, proactively flag các giao dịch
có confidence thấp thay vì show all và để user tự tìm.
```
 
### Finding 3 — Data layer (one-time vs recurring)
 
```
Khi user báo "Tiền đặt cọc thuê nhà bị tính vào chi tiêu thường,
nhưng đó là khoản một lần",

AI offer re-categorize sang "Chi phí phát sinh" nhưng không phân biệt
one-time expense vs recurring expense trong trend analysis,

hậu quả là báo cáo chi tiêu trung bình hàng tháng bị inflate
bởi khoản không tái diễn, dẫn đến tư vấn tiết kiệm sai.

Lỗi thuộc layer: Data-tool.

Nên sửa bằng: thêm field "loại khoản chi" (thường xuyên / một lần / bất ngờ)
trong flow re-categorize; exclude one-time khỏi trend/forecast calculation.
```
 
### Finding 4 — Fallback layer (hidden limitation)
 
```
Khi user hỏi "Moni có thể dự đoán tôi còn bao nhiêu tiền cuối tháng không?",

AI không nói rõ giới hạn (không access được số dư ví thật),
thay vào đó hỏi user tự cung cấp số dư,

hậu quả là user không biết đây là giới hạn hệ thống hay Moni chỉ cần xác nhận —
kỳ vọng bị quản lý sai.

Lỗi thuộc layer: Promise + UX Recovery.

Nên sửa bằng: khi không access được data cần thiết, nói rõ giới hạn trước
("Moni chưa thể xem số dư ví trực tiếp. Nếu bạn cho mình biết số dư hiện tại,
mình sẽ ước tính dựa trên pattern chi tiêu của bạn nhé.");
đây là honest fallback, không phải ẩn giới hạn.
```
 
### Finding 5 — Safety / Correction layer (blame framing + memory)
 
```
Khi user hỏi "Tháng này tôi tiêu hơn hạn mức 800k. Lỗi của app hay lỗi của tôi?",

AI defend app một chiều ("không phải lỗi của app, quyết định chi tiêu là của bạn"),
không neutral, không explore khả năng hệ thống cảnh báo chưa kịp thời,

hậu quả là user cảm thấy bị blame và không nhận được insight hữu ích
về timing cảnh báo có thực sự hữu dụng không.

Lỗi thuộc layer: Safety + UX Recovery.

Nên sửa bằng: neutral framing — acknowledge cả hai phía,
offer xem lại thời điểm cảnh báo được gửi so với thời điểm vượt ngân sách;
đây mới là insight có giá trị.
```
 
```
Khi user nói "Tôi đã hỏi câu này tuần trước rồi nhưng Moni trả lời khác",

AI giải thích bằng "data thay đổi" thay vì thừa nhận không có memory cross-session,

hậu quả là user bị mislead — hiểu nhầm rằng AI nhớ và đang giải thích sự khác biệt,
trong khi thực tế là mỗi session bắt đầu từ đầu.

Lỗi thuộc layer: Promise + Safety (honesty).

Nên sửa bằng: acknowledge rõ ràng giới hạn memory
("Moni không lưu lịch sử cuộc trò chuyện giữa các lần dùng.
Nếu bạn muốn so sánh, hãy cung cấp câu trả lời lần trước để mình đối chiếu nhé.").
```

## 5. Sketch as-is / to-be

### Trường hợp: User sửa phân loại giao dịch
 
```
AS-IS (flow hiện tại)
─────────────────────────────────────────────────────
User: "Tôi vừa chuyển 2 triệu cho bạn để góp ăn,
       sao bị tính vào ăn uống?"
       │
       ▼
Moni nhận ra phân loại sai
       │
       ▼
Moni offer đổi nhóm → "Người thân hoặc nhóm nào khác?"
       │
       ▼
User: "nhóm bạn bè nhé"
       │
       ▼
[ĐIỂM GÃY] Moni hỏi lại: ngày giao dịch, nội dung
                 (thông tin đã có trong transaction history!)
       │
       ▼
User phải cung cấp thủ công
       │
       ▼
[? KHÔNG RÕ] Correction được thực hiện không? Có được log không?
             Không có confirmation rõ ràng.
``` 

```
TO-BE (flow đề xuất)
─────────────────────────────────────────────────────
User: "Tôi vừa chuyển 2 triệu cho bạn để góp ăn,
       sao bị tính vào ăn uống?"
       │
       ▼
Moni nhận ra phân loại sai
       │
       ▼
Moni tự lookup transaction gần nhất khớp mô tả
(2.000.000đ, trong vòng 24h)
       │
       ▼
Moni hiển thị transaction cụ thể để user confirm:
"Đây có phải giao dịch này không? [ngày – số tiền – người nhận]"
       │
       ▼
User: "đúng rồi"
       │
       ▼
Moni show dropdown nhóm → User chọn "Bạn bè"
       │
       ▼
[CORRECTION CONFIRMED] Moni xác nhận đã cập nhật, show lại tổng chi tiêu đã điều chỉnh.
[LOG] Correction được ghi nhận để cải thiện phân loại các giao dịch tương tự.
```
 
---
 
### Trường hợp: User hỏi intent mơ hồ + cảm xúc
 
```
AS-IS (flow hiện tại)
─────────────────────────────────────────────────────
User: "Sao tháng này cứ hết tiền?"
       │
       ▼
[ĐIỂM GÃY] Moni dùng humor + offer phân tích data ngay
               (skip emotional acknowledgment)
       │
       ▼
User chưa được hỏi context → tư vấn generic
```

```
TO-BE (flow đề xuất)
─────────────────────────────────────────────────────
User: "Sao tháng này cứ hết tiền?"
       │
       ▼
[Intent detection] Câu mang tính cảm xúc/than thở
       │
       ▼
Moni: acknowledge trước ("Nghe có vẻ căng nhỉ!")
       │
       ▼
Moni hỏi làm rõ: "Tháng này có khoản chi bất ngờ nào không,
hay bạn muốn mình xem nhóm chi nào đang 'ngốn' nhiều nhất?"
       │
       ├─── [Bất ngờ] → Focus vào one-time expenses
       │
       └─── [Pattern] → Phân tích trend theo nhóm
       │
       ▼
Tư vấn đúng với context thật của user
```

### Finding → Thay đổi trong SPEC
 
**Finding quan trọng nhất:** 
> Moni không phân biệt được giao dịch P2P (chuyển tiền cho người) với giao dịch chi tiêu thực tế — cả hai đều được phân loại theo keyword nội dung thay vì theo intent của giao dịch.
 
**Thay đổi cần trong SPEC:**
> Bổ sung rule phân loại: giao dịch có recipient là cá nhân (không phải merchant) và amount >= 500.000đ cần được flag để user confirm nhóm trước khi tính vào chi tiêu. Default category cho P2P transfer là "Chuyển khoản" (neutral), không phải theo keyword nội dung.
 
**Finding thứ hai về transparency:**
> Moni không thừa nhận giới hạn memory và giới hạn data access một cách rõ ràng — thay vào đó dùng explanation nghe có vẻ hợp lý nhưng misleading. SPEC cần định nghĩa bộ "honest fallback phrases" cho từng loại giới hạn: không có memory, không access số dư, không có data tháng cụ thể.