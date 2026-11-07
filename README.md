# Chạy thử UTMOSv2 trên tiếng Việt

Kiểm tra xem [UTMOSv2](https://github.com/sarulab-speech/UTMOSv2) (model dự đoán MOS, train không có dữ liệu tiếng Việt) có dự đoán đúng cảm nhận độ tự nhiên của người nghe Việt hay không.

**Kết quả chính:** trên 20 clip tiếng Việt, tương quan Spearman giữa điểm UTMOSv2 và điểm trung bình của 4 người nghe là −0.01. Người nghe chấm giọng người thật 3.63 và TTS 1.68; UTMOSv2 chấm TTS 2.68 và người thật 2.49. Chi tiết trong `bao_cao_utmosv2_tieng_viet.pdf`.

## Cấu trúc repo

| Đường dẫn | Nội dung |
| --- | --- |
| `bao_cao_utmosv2_tieng_viet.pdf` | Báo cáo đầy đủ: thiết lập, kết quả, nhận xét, hạn chế |
| `UTMOSv2_TiengViet_Demo.ipynb` | Notebook Colab tái lập toàn bộ (GPU T4, Run all) |
| `clips/` | 20 clip tiếng Việt (`clip_01.mp3` … `clip_20.mp3`) đúng như người nghe đã nghe |
| `ket_qua_cham_diem.csv` | **File chính:** mỗi clip một dòng, điểm 4 người nghe + điểm UTMOSv2 |
| `human_ratings/` | 4 file điểm gốc của từng người nghe, xuất từ web khảo sát; có thêm ghi chú, thứ tự nghe, số lần nghe |
| `utmosv2_scores_480_file.csv` | Điểm UTMOSv2 cho toàn bộ 480 file của thí nghiệm gián tiếp (Việt vs Anh) |
| `figures/` | Biểu đồ dùng trong báo cáo |

## Dữ liệu

**Nguồn audio.** 30 câu song song Việt–Anh từ [FLEURS](https://huggingface.co/datasets/google/fleurs) (tập test). Mỗi câu có 8 điều kiện: giọng người thật (`natural`), 3 hệ TTS đọc cùng câu (`edge`, `mms`, `gtts`), và giọng người thật thêm nhiễu trắng (`noise20`, `noise10`, `noise5`, `noise0`, theo SNR dB).

**Chọn 20 clip khảo sát.** Từ 240 file tiếng Việt, chọn 20 file sao cho điểm UTMOSv2 trải đều từ thấp đến cao (chia 20 khoảng phân vị, mỗi khoảng 1 file), rồi đổi tên ngẫu nhiên thành `clip_01…clip_20`.

**Khảo sát.** 4 thành viên nhóm chấm độc lập trên [web khảo sát](https://thachhaoo.github.io/voice-survey/): thang ACR 1–5 (1 = rất thiếu tự nhiên, 5 = rất tự nhiên), không biết clip thuộc hệ nào, không xem câu gốc, mỗi người nghe theo thứ tự ngẫu nhiên riêng, phải nghe hết clip mới chấm được.

### Các cột của `ket_qua_cham_diem.csv`

| Cột | Ý nghĩa |
| --- | --- |
| `clip_id` | Tên file trong `clips/` |
| `he` | Điều kiện: `natural`, `mms`, `gtts`, `noise20`, `noise10`, `noise5`, `noise0` |
| `fleurs_id` | Mã câu trong FLEURS |
| `cau_goc` | Văn bản câu (người nghe không được xem) |
| `diem_<tên>` | Điểm 1–5 của từng người nghe |
| `mos_nguoi_nghe` | Trung bình 4 người nghe |
| `mos_utmosv2` | Điểm UTMOSv2 (`fusion_stage3`, fold 0, `predict_dataset='sarulab'`, commit `cc2700d`) |
| `chenh_lech_utmos_tru_nguoi` | `mos_utmosv2 − mos_nguoi_nghe`; dương = model chấm cao hơn người |

### Các cột của `utmosv2_scores_480_file.csv`

`lang` (`vi`/`en`), `system` (điều kiện như trên, thêm `edge`), `sent_id` (mã câu FLEURS), `pred_mos` (điểm UTMOSv2).

## Biểu đồ

| File | Nội dung |
| --- | --- |
| `nguoi_nghe_vs_utmosv2_theo_dieu_kien.png` | Điểm người nghe và UTMOSv2 theo từng điều kiện (biểu đồ chính) |
| `scatter_nguoi_nghe_vs_utmosv2.png` | Từng clip: điểm người nghe (trục ngang) và UTMOSv2 (trục dọc) |
| `utmosv2_theo_he_va_ngon_ngu.png` | Điểm UTMOSv2 theo hệ, tiếng Việt và tiếng Anh |
| `utmosv2_phan_ung_voi_nhieu.png` | Điểm UTMOSv2 khi tăng mức nhiễu, hai thứ tiếng |

## Hạn chế

Mẫu nhỏ (20 clip, 4 người nghe là thành viên nhóm, đồng thuận từng cặp SRCC 0.37), chưa có bài nghe tiếng Anh để đối chứng. Xem mục Hạn chế trong báo cáo.
