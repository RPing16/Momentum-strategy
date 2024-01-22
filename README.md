# Momentum strategy
將動能策略應用於台股，每日買進過去D天漲幅最多的前N檔股票，權重依據當日漲幅進行加權

# files
1. **Momentum.ipynb**
- 動能策略code
2. **Strategy performance analysis.html**
- 動能策略績效分析code
3. **Strategic Performance Analysis Chart**
- 動能策略績效分析圖表

## data
- benchmark : 台灣加權股價指數(Y9999)
- Strategy : 台灣所有上市上櫃公司
- 類別 : 報酬率資料
- 時間 : 2010/01/01 ~ 2023/06/30
- sources : TEJ

## 內容摘要
1. **讀取並處理data**
2. **設計動能策略**
3. **回測**
4. **參數最佳化**
5. **檢驗每日持股數量是否有成功固定住**
6. **使用最佳參數生成分析圖表**

## 結論
- 期間 : 動能策略短期優於長期
- 適用性 : 動能策略在扣除交易成本後sharpe ratio仍能>1，最大sharpe ratio = 4.0，績效很好，說明台股適用動能策略

## code重點說明
1. **normalize**
- 計算每日股票持有權重時進行標準化，依據當日漲幅進行加權
2. **signal**
- signal>=0，signal>0買進持有，signal=0不持有。signal>0的幅度愈大，持有權重愈大
3. **異常表現**
- 跡象 : 在2021~2024策略累積報酬突然暴增
- 推測 : 某些股票的在這段時間的報酬率極高，導致這些股票的signal極大(持有權重極大)
- 檢驗方法 : 將股票總報酬貢獻度進行排序，查看是否貢獻度集中在某些股票上
- 結論 : 2021~2024總報酬貢獻度集中在少數股票上，貢獻最高的為 - 陽明海運(2609)，原因可能為該期間航運股飆漲
4. **參數最佳化 - days**
- 使用不同參數days，尋找哪個days使策略報酬極大
- 結果 : 短期動能策略績效明顯優於長期

## 策略績效分析圖表
![示例圖片](https://github.com/RPing16/Momentum-strategy/blob/main/test.jpg)
