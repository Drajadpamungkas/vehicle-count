# Vehicle Detection and Counting System

## Deskripsi
sistem deteksi objek dan penghitungan objek menggunakan model YOLO. Program ini memproses video input untuk mendeteksi dan menghitung objek yang melewati area tertentu dalam video, kemudian menyimpan hasilnya dalam video output. Berikut adalah penjelasan dari setiap bagian program:

## Penjelasan Kode
# Import Library:
 ```bash
import cv2
from ultralytics import YOLO, solutions
```
- cv2 adalah pustaka OpenCV untuk pemrosesan gambar dan video.
- YOLO dan solutions diimpor dari pustaka ultralytics untuk menggunakan model YOLO dan solusi terkait.

# Load Model:
 ```bash
model = YOLO("yolov10s.pt")
```

# Baca Video:
 ```bash
cap = cv2.VideoCapture("/content/toll_gate.mp4")
assert cap.isOpened(), "Error reading video file"
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
```
- Video dimuat dari file dengan path "/content/toll_gate.mp4".
- Lebar, tinggi, dan frame rate video diambil untuk digunakan dalam penulisan video output.

# Tentukan Titik Wilayah:
 ```bash
region_points = [(0, 102), (592, 250), (592, 301), (0, 153)]
```

# Tentukan Titik Wilayah:
 ```bash
region_points = [(0, 102), (592, 250), (592, 301), (0, 153)]
```
# Video Writer:
```bash
video_writer = cv2.VideoWriter("object_counting_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))
```
- Objek cv2.VideoWriter digunakan untuk menyimpan video output dengan nama "object_counting_output.avi".

# Inisialisasi Penghitung Objek:
 ```bash
counter = solutions.ObjectCounter(
    view_img=True,
    reg_pts=region_points,
    names=model.names,
    draw_tracks=True,
    line_thickness=2,
)
```
ObjectCounter digunakan untuk menghitung objek dalam video berdasarkan wilayah yang ditentukan.

# Proses Video Frame:
 ```bash
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or video processing has been successfully completed.")
        break
    tracks = model.track(im0, persist=True, show=False)
    im0 = counter.start_counting(im0, tracks)
    video_writer.write(im0)
```
- Frame video dibaca satu per satu.
- Deteksi dan pelacakan objek dilakukan pada setiap frame.
- Penghitung objek dijalankan untuk menghitung objek dalam wilayah yang ditentukan.
- Frame yang telah diproses disimpan ke video output.

# Tutup Semua:
 ```bash
cap.release()
video_writer.release()
cv2.destroyAllWindows()
```
- Semua sumber daya dilepaskan dan jendela OpenCV ditutup.

## Fitur
- **Membaca Berkas Video Input**: Membaca video dari berkas dan mengekstrak bingkai video.
- **Deteksi Kendaraan**: Mendeteksi kendaraan dalam bingkai video menggunakan model YOLO.
- **Pelacakan Kendaraan**: Melacak kendaraan di seluruh bingkai video dan menetapkan ID unik untuk setiap kendaraan.
- **Penghitungan Kendaraan**: Menghitung jumlah kendaraan (mobil dan bus) yang melewati gerbang tertentu dalam video.
- **Visualisasi**: Menampilkan kotak pembatas untuk kendaraan yang terdeteksi, ID unik untuk kendaraan yang dilacak, dan jumlah kendaraan yang melewati gerbang.
- **Video Keluaran**: Menyimpan video keluaran dengan visualisasi sistem penghitung kendaraan.

## Persyaratan
- Python 3.x
- OpenCV (`cv2`)
- Ultralytics YOLO library
- Model YOLO yang telah dilatih (`yolov10s.pt`)

## Instalasi
1. **Clone Repository**
   ```bash
   git clone https://github.com/Drajadpamungkas/vehicle-detection-and-counting.git
   cd vehicle-detection-and-counting
