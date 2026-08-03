# Báo Cáo Nhóm — Lab 7: Embedding & Vector Store

**Nhóm:** [Tên nhóm]
**Thành viên:** [Họ tên từng thành viên]
**Ngày:** [Ngày nộp]

> **Nộp 1 bản / nhóm.** Phần cá nhân (hướng tiếp cận, kết quả riêng, dự đoán…) mỗi thành viên nộp riêng trong `REPORT_CANHAN.md`. Chi tiết thang điểm: `docs/SCORING.md`.

**Tổng điểm phần nhóm: 40** = Lựa chọn tài liệu (10) + Thiết kế chiến lược (15) + Chất lượng truy xuất (10) + Thuyết trình (5).

---

## 1. Lựa chọn tài liệu (Document Set Quality) — Nhóm (10 điểm)

### Phạm vi bộ tài liệu (Scope)

**Chủ đề (cố định theo lớp K3):** Dịch vụ hoặc quy định đại học

**Phạm vi cụ thể nhóm tập trung:**

> \*Quy chế đào tạo đại học tại Đại học Quốc gia Hà Nội, tập trung vào đăng ký học phần, quyền và nghĩa vụ, kiểm tra - đánh giá, xử lý học vụ, cố vấn học tập và tốt nghiệp.

Nguồn gốc là một văn bản công khai gồm 51 điều. Để phục vụ benchmark và chunking theo heading, văn bản được chia thành 8 tài liệu logic theo nhóm điều khoản, không trộn thêm nội dung ngoài nguồn.\*

### Danh sách tài liệu (Data Inventory)

