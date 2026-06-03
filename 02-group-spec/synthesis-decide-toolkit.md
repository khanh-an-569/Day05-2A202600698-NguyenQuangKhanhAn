# Toolkit — Từ Evidence Đến Build Slice

Dùng sau khi nhóm đã có evidence. Mục tiêu là chốt một build slice đủ nhỏ cho Day 06.

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

Các cụm evidence của nhóm:

- **"Không biết mình thiếu skill gì so với job mục tiêu"** — User muốn apply vị trí mới nhưng không biết bắt đầu từ đâu. Evidence: self-use Coursera (không có gap analysis), Reddit reviews ("I spend more time researching which course to take than actually learning").

- **"Lộ trình học one-size-fits-all, bắt buộc học lại phần đã biết"** — Coursera career track cố định 8 khóa từ beginner dù user đã biết 60%. Evidence: self-use Coursera, user review ("The track forces me to start from scratch").

- **"AI phân tích được nhưng không lưu, không track, phải làm lại mỗi lần"** — User dùng ChatGPT paste CV+JD → output tốt nhưng mất sau khi đóng tab. Evidence: Reddit r/careerguidance, self-use ChatGPT.

- **"Công cụ gap analysis có sẵn nhưng dừng ở danh sách skill, không chỉ đường học"** — Kickresume, ResuFlex chỉ list "bạn thiếu X, Y, Z" nhưng không gợi ý nguồn học cụ thể, không có timeline. Evidence: self-use competitor apps.

## 2. Viết insight

```text
User người đi làm 1-2 năm hoặc mới ra trường muốn apply vị trí mới không chỉ cần danh sách khóa học hay hoặc career track có sẵn.

Họ thật ra cần một hệ thống hiểu profile hiện tại, so sánh với mục tiêu cụ thể, và chỉ ra CHÍNH XÁC phần nào cần học — theo thứ tự, có nguồn, có timeline, và track được tiến độ,

vì 4/4 cụm evidence đều cho thấy: platform hiện tại hoặc không đọc profile user (Coursera), hoặc chỉ dừng ở gap analysis mà không đưa learning path (Kickresume/ResuFlex), hoặc có analysis tốt nhưng không persist (ChatGPT). Kết quả: tỷ lệ dropout 85-95%, user mất hàng giờ tự research nhưng vẫn không chắc mình học đúng thứ.
```

## 3. Viết opportunity

```text
Cơ hội là dùng AI để đọc CV + JD → tự động phân tích skill gap → gợi ý lộ trình học cá nhân hoá chỉ gồm phần user thật sự thiếu, kèm nguồn học cụ thể và timeline,

giúp user biết chính xác cần học gì, theo thứ tự nào, từ nguồn nào, trong bao lâu — thay vì tự research hàng giờ hoặc học lại phần đã biết,

trong khi vẫn kiểm soát rủi ro AI đánh giá sai skill level bằng cách: hiển thị confidence score, cho user review + chỉnh từng skill trước khi tạo lộ trình, show source "skill này lấy từ JD dòng X".
```

## 4. Chọn build slice

Build slice tốt phải qua 5 câu hỏi:

| Câu hỏi | Đạt khi |
|---|---|
| User cụ thể chưa? | Sinh viên mới ra trường hoặc fresher hoặc người đi làm 1-2 năm, muốn apply vị trí mới, đang tự học |
| Task đủ hẹp chưa? | Chỉ 1 task: CV + JD → gap analysis → learning path |
| AI decision rõ chưa? | AI decide: skill nào thiếu, thứ tự ưu tiên, nguồn học nào phù hợp |
| Failure path rõ chưa? | AI đánh giá sai skill level → user review + correct → AI adjust |
| Có evidence không? | Self-use 4 platform + Reddit reviews + dropout research data + competitor analysis |

## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Quyết định của nhóm |
|---|---|
| Evidence yếu, user mơ hồ | Evidence đủ mạnh: 4 nguồn self-use + reviews + research data. User rõ ràng (người muốn chuyển việc). |
| Ý tưởng quá rộng | Ban đầu định build cả "Learning OS" (LMS + tracking + community). Đã cắt xuống 1 flow: CV + JD → gap → path. |
| AI không cần thiết | Task này buộc phải có AI vì cần NLP để đọc CV/JD, extract skills, so sánh, và generate lộ trình. Rule-based không đủ linh hoạt. |
| Rủi ro cao | AI có thể đánh giá sai skill level → đã chọn Augmentation: AI gợi ý, user review + quyết định cuối. |
| Không demo được trong 1 ngày | Tracking, quiz, social, API integration → đưa vào backlog. Giữ lại: upload CV + paste JD → show gap → show path. |

## 6. Câu chốt cuối

Điền câu này trước khi rời lớp:

```text
Dựa trên [self-use Coursera/LinkedIn Learning showing one-size-fits-all paths + user reviews about overwhelm/dropout + competitor gap analysis tools thiếu learning path actionable],

nhóm sẽ build [prototype nhận CV + JD → AI phân tích skill gap → tạo lộ trình học cá nhân hoá với nguồn + timeline],

cho [người đi làm 1-5 năm muốn apply vị trí mới],

để giải quyết [pain không biết mình thiếu skill gì và nên học gì trước],

bằng cách AI [augment: đọc CV+JD, extract skills, so sánh gap, gợi ý lộ trình — user review + adjust],

và sẽ test failure path [AI đánh giá sai skill level khi CV không ghi rõ hoặc JD có jargon chuyên ngành].
```

## 7. Backlog

Những thứ **không build trong Day 06**:

- Tích hợp trực tiếp với Coursera/Udemy API để enroll tự động
- Hệ thống tracking tiến độ + quiz/assessment sau mỗi module
- Social features (learning community, study groups)
- Resume auto-update sau khi hoàn thành skill
- Multi-language support
- Enterprise version cho HR/L&D teams
- Mobile app
