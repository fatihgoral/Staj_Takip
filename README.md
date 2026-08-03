# Gün 01 - Günlük Çalışma Raporu

**Tarih:** 3 Ağustos 2026  
**Konu:** Sentetik Veri Benchmark'ı — Ortam Kurulumu ve Proje Okuması

---

## Günün Özeti

Bugün stajın ilk gününde, sentetik veri üretim ve eğitim pipeline'ını kurmaya başladım. AutoLabel Loop (IDR) ve Railway-Anomaly Synthetic Data sistemlerinin lokal bilgisayarda çalışmasını sağladım. Proje yapısını, iki sistem arasındaki bağlantıyı ve 30 günlük görev planını detaylı olarak öğrendim.

---

## Yapılan İşler

### 1. Proje Dokümantasyonunun Okunması ve Anlaşılması

Staj paketinin tamamını inceledim:
- **BASLANGIC:** 6 haftalık stajın yapısı, paket haritası ve ön koşullar öğrenildi
- **Görev Planı:** Projenin amacı (iki sistem arası kopuk halkayı kapamak), bilimsel kontrol prensibi (sabit kontrol, tek değişken = eğitim verisi) ve 30 günlük haftalı roadmap anlaşıldı
- **IDR Level 0–5 Merdiveni:** Sentetik veri üretiminin 6 seviyesi (gerçek veri → klasik augmentasyon → copy-paste → SDXL → ControlNet → BlenderProc 3D) kavrandı

### 2. Python Ortamı ve Virtual Environment Kurulumu

Masaüstü paketindeki `autolabel-loop` klasöründe Python ortamı hazırlandı:
- Python 3.11.9 tespit edildi 
- Virtual environment (venv) oluşturuldu
- venv aktivasyon başarılı oldu (terminal prefix: `(venv)` görüldü)
- `requirements.txt` dosyasındaki 50+ paket kuruldu (PyTorch, Streamlit, YOLOv8, Transformers, OpenCV vb.)

**Kurulum Süresi:** 7 dakika, tamamı hatasız

### 3. Streamlit UI (AutoLabel Loop) Başlatılması

IDR web arayüzü lokal bilgisayarda çalıştırıldı:
```bash
streamlit run app.py 
