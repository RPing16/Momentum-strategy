# Momentum strategy
將動能策略應用於台股，每日買進過去D天漲幅最多的前N檔股票，權重依據當日漲幅進行加權

## 動機
- 根據過去文獻，在美股動能屬於長期現象，大約發生在3~12個月
- 好奇台股的動能策略表現如何
- 參考文獻 : https://papers.ssrn.com/sol3/papers.cfm?abstract_id=299107

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
- 類別 : 日報酬率資料
- 時間 : 2010/01/01 ~ 2023/06/30
- sources : TEJ

## 交易成本
台股目前券商手續費有優惠，普遍有6折，部分券商提供1~2.8折優惠。若以3折優惠計算:
- 手續費 : 0.1425%*0.3，買賣都收
- 證交稅 : 0.3%，賣出才收
- 買入交易成本 = 0.1425%*0.3 = 0.04275%
- 賣出交易成本 = 0.1425%*0.3 + 0.3% = 0.34275%
- total交易成本 = 0.3855%
- 平均total交易成本 = 0.19275%
- 本專案交易成本計算方式採「單邊0.2%」

## 內容摘要
1. **讀取並處理data**
2. **設計動能策略**
3. **回測**
4. **檢驗每日持股數量是否有成功固定住**
5. **報酬貢獻度分析**
6. **參數分析**
7. **結論**
8. **使用最佳參數生成分析圖表**

## 結論
1. **在台股動能現象時間愈短愈顯著，與美股實證研究不同**
   - 推測可能原因 : 台股資金規模較小，短期容易受到炒作，長期下來，受炒作的股票報酬反而會下降
   - 根據 : 過去文獻，在資金規模較大，市場較成熟的美國、歐洲市場進行動能策略實證研究，動能現象均發生在3~12個月
2. **短期動能策略sharpe > 1，說明動能策略可能適用於台股**

## code重點說明
1. **rank**
- 在計算signal時，給予漲幅rank轉換原始訊號 : 報酬率數值 -> 報酬率rank
- 用意 :
    - 使用原始信號分配權重可能發生權重過度集中在某幾個股票(離群值)
    - 為降低風險，將原始信號轉換成rank，如此一來，既保留原始訊號的大小順序關係，又可以控制住權重不會過度集中
- 正面效果 : 降低策略風險，使策略報酬不易受離群值影響，同時使得策略報酬貢獻度較平均   
2. **signal**
- signal>=0，signal>0買進持有，signal=0不持有。signal>0的幅度愈大，持有權重愈大
3. **normalize**
- 計算每日股票持有權重時進行標準化，依據當日漲幅rank進行加權
4. **報酬貢獻度分析**
- 跡象 : 在2021~2023策略累積報酬突然暴增
- 推測 : 某些股票的在這段時間的報酬率極高，導致這些股票的signal極大(持有權重極大)
- 檢驗方法 : 將股票總報酬貢獻度進行排序，查看是否貢獻度集中在某些股票上
- 結論 : 2021~2023總報酬貢獻度集中在少數股票上，貢獻最高的為 - 美德向邦(9103)，原因可能因醫療股在COVID-19期間大漲

## 策略績效分析圖表
![示例圖片](https://github.com/RPing16/Momentum-strategy/blob/main/Strategic%20Performance%20Analysis%20Chart/cum%20ret%20line%20chart.jpg)

![示例圖片](https://github.com/RPing16/Momentum-strategy/blob/main/Strategic%20Performance%20Analysis%20Chart/key%20performance%20table.jpg)

![示例圖片](https://github.com/RPing16/Momentum-strategy/blob/main/Strategic%20Performance%20Analysis%20Chart/EOY%20Returns%20vs%20Benchmark.jpg)

![示例圖片](https://github.com/RPing16/Momentum-strategy/blob/main/Strategic%20Performance%20Analysis%20Chart/rolling%20index.jpg)
