# Shopee Coin Summary API Specification

Tài liệu đặc tả kỹ thuật cho API truy vấn thông tin số dư Shopee Xu dành cho các hệ thống tự động hóa và tích hợp.

## 1. Thông tin chung
API này cung cấp dữ liệu tóm tắt về trạng thái ví Shopee Xu của người dùng, bao gồm số dư khả dụng hiện tại, các khoản xu sắp hết hạn và thời gian cập nhật hệ thống.

## 2. Đặc tả Endpoint
- **URL:** `https://shopee.vn/api/v4/coin/get_user_coins_summary`
- **Phương thức:** `GET`
- **Định dạng phản hồi:** `JSON`

## 3. Cấu trúc Request Headers
Các yêu cầu truy vấn cần đính kèm các tham số tiêu đề sau để xác thực và đảm bảo tính tương thích:

| Tham số | Trạng thái | Mô tả chi tiết |
| :--- | :--- | :--- |
| `Cookie` | Bắt buộc | Chứa thông tin định danh phiên làm việc (SPC_EC, SPC_ST, SPC_U). |
| `x-api-source` | Bắt buộc | Nguồn gốc yêu cầu, giá trị mặc định là `pc`. |
| `referer` | Bắt buộc | Đường dẫn tham chiếu: `https://shopee.vn/shopee-coins`. |
| `User-Agent` | Bắt buộc | Chuỗi định danh trình duyệt để vượt qua bộ lọc Anti-Bot. |

## 4. Cấu trúc dữ liệu phản hồi (Response)
Dữ liệu phản hồi được trả về dưới dạng JSON với các trường thông tin trọng yếu:

- **available_amount (Integer):** Tổng số lượng xu khả dụng có thể sử dụng.
- **expiry_info (Object):** Chứa mảng các đối tượng mô tả chi tiết thời gian (ngày/tháng/năm) và số lượng xu sắp hết hạn.
- **mtime (Long):** Thời điểm cập nhật dữ liệu gần nhất theo định dạng Unix Timestamp.

## 5. Ví dụ triển khai (Python)

```python
import requests

def get_coin_summary(cookie_string):
    """
    Truy vấn thông tin tóm tắt xu từ Shopee API.
    """
    url = "[https://shopee.vn/api/v4/coin/get_user_coins_summary](https://shopee.vn/api/v4/coin/get_user_coins_summary)"
    headers = {
        "cookie": cookie_string,
        "x-api-source": "pc",
        "referer": "[https://shopee.vn/shopee-coins](https://shopee.vn/shopee-coins)",
        "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
    }
    
    try:
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        return {"status": "error", "message": str(e)}