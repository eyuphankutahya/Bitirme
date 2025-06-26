Karmaşık Sahnede Araç Logosu Tespiti – Streamlit Uygulaması
YOLOv8 + ByteTrack / BoT-SORT destekli fotoğraf & video logo algılama

İçindekiler
Genel Bakış

Öne Çıkan Yetenekler

Demo

Hızlı Kurulum

Kullanım

Proje Dizini

Modeli Yeniden Eğitmek

Sık Sorulan Sorular

Katkı Sağlama

Lisans

Genel Bakış
Bu repo, karmaşık arka planlı görüntü / videolarda araç logosu tespiti yapmak için hazırlanmış bir Streamlit uygulamasını içerir.

YOLOv8 tabanlı best.pt dedektörü

Fotoğraf için Yüksek Doğruluk Modu (parçalı tarama + 2 aşamalı NMS)

Video için ByteTrack veya BoT-SORT takipçili gerçek-zamanlı izleme

FFmpeg ile otomatik MP4 yeniden kodlama

Opsiyonel kare atlama, video sabitleme (VidStab) ve hızlı ön-izleme

<p align="center"> <img src="docs/screenshot.jpg" width="80%" alt="uygulama ekran görüntüsü"> </p>
Öne Çıkan Yetenekler
Özellik	Açıklama
Tek tuşla kurulum	pip install -r requirements.txt
GPU desteği	CUDA algılanırsa model otomatik olarak GPU’ya taşınır
Yüksek doğruluk modu	Küçük logoları yakalamak için görüntüyü 4 tile’a böler
Gerçek zamanlı takip	ByteTrack / BoT-SORT, kayıp kutuları kareler arasında telafi eder
FFmpeg yeniden kodlama	Geçici AVI → H.264 MP4, tarayıcı uyumluluğu
Kolay arayüz	Tamamen Streamlit; yükle-tetikle-indir akışı

Demo
bash
Kopyala
Düzenle
# Örnek fotoğraf
streamlit run app.py -- --demo demo/car.jpg

# Örnek video (hızlı ön-izleme, ByteTrack)
streamlit run app.py -- --demo demo/traffic.mp4 --fast-preview --tracker ByteTrack
Demo dosyaları demo/ klasöründedir.

Hızlı Kurulum
bash
Kopyala
Düzenle
git clone https://github.com/kullaniciadi/vehicle-logo-detection.git
cd vehicle-logo-detection

# Sanal ortam (önerilir)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Gereksinimler
pip install -r requirements.txt
#  ↳ streamlit ultralytics opencv-python torch torchvision pillow vidstab imageio-ffmpeg ...

# YOLO modeli
models/
└── best.pt   # ⭐️ eğitimli dedektör (repo ile birlikte gelir)
Kullanım
bash
Kopyala
Düzenle
# Varsayılan ayarlarla başlat
streamlit run app.py
Akış	Adım
Fotoğraf	Fotoğrafı yükle → “Yüksek Doğruluk” seç/ seçme → İşle
Video	Videoyu yükle → Kare Atlama / Takipçi / Sabitleme ayarla → İşle → Tamamlanan MP4’ü indir
CLI varyantı	python tools/cli.py --source myvideo.mp4 --tracker botsort --skip 2

Proje Dizini
bash
Kopyala
Düzenle
.
├── app.py                 # Streamlit arayüzü
├── README.md
├── requirements.txt
├── models/
│   └── best.pt            # 🚗 Logo dedektörü
├── utils/
│   ├── detector.py        # LogoDetector sınıfı
│   ├── video.py           # Video işleme yardımcıları
│   └── image.py           # Görüntü işleme yardımcıları
└── demo/
    ├── car.jpg
    └── traffic.mp4
Modeli Yeniden Eğitmek (isteğe bağlı)
Veri Kümesi: VOC / COCO formatında etiketlenmiş datasets/vehicle-logo/*

Eğitim

bash
Kopyala
Düzenle
yolo detect train data=vehicle-logo.yaml model=yolov8m.pt imgsz=640 epochs=200 batch=16
cp runs/detect/train/weights/best.pt models/
Streamlit uygulamasını yeniden başlatın.

Sık Sorulan Sorular
Soru	Yanıt
CUDA not available hatası?	NVIDIA sürücünüzü + CUDA/CuDNN’i kontrol edin veya CPU’da çalıştırın.
Takip performansı düşük.	skip_n değerini artırın (örn. 2-3) veya fast_preview modunu kapatın.
best.pt nereden?	Çalışmanın eğitim çıktısıdır; models/ altında hazır olarak bulunur.

Katkı Sağlama
Issue veya Pull Request açın

Kod stilini koruyun (black, isort)

Açık-net açıklamalar ekleyin

Lisans
MIT Lisansı • Ayrıntılar için LICENSE dosyasına bakın.
