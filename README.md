# FDA_HW3-1

以三種不同的 classifier 進行股價漲跌預測

## 討論

1. How did you preprocess this dataset? 

	- 加入 label 欄位：相較前一天股票收盤價為高，則為 1
	- 正規化：讓資料的差異尺度縮小 
	- 以 n 天作為一個運算單位：以n 天判斷股價漲幅比較有意義 
	- 將 Volume 欄位去掉 (實驗過後發現這個欄位不太適合拿來當 input column)

2. Which classifier reaches the highest classification accuracy in this dataset? 
	
| Classifier                    | Accuracy |
| ----------------------------- | -------- |
| Logistic Regression           | 52.38 %  |
| Neural Network - 1 (LSTM)     | 51.63 %  |
| **Neural Network – 2** (LSTM) | 69.23 %  |
| Random Forest Classifier      | 53.17 %  |
| SVM                           | 52.38 %  |


- Why? 	

	股價屬於`連續型資料`，在 Neural Network – 2 方法當中先使用預測股價的方法，接著透過預測的股價趨勢間接算出相對於天數的股價的漲跌結果，以上述幾組實驗結果來說，達到了最高的準確率。  
	
	在 Logistic Regression 的實驗當中，最後測試資料的預測結果偏向於某一個特定類別(0 或 1)，推論在這個實驗當中的數據可能對於模型來說無法學習到什麼，而從股價的波動來看，**漲跌比例也接近 1:1** ，因此對於模型來說可能全部猜同一個也算是它的策略。  
	
	而實驗 Neural Network - 1 和 Neural Network - 2 之間的差異在於對於 loss function 的不同，前者使用 `binary_crossentropy` (for classification)，而後者使用 `mean_squared_error` (for regression)，從結果得出，直接透過股價讓 model 直接推算出 classification 的準確度只有在 50% 上下，而先行運算出數值部份再行推算出 classification 的效果較好。  

- Can this result remain if the dataset is different?

	如果改變資料集，這個方法也會達到在上述所提及的分類器中表現最好的現象。本身股票是一個連續型資料，在判斷「漲」或「跌」的二分類法時，由於每天**股價資料的數字與「漲」和「跌」的兩分類並沒有直接明確的關係**，因而對於「股價」來說，用數字預測數字，再透過數字推算出隸屬於哪種類型，應該還是最好的方法。 

3. How did you improve your classifiers? 

	- 將判斷的`天數拉長`，讓一段時間產生的趨勢可以讓準確率提高
	- 分類問題先求出預測股價再行轉換類別
