# final

# 環境創建
 首先克隆模型
```
!git clone https://github.com/ultralytics/yolov5
```
![1](https://github.com/user-attachments/assets/dabf2a64-1a06-44a3-add7-711756629b59)

然後把裡面requirements的txt文件環境給pip下來
```
%cd yolov5
%pip install -r requirements.txt
```
因為colab已經幫我們下載好這些環境了，所以看到很多都顯示‘already satisfied’
![2](https://github.com/user-attachments/assets/49b1c231-b415-45ee-b7bd-6b6f74ccef89)

接著檢查一下notebook初始化情況
```
import torch
import utils
display=utils.notebook_init()
```
![3](https://github.com/user-attachments/assets/45959d20-94a3-49d2-987d-7020e652b169)
# 推理
先用自帶的yolov5.pt來推理一下
```
!python detect.py
```
其實這裡可以改的參數有很多，具體都在detect.py裡有說明
![4](https://github.com/user-attachments/assets/6ce0f5bf-ef45-4b2f-a369-975b6fff2f78)

把圖片打印出來（也可以直接在文件夾裡看）
![5](https://github.com/user-attachments/assets/3b8070e2-b06f-4b8c-a188-89ce5f238d5b)

# 驗證
首先下載用來驗證的數據集，這裡下的是coco128
```
torch.hub.download_url_to_file('https://ultralytics.com/assets/coco2017val.zip', 'tmp.zip')  # download (780M - 5000 images)
!unzip -q tmp.zip -d ../datasets && rm tmp.zip  # unzip
```
![6](https://github.com/user-attachments/assets/f3086707-c9b9-40c2-9a6e-0619adb8357a)

接著運行val.py進行驗證（這裡的可選參數也都有寫在val.py裡）
```
!python val.py --data coco.yaml
```
![7](https://github.com/user-attachments/assets/816d958f-272f-486e-b05f-35318022bb09)

# 訓練
執行train.py來訓練模型，這裡自己設置了一些參數，比如batchsize設置成64，訓練次數5次，訓練的數據集用的是coco128（必須要之前下載過的並且在data文件夾裡有對應的yaml文件），優化器用的Adam，具體的參數在train.py文件裡也有說明
```
!python train.py --batch 64 --epochs 5 --data coco128.yaml --optimizer Adam
```
![8](https://github.com/user-attachments/assets/16d6c7e4-dd00-4d45-a084-368f3c399aba)
# 最後用自己訓練出的pt文件去推理一次
首先把best.pt文件放到yolov5主文件夾下，不然會報錯找不到該文件
![11](https://github.com/user-attachments/assets/220033c3-1318-4301-8e39-2eebd80637e8)

再運行detect文件，指定weight為剛剛訓練出的best.pt(修改過)
```
!python detect.py --weight /content/yolov5/runs/train/exp/weights/best.pt
```
![9](https://github.com/user-attachments/assets/b81f5633-744e-41c2-bef3-d79cbf08240a)

結果保存在runs/detect/exp2裡
最後打印出來看一下
```
display.Image(filename='/content/yolov5/runs/detect/exp2/zidane.jpg', width=800)
```
![10](https://github.com/user-attachments/assets/898c63df-395b-482e-ae3a-42b3773a40b5)
