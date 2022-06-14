# pix2pix繪圖模型訓練
pix2pix繪圖模型訓練
## Credits: 該模型是基於 [affinelayer's pix2pix-tensorflow](https://github.com/affinelayer/pix2pix-tensorflow) 的程式進行訓練，該程式由[Christopherhesse](https://github.com/christopherhesse)提供

## 訓練程式下載及資料放置
Step 1: 請先點選連結[pix2pix-tensorflow](https://github.com/affinelayer/pix2pix-tensorflow) 把整個repository下載下來並解壓縮

Step 2: 解壓縮後點選進入pix2pix-tensorflow-master資料夾後先新設一個facades資料夾(若不想依照以下檔名更改請自行於colab調整)，點選進入後設定三個資料夾：train，test，val

Step 3: 下載訓練資料將裡面的圖片放入train資料夾，若要進行模型測試，則留下10到15張照片放置到val資料夾

Step 4: 若想自行訓練自己的資料，請將照片及色塊自行組合成像訓練資料的圖片對，圖片尺寸為64的倍數，或參考[pix2pix-tensorflow](https://github.com/affinelayer/pix2pix-tensorflow)資料製作方式

## 模型訓練
Step 1: 到 google colab開啟**繪圖模型訓練.ipynb**訓練檔案

Step 2: 到**執行階段/變更執行階段類型**將硬體加速器更改為GPU(強烈推薦，不然訓練會很久!!)

Step 3: 在該程式碼進行訓練設置
```
! python pix2pix.py \
  --mode train \
  --output_dir facades_trains \
  --max_epochs 200 \
  --input_dir facades/train \
  --which_direction BtoA
  ```
 說明:  
 mode train: 訓練模式  
 output_dir: 輸出的資料夾，預設為facades_trains  
 max_epoch: 最大訓練循環，預設為200輪  
 input_dir: 輸入訓練資料，預設為 facades/train  
 which_direction: 執行模式，預設為 BtoA，若要訓練其他模式，請參考[pix2pix-tensorflow](https://github.com/affinelayer/pix2pix-tensorflow)
 
Step 4: 測試訓練模型
 ```
 ! python pix2pix.py \
  --mode test \
  --output_dir facades_test \
  --input_dir facades/val \
  --checkpoint facades_train
  ```
  說明:  
  mode test:  測試模式  
  output_dir: 輸出的資料夾，預設為facades_test  
  input_dir:  輸入訓練資料，預設為facades/val  
  checkpoint: 模型訓練期間的斷點儲存，預設儲存位置為facades_train  
  
Step 5: 儲存模型
 ```
 %cd /content/drive/"My Drive"/"Colab Notebooks"/"pix2pix-tensorflow-master"/"pix2pix-tensorflow-master-exp"
! python ./server/tools/export-checkpoint.py \
  --checkpoint facades_train \
  --output_file ./server/static/models/facades_BtoA0108_1.json #注意model資料夾要自己新增，不然檔案沒有資料夾存入
  ```
  說明:  
  checkpoint: 模型訓練期間的斷點儲存，預設儲存位置為facades_train  
  output_file: 模型輸出，可以儲存json或pict，若要做後續介面使用，請使用pict。  
  ### "注意model資料夾要自己新增，不然檔案沒有資料夾存入"
  
  介面使用請至以下連結參考: [sketchkun-archdrawer](https://github.com/manosaki/sketchkun-archdrawer)
  
  
 
