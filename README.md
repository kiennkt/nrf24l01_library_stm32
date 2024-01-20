# nrf24l01_library_stm32
## Mục đích
> Thư viện này được viết để hiểu rõ cách truyền nhận dữ liệu thông qua sóng vô tuyến giữa các node stm32 có kết nối với module nrf24l01 thông qua giao thức SPI
## Giới thiệu
### Module NRF24L01
#### 1. Tần số
- Hoạt động ở dải tần số 2.4 GHz trên thế giới
- Có 126 kênh tần số, mỗi module có thể nhận dữ liệu từ 6 module khác
#### 2. Pinout
![image](https://github.com/kiennkt/nrf24l01_library_stm32/assets/89715566/0c7e03a3-26c6-493f-b54c-4aa82a0955eb)
- ***GND***: Chân nối đất, được nối với chân GND của STM32
- ***VCC***: Nguồn 1.9-3.6V
- ***CE***: Chip Enable, chân chọn chip, chân này hoạt động ở mức tích cực cao, chân này có nhiệm vụ nhận tín hiệu từ STM32 (kéo xuống thấp hoặc kéo lên cao) trước khi setup 1 số lệnh vào thanh ghi, từ đó để chọn chế độ truyền hoặc nhận cho module
- ***CSN***: Chip Select Not( hay NSS: Not Select Slave), chân này hoạt động ở mức tích cực thấp, lắng nghe tín hiệu từ STM32 thông qua SPI( kéo chân này xuống mức thấp trước khi truyền hoặc nhận, sau đó lại kéo lên cao). Đọc kỹ cách hoạt động của SPI qua reference manual bên dưới
- ***SCK***: Serial clock
- ***MOSI***: Multiple output single input
- ***MISO***: Multiple input single output
- ***IRQ***: Interupt request (không sử dụng trong bài này)
#### 3. Cách hoạt động
- Module nrf24l01 truyền và nhận dữ liệu trong cùng 1 kênh. Để 2 module cùng truyền thông được với nhau chúng phải cùng 1 kênh tần số, có thể có 126 kênh từ 2400Mhz->2525MHz (2.400->2.525GHz). Tùy vào mức độ số lượng module và tốc độ truyền thì ta nên chọn kênh trong dải trên cho phù hợp. Lưu ý: nếu ta sử dụng tối đa số kênh và tốc độ tối đa (2Mbps) thì ta nên chọn các kênh cách nhau 2-4 kênh để tránh bị nhiễu, suy hao do chồng phổ
- 1 module có thể nhận dữ liệu từ 6 module khác
- Đọc tài liệu bên dưới để rõ chi tiết nhất về các cách thức truyền nhận
### STM32F103
#### 1. Pinout
***NRF24L01-------------------------STM32F103***
- GND  ………………………………………… GND
- VCC  ………………………………………… 3.3V
- CE   ………………………………………… PA1
- CSN  ………………………………………… PA2
- SCK  ………………………………………… PA5
- MISO ………………………………………… PA6
- MOSI ………………………………………… PA7
## Cài đặt
Thư viện viết dựa trên STM32CubeIDE, sử dụng HAL để có thể dùng các hàm truyền nhận SPI
> 1. Cài đặt STM32CubeIDE tại https://www.st.com/en/development-tools/stm32cubeide.html
> 2. Download hoặc copy source về
> 3. Khởi tạo project
> 4. Chọn SPI1 -> Mode: Mode: Full-Duplex Master, Disable cái Hardware NSS Signal(vì ta dùng code để tạo tín hiệu tại chân NSS/CSN)
> 5. Confifuration: ![image](https://github.com/kiennkt/nrf24l01_library_stm32/assets/89715566/2921fa17-056a-4405-908b-dfbd4d9fa7a2)
> 6. Cấu hình các chân pin như sau: ![image](https://github.com/kiennkt/nrf24l01_library_stm32/assets/89715566/b73974d4-1fec-4433-a5cf-73c288c56869)
> 7. SYSCLK cho max 72Mhz
> 8. Copy lại file main vào project
## Tài liệu
- Reference manual STM32: [Reference Manual](https://github.com/kiennkt/STM32F1/blob/main/Documents/Reference.pdf)
- Product Specification nRF24L01: [Product Specification nRF24L01](https://github.com/kiennkt/STM32F1/blob/main/Documents/nRF24L01_Product_Specification_v2_0-9199.pdf)
