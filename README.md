# SurvivalAnalysisForCervicalCancer

## Finding Cutpoints for Continuous Variables

**【本段簡述】**

首先觀察各變數的Log Relative Hazard，依據變數與Log Relative Hazard 的關係使用Optimal Equal-HR Method和Hiearchical Bayes Model兩種方法對變項做切割，兩種方法的主要差異是在於前者主要用於切割Log Relative Hazard和變數間的是「U-shaped relationship」，而後者主要用於切割兩者屬於「Lineaer relationship」。

**切割方式介紹：**

1. Optimal Equal-HR Method：We use findcutpoints() in R package named CutpointsOEHR to determine two optimal cut-points of a continuous biomarker based on the U-shaped relationship between the value of the biomarker and its hazard ratio.

2. Hiearchical Bayes Model：We use rhier() in the R package named rolr to determine two cut points of a continuous biomarker. Making use of the running logrank test rlr(), the method first identifies an optimal cutpoint that gives the largest logrank statistic to split into two groups, and then repeats the process in each of the resulting groups to find additional two cutpoints. It then takes the cutpoint that gives the larger test statistic between the two as the second optimal cutpoint.


## Fit Univariate Cox Model

**【本段簡述】**

將目前的兩種切割方式分別進行univariate cox model fitting，接著比較兩種切割方式和原本未切割時的Cox Model在Log Rank Test之結果，每個變數皆取三者中結果中「達顯著水準p-value = 0.05」且「最為顯著」的方式，並依據結果重新建立新的dataset，以進行Full Model Selection。

## Full Model Selection
**【本段簡述】**

這邊我們想依據Univariate Cox Model中顯著的變數，進行Full Model Selection。在建立模型前，先捨棄遺失值過多的變數，以免影響模型配適，接著使用Lasso Model Selection協助篩選變數。最後，我們合併其他在univariate cox model中表現顯著的Biomarkers，希望能得到更完整的模型。

### Lasso Model Selection
選定要進行變數篩選的16個變數後，我們使用R packageglmnet中的cv.fit進行lasso正則化Cox Model。參數考量上，判斷模型好壞的lambda我們選擇使用「Partial Likelihood Deviance」，而cross validation我們則選擇10-fold cv，最後參考使用lasso模型中lambda最小的模型。

