# Auto Game Tool — Phiên bản nâng cấp (đa game, config-driven)

Tool tự động hóa cho game, hỗ trợ **nhiều game khác nhau thông qua file config**,
có GUI điều khiển và Config Editor để người dùng tự tạo cấu hình mới — không cần sửa code.

---

## 1. Điểm khác biệt so với bản gốc

| Bản gốc | Bản nâng cấp |
|---|---|
| Hard-code cho Mech Arena | Config-driven: mỗi game 1 file JSON riêng |
| Chỉ 1 nút auto-click | Danh sách nút click tùy ý (array trong config) |
| Không có GUI | GUI chính (Start/Stop, chọn game, log) |
| Không thể tự tạo config | Config Editor: vẽ vùng scan, chọn điểm click, color picker bằng chuột |
| Chỉ đứng im camp | Có thêm **xoay camera tự động quét địch** (camera_rotation) |
| Chạy qua Python + venv | Đóng gói `.exe` độc lập bằng PyInstaller |

---

## 2. Cấu trúc thư mục

```
auto_game_tool/
├── core/
│   ├── config_loader.py       # đọc & validate config JSON
│   ├── vision_core.py         # nhận diện địch (config-driven)
│   ├── input_controller.py    # bấm skill, hotkeys (config-driven)
│   ├── mouse_clicker.py       # auto-click nhiều nút (config-driven)
│   └── camera_controller.py   # [MỚI] xoay camera quét địch
├── configs/
│   ├── CONFIG_SCHEMA.md       # tài liệu mô tả schema config
│   └── mech_arena.json        # config mẫu cho Mech Arena
├── gui/
│   ├── main_window.py         # control panel chính
│   └── config_editor.py       # công cụ tự tạo config bằng chuột
├── app.py                     # entry point (dùng để đóng gói .exe)
└── README.md                  # file này
```

---

## 3. Tính năng mới: Xoay camera tự động quét địch

Thay vì chỉ đứng im "camp" chờ địch xuất hiện trong vùng quét cố định (bản gốc),
tool sẽ **chủ động xoay góc nhìn** để mở rộng phạm vi phát hiện:

- Xoay theo bước nhỏ (`step_degrees`) theo kiểu quét trái-phải hoặc toàn vòng.
- Sau mỗi bước xoay → quét lại vùng ảnh để tìm thanh máu địch.
- Phát hiện địch → **dừng xoay ngay**, giữ nguyên góc nhìn, chuyển sang bấm skill.
- Mất dấu địch sau X giây → tiếp tục xoay quét.
- Hệ số `sensitivity_px_per_degree` cho phép quy đổi chính xác giữa các game
  có độ nhạy chuột (mouse sensitivity) khác nhau.

Chi tiết tham số: xem mục `camera_rotation` trong `configs/CONFIG_SCHEMA.md`.

---

## 4. Trạng thái hiện tại

Đây là kết quả của **Giai đoạn 0**:
- ✅ Cấu trúc thư mục dự án đã được tạo.
- ✅ Schema config JSON đã thiết kế xong (bao gồm tham số xoay camera mới).
- ✅ File config mẫu `mech_arena.json` đã hoàn chỉnh theo schema mới.
- ✅ Các file core/gui đã tạo dạng stub (chưa có logic, chỉ có docstring + TODO), sẵn sàng để refactor ở Giai đoạn 1.

→ Xem tiến độ chi tiết từng bước tại `ROADMAP.md`.
