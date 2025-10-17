# YOLO Model Eğitimi ve Testi

Bu doküman, **Ultralytics YOLO** kullanarak nesne tespiti modeli eğitmek için gereken adımları içerir.

---

## 1. Ortam Kurulumu

### Windows

```bash
python -m venv yolovenv
yolovenv\Scripts\activate
pip install -U pip
pip install ultralytics
```

### Linux / macOS

```bash
python -m venv yolovenv
source yolovenv/bin/activate
pip install -U pip
pip install ultralytics
```

> Sanal ortam **isteğe bağlı**. İstersen direkt `pip install ultralytics` komutunu çalıştırabilirsin.

---

## 2. GPU Uyum Kontrolü (Opsiyonel)

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

`True` dönerse GPU kullanılabilir.

---

## 3. Veri Yapısı

```
dataset/
 ├── images/
 │    ├── train/
 │    └── val/
 └── labels/
      ├── train/
      └── val/
```

Her `.txt` dosyasında:

```
class x_center y_center width height
```

> Değerler 0–1 aralığında olmalı.

---

## 4. data.yaml Dosyası

```yaml
train: dataset/images/train
val: dataset/images/val
nc: 3
names: ['class0', 'class1', 'class2']
```

---

## 5. Etiketleme

LabelImg veya Roboflow gibi araçlarla verileri etiketleyip `.txt` dosyalarını oluştur ya da halihazırda etiketlenmiş bir veri seti indir.

---

## 6. Modeli Eğitme

```bash
yolo task=detect mode=train data=data.yaml model=yolov8n.pt epochs=100 imgsz=640 batch=16
```

Eğitim tamamlandığında eğitim kayıtlıları,

```
runs/detect/train/weights/best.pt
```
dosyasına kaydedilir. Üst üste birden fazla eğitim olması durumunda `runs/detect` klasörü içerisinde `train1` `train2` ... şeklinde isimlendirilir.

---

## 7. Doğrulama (Validation)

```bash
yolo task=detect mode=val model=runs/detect/train/weights/best.pt data=data.yaml
```

---

## 8. Tahmin (Inference)

### Görsel üzerinde test etme:

```bash
yolo task=detect mode=predict model=runs/detect/train/weights/best.pt source=images/test.jpg
```

### Webcamde (canlı görüntüyle) test etme:

```bash
yolo task=detect mode=predict model=runs/detect/train/weights/best.pt source=0 show=True
```

> `show=True` parametresi, kamera penceresi açarak canlı görüntüyü gösterir.

---

## 9. Modeli Dışa Aktarma

```bash
yolo export model=runs/detect/train/weights/best.pt format=ncnn
```

Desteklenen formatlar: `onnx`, `tflite`, `torchscript`, `engine` vb. ama biz raspide ncnn kullanıyoruz.

---

## 10. Eğitime Devam Etme

```bash
yolo task=detect mode=train data=data.yaml model=runs/detect/train/weights/last.pt epochs=50
```

---

## 11. Yaygın Sorunlar

| Sorun           | Çözüm
| --------------- | ----------------------------------------------
| CUDA bulunamadı | PyTorch sürümünü ve CUDA kurulumunu kontrol et
| Etiket hatası   | `.txt` formatını ve `nc` değerini kontrol et         
| Düşük doğruluk  | Daha fazla veri ekle ya da daha çok batch ile eğit

---

## 12. Son Adım

```bash
pip freeze > requirements.txt
```

Bu, sanal ortamda (env de) yüklü paketleri kaydeder.

---

> İşte bu kadar :))
