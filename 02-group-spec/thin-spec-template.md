## Thin SPEC

### 1. Track, product/app và user

- **Track:** Learning OS
- **Product/app thật:** SkillBridge — AI-powered personalized learning path generator từ CV + JD
- **User cụ thể:** Người mới ra trường hoặc đi làm 1-2 năm kinh nghiệm, muốn apply vị trí mới (chuyển ngành hoặc thăng tiến), đang tự học online nhưng không biết nên học gì trước
- **Nhóm có phải user thật không?:** Có — thành viên nhóm đều là người đang học thêm để nâng cao kỹ năng AI/Tech, đã từng gặp vấn đề không biết bắt đầu từ đâu khi muốn chuyển sang một role mới.

### 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Coursera career track cố định 8 khóa, bắt đầu từ beginner dù user đã biết 60% nội dung | Self-use Coursera | User có kinh nghiệm bị ép học lại <br>→ lãng phí thời gian <br>→ dropout | Build slice phải có CV input để skip phần đã biết |
| Tỷ lệ dropout online learning 85-95%, nguyên nhân chính: overwhelmed, không phù hợp level | Research papers, reviews | Personalization hiện tại không đủ, user cần lộ trình đúng level | AI phải assess skill level từ CV, không dùng one-size-fits-all |
| User tự dùng ChatGPT để phân tích CV vs JD → output tốt nhưng không persist, không track | Self-use | AI engine đã đủ tốt, thiếu UX wrapper: persistence, tracking, structure | Prototype cần có structured UI, không chỉ chat |
| Các tool gap analysis (ResuFlex, Kickresume) chỉ list skills thiếu, không gợi ý nguồn học cụ thể + timeline | Self-use competitor apps | Gap analysis → learning path là bước chưa ai làm tốt | Build slice phải end-to-end: gap → path → resources → timeline |

### 3. Pain statement

```text
User [người đi làm 1-2 năm muốn apply vị trí mới] đang gặp khó ở [bước lên kế hoạch tự học],

vì [không biết chính xác mình thiếu kỹ năng gì so với JD mục tiêu, và các nền tảng learning chỉ đưa lộ trình chung chung không dựa trên profile cá nhân],

dẫn tới [mất hàng giờ research, học nhầm thứ đã biết, overwhelmed bởi quá nhiều lựa chọn, và cuối cùng dropout (tỷ lệ 70-85%)].

Bằng chứng chính là [self-use Coursera/LinkedIn Learning showing fixed tracks; user reviews trên Reddit; dropout research data; competitor analysis showing gap giữa "gap analysis" và "learning path"].
```

### 4. Build slice

```text
Cho [người mới tốt nghiệp hoặc đi làm 1-2 năm đang muốn apply vị trí mới] đang [tự lên kế hoạch học thêm để match JD],

prototype sẽ dùng AI để [đọc CV + JD → phân tích skill gap → tạo lộ trình học cá nhân hoá],

tạo ra [danh sách skill gaps được ưu tiên + lộ trình học có nguồn cụ thể (link khóa/video) + timeline gợi ý],

và xử lý [trường hợp AI đánh giá sai skill level hoặc CV/JD quá mơ hồ] bằng [hiển thị confidence level cho mỗi skill assessment + cho user confirm/chỉnh sửa trước khi generate lộ trình].
```

### 5. Auto/Aug decision

- [x] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [ ] Conditional automation
- [ ] Automation

**Lý do chọn Augmentation:**
1. Skill assessment từ CV có thể sai (CV không phản ánh hết năng lực thật)
2. Mỗi người có bối cảnh riêng (thời gian rảnh, budget, learning style)
3. Gợi ý khóa học có thể không match preference cá nhân (thích video vs text, free vs paid)
4. Rủi ro nếu AI tự quyết sai: user mất weeks/months học sai thứ → trust collapse
5. Domain này không life-critical nhưng high-effort → cần user có quyền review + adjust

**Human role:** reviewer / decider

### 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| **Happy** | User upload CV (Junior Frontend Dev, biết HTML/CSS/JS/React) + paste JD (Senior Frontend, yêu cầu thêm TypeScript, Next.js, Testing, System Design) <br> → AI hiển thị: "Bạn đã có 4/8 skills, cần bổ sung 4 skills" <br>→ Show lộ trình 3 tháng với 4 modules, mỗi module có 2-3 nguồn học (free YouTube + paid Udemy) + timeline tuần <br>→ User approve <br>→ bắt đầu track |
| **Low-confidence** | User upload CV không rõ ràng (ví dụ: "đã làm việc với data") + JD yêu cầu "Advanced SQL, Python, Tableau" <br>→ AI không chắc user biết SQL ở mức nào <br>→ Hiển thị: "Tôi thấy bạn có kinh nghiệm data nhưng chưa rõ mức SQL. Bạn tự đánh giá: Beginner / Intermediate / Advanced?" <br>→ User chọn <br>→ AI adjust lộ trình |
| **Failure** | User upload CV hoàn toàn khác ngành (ví dụ: CV giáo viên Tiếng Anh, JD cho Software Engineer) <br>→ Gap quá lớn (thiếu 90% skills) <br>→ AI cảnh báo: "Khoảng cách skill rất lớn, lộ trình ước tính 12-18 tháng. Bạn có muốn xem các vị trí trung gian (như QA Tester, Technical Writer) gần hơn với profile hiện tại không?" <br>→ Đưa ra lộ trình thay thế thực tế hơn |
| **Correction** | Sau khi nhận lộ trình, user thấy AI gợi ý học "Python cơ bản" nhưng mình đã biết Python (chỉ là CV không ghi) <br>→ User click "Tôi đã biết skill này" <br>→ AI remove khỏi lộ trình + adjust timeline <br>→ Lưu lại preference cho lần sau |

### 7. Failure mode nguy hiểm nhất

```text
Nếu user [upload CV tốt nhưng JD rất chuyên ngành với jargon/kỹ năng niche mà AI không hiểu chính xác],

AI có thể [đánh giá sai skill gap — hoặc miss skills quan trọng, hoặc thêm skills không cần thiết],

hậu quả là [user học sai thứ trong nhiều tuần/tháng → mất thời gian + mất trust → không dùng nữa].

Prototype sẽ xử lý bằng [show source: hiển thị rõ "skill này lấy từ JD dòng X" và "CV của bạn có/không có mention" + cho user review & chỉnh từng skill trước khi tạo lộ trình + confidence score cho mỗi skill assessment].

Owner kiểm thử path này là [thành viên phụ trách Test / failure path].
```

### 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| Khánh An | Research / evidence | Evidence pack hoàn chỉnh: screenshots self-use, competitor analysis, user reviews, research data |
| Yến Phương | SPEC | Thin SPEC hoàn chỉnh: pain statement, build slice, 4 paths, failure mode |
| Duy Quyền | Prototype | Working prototype: upload CV + JD → AI gap analysis → learning path output (có thể dùng Streamlit/Gradio + GPT API) |
| Minh Quang | Test / failure path | Test script cho 4 paths: happy/low-confidence/failure/correction. Ghi lại kết quả test + video demo |
| Xuân Hải | Demo script / repo | Repo GitHub clean + README + demo script 5 phút + slide nếu cần |
