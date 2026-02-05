# NHẬN DẠNG GIỌNG NÓI VÀ ỨNG DỤNG MÔ HÌNH NGÔN NGỮ LỚN TRONG DỊCH TỰ ĐỘNG NGÔN NGỮ KÝ HIỆU VIỆT NAM 🇻🇳

**Luận văn Tốt nghiệp - Khoa Thương mại Điện tử**
**Trường Đại học Kinh tế - Đại học Đà Nẵng**

---

## 1. Giới thiệu Đề tài

Dự án này xuất phát từ nhu cầu thực tế tại **Angel Coffee (Đà Nẵng)**, nơi các nhân viên là người khiếm thính gặp khó khăn trong giao tiếp với khách hàng. Hệ thống được xây dựng nhằm mục đích xóa bỏ rào cản này bằng cách chuyển đổi tự động:

**Giọng nói Tiếng Việt (Input) ➡️ Văn bản Tiếng Việt ➡️ Văn bản cú pháp Ngôn ngữ Ký hiệu (Output)**

Đầu ra của hệ thống là dạng **VSL Gloss** (Văn bản biểu diễn đúng cú pháp NNKH Việt Nam), đóng vai trò là dữ liệu đầu vào chuẩn hóa cho các mô hình diễn họa Avatar 3D trong tương lai.

### 🌟 Đóng góp chính
1.  **Tài nguyên dữ liệu:** Xây dựng bộ dữ liệu song ngữ chuyên biệt cho lĩnh vực F&B (dịch vụ cà phê) thông qua thu thập thực tế và tăng cường dữ liệu bằng LLMs (ChatGPT, Gemini).
2.  **Mô hình tối ưu:** Đề xuất quy trình huấn luyện **Fine-tuning 2 giai đoạn** giúp mô hình vừa nắm bắt ngữ pháp chung, vừa hiểu sâu từ vựng chuyên ngành.
3.  **Hệ thống tích hợp:** Xây dựng pipeline hoàn chỉnh kết hợp nhận dạng giọng nói (ASR) và dịch máy (MT).

---

## 2. Kiến trúc Hệ thống

Hệ thống hoạt động theo mô hình nối tầng (Pipeline) gồm 2 module:

* **Module 1 (Speech-to-Text):** Sử dụng mô hình **PhoWhisper-small** để chuyển đổi giọng nói tiếng Việt thành văn bản với độ chính xác cao (WER thấp).
* **Module 2 (Machine Translation):** Sử dụng mô hình **ViT5-base** để dịch văn bản tiếng Việt sang văn bản cú pháp NNKH (Gloss).

![Architecture Diagram](Picture1.jpg)
*(Sơ đồ luồng dữ liệu tổng quan của hệ thống)*

---

## 3. Cấu trúc Thư mục & File Code

Dự án được chia thành 6 notebook, tương ứng với quy trình nghiên cứu từ xử lý dữ liệu đến triển khai.

### 📂 Danh sách File Code

| STT | Tên File | Mô tả Chức năng |
| :--- | :--- | :--- |
| **01** | `01_Data_Analysis_and_Augmentation_Eval.ipynb` | **Tiền xử lý & Phân tích:** Thống kê dữ liệu gốc (146 câu), thực hiện tăng cường dữ liệu (Data Augmentation) bằng ChatGPT/Gemini lên 1.691 câu, và đánh giá chất lượng (TTR, Cosine Similarity). |
| **02** | `02_FineTuning01_Baseline_Train_BARTpho_General10k.ipynb` | **Huấn luyện Cơ sở (Baseline 1):** Fine-tuning mô hình **BARTpho** trên tập dữ liệu tổng quát 10k câu để làm mốc so sánh. |
| **03** | `03_FineTuning01_Baseline_Train_ViT5_General10k.ipynb` | **Huấn luyện Cơ sở (Baseline 2):** Fine-tuning và so sánh hiệu năng giữa **ViT5-base** và **ViT5-large** trên tập dữ liệu tổng quát. |
| **04** | `04_FineTuning02_DomainAdapt_ViT5_Coffee_Augmented.ipynb` | **Chiến lược A:** Fine-tuning 2 giai đoạn (2-stage Fine-tuning) trên mô hình ViT5-base với tập dữ liệu Cà phê đã tăng cường. |
| **05** | `05_TransferLearning_ViT5_LoRA.ipynb` | **Chiến lược B (So sánh):** Áp dụng kỹ thuật **Transfer Learning** kết hợp **LoRA** trên mô hình ViT5-base để so sánh hiệu quả với chiến lược A. |
| **06** | `06_Inference_Demo_System.ipynb` | **Triển khai (Deployment):** Tích hợp PhoWhisper và model ViT5 tốt nhất vào giao diện Web App (Gradio) để demo sản phẩm thực tế. |

### 📂 Dữ liệu (Data)

* **`Corpus-Vie-VSL-10K`**: Dữ liệu nền tảng (10.000 câu) kế thừa từ nghiên cứu trước.
* **`Coffee_Vie_VSL`**: Dữ liệu gốc (146 câu) thu thập thực tế tại quán cà phê.
* **`Combined_Augmented_Coffee`**: Dữ liệu tổng hợp sau khi tăng cường (1.691 câu).

---

## 4. Kết quả Thực nghiệm

Sau quá trình thử nghiệm và so sánh, nghiên cứu đã rút ra các kết luận quan trọng:

* **Mô hình tối ưu nhất:** **ViT5-base** áp dụng chiến lược **Fine-tuning 2 giai đoạn**.
    * *Giai đoạn 1:* Học ngữ pháp chung từ tập 10k.
    * *Giai đoạn 2:* Tinh chỉnh từ vựng chuyên ngành Cà phê.
* **Hiệu năng:**
    * **BLEU:** 95.75%
    * **WER (Word Error Rate):** 2.7%
* **So sánh:** Chiến lược Fine-tuning 2 giai đoạn cho kết quả vượt trội hơn so với Transfer Learning (BLEU ~90%) và Fine-tuning đơn lẻ.

---

## 5. Hướng dẫn Cài đặt & Chạy Demo

Để trải nghiệm hệ thống, vui lòng mở file số **06** (`06_Inference_Demo_System.ipynb`) trên Google Colab:

1.  **Chuẩn bị:** Upload file notebook và các file dữ liệu cần thiết lên Google Drive.
2.  **Môi trường:** Chọn Runtime type là **T4 GPU**.
3.  **Thực thi:** Chạy toàn bộ các cell (`Runtime` -> `Run all`).
4.  **Trải nghiệm:** Truy cập đường link `Gradio Public URL` ở cuối notebook để sử dụng giao diện demo (nhập văn bản hoặc ghi âm).

---

**Sinh viên thực hiện:** Lê Thị Hà Vy

**GVHD:** Th.S Nguyễn Văn Chức

**Đơn vị:** Khoa Thương mại Điện tử - Trường Đại học Kinh tế (ĐH Đà Nẵng)