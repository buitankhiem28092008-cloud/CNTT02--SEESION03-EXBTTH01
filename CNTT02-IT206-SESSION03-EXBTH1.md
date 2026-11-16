# [Bài thực hành 1] Trích xuất và định dạng thông tin tuyển dụng
## khung cấu trúc CREATE áp dụng vào bài toán
- [x] C (Context): Đóng vai trò Chuyên viên xử lý dữ liệu nhân sự cấp cao.
- [x] R (Request): Trích xuất các trường: Họ tên, Số điện thoại, Email, Vị trí ứng tuyển, Số năm kinh nghiệm, Kỹ năng chính.
- [x] E (Explanation): Dữ liệu dùng để nạp trực tiếp vào database nên yêu cầu độ chính xác tuyệt đối, không được tự ý bịa đặt thông tin (thiếu thì điền null).
- [x] A (Action): Quét email thô, chuẩn hóa định dạng (Họ tên viết hoa chữ cái đầu, kỹ năng gom thành mảng không trùng lặp) và trả về đồng thời Bảng Markdown cùng JSON thuần túy.
- [x] T (Tone): Khách quan, chính xác, không dùng văn phong cảm xúc hay giải thích rườm rà.
- [x] E (Extras): Cung cấp ví dụ (Few-shot) để AI định hình chính xác đầu ra.
## 2. Mẫu Prompt hoàn chỉnh để sử dụng
[Context]
Bạn là Chuyên viên xử lý dữ liệu nhân sự cấp cao tại công ty công nghệ.

[Request]
Trích xuất các trường thông tin từ email ứng tuyển thô: Họ tên, Số điện thoại, Email, Vị trí ứng tuyển, Năm kinh nghiệm, Kỹ năng chính.

[Explanation]
Dữ liệu này dùng để import thẳng vào database tuyển dụng, đòi hỏi độ chính xác 100%. Tuyệt đối không tự bịa đặt thông tin; nếu thiếu, hãy điền giá trị null.

[Action]
- Họ tên: Chuẩn hóa viết hoa chữ cái đầu (Ví dụ: Dấu Chân Người Cụt Dò).
- Số điện thoại: Chuẩn hóa đầu số, loại bỏ dấu cách thừa.
- Kỹ năng chính: Gom thành danh sách mảng không trùng lặp (Ví dụ: ["Python", "SQL"]).
- Trả về đồng thời:
  1. Một bảng Markdown tóm tắt thông tin.
  2. Một cấu trúc JSON thô theo mẫu:
  { "name": "...", "phone": "...", "email": "...", "position": "...", "experience_years": , "skills": [...] }

[Tone]
Chính xác, khách quan, không thêm văn phong cảm xúc, không viết lời dẫn giải thừa ở đầu/cuối.

[Extras - Ví dụ mẫu Few-shot]
Input: "Chào anh chị, em là trần thị mai, sđt: 0987 654 321, email: maitran@gmail.com. Em muốn ứng tuyển vị trí Data Analyst, kinh nghiệm 3 năm, kỹ năng chính gồm Python, SQL và Tableau."

Output:
| Trường thông tin | Giá trị |
| :--- | :--- |
| Họ tên | Trần Thị Mai |
| Số điện thoại | 0987654321 |
| Email | maitran@gmail.com |
| Vị trí ứng tuyển | Data Analyst |
| Số năm kinh nghiệm | 3 |
| Kỹ năng chính | Python, SQL, Tableau |

```json
{
  "name": "Trần Thị Mai",
  "phone": "0987654321",
  "email": "maitran@gmail.com",
  "position": "Data Analyst",
  "experience_years": 3,
  "skills": ["Python", "SQL", "Tableau"]
}