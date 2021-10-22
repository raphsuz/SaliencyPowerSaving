# 程式操作手冊

# 操作環境：

## 程式開發使用系統：

1. CentOS 8.0
2. Arch Linux 2020

## 推薦適用系統：

以下按推薦度編排，推薦度高者由上而下

1. Linux （依賴套件和路徑配置簡易）
2. Mac
3. Windows（Python2和Python3的共存設定繁複、theano執行效率差）

## 安裝軟體：

- Python （建議使用 Anaconda Python，在安裝依賴時更為便利）
    - Python 2.7 （MLnet 模型所需，以別名 python2 執行）
    - Python 3.6 以上 （分析 Saliency 所需）

    須同時存在 2 種版本

## 依賴套件：

### Python 2 環境相關

使用 Python 2 環境的原因：因為其中使用到的視覺顯著性分佈預測類神經網路模型[MLnet](https://github.com/marcellacornia/mlnet)，其開發環境在 Python 2 環境上並使用 Theano + Keras 達成，故必須將 Python 2 環境設定好。

首先，在 **Python 2.7 環境**中安裝以下依賴套件：

1. Theano 0.9.0
2. Keras 1.2.2
3. Python OpenCV 3.4.3
  or
  Python OpenCV 3.2.0

以下提供用 Anaconda Python 安裝以上依賴套件的方法：

```python
conda install -c conda-forge theano=0.9.0
conda install -c conda-forge keras=1.0.7
conda install -c conda-forge opencv=3.4.3
```

Keras 安裝示意：

![InstallKeras](F:\Thesis\SPER\README\InstallKeras.png)

### Python 3 環境相關

Python 3 是省電系統開發的環境，而為了在 Python 3 環境下調用 Python 2 的類神經網路模型，推薦使用 Python 3.6 以上的版本，因其內建 subprocesses.run() 函式；若使用舊版，則必須自行安裝。

subprocesses.run 安裝方法：

```
pip install subprocess.run
```

以下則為省電色盤系統開發時所使用的套件：

1. [ColorThief](https://pypi.org/project/colorthief/)

   ```python
   pip install colorthief
   ```

2. Pillow

   ```python
   pip install Pillow
   ```

3. pandas

   ```
   pip install pandas
   ```

4. numpy

   ```
   pip install numpy
   ```

5. elementpath

   ```
   pip install elementpath
   ```

## 程式執行

由於本程式涉及 Python 2 及 Python 3 環境的相互呼叫，**打包後的執行程式檔極其容易出錯**（目前僅成功於原開發機CentOS 8.0系統完美執行，於其他主機均會報錯），故以下操作均為**對程式原始碼的運行**（若以上套件安裝完成，此步驟於 Linux、MacOS及Windows都能有效執行）。

### 基本操作方式

於各編輯器中，開啓「tkSaliencyMain.py」，並按下「run」或「debug」。

以 Spyder 編輯器示範圖如下：

![LaunchFromSpyder](F:\Thesis\SPER\README\LaunchFromSpyder.png)

接著會看到~~極其陽春的~~程式介面浮現：

![SystemInitial](F:\Thesis\SPER\README\SystemInitial.png)

### 預測介面圖片的視覺顯著性分佈

點擊左上角的 Open Image，選擇要進行預測的程式介面截圖，推薦使用 .jpg 格式，Python 對 .jpg 格式的圖像支援最為全面：

![OpenLayoutImage](F:\Thesis\SPER\README\OpenLayoutImage.png)

出現「等待提示」視窗，點擊確定，等待30秒左右即可看到預測出來的視覺顯著性分佈圖：

![WaintingHint](F:\Thesis\SPER\README\WaintingHint.png)

接著即預測出該介面的視覺顯著性分佈：

![OriginalLayoutSalience](F:\Thesis\SPER\README\OriginalLayoutSalience.png)

備註：如果此步驟無回應，極可能是系統並沒有識別到 Python 2 環境，或是 Python 2 環境中的依賴套件未安裝好所致。

### 劃分應用程式元件區域

選擇主題色系（明亮主題、暗沉主題）並點擊確認：

![ConfirmTheme](F:\Thesis\SPER\README\ConfirmTheme.png)

讀取介面元件描述檔（.uix）：

![OpenXML](F:\Thesis\SPER\README\OpenXML.png)

備註：介面元件描述檔（.uix）取自於 Google 的 uiautomator 工具，該工具被打包於 Android SDK 中，使用該工具對應用程式進行截圖時，便會取得該檔案（.uix）。

元件資料表便會顯示於介面中：

![ElementSheet](F:\Thesis\SPER\README\ElementSheet.png)

選擇容許的視覺顯著性差異目標參數及矚目元件參數K，即可輸出元件劃分區域，並且取得初始省電介面。

在計算期間，如果使用的是 Windows 系統，程式顯示無回應純屬正常現象，在 Windows 上對 Python  2 模型的調用導致該問題，可至終端介面查看計算過程：

![NoResponseWhenCal](F:\Thesis\SPER\README\NoResponseWhenCal.png)

![TerminalOutput](F:\Thesis\SPER\README\TerminalOutput.png)

完畢後，輸出元件範圍劃分：

![ElementBorder](F:\Thesis\SPER\README\ElementBorder.png)

點擊確認後，會追加顯示元件排名，以及初始省電介面預覽：

![FirstPowerSavingResult](F:\Thesis\SPER\README\FirstPowerSavingResult.png)

### 測試是否能評估省電介面的視覺顯著性變化

透過下列的操作方式，可以測試輸出省電介面的視覺顯著性分佈與差異評估：

![NewLayoutEvaluationInOnePhase](F:\Thesis\SPER\README\NewLayoutEvaluationInOnePhase.png)

首先，點擊 Predict Saliency Map of New Layout ，可於約30秒等待後，取得 SIM、CC分數，以及最右方產生預測的視覺顯著性分佈：

![ClickNnewLayoutPredict](F:\Thesis\SPER\README\ClickNnewLayoutPredict.png)

再點擊 After Saliency Recalculation ，可以取得新舊介面的元件視覺顯著性差異（於元件資料表處顯示）：

![AfterAPhase](F:\Thesis\SPER\README\AfterAPhase.png)

最終能透過點擊Export2csv，在目錄資料夾中產生報告檔案（.csv）：

![Export2csv](F:\Thesis\SPER\README\Export2csv.png)

此步驟僅作為測試、除錯使用，若想進行遞迴改進省電介面，則需參考

### 依照目標遞迴改進省電介面

若前面測試時，功能均正常執行，則能直接執行右下方 Evaluation and Proceeding 按鈕功能，此功能整合前述3項功能，並輸出最終省電介面建議及色彩表：

![RecurrentImprovedandEvaluation](F:\Thesis\SPER\README\RecurrentImprovedandEvaluation.png)

點擊該按鈕後，通常需等待至少3分鐘，待程式計算完畢後，方有輸出（於Windows系統時，程式易進入無回應狀態）：

![RecurrentWaiting](F:\Thesis\SPER\README\RecurrentWaiting.png)

計算完成後，程式跳出提示：

![MissionCompleted](F:\Thesis\SPER\README\MissionCompleted.png)

### 輸出檔案位置

程式會根據當初載入的介面圖片檔名，建立一個資料夾，內部生成的檔案即計算完成的檔案，其中

- newlayout.jpg資料夾存放省電版介面的預覽圖及其視覺顯著性分佈
- .csv檔案則是該介面的元件資料表及其色彩等級表

![ExportFiles](F:\Thesis\SPER\README\ExportFiles.png)

### 後續運作

有了輸出色碼及省電介面預覽後，就可以至 Android Projects 中修改自己的應用程式介面，例如新增「省電版色彩」的切換按鈕，或是直接將程式介面遵照建議修改。