|   # | Tên tài liệu                                              | Nguồn (Source URL)                                                                                                                                 | Ngày lấy / Phiên bản             | Số ký tự | Metadata đã gán                                                                                   |
| --: | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- | -------: | ------------------------------------------------------------------------------------------------- |
|   1 | Quy định chung và chương trình đào tạo tại ĐHQGHN         | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |   23,045 | `audience: all`, `category: general-and-curriculum-regulations`, `department: academic-affairs`   |
|   2 | Tổ chức đào tạo, đăng ký và rút học phần                  | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |   15,184 | `audience: student`, `category: course-registration`, `department: academic-affairs`              |
|   3 | Chương trình đặc thù và nghiên cứu khoa học của sinh viên | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |   20,007 | `audience: student`, `category: student-programs-and-research`, `department: academic-affairs`    |
|   4 | Giảng viên, giáo viên chủ nhiệm và cố vấn học tập         | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |    6,454 | `audience: faculty`, `category: faculty-and-advising`, `department: academic-affairs`             |
|   5 | Nghĩa vụ và quyền lợi của sinh viên                       | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |    6,132 | `audience: student`, `category: student-rights-and-obligations`, `department: student-affairs`    |
|   6 | Kiểm tra, thi, đánh giá kết quả và xử lý học vụ           | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |   16,542 | `audience: student`, `category: assessment-and-academic-standing`, `department: academic-affairs` |
|   7 | Kỷ luật đối với cán bộ coi thi, tổ chức thi và chấm thi   | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |    2,141 | `audience: staff`, `category: exam-staff-discipline`, `department: examination-office`            |
|   8 | Công nhận tốt nghiệp và tổ chức thực hiện                 | [Nguồn chính thức](https://ussh.vnu.edu.vn/vi/van-ban/detail/Quy-che-dao-tao-dai-hoc-tai-Dai-hoc-Quoc-gia-Ha-Noi-Ap-dung-tu-khoa-QH-2022-X-19452/) | 2026-08-03 / 3626/QD-DHQGHN-2022 |    9,194 | `audience: all`, `category: graduation-and-governance`, `department: academic-affairs`            |

**Danh sách kiểm tra quản trị dữ liệu (Data governance checklist):**

- [x] Chỉ dùng nguồn công khai chính thức và không chứa dữ liệu cá nhân hoặc thông tin đăng nhập.
- [x] Mỗi tài liệu có `doc_id`, `title`, `source_url`, `retrieved_at`, `document_version`, `audience`.
- [x] `sources.csv` khớp một-một với 8 file Markdown.
- [x] Có nhiều giá trị `audience`: `student`, `faculty`, `staff`, `all`.
- [x] Nội dung được làm sạch từ PDF, loại bỏ số trang và định dạng lặp; không thêm quy định mới.

### Cấu trúc Metadata (Metadata Schema)

| Trường metadata    | Kiểu   | Ví dụ giá trị                              | Tại sao hữu ích cho retrieval?                          |
| ------------------ | ------ | ------------------------------------------ | ------------------------------------------------------- |
| `doc_id`           | string | `to-chuc-dao-tao-dang-ky-hoc-phan`         | Định danh duy nhất, truy vết chunk về đúng tài liệu.    |
| `title`            | string | `Tổ chức đào tạo, đăng ký và rút học phần` | Hiển thị kết quả và hỗ trợ hiểu nhanh phạm vi tài liệu. |
| `source_url`       | URL    | URL văn bản chính thức                     | Kiểm chứng nguồn và provenance.                         |
| `retrieved_at`     | date   | `2026-08-03`                               | Biết thời điểm thu thập dữ liệu.                        |
| `document_version` | string | `3626/QD-DHQGHN-2022`                      | Phân biệt phiên bản quy chế.                            |
| `audience`         | enum   | `student`, `faculty`, `staff`, `all`       | Lọc đúng nhóm đối tượng, giảm tài liệu nhiễu.           |
| `department`       | string | `academic-affairs`                         | Hữu ích khi corpus sau này có nhiều đơn vị dịch vụ.     |
| `category`         | string | `course-registration`                      | Lọc nhanh theo nghiệp vụ học vụ.                        |
| `effective_date`   | date   | `2022-10-21`                               | Hỗ trợ so sánh quy định theo thời gian.                 |
| `language`         | string | `vi`                                       | Hữu ích khi corpus đa ngôn ngữ.                         |
| `source_pages`     | string | `14-21`                                    | Truy vết về trang trong PDF gốc.                        |

---

|   # | Query                                                                                                                    | Metadata filter                           | Gold answer                                                                                                                                                                                                                                                                                                                        | Vị trí kiểm chứng trong corpus                              |
| --: | ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
|   1 | Sinh viên phải hoàn thành đăng ký học phần chậm nhất khi nào trước khi bắt đầu mỗi học kỳ?                               | `metadata_filter={"audience": "student"}` | Chậm nhất 01 tháng trước khi bắt đầu mỗi học kỳ, sinh viên phải hoàn thành đăng ký các học phần.                                                                                                                                                                                                                                   | `to-chuc-dao-tao-dang-ky-hoc-phan.md` - Điều 21, khoản 3(b) |
|   2 | Sinh viên được rút bớt học phần trong thời hạn nào và có được trả lại học phí không?                                     | `metadata_filter={"audience": "student"}` | Việc rút bớt học phần chỉ được chấp nhận trong 02 tuần kể từ đầu học kỳ chính hoặc 01 tuần kể từ đầu học kỳ phụ và sinh viên được trả lại học phí. Ngoài thời hạn này, học phần vẫn được giữ và nếu không học thì nhận điểm F, không được hoàn học phí.                                                                            | `to-chuc-dao-tao-dang-ky-hoc-phan.md` - Điều 23, khoản 2    |
|   3 | Sinh viên có điểm D hoặc D+ có được đăng ký học cải thiện không, và điểm cũ được xử lý thế nào?                          | `metadata_filter={"audience": "student"}` | Sinh viên đạt điểm D hoặc D+ được đăng ký học lại học phần đó, hoặc đổi sang học phần khác nếu là học phần tự chọn có điều kiện. Khi đăng ký cải thiện được chấp nhận, điểm cũ bị hủy và được thay bằng điểm học phần cải thiện.                                                                                                   | `to-chuc-dao-tao-dang-ky-hoc-phan.md` - Điều 21, khoản 5    |
|   4 | Các điều kiện chính về tín chỉ, điểm trung bình, ngoại ngữ và học phần điều kiện để sinh viên được xét tốt nghiệp là gì? | `metadata_filter={"audience": "all"}`     | Sinh viên phải tích lũy đủ số tín chỉ của chương trình; có điểm trung bình chung tích lũy từ 2,00 trở lên, hoặc từ 2,50 đối với chương trình tài năng/chất lượng cao; đạt chuẩn ngoại ngữ theo Điều 13; và đạt các học phần giáo dục quốc phòng - an ninh, giáo dục thể chất, kỹ năng bổ trợ, cùng các điều kiện khác của Điều 46. | `tot-nghiep-va-to-chuc-thuc-hien.md` - Điều 46, khoản 1     |
|   5 | Cố vấn học tập có trách nhiệm gì trong việc hỗ trợ sinh viên lập kế hoạch và đăng ký học phần?                           | `metadata_filter={"audience": "faculty"}` | Cố vấn học tập/giáo viên chủ nhiệm phải nắm vững chương trình đào tạo; hướng dẫn sinh viên xây dựng kế hoạch học tập và lựa chọn học phần phù hợp; hướng dẫn phương pháp học tập, theo dõi kết quả; hỗ trợ giải quyết khó khăn và phối hợp với các đơn vị liên quan.                                                               | `giang-vien-co-van-hoc-tap.md` - Điều 34, khoản 3           |

## 2. Thiết kế chiến lược (Strategy Design) — Nhóm (15 điểm)

> Mỗi thành viên thử **một chiến lược khác nhau** trên cùng bộ tài liệu `data/k3_university` và cùng 5 benchmark query. Mỗi thành viên phải tự chạy đầy đủ 5 query với chiến lược của mình; nhóm chỉ chia nhau phần chuẩn bị, tổng hợp và trình bày kết quả.

### Phân công công việc

| Thành viên          | Công việc chính                                                          | Sản phẩm cần hoàn thành                                                                                                     |
| ------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| **Lê Mạnh Cương**   | Data curator, benchmark owner; xây dựng chiến lược chunking theo heading | Kiểm tra corpus và metadata; quản lý 5 benchmark query; cài đặt `HeadingChunker`; chạy đủ 5 query; lưu kết quả và phân tích |
| **Nguyễn Tuấn Anh** | Strategy owner, demo coordinator; thử nghiệm chiến lược recursive        | Cấu hình và tinh chỉnh `RecursiveChunker`; chạy đủ 5 query; lưu kết quả; tổng hợp bảng so sánh và chuẩn bị demo             |

### Phân tích đường cơ sở (Baseline Analysis)

Chạy `ChunkingStrategyComparator().compare()` trên 3 tài liệu tiêu biểu:

1. `to-chuc-dao-tao-dang-ky-hoc-phan.md`
2. `kiem-tra-thi-danh-gia-hoc-tap.md`
3. `tot-nghiep-va-to-chuc-thuc-hien.md`

Phân công:

- **Lê Mạnh Cương:** chạy comparator trên tài liệu đăng ký học phần và tốt nghiệp.
- **Nguyễn Tuấn Anh:** chạy comparator trên tài liệu kiểm tra, thi và đánh giá.
- **Cả nhóm:** tổng hợp kết quả vào một bảng chung.

| Tài liệu                              | Chiến lược (Strategy)            | Số lượng Chunk | Độ dài trung bình | Giữ được ngữ cảnh không? |
| ------------------------------------- | -------------------------------- | -------------: | ----------------: | ------------------------ |
| `to-chuc-dao-tao-dang-ky-hoc-phan.md` | FixedSizeChunker (`fixed_size`)  |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `to-chuc-dao-tao-dang-ky-hoc-phan.md` | SentenceChunker (`by_sentences`) |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `to-chuc-dao-tao-dang-ky-hoc-phan.md` | RecursiveChunker (`recursive`)   |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `kiem-tra-thi-danh-gia-hoc-tap.md`    | FixedSizeChunker (`fixed_size`)  |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `kiem-tra-thi-danh-gia-hoc-tap.md`    | SentenceChunker (`by_sentences`) |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `kiem-tra-thi-danh-gia-hoc-tap.md`    | RecursiveChunker (`recursive`)   |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `tot-nghiep-va-to-chuc-thuc-hien.md`  | FixedSizeChunker (`fixed_size`)  |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `tot-nghiep-va-to-chuc-thuc-hien.md`  | SentenceChunker (`by_sentences`) |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |
| `tot-nghiep-va-to-chuc-thuc-hien.md`  | RecursiveChunker (`recursive`)   |      Chưa chạy |         Chưa chạy | Chưa đánh giá            |

### Chiến lược của từng thành viên

**Thành viên 1 — Lê Mạnh Cương**

- **Loại chiến lược:** Custom — `HeadingChunker`
- **Mô tả & lý do chọn cho chủ đề này:**  
  Tài liệu quy chế đào tạo được tổ chức rõ ràng theo Chương, Điều và Khoản. Chunking theo heading giúp giữ các nội dung thuộc cùng một quy định trong một chunk, hạn chế việc tách rời điều kiện và kết luận.
- **Thông số dự kiến:**
  - Tách theo heading Markdown: `#`, `##`, `###`
  - Ưu tiên giữ nguyên từng Điều
  - `max_chunk_size`: 1200 ký tự
  - `chunk_overlap`: 100 ký tự
- **Công việc cần thực hiện:**
  1. Cài đặt `HeadingChunker`.
  2. Giữ lại metadata của tài liệu trong từng chunk.
  3. Chạy đủ 5 benchmark query.
  4. Lưu top-3 kết quả của từng query.
  5. Chấm điểm truy xuất trên thang 10.
  6. Phân tích trường hợp truy xuất tốt và thất bại.
- **Code snippet (custom):**

```python
import re
from typing import List


class HeadingChunker:
    """Tách văn bản theo heading Markdown và các mục Điều."""

    def __init__(
        self,
        max_chunk_size: int = 1200,
        chunk_overlap: int = 100,
    ) -> None:
        if max_chunk_size <= 0:
            raise ValueError("max_chunk_size phải lớn hơn 0")

        if chunk_overlap < 0 or chunk_overlap >= max_chunk_size:
            raise ValueError(
                "chunk_overlap phải từ 0 đến nhỏ hơn max_chunk_size"
            )

        self.max_chunk_size = max_chunk_size
        self.chunk_overlap = chunk_overlap

    def split_text(self, text: str) -> List[str]:
        sections = re.split(
            r"(?=^#{1,3}\s+)|(?=^Điều\s+\d+)",
            text,
            flags=re.MULTILINE,
        )

        chunks: List[str] = []

        for section in sections:
            section = section.strip()

            if not section:
                continue

            if len(section) <= self.max_chunk_size:
                chunks.append(section)
            else:
                chunks.extend(self._split_long_section(section))

        return chunks

    def _split_long_section(self, section: str) -> List[str]:
        chunks: List[str] = []
        start = 0
        step = self.max_chunk_size - self.chunk_overlap

        while start < len(section):
            end = min(start + self.max_chunk_size, len(section))
            chunk = section[start:end].strip()

            if chunk:
                chunks.append(chunk)

            if end == len(section):
                break

            start += step

        return chunks
```

> Khi đưa vào repository, cần điều chỉnh tên phương thức và kiểu dữ liệu trả về theo interface chunker hiện có.

**Thành viên 2 — Nguyễn Tuấn Anh**

- **Loại chiến lược:** `RecursiveChunker`
- **Mô tả & lý do chọn:**  
  Recursive chunking ưu tiên tách theo heading, đoạn văn và câu trước khi cắt theo kích thước. Cách này phù hợp với các Điều dài, đồng thời kiểm soát được độ dài chunk để tạo embedding và truy xuất.
- **Thông số dự kiến:**
  - `chunk_size`: 1000 ký tự
  - `chunk_overlap`: 150 ký tự
  - Tách lần lượt theo heading, đoạn văn, dòng, câu và khoảng trắng
- **Công việc cần thực hiện:**
  1. Cấu hình `RecursiveChunker`.
  2. Thử ít nhất hai bộ thông số.
  3. Chọn cấu hình tốt hơn trước khi benchmark chính thức.
  4. Chạy đủ 5 benchmark query.
  5. Lưu top-3 kết quả của từng query.
  6. Chấm điểm truy xuất trên thang 10.
  7. Tổng hợp kết quả và chuẩn bị demo.
- **Code snippet:**

```python
recursive_chunker = RecursiveChunker(
    chunk_size=1000,
    chunk_overlap=150,
    separators=[
        "\n# ",
        "\n## ",
        "\n### ",
        "\n\n",
        "\n",
        ". ",
        " ",
    ],
)
```

Cấu hình thứ hai để thử nghiệm:

```python
recursive_chunker_small = RecursiveChunker(
    chunk_size=800,
    chunk_overlap=120,
    separators=[
        "\n# ",
        "\n## ",
        "\n### ",
        "\n\n",
        "\n",
        ". ",
        " ",
    ],
)
```

### Quy trình benchmark chung

Cả hai thành viên phải sử dụng:

- Cùng corpus: `data/k3_university`
- Cùng file query: `benchmarks/k3_university_queries.json`
- Cùng embedding model
- Cùng `top_k=3`
- Cùng quy tắc chấm điểm
- Không thay đổi query và gold answer sau khi đã xem kết quả

Quy tắc chấm điểm:

- **2 điểm:** Top-3 chứa đúng tài liệu và đúng đoạn thông tin cần thiết.
- **1 điểm:** Có tài liệu liên quan nhưng chunk thiếu thông tin hoặc xếp hạng chưa tốt.
- **0 điểm:** Top-3 không chứa thông tin trả lời đúng.

Tổng điểm tối đa của mỗi chiến lược:

```text
5 query × 2 điểm = 10 điểm
```

### File kết quả cần tạo

```text
results/
├── le_manh_cuong_heading_results.csv
├── nguyen_tuan_anh_recursive_results.csv
└── strategy_comparison.csv
```

Cấu trúc đề xuất:

```csv
query_id,strategy,rank,doc_id,score,is_relevant,query_score,notes
```

### So Sánh Giữa Các Thành Viên

| Thành viên | Chiến lược (Strategy) | Điểm truy xuất (/10) | Điểm mạnh | Điểm yếu |
| ---------- | --------------------- | -------------------- | --------- | -------- |
|            |                       |                      |           |          |
|            |                       |                      |           |          |
|            |                       |                      |           |          |

**Chiến lược nào tốt nhất cho chủ đề này? Tại sao?**

> _Viết 2-3 câu — đây là phần được đánh giá cao nhất (khả năng suy nghĩ & giải thích):_

---

## 3. Câu hỏi đánh giá & Chất lượng truy xuất (Retrieval Quality) — Nhóm (10 điểm)

### Câu hỏi đánh giá & Câu trả lời chuẩn (nhóm thống nhất)

> **Đúng 5 câu hỏi**, đa dạng, có thể kiểm chứng; **ít nhất 1 câu** cần lọc metadata mới trả lời tốt. Đây là bộ câu hỏi chung cho mọi thành viên chạy.

| #   | Câu hỏi (Query) | Câu trả lời chuẩn (Gold Answer) | Chunk nào chứa thông tin? |
| --- | --------------- | ------------------------------- | ------------------------- |
| 1   |                 |                                 |                           |
| 2   |                 |                                 |                           |
| 3   |                 |                                 |                           |
| 4   |                 |                                 |                           |
| 5   |                 |                                 |                           |

### Tổng hợp chất lượng truy xuất của nhóm

> Cách chấm (theo `docs/SCORING.md`): **2 điểm/câu** — top-3 chứa chunk liên quan + agent trả lời đúng (2), có liên quan nhưng thiếu/không ở top-1 (1), không có trong top-3 (0).

| #   | Câu hỏi | Chiến lược tốt nhất cho câu này | Có chunk liên quan trong top-3? | Ghi chú |
| --- | ------- | ------------------------------- | ------------------------------- | ------- |
| 1   |         |                                 |                                 |         |
| 2   |         |                                 |                                 |         |
| 3   |         |                                 |                                 |         |
| 4   |         |                                 |                                 |         |
| 5   |         |                                 |                                 |         |

**Lọc bằng metadata có giúp ích không? Ở câu hỏi nào?**

> _Viết 2-3 câu:_

---

## 4. Thuyết trình (Demo) & Bài học nhóm — Nhóm (5 điểm)

**Những phân tích (insights) hay nhất nhóm sẽ trình bày:**

> _Liệt kê 2-3 ý:_

**Bài học rút ra khi so sánh trong nhóm:**

> _Viết 2-3 câu — cùng tài liệu nhưng chiến lược khác nhau dẫn tới khác biệt gì?_

**Nếu làm lại, nhóm sẽ thay đổi gì trong chiến lược dữ liệu (data strategy)?**

> _Viết 2-3 câu:_

---

## Tự Đánh Giá (Phần Nhóm)

| Tiêu chí                                 | Điểm tự đánh giá |
| ---------------------------------------- | ---------------- |
| Lựa chọn tài liệu (Document Set Quality) | / 10             |
| Thiết kế chiến lược (Strategy Design)    | / 15             |
| Chất lượng truy xuất (Retrieval Quality) | / 10             |
| Thuyết trình (Demo)                      | / 5              |
| **Tổng phần nhóm**                       | **/ 40**         |
