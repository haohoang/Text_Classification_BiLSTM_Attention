# Text_Classification_BiLSTM_Attention
Phân  loại  văn bản tiếng  Việt dựa trên học sâu
1. Dữ liệu
- Tiếng Việt
- 10 Chủ đề 
2. Tiền xử lý
- Bước xử lý: tách từ, loại bỏ stopwords, ký tự đặc biệt
- Lấy vector nhúng của từ (Word2Vec)
3. Mô hình
- Thay vì cho trực tiếp vector pre-train của từ làm đầu vào của mạng thì vector của từ sẽ được tính bằng trung bình cộng vector của từ đứng liền trước từ đó, vector của từ đó và vectơ của từ đứng liền sau từ đó.
- Sử dụng mạng LSTM hai chiều để học các đặc trưng của văn bản vì ý nghĩa của một từ không chỉ liên quan đến những những từ đứng trước nó mà còn có cả từ đứng sau nó.
- Hai trạng thái ẩn của mỗi chiều LSTM của từ xi sẽ được kết hợp lại (hi) làm đầu vào cho seft-attention để tính trọng số i cho nó.
- Trọng số i được tính bằng softmax(hi, V) với V là vector biểu diễn cho category, sẽ được học trong quá trình huấn luyện. Kết hợp các trọng số i và các trạng thái ẩn hi được vector đặc trưng có trọng số ha. 
- Biểu diễn văn bản cuối cùng s chính là kết hợp của ha và hai trạng thái ẩn cuối cùng của hai mạng LSTM.
- Cuối cùng cho s làm đầu vào một tầng kết nối đầy đủ có sử dụng dropout (nhằm tránh việc overfitting) có số đầu ra bằng số thể loại. Áp dụng softmax cho đầu ra cuối để dự đoán thể loại của văn bản.
![Picture1](https://user-images.githubusercontent.com/50910747/129135641-a9557c5b-a034-4db0-ad02-1ee23e9a2c28.png)

4. Hàm mục tiêu
Hàm mục tiêu là cross-entropy:
𝐿 = ∑1_(𝑖=1)^𝑇▒log⁡〖𝑝(𝑦_𝑖 ┤|  𝑋_𝑖, 𝜃)+ 𝜆‖𝜃‖_2^2 〗 
Với (𝑋_𝑖, 𝑦_𝑖) là các cặp ví dụ học, T là số ví dụ học, 𝜃 là tất cả các tham số của mô hình, 𝜆 là tham số của thành phần regularization. Các trọng số được cập nhật bằng giải thuật Adam.

5. Tham khảo
[1] Du, Changshun, and Lei Huang. "Text classification research with attention-based recurrent neural networks." International Journal of Computers Communications & Control 13.1 (2018): 50-61

