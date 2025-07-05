# Tracking-Train
# 🚶‍♂️ YOLOv5 + DeepSORT 객체 추적 

## 📌 개요
`train.zip`에 포함된 연속 이미지 프레임들을 이용해 YOLOv5로 객체 감지하고, DeepSORT로 추적을 수행하였습니다.

## 🧪 환경
- Google Colab
- YOLOv5 (v6.1)
- DeepSORT
- PyTorch, OpenCV

## ▶️ 실행 명령어
```bash
python track.py --source images/ --yolo_model yolov5s.pt --img 640 --conf-thres 0.4

##코드##
1. 오픈소스 클론 및 설치
# YOLOv5 + DeepSORT 통합 오픈소스 클론
!git clone https://github.com/mikel-brostrom/Yolov5_DeepSort_Pytorch.git
%cd Yolov5_DeepSort_Pytorch

# 의존성 설치
!pip install -r requirements.txt

# YOLOv5 모델 다운로드
!wget https://github.com/ultralytics/yolov5/releases/download/v6.1/yolov5s.pt

2. 이미지 프레임들 업로드
from google.colab import files
uploaded = files.upload() 

# 압축 해제
import zipfile
import os

# images 폴더 생성 후 압축 해제
os.makedirs("images", exist_ok=True)

with zipfile.ZipFile("train.zip", 'r') as zip_ref:
    zip_ref.extractall("images")

3. 추적 실행
!python track.py --source images/ --yolo_model yolov5s.pt --img 640 --conf-thres 0.2 --device cpu

4. 결과 프레임 확인
import glob
from IPython.display import Image, display

exp_dirs = sorted(glob.glob("runs/track/exp*"))
if not exp_dirs:
    print("❌ 추적 결과 없음. 다시 실행해보세요.")
else:
    latest = exp_dirs[-1]
    imgs = sorted(glob.glob(f"{latest}/*.jpg"))
    for img in imgs[:3]:  # 결과 중 앞 3장 미리보기
        display(Image(filename=img))

5. 결과 영상으로 저장
!apt install ffmpeg -y
!ffmpeg -framerate 10 -i {latest}/frame%03d.jpg -c:v libx264 -pix_fmt yuv420p output.mp4

6. 결과 영상 다운로드
from google.colab import files
files.download("output.mp4")
