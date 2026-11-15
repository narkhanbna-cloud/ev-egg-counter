# E. vermicularis өндөг тоолуур (Vercel)

YOLO26s + SAHI илрүүлэлт, AKAZE/RANSAC бүртгэл + Global Tracking. **Бүх тооцоолол хэрэглэгчийн хөтөч дотор** (ONNX Runtime Web, OpenCV.js) явагдана — backend/сервер шаардлагагүй, зураг, видео хаашаа ч илгээгдэхгүй.

## Файлын бүтэц

```
index.html                              ← апп (зураг + видео)
vercel.json                             ← загварын файлыг кэшлэх тохиргоо
model/ev_yolo26s_640_nms_fp16.onnx      ← загвар (≈19 MB) — ӨӨРӨӨ НЭМНЭ
```

## Загварын файл хаана байна

`narkhanbna@gmail.com` → Google Drive → `MyDrive/EV_YOLO26_SAHI/export/` дотор:

| Файл | Хэмжээ | Тайлбар |
|---|---|---|
| `ev_yolo26s_640_nms_fp16.onnx` | 19 MB | **Үүнийг ашиглана** (GitHub вэбээр оруулж болно) |
| `ev_yolo26s_640_nms.onnx` | 38 MB | fp32 хувилбар (git-ээр push хийх шаардлагатай) |
| `ev_yolo26s_final.pt` | 20 MB | PyTorch жин (Colab/Python-д) |
| `model_meta.json` | — | оролт/гаралт, санал болгох босго |

fp32-ийг ашиглах бол `index.html` дотор `MODEL_URL`-ийг `'model/ev_yolo26s_640_nms.onnx'` болгоно.

## Vercel-д байршуулах (GitHub-аар)

1. **GitHub** → New repository → нэр: `ev-egg-counter` → Public эсвэл Private → Create.
2. Репо дээр **Add file → Upload files** → `index.html`, `vercel.json`, `README.md`-ийг чирж оруулна → Commit.
3. Дахин **Add file → Upload files** → `model` нэртэй хавтсыг (дотор нь `ev_yolo26s_640_nms_fp16.onnx`) чирж оруулна → Commit.
   (Вэбээр оруулахад нэг файл ≤ 25 MB байх ёстой — fp16 загвар багтана.)
4. **vercel.com** → Add New → Project → GitHub-аа холбоод `ev-egg-counter`-ийг **Import**.
5. Framework Preset: **Other**, Build Command: хоосон, Output Directory: хоосон (эсвэл `.`) → **Deploy**.
6. 1 минутын дараа `https://ev-egg-counter.vercel.app` маягийн хаяг гарна. Цаашид GitHub-д өөрчлөлт оруулах бүрт Vercel автоматаар шинэчилнэ.

## Ашиглах

- **Зураг** таб: микроскопын зураг оруулахад шууд өндөг тоолно (босгыг гулсуураар өөрчилнө).
- **Видео** таб: видео файл сонгомогц тоолол автоматаар эхэлнэ.
- **📷 Камераас шууд тоолох**: микроскопын USB камер эсвэл утасны камераас бодит хугацаанд тоолно, «Зогсоох» дарахад хураангуй гарна. Камер зөвхөн HTTPS дээр ажилладаг — Vercel автоматаар HTTPS өгнө.

## Анхааруулга

- Анх нээхэд загвар (19 MB), видеоны таб руу ороход OpenCV.js (≈10 MB) татагдана. Дараа нь кэшлэгдэнэ.
- WebGPU-тэй хөтөчид (Chrome/Edge) хурдан, бусад нь WASM (CPU)-аар ажиллана.
- Хөтөчийн OpenCV.js-д SIFT байдаггүй тул видеоны бүртгэлд AKAZE онцлог цэг + өөрийн RANSAC (эргэлт + масштаб + шилжилт) ашигласан. Туршилтын видеон дээр 200 фреймээс 18-д бүртгэл амжилтгүй болсон (Python SIFT: 23).
- Судалгааны туршилтын хэрэгсэл; лабораторийн дүгнэлтийг орлохгүй.
