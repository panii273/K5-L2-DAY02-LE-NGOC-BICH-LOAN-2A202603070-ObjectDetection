# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Xe con chở người cỡ nhỏ đến vừa: sedan, SUV, taxi và xe bán tải cabin nhỏ. Tôi gán theo loại xe, không phụ thuộc xe đang chạy hay đỗ. | Không gán khi là xe buýt, xe tải hay mô tô. Bán tải có thùng hở phía sau vẫn là car, không phải truck.|
| `truck` |Xe tải chở hàng và xe đầu kéo: có cabin phía trước và thùng hàng lớn phía sau, thường to và dài hơn xe con.|Phân biệt với bán tải: bán tải cỡ nhỏ thuộc car. Phân biệt với van: van là khối thân liền, không có thùng hàng tách riêng. |
| `bus` | Xe chở khách cỡ lớn, thân dài, cao, nhiều cửa sổ bên hông. Minibus chở khách cũng là bus.|Phân biệt với van: bus dài và có hàng cửa sổ liên tục. Xe van chở người ngắn hơn, ít cửa sổ hơn. |
| `van` | Xe van hoặc minivan chở hàng hoặc chở người: thân hộp liền một khối, cao hơn xe con.| Không phải xe con thông thường (car), cũng không phải xe buýt lớn (bus). Khi chưa chắc van hay bus/car, tôi không tự đoán mà ghi lại để hỏi giảng viên.|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: mép box nằm sát phần thân xe nhìn thấy được, không lấy thêm mặt đường, trời hay xe bên cạnh; mỗi xe một box riêng.
- Vật thể bị che khuất: vẽ box quanh phần nhìn thấy nếu còn nhận ra lớp; hai xe chồng nhau thì vẽ hai box riêng.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh, không đoán phần nằm ngoài ảnh.
- Vật thể rất nhỏ/mơ hồ: Nếu chỉ còn mảnh rất nhỏ không xác định được lớp thì không đoán

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_029` |Nhiều xe chồng lên nhau (xe con đứng trước xe buýt, các xe con che nhau), kèm nhiều xe nhỏ ở xa vẫn nhìn ra là xe |Vẽ mỗi xe một box riêng trên phần nhìn thấy của nó, cho phép các box chồng lấn nhau; xe ở xa vẫn vẽ khi nhận ra được là xe và biết lớp |GUIDE.md: hai xe chồng nhau thì vẽ hai box riêng; vật thể bị che vẫn gán nếu nhận diện được lớp |
| `drive_038` |Cụm nhiều xe con chồng nhau, ảnh mờ, nhiều xe ở xa |Vẽ box cho xe nhận ra được lớp; bỏ những xe không xác định được lớp |GUIDE.md: che khuất vẫn gán nếu nhận diện được lớp; mảnh quá nhỏ không xác định được lớp thì không đoán, ghi lại |
| `drive_044` |Xe van bị nhòe do chuyển động; xe buýt ở xa; nhiều xe car nhỏ ở xa |Gán van theo mắt thường thấy và nghĩ đó là van ; gán bus theo theo mắt thường ; gán car cho xe nhận ra được, bỏ xe không xác định được lớp |GUIDE.md: nhận diện được lớp thì gán; không xác định được lớp thì không đoán, ghi lại; van mơ hồ thì ghi case ID và hỏi |
