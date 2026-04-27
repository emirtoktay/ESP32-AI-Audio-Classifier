# ESP32 Real-Time Sound Classification 

Bu proje, ESP32 mikrodenetleyicisi ve INMP441 I2S mikrofon kullanarak ortamı gerçek zamanlı (real-time) dinleyen ve yapay zeka ile sesleri sınıflandıran bir güvenlik sistemidir. 

Model, ortamdaki sesin **Polis Sireni**, **Silah Sesi (Gunshot)** veya **Arka Plan Gürültüsü** olup olmadığını saniyenin çok küçük bir bölümünde (yaklaşık 70ms) tespit eder. Sinyal işleme (MFCC) ve makine öğrenmesi (TensorFlow Lite) adımları tamamen ESP32 üzerinde koşmaktadır.

##  Donanım Gereksinimleri
* ESP32 Geliştirme Kartı
* INMP441 Omnidirectional I2S Mikrofon Modülü
* Jumper Kablolar

##  Pin Bağlantıları (Wiring)
Mikrofonun I2S protokolü ile doğru çalışabilmesi için L/R pininin GND'ye (veya boşta bırakılarak test edilmesi) dikkat edilmelidir.

| INMP441 Pin | ESP32 Pin | Not |
| :--- | :--- | :--- |
| **VDD** | 3.3V | Kesinlikle 5V kullanmayın! |
| **GND** | GND | |
| **L/R** | GND / Boş | Sol kanal seçimi için |
| **SCK** | GPIO 26 | Serial Clock |
| **WS** | GPIO 32 | Word Select |
| **SD** | GPIO 33 | Serial Data (Mikrofon Çıkışı) |

##  Yapay Zeka Modeli ve Eğitim
Projenin beyni **Edge Impulse** kullanılarak eğitilmiştir. 
1. Ham ses verileri (Siren, Silah, Arka plan) 16000Hz formatında sisteme yüklendi.
2. **MFCC (Mel-Frequency Cepstral Coefficients)** algoritması ile seslerin frekans parmak izleri çıkarıldı.
3. 1D Convolutional Neural Network (CNN) mimarisi ile model eğitildi ve C++ Arduino kütüphanesi olarak derlendi.

##  Nasıl Çalıştırılır?
1. Bu repoyu bilgisayarınıza indirin.
2. Edge Impulse'tan indirdiğiniz (veya kendi eğittiğiniz) `.zip` kütüphanesini Arduino IDE'de `Taslak > library'i İçer > .ZIP Kitaplığı Ekle` yoluyla projeye dahil edin.
3. ESP32'yi bilgisayara bağlayın (Yükleme hatası alırsanız Upload Speed'i `115200` olarak değiştirin).
4. Kodu karta yükleyin ve Arduino IDE Seri Port Ekranını (Baud: `115200`) açın.
5. Ortama bir siren veya silah sesi dinletin ve yapay zekanın tepkisini izleyin!