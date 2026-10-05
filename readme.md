Phân tích lỗi

Đề bài có tiêu đề "Shallow Copy" nhưng nội dung thực tế là 3 lỗi truy cập và xóa thuộc tính, không có thao tác sao chép đối tượng nào trong mã.

Lỗi 1: Dot Notation với biến động

js
const basePrice = bookingReservation.priceKey;

Dot Notation hiểu priceKey là tên chữ của thuộc tính, không đọc giá trị biến. Đối tượng không có thuộc tính "priceKey" nên kết quả là undefined. Hệ quả: earlySurcharge = NaN, totalAmount = NaN.

Lỗi 2: Gán undefined thay vì xóa thuộc tính

js
bookingReservation.discountCode = undefined;

Phép gán chỉ đổi giá trị thành undefined, còn key 'discountCode' vẫn tồn tại. Do đó 'discountCode' in bookingReservation vẫn trả về true, và vòng lặp for...in vẫn in ra dòng discountCode: undefined. Phải dùng delete để xóa hẳn key.

Lỗi 3: bookingReservation.key trong for...in

js
console.log(key + ": " + bookingReservation.key);

key là biến chứa tên thuộc tính ở mỗi vòng lặp. Dot Notation tìm thuộc tính tên chữ "key" (không tồn tại) nên mọi dòng đều in undefined. Phải dùng bookingReservation[key].

Bảng Test Cases
Trường hợp kiểm thử	Dữ liệu đầu vào	Kết quả sai thực tế	Kết quả đúng mong đợi
TC1: Voucher không hợp lệ, nhận phòng sớm	isVoucherValid = false, roomPrice: 1500000, checkInHour: 9, priceKey = "roomPrice"	earlySurcharge: NaN, totalAmount: NaN; discountCode: undefined vẫn hiện trong danh sách; mọi dòng in giá trị undefined; 'discountCode' in bookingReservation → true	earlySurcharge: 450000, totalAmount: 1950000; không còn key discountCode; mỗi dòng in đúng giá trị; 'discountCode' in bookingReservation → false
TC2: Voucher hợp lệ, nhận phòng đúng giờ	isVoucherValid = true, roomPrice: 1500000, checkInHour: 14	totalAmount: NaN; mọi dòng in giá trị undefined	totalAmount: 1500000; discountCode: SUMMER10 được giữ lại; 'discountCode' in bookingReservation → true