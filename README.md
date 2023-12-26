# Transformer-Encoder-Text-Classification là một bài phân loại văn bản báo chí dữ liệu được lấy từ <a href='https://github.com/duyvuleo/VNTC/tree/master/Data/10Topics/Ver1.1'>VNTC</a>
## Giới thiệu
- Các bước xử lý dữ liệu
  - Đầu tiên tải bộ dữ liệu rồi giải nén
  - Đọc dữ liệu
  - Sử dụng thư viện pyvi cho word_segmentation
  - Chuyển dữ liệu từ nhiều file txt thành 1 file csv
- Trong bài này mình sử dụng 2 model đã được train sẵn của vinai
  - một model dùng để tokenize văn bản (dùng để chuyển văn bản thành số)
  - một model dùng để huấn luyện (huấn luyện model dự đoán các nhãn)  
- Note: <a href='https://www.kaggle.com/code/hunguyen01/encoder/notebook'>bài này</a> mình sử dụng GPU P100 trên kaggle để chạy
## Chi tiết model
- Đây là model encoder là một phần của cấu trúc transformer cụ thể là lớp encode
- Transformer có 2 phần là encode (được gọi là BERT) và decode (được gọi là GPT)
- Mô hình này mình sử dụng 12 lớp encoder sau đó đưa qua lớp fully connected để tạo đầu ra cho model
- Trước khi qua lớp fully connected, ta phải sử dụng cơ chế global max pooling để giữ lại số chiều của model (d_model)
- Cơ chế của lớp global max pooling là ta có một tensor(batch_size, seq_len, d_model) lớp này sẽ giảm chiều lại còn (batch_size, d_model)
- Việc này ngoài giúp cho model giảm chi phí tính toán, còn làm cho model tập trung vào đặc trưng quan trọng làm model dự đoán chính xác hơn (soi với global average pooling) 
## Ví dụ về global max pooling
- Ta có tensor(2,2,3)  
[[[87, 22, 90],  
[85, 88, 71]],  
[[65, 64, 15],  
[73, 99,  4]]]  
- Sau khi pooling sẽ thành  
[[87, 88, 90],  
[73, 99, 15]]  
- Ta thấy lớp pooling này đi duyệt qua từng cặp phần tử và chọn ra giá trị lớn nhất, 87 > 85 -> 87, 88 > 22 -> 88, tương tự với các số còn lại
        
## Mô tả cấu trúc thư mục
Train và Test sẽ có cấu trúc chung các file như nhau:  
Train_Full
- label 1 (tên của thư mục cũng là label của các file con trong nó)
  - text file 1 (chứa nội dung của bài báo)
  - text file 2
- label 2
  - text file 1
  - text file 2  
 
Test_Full
- label 1
  - text file 1
  - text file 2
- label 2
  - text file 1
  - text file 2
