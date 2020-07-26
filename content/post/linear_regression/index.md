---
title: Stataによる線形回帰モデル

date: 2020-07-26

---

# Stataによる線形回帰モデル

Stataによる線形回帰モデルを実装する。


```stata
# dataの読み込み

use https://www.stata-press.com/data/r16/lbw
```

    
    Unknown #command
    (Hosmer & Lemeshow data)
    

公式サイトのデータより読み込み

189人の出生児の体重に関する研究のデータ


```stata
describe
```

    
    Contains data from https://www.stata-press.com/data/r16/lbw.dta
      obs:           189                          Hosmer & Lemeshow data
     vars:            11                          15 Jan 2018 05:01
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    id              int     %8.0g                 identification code
    low             byte    %8.0g                 birthweight<2500g
    age             byte    %8.0g                 age of mother
    lwt             int     %8.0g                 weight at last menstrual period
    race            byte    %8.0g      race       race
    smoke           byte    %9.0g      smoke      smoked during pregnancy
    ptl             byte    %8.0g                 premature labor history (count)
    ht              byte    %8.0g                 has history of hypertension
    ui              byte    %8.0g                 presence, uterine irritability
    ftv             byte    %8.0g                 number of visits to physician
                                                    during 1st trimester
    bwt             int     %8.0g                 birthweight (grams)
    --------------------------------------------------------------------------------
    Sorted by: 
    

## データの分析

まずデータの分析を行う。

目的変数は出生時の体重である。

母体の年齢や喫煙などが関与していることが予想される。


```stata
su age bwt smoke, detail
```

    
                            age of mother
    -------------------------------------------------------------
          Percentiles      Smallest
     1%           14             14
     5%           16             14
    10%           17             14       Obs                 189
    25%           19             15       Sum of Wgt.         189
    
    50%           23                      Mean            23.2381
                            Largest       Std. Dev.      5.298678
    75%           26             35
    90%           31             36       Variance       28.07599
    95%           32             36       Skewness       .7164391
    99%           36             45       Kurtosis       3.568442
    
                         birthweight (grams)
    -------------------------------------------------------------
          Percentiles      Smallest
     1%         1021            709
     5%         1790           1021
    10%         1970           1135       Obs                 189
    25%         2414           1330       Sum of Wgt.         189
    
    50%         2977                      Mean           2944.286
                            Largest       Std. Dev.       729.016
    75%         3475           4174
    90%         3884           4238       Variance       531464.4
    95%         3997           4593       Skewness      -.2069782
    99%         4593           4990       Kurtosis       2.888821
    
                       smoked during pregnancy
    -------------------------------------------------------------
          Percentiles      Smallest
     1%            0              0
     5%            0              0
    10%            0              0       Obs                 189
    25%            0              0       Sum of Wgt.         189
    
    50%            0                      Mean           .3915344
                            Largest       Std. Dev.      .4893898
    75%            1              1
    90%            1              1       Variance       .2395024
    95%            1              1       Skewness       .4444461
    99%            1              1       Kurtosis       1.197532
    


```stata
tabulate low smoke, chi2 column
```

    
    +-------------------+
    | Key               |
    |-------------------|
    |     frequency     |
    | column percentage |
    +-------------------+
    
               |     smoked during
    birthweigh |       pregnancy
       t<2500g | nonsmoker     smoker |     Total
    -----------+----------------------+----------
             0 |        86         44 |       130 
               |     74.78      59.46 |     68.78 
    -----------+----------------------+----------
             1 |        29         30 |        59 
               |     25.22      40.54 |     31.22 
    -----------+----------------------+----------
         Total |       115         74 |       189 
               |    100.00     100.00 |    100.00 
    
              Pearson chi2(1) =   4.9237   Pr = 0.026
    

低体重と母体の喫煙歴の関係を見ると、明らかに喫煙歴のある母体の方が低体重出生児が多い。

$\chi^2$検定でも有意に喫煙歴がある方が低体重出生児が多いという結果であった。

### グラフで可視化する


```stata
# ヒストグラム作成

hist bwt, bin(25)
```

    
    Unknown #command
    (bin=25, start=709, width=171.24)
    


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1736.76&quot; x2=&quot;3859.02&quot; y2=&quot;1736.76&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;984.21&quot; x2=&quot;3859.02&quot; y2=&quot;984.21&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;231.79&quot; x2=&quot;3859.02&quot; y2=&quot;231.79&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;519.50&quot; y=&quot;2372.98&quot; width=&quot;130.80&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;523.82&quot; y=&quot;2377.30&quot; width=&quot;122.16&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;650.31&quot; y=&quot;2372.98&quot; width=&quot;130.68&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;654.63&quot; y=&quot;2377.30&quot; width=&quot;122.04&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;780.99&quot; y=&quot;2372.98&quot; width=&quot;130.80&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;785.31&quot; y=&quot;2377.30&quot; width=&quot;122.16&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;911.79&quot; y=&quot;2372.98&quot; width=&quot;130.68&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;916.11&quot; y=&quot;2377.30&quot; width=&quot;122.04&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1042.47&quot; y=&quot;2372.98&quot; width=&quot;130.80&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1046.79&quot; y=&quot;2377.30&quot; width=&quot;122.16&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1173.27&quot; y=&quot;2024.24&quot; width=&quot;130.68&quot; height=&quot;464.94&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1177.59&quot; y=&quot;2028.56&quot; width=&quot;122.04&quot; height=&quot;456.30&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1303.95&quot; y=&quot;1907.91&quot; width=&quot;130.80&quot; height=&quot;581.27&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1308.27&quot; y=&quot;1912.23&quot; width=&quot;122.16&quot; height=&quot;572.63&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1434.76&quot; y=&quot;1675.38&quot; width=&quot;130.68&quot; height=&quot;813.81&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1439.08&quot; y=&quot;1679.70&quot; width=&quot;122.04&quot; height=&quot;805.17&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1565.44&quot; y=&quot;1094.23&quot; width=&quot;130.80&quot; height=&quot;1394.95&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1569.76&quot; y=&quot;1098.55&quot; width=&quot;122.16&quot; height=&quot;1386.31&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1696.24&quot; y=&quot;745.37&quot; width=&quot;130.68&quot; height=&quot;1743.82&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1700.56&quot; y=&quot;749.69&quot; width=&quot;122.04&quot; height=&quot;1735.18&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1826.92&quot; y=&quot;861.70&quot; width=&quot;130.80&quot; height=&quot;1627.49&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1831.24&quot; y=&quot;866.02&quot; width=&quot;122.16&quot; height=&quot;1618.85&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1957.72&quot; y=&quot;1210.44&quot; width=&quot;130.68&quot; height=&quot;1278.75&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1962.04&quot; y=&quot;1214.76&quot; width=&quot;122.04&quot; height=&quot;1270.11&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2088.41&quot; y=&quot;396.63&quot; width=&quot;130.80&quot; height=&quot;2092.55&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2092.72&quot; y=&quot;400.95&quot; width=&quot;122.16&quot; height=&quot;2083.91&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2219.21&quot; y=&quot;164.22&quot; width=&quot;130.68&quot; height=&quot;2324.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2223.53&quot; y=&quot;168.54&quot; width=&quot;122.04&quot; height=&quot;2316.32&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2349.89&quot; y=&quot;745.37&quot; width=&quot;130.80&quot; height=&quot;1743.82&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2354.21&quot; y=&quot;749.69&quot; width=&quot;122.16&quot; height=&quot;1735.18&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2480.69&quot; y=&quot;1094.23&quot; width=&quot;130.68&quot; height=&quot;1394.95&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2485.01&quot; y=&quot;1098.55&quot; width=&quot;122.04&quot; height=&quot;1386.31&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2611.37&quot; y=&quot;1094.23&quot; width=&quot;130.80&quot; height=&quot;1394.95&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2615.69&quot; y=&quot;1098.55&quot; width=&quot;122.16&quot; height=&quot;1386.31&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2742.18&quot; y=&quot;745.37&quot; width=&quot;130.68&quot; height=&quot;1743.82&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2746.50&quot; y=&quot;749.69&quot; width=&quot;122.04&quot; height=&quot;1735.18&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2872.86&quot; y=&quot;1210.44&quot; width=&quot;130.80&quot; height=&quot;1278.75&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2877.18&quot; y=&quot;1214.76&quot; width=&quot;122.16&quot; height=&quot;1270.11&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3003.66&quot; y=&quot;1675.38&quot; width=&quot;130.68&quot; height=&quot;813.81&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3007.98&quot; y=&quot;1679.70&quot; width=&quot;122.04&quot; height=&quot;805.17&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3134.34&quot; y=&quot;2024.24&quot; width=&quot;130.80&quot; height=&quot;464.94&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3138.66&quot; y=&quot;2028.56&quot; width=&quot;122.16&quot; height=&quot;456.30&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3395.82&quot; y=&quot;2372.98&quot; width=&quot;130.80&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3400.14&quot; y=&quot;2377.30&quot; width=&quot;122.16&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3657.31&quot; y=&quot;2372.98&quot; width=&quot;130.80&quot; height=&quot;116.20&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3661.63&quot; y=&quot;2377.30&quot; width=&quot;122.16&quot; height=&quot;107.57&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;350.83&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2489.19&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2489.19)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1736.76&quot; x2=&quot;350.83&quot; y2=&quot;1736.76&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1736.76&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1736.76)&quot; text-anchor=&quot;middle&quot;&gt;2.0e-04&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;984.21&quot; x2=&quot;350.83&quot; y2=&quot;984.21&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;984.21&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,984.21)&quot; text-anchor=&quot;middle&quot;&gt;4.0e-04&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;231.79&quot; x2=&quot;350.83&quot; y2=&quot;231.79&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;231.79&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,231.79)&quot; text-anchor=&quot;middle&quot;&gt;6.0e-04&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Density&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;741.63&quot; y1=&quot;2489.19&quot; x2=&quot;741.63&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;741.63&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;1000&lt;/text&gt;
	&lt;line x1=&quot;1505.17&quot; y1=&quot;2489.19&quot; x2=&quot;1505.17&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1505.17&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;2000&lt;/text&gt;
	&lt;line x1=&quot;2268.71&quot; y1=&quot;2489.19&quot; x2=&quot;2268.71&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2268.71&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;3000&lt;/text&gt;
	&lt;line x1=&quot;3032.25&quot; y1=&quot;2489.19&quot; x2=&quot;3032.25&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3032.25&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;4000&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2489.19&quot; x2=&quot;3795.66&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;5000&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;birthweight (grams)&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



    
    


```stata
# 散布図を作成

scatter bwt age
```

    
    Unknown #command
    


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2272.49&quot; x2=&quot;3859.02&quot; y2=&quot;2272.49&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1745.42&quot; x2=&quot;3859.02&quot; y2=&quot;1745.42&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1218.36&quot; x2=&quot;3859.02&quot; y2=&quot;1218.36&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;691.29&quot; x2=&quot;3859.02&quot; y2=&quot;691.29&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;3859.02&quot; y2=&quot;164.22&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1469.82&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1469.82&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2375.51&quot; cy=&quot;1454.97&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2375.51&quot; cy=&quot;1454.97&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1451.88&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1451.88&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1432.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1432.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1429.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1429.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1417.60&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1417.60&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1409.68&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1409.68&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1409.68&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1409.68&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;1395.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;1395.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1394.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1394.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1364.88&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1364.88&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1359.06&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1359.06&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1350.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1350.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1350.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1350.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1340.13&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1340.13&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1340.13&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1340.13&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;871.82&quot; cy=&quot;1335.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;871.82&quot; cy=&quot;1335.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1333.32&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1333.32&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1320.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1320.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1312.78&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1312.78&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;1305.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;1305.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1305.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1305.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2626.10&quot; cy=&quot;1304.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2626.10&quot; cy=&quot;1304.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1290.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1290.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1283.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1283.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1283.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1283.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1267.98&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1267.98&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;1260.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;1260.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1260.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1260.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1260.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1260.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1260.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1260.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1245.83&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1245.83&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2542.57&quot; cy=&quot;1245.83&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2542.57&quot; cy=&quot;1245.83&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1230.48&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1230.48&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1230.48&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1230.48&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;1230.48&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;1230.48&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1230.48&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1230.48&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1874.19&quot; cy=&quot;1259.44&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1874.19&quot; cy=&quot;1259.44&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1215.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1215.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2375.51&quot; cy=&quot;1201.03&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2375.51&quot; cy=&quot;1201.03&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1196.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1196.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1185.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1185.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1185.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1185.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1185.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1185.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1178.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1178.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1178.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1178.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;1176.16&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;1176.16&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1170.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1170.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1170.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1170.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1170.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1170.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1165.64&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1165.64&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1163.53&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1163.53&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1148.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1148.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1140.89&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1140.89&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1126.16&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1126.16&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1126.16&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1126.16&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1111.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1111.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1111.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1111.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1111.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1111.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1099.80&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1099.80&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1099.80&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1099.80&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1096.09&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1096.09&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1096.09&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1096.09&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1095.10&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1095.10&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1081.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1081.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1073.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1073.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1073.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1073.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1058.71&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1058.71&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1051.29&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1051.29&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1051.29&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1051.29&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1051.29&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1051.29&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1049.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1049.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1043.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1043.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1021.22&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1021.22&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1021.22&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1021.22&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1006.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1006.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;999.06&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;999.06&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;991.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;991.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;984.34&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;984.34&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;976.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;976.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;975.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;975.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;969.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;969.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;968.00&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;968.00&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;961.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;961.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;931.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;931.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;916.89&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;916.89&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;916.89&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;916.89&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;909.47&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;909.47&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2626.10&quot; cy=&quot;902.17&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2626.10&quot; cy=&quot;902.17&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;894.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;894.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;894.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;894.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;886.82&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;886.82&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;886.82&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;886.82&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;882.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;882.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;879.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;879.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;875.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;875.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;875.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;875.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;875.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;875.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;875.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;875.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;849.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;849.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;834.72&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;834.72&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;819.87&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;819.87&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;812.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;812.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;812.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;812.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;812.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;812.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;801.93&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;801.93&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;797.22&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;797.22&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;782.50&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;782.50&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;767.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;767.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;765.05&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;765.05&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;765.05&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;765.05&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;752.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;752.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;752.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;752.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2375.51&quot; cy=&quot;737.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2375.51&quot; cy=&quot;737.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;722.97&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;722.97&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;722.35&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;722.35&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;788.29&quot; cy=&quot;722.35&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;788.29&quot; cy=&quot;722.35&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;707.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;707.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;700.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;700.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;692.90&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;692.90&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;692.90&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;692.90&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;662.83&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;662.83&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;662.83&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;662.83&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;632.75&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;632.75&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;610.60&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;610.60&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;603.30&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;603.30&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2542.57&quot; cy=&quot;599.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2542.57&quot; cy=&quot;599.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;565.80&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;565.80&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;378.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;378.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3378.00&quot; cy=&quot;169.54&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3378.00&quot; cy=&quot;169.54&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;2425.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;2425.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;2261.48&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.38&quot; cy=&quot;2261.48&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2459.04&quot; cy=&quot;2201.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2459.04&quot; cy=&quot;2201.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;2098.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;2098.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;2022.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;2022.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1874.19&quot; cy=&quot;1962.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1874.19&quot; cy=&quot;1962.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1962.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1962.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1903.09&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1903.09&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1888.24&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1888.24&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1856.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1856.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;1841.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2291.97&quot; cy=&quot;1841.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1806.06&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1806.06&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1801.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1801.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1798.64&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;955.35&quot; cy=&quot;1798.64&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1783.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1783.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1783.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1783.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1783.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1783.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1779.21&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1779.21&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1761.27&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1761.27&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1716.47&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1716.47&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1716.47&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1716.47&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1702.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1702.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1701.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1205.94&quot; cy=&quot;1701.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1701.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1701.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1692.71&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1692.71&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1679.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1679.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1679.09&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1679.09&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1646.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1646.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1874.19&quot; cy=&quot;1646.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1874.19&quot; cy=&quot;1646.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1634.29&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1634.29&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1626.87&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1626.87&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1618.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1707.13&quot; cy=&quot;1618.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1618.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1618.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1596.80&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1596.80&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1589.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1122.41&quot; cy=&quot;1589.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1589.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1589.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1586.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1586.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1574.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1574.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1559.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2208.44&quot; cy=&quot;1559.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;871.82&quot; cy=&quot;1559.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;871.82&quot; cy=&quot;1559.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1552.00&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1552.00&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1544.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1544.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1544.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.60&quot; cy=&quot;1544.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;871.82&quot; cy=&quot;1544.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;871.82&quot; cy=&quot;1544.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1537.27&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1537.27&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1529.35&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1529.35&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1529.35&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1456.54&quot; cy=&quot;1529.35&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1527.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1527.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1521.93&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1521.93&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1514.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1514.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1512.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1512.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1508.31&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1289.47&quot; cy=&quot;1508.31&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1499.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.66&quot; cy=&quot;1499.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;788.29&quot; cy=&quot;1499.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;788.29&quot; cy=&quot;1499.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1499.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1957.85&quot; cy=&quot;1499.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;788.29&quot; cy=&quot;1484.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;788.29&quot; cy=&quot;1484.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1484.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.07&quot; cy=&quot;1484.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1484.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1038.88&quot; cy=&quot;1484.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1484.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1373.01&quot; cy=&quot;1484.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2272.49&quot; x2=&quot;350.83&quot; y2=&quot;2272.49&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2272.49&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2272.49)&quot; text-anchor=&quot;middle&quot;&gt;1000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1745.42&quot; x2=&quot;350.83&quot; y2=&quot;1745.42&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1745.42&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1745.42)&quot; text-anchor=&quot;middle&quot;&gt;2000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1218.36&quot; x2=&quot;350.83&quot; y2=&quot;1218.36&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1218.36&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1218.36)&quot; text-anchor=&quot;middle&quot;&gt;3000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;691.29&quot; x2=&quot;350.83&quot; y2=&quot;691.29&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;691.29&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,691.29)&quot; text-anchor=&quot;middle&quot;&gt;4000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;350.83&quot; y2=&quot;164.22&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;164.22&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,164.22)&quot; text-anchor=&quot;middle&quot;&gt;5000&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;birthweight (grams)&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2489.19&quot; x2=&quot;454.16&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;10&lt;/text&gt;
	&lt;line x1=&quot;1289.47&quot; y1=&quot;2489.19&quot; x2=&quot;1289.47&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1289.47&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;20&lt;/text&gt;
	&lt;line x1=&quot;2124.91&quot; y1=&quot;2489.19&quot; x2=&quot;2124.91&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;30&lt;/text&gt;
	&lt;line x1=&quot;2960.35&quot; y1=&quot;2489.19&quot; x2=&quot;2960.35&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2960.35&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;40&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2489.19&quot; x2=&quot;3795.66&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;50&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;age of mother&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



    
    

## 線形回帰モデルの作成

出生時の体重と母体の年齢が線形であると仮定すると、

$Y_{i(bwt)} = \beta_0 + \beta*x_{i(age)} + \epsilon$

と表すことができる。

$\epsilon$は残差である。


```stata
regress bwt age
```

    
          Source |       SS           df       MS      Number of obs   =       189
    -------------+----------------------------------   F(1, 187)       =      1.51
           Model |  800428.169         1  800428.169   Prob > F        =    0.2207
        Residual |  99114870.4       187  530026.045   R-squared       =    0.0080
    -------------+----------------------------------   Adj R-squared   =    0.0027
           Total |  99915298.6       188  531464.354   Root MSE        =    728.03
    
    ------------------------------------------------------------------------------
             bwt |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             age |   12.31444   10.02079     1.23   0.221     -7.45389    32.08277
           _cons |   2658.122   238.8097    11.13   0.000     2187.014    3129.229
    ------------------------------------------------------------------------------
    

回帰式のモデルの結果は、

$Y_{i(bwt)} = 12.31*x_{i(age)} + 2658.122$

である。

つまり母体の年齢が1歳増えるごとに12.31gずつ体重が増えることになる。

しかし、そもそも母体の年齢と体重には線形の関係がはっきりはなさそうである。

今回のモデルの決定係数も0.0027と非常に低い。

むしろ他のモデルを考えた方が良さそうである。

## 重回帰モデル

先のモデルでは、年齢と低体重出生児との関係はあまりなさそうであった。

むしろモデルには喫煙歴などを含めた方がよいモデルができる可能性が高い。

今度は母体の喫煙歴、年齢、人種で調整したモデルの作成を行う。

$Y_{i(bwt)} = \beta_0 + \beta_1*x_{i(smoke)} + \beta_2*x_{i(age)} + \beta_3*x_{i(race)} + \epsilon$


```stata
regress bwt smoke age i.race
```

    
          Source |       SS           df       MS      Number of obs   =       189
    -------------+----------------------------------   F(4, 184)       =      6.50
           Model |  12366825.4         4  3091706.34   Prob > F        =    0.0001
        Residual |  87548473.2       184   475806.92   R-squared       =    0.1238
    -------------+----------------------------------   Adj R-squared   =    0.1047
           Total |  99915298.6       188  531464.354   Root MSE        =    689.79
    
    ------------------------------------------------------------------------------
             bwt |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           smoke |  -425.5563   109.9505    -3.87   0.000    -642.4822   -208.6304
             age |   1.998899   9.767361     0.20   0.838    -17.27152    21.26932
                 |
            race |
          black  |  -444.6489   156.1404    -2.85   0.005    -752.7047   -136.5931
          other  |   -449.481   118.9765    -3.78   0.000    -684.2147   -214.7474
                 |
           _cons |   3284.964   260.5749    12.61   0.000     2770.865    3799.062
    ------------------------------------------------------------------------------
    

今回のモデルではProb > Fを見ると、このモデルは有意であり、調整決定係数も最初のモデルより改善している。

それでも係数は低く、あまりこのモデルでは説明できていないことになる。


```stata
estat esize
```

    
    Effect sizes for linear models
    
    -----------------------------------------------------------------
                 Source | Eta-Squared     df     [95% Conf. Interval]
    --------------------+--------------------------------------------
                  Model |   .1237731       4     .0358881    .1997528
                        |
                  smoke |   .0752852       1     .0183756    .1560631
                    age |   .0002276       1            .    .0199652
                   race |   .0847912       2     .0197304    .1627342
    -----------------------------------------------------------------
    Note: Eta-Squared values for individual model terms are partial.
    

このコマンドをすることでこのモデル式の全体での効果サイズが分かる。

全体では0.124であり、修正前の決定係数と同じ値である。

つまり修正前の決定係数によれば、bwtの変化の約12.4%がこのモデルによって説明されることを示している。

下の数値は各変数の効果サイズで、smokeは他のすべての項で説明されている変化を除いて約7.7%bwtの変化を説明できる
ことを意味する。

## モデルの評価

線形回帰モデルでは残差の正規性と等分散性が前提となっている。

そのため、それを確認する必要がある。


```stata
# 推定値を求める

regress bwt smoke age i.race
predict re, residuals
```

    
    Unknown #command
    
          Source |       SS           df       MS      Number of obs   =       189
    -------------+----------------------------------   F(4, 184)       =      6.50
           Model |  12366825.4         4  3091706.34   Prob > F        =    0.0001
        Residual |  87548473.2       184   475806.92   R-squared       =    0.1238
    -------------+----------------------------------   Adj R-squared   =    0.1047
           Total |  99915298.6       188  531464.354   Root MSE        =    689.79
    
    ------------------------------------------------------------------------------
             bwt |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           smoke |  -425.5563   109.9505    -3.87   0.000    -642.4822   -208.6304
             age |   1.998899   9.767361     0.20   0.838    -17.27152    21.26932
                 |
            race |
          black  |  -444.6489   156.1404    -2.85   0.005    -752.7047   -136.5931
          other  |   -449.481   118.9765    -3.78   0.000    -684.2147   -214.7474
                 |
           _cons |   3284.964   260.5749    12.61   0.000     2770.865    3799.062
    ------------------------------------------------------------------------------
    
    


```stata
### 残差の正規性の確認

hist re, normal
```

    
    Unknown #command
    (bin=13, start=-2321.9316, width=302.84752)
    


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1714.24&quot; x2=&quot;3859.02&quot; y2=&quot;1714.24&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;939.17&quot; x2=&quot;3859.02&quot; y2=&quot;939.17&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;3859.02&quot; y2=&quot;164.22&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;567.27&quot; y=&quot;2421.49&quot; width=&quot;226.22&quot; height=&quot;67.69&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;571.59&quot; y=&quot;2425.81&quot; width=&quot;217.58&quot; height=&quot;59.05&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;793.49&quot; y=&quot;2421.49&quot; width=&quot;226.22&quot; height=&quot;67.69&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;797.80&quot; y=&quot;2425.81&quot; width=&quot;217.58&quot; height=&quot;59.05&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1019.70&quot; y=&quot;2421.49&quot; width=&quot;226.21&quot; height=&quot;67.69&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1024.02&quot; y=&quot;2425.81&quot; width=&quot;217.58&quot; height=&quot;59.05&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1245.91&quot; y=&quot;1879.94&quot; width=&quot;226.22&quot; height=&quot;609.24&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1250.23&quot; y=&quot;1884.26&quot; width=&quot;217.58&quot; height=&quot;600.60&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1472.13&quot; y=&quot;1541.35&quot; width=&quot;226.21&quot; height=&quot;947.83&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1476.45&quot; y=&quot;1545.67&quot; width=&quot;217.58&quot; height=&quot;939.19&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1698.35&quot; y=&quot;1541.35&quot; width=&quot;226.21&quot; height=&quot;947.83&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1702.66&quot; y=&quot;1545.67&quot; width=&quot;217.58&quot; height=&quot;939.19&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1924.56&quot; y=&quot;255.06&quot; width=&quot;226.22&quot; height=&quot;2234.13&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1928.88&quot; y=&quot;259.38&quot; width=&quot;217.58&quot; height=&quot;2225.49&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2150.78&quot; y=&quot;390.44&quot; width=&quot;226.34&quot; height=&quot;2098.74&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2155.09&quot; y=&quot;394.76&quot; width=&quot;217.70&quot; height=&quot;2090.10&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2377.11&quot; y=&quot;525.83&quot; width=&quot;226.22&quot; height=&quot;1963.36&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2381.43&quot; y=&quot;530.15&quot; width=&quot;217.58&quot; height=&quot;1954.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2603.33&quot; y=&quot;999.81&quot; width=&quot;226.21&quot; height=&quot;1489.38&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2607.65&quot; y=&quot;1004.13&quot; width=&quot;217.58&quot; height=&quot;1480.74&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2829.54&quot; y=&quot;864.42&quot; width=&quot;226.22&quot; height=&quot;1624.76&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2833.86&quot; y=&quot;868.74&quot; width=&quot;217.58&quot; height=&quot;1616.12&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3055.76&quot; y=&quot;1947.64&quot; width=&quot;226.22&quot; height=&quot;541.55&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3060.08&quot; y=&quot;1951.96&quot; width=&quot;217.58&quot; height=&quot;532.91&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3281.97&quot; y=&quot;2353.80&quot; width=&quot;226.21&quot; height=&quot;135.39&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3286.29&quot; y=&quot;2358.12&quot; width=&quot;217.58&quot; height=&quot;126.75&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;path d=&quot; M567.15 2482.38 L577.05 2481.88 L586.82 2481.39 L596.72 2480.89 L606.50 2480.28 L616.40 2479.66 L626.17 2479.04 L636.08 2478.42 L645.85 2477.68 L655.75 2476.93 L665.53 2476.19 L675.43 2475.32 L685.20 2474.46 L695.10 2473.47 L704.88 2472.48 L714.78 2471.49 L724.56 2470.37 L734.46 2469.26 L744.23 2468.02 L754.13 2466.66 L763.91 2465.30 L773.68 2463.94 L783.59 2462.45 L793.36 2460.85 L803.26 2459.11 L813.04 2457.38 L822.94 2455.52 L832.71 2453.67 L842.61 2451.56 L852.39 2449.46 L862.29 2447.23 L872.07 2444.88 L881.97 2442.41 L891.74 2439.81 L901.64 2437.08 L911.42 2434.36 L921.32 2431.39 L931.10 2428.30 L941.00 2424.96 L950.77 2421.62 L960.67 2418.15 L970.45 2414.44 L980.35 2410.60 L990.12 2406.52 L1000.02 2402.31 L1009.80 2397.98 L1019.70 2393.40 L1029.48 2388.70 L1039.25 2383.75 L1049.15 2378.55 L1058.93 2373.23 L1068.83 2367.66 L1078.61 2361.84 L1088.51 2355.90 L1098.28 2349.59 L1108.18 2343.16 L1117.96 2336.35 L1127.86 2329.42 L1137.63 2322.24 L1147.53 2314.69 L1157.31 2306.90 L1167.21 2298.85 L1176.99 2290.56 L1186.89 2282.02 L1196.66 2273.11 L1206.56 2263.83 L1216.34 2254.42 L1226.24 2244.65 L1236.02 2234.50 L1245.91 2223.98 L1255.69 2213.21 L1265.59 2202.20 L1275.37 2190.69 L1285.27 2178.93 L1295.04 2166.93 L1304.94 2154.43 L1314.72 2141.56 L1324.50 2128.44 L1334.40 2114.95 L1344.17 2100.97 L1354.07 2086.74 L1363.85 2072.13 L1373.75 2057.16 L1383.53 2041.82 L1393.42 2026.10 L1403.20 2009.89 L1413.10 1993.43 L1422.88 1976.60 L1432.78 1959.39 L1442.55 1941.70 L1452.45 1923.75 L1462.23 1905.31 L1472.13 1886.63 L1481.91 1867.57 L1491.81 1848.02 L1501.58 1828.22 L1511.48 1808.04 L1521.26 1787.50 L1531.16 1766.59 L1540.93 1745.30 L1550.84 1723.64 L1560.61 1701.74 L1570.51 1679.59 L1580.29 1656.94 L1590.19 1634.05 L1599.96 1610.90 L1609.74 1587.39 L1619.64 1563.63 L1629.42 1539.62 L1639.32 1515.37 L1649.09 1490.86 L1658.99 1465.99 L1668.77 1440.99 L1678.67 1415.87 L1688.44 1390.37 L1698.35 1364.76 L1708.12 1339.02 L1718.02 1313.15 L1727.80 1287.04 L1737.70 1260.93 L1747.47 1234.69 L1757.37 1208.33 L1767.15 1181.85 L1777.05 1155.37 L1786.83 1128.88 L1796.73 1102.40 L1806.50 1075.92 L1816.40 1049.43 L1826.18 1022.95 L1836.08 996.59 L1845.86 970.35 L1855.76 944.24 L1865.53 918.25 L1875.31 892.39 L1885.21 866.65 L1894.98 841.16 L1904.88 815.91 L1914.66 790.91 L1924.56 766.16 L1934.34 741.78 L1944.24 717.65 L1954.01 693.89 L1963.91 670.37 L1973.69 647.36 L1983.59 624.71 L1993.37 602.43 L2003.27 580.65 L2013.04 559.37 L2022.94 538.45 L2032.72 518.16 L2042.62 498.36 L2052.39 479.17 L2062.29 460.49 L2072.07 442.30 L2081.97 424.85 L2091.75 408.02 L2101.65 391.80 L2111.42 376.21 L2121.32 361.36 L2131.10 347.25 L2141.00 333.76 L2150.78 321.02 L2160.55 309.01 L2170.45 297.75 L2180.23 287.23 L2190.13 277.46 L2199.90 268.55 L2209.80 260.38 L2219.58 253.08 L2229.48 246.52 L2239.26 240.70 L2249.16 235.87 L2258.93 231.79 L2268.83 228.57 L2278.61 226.22 L2288.51 224.61 L2298.28 223.87 L2308.18 223.99 L2317.96 224.98 L2327.86 226.84 L2337.64 229.56 L2347.54 233.03 L2357.31 237.36 L2367.21 242.43 L2376.99 248.50 L2386.89 255.30 L2396.67 262.85 L2406.57 271.27 L2416.34 280.43 L2426.12 290.45 L2436.02 301.22 L2445.80 312.73 L2455.70 324.98 L2465.47 337.97 L2475.37 351.58 L2485.15 366.06 L2495.05 381.16 L2504.82 396.88 L2514.72 413.34 L2524.50 430.42 L2534.40 447.99 L2544.18 466.30 L2554.08 485.12 L2563.85 504.54 L2573.75 524.59 L2583.53 545.01 L2593.43 566.05 L2603.20 587.58 L2613.11 609.49 L2622.88 631.89 L2632.78 654.66 L2642.56 677.80 L2652.46 701.31 L2662.23 725.32 L2672.13 749.45 L2681.91 774.08 L2691.81 798.83 L2701.59 823.95 L2711.36 849.32 L2721.26 874.82 L2731.04 900.56 L2740.94 926.42 L2750.72 952.53 L2760.61 978.65 L2770.39 1005.01 L2780.29 1031.36 L2790.07 1057.85 L2799.97 1084.33 L2809.74 1110.81 L2819.64 1137.30 L2829.42 1163.78 L2839.32 1190.26 L2849.10 1216.62 L2859.00 1242.98 L2868.77 1269.22 L2878.67 1295.33 L2888.45 1321.44 L2898.35 1347.31 L2908.13 1372.93 L2918.03 1398.54 L2927.80 1423.79 L2937.70 1449.03 L2947.48 1473.91 L2957.38 1498.66 L2967.15 1523.04 L2976.93 1547.29 L2986.83 1571.30 L2996.61 1594.94 L3006.51 1618.33 L3016.28 1641.35 L3026.18 1664.12 L3035.96 1686.64 L3045.86 1708.79 L3055.64 1730.57 L3065.53 1752.11 L3075.31 1773.27 L3085.21 1794.06 L3094.99 1814.48 L3104.89 1834.53 L3114.66 1854.33 L3124.56 1873.63 L3134.34 1892.57 L3144.24 1911.25 L3154.02 1929.45 L3163.92 1947.39 L3173.69 1964.84 L3183.59 1982.04 L3193.37 1998.75 L3203.27 2015.08 L3213.05 2031.05 L3222.95 2046.77 L3232.72 2061.99 L3242.62 2076.84 L3252.40 2091.32 L3262.17 2105.42 L3272.07 2119.28 L3281.85 2132.65 L3291.75 2145.77 L3301.53 2158.39 L3311.43 2170.77 L3321.20 2182.77 L3331.10 2194.40 L3340.88 2205.79 L3350.78 2216.68 L3360.55 2227.45 L3370.45 2237.72 L3380.23 2247.74 L3390.13 2257.39 L3399.91 2266.80 L3409.81 2275.96 L3419.58 2284.74 L3429.48 2293.28 L3439.26 2301.45 L3449.16 2309.37 L3458.94 2317.04 L3468.84 2324.47 L3478.61 2331.65 L3488.51 2338.58 L3498.29 2345.26 L3508.19 2351.57&quot; stroke-linejoin=&quot;round&quot; style=&quot;fill:none;stroke:#2D6D66;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;350.83&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2489.19&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2489.19)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1714.24&quot; x2=&quot;350.83&quot; y2=&quot;1714.24&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1714.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1714.24)&quot; text-anchor=&quot;middle&quot;&gt;2.0e-04&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;939.17&quot; x2=&quot;350.83&quot; y2=&quot;939.17&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;939.17&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,939.17)&quot; text-anchor=&quot;middle&quot;&gt;4.0e-04&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;350.83&quot; y2=&quot;164.22&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;164.22&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,164.22)&quot; text-anchor=&quot;middle&quot;&gt;6.0e-04&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Density&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;807.72&quot; y1=&quot;2489.19&quot; x2=&quot;807.72&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;807.72&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-2000&lt;/text&gt;
	&lt;line x1=&quot;1554.67&quot; y1=&quot;2489.19&quot; x2=&quot;1554.67&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1554.67&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-1000&lt;/text&gt;
	&lt;line x1=&quot;2301.75&quot; y1=&quot;2489.19&quot; x2=&quot;2301.75&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2301.75&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;3048.70&quot; y1=&quot;2489.19&quot; x2=&quot;3048.70&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3048.70&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;1000&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2489.19&quot; x2=&quot;3795.66&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;2000&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Residuals&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



    
    


```stata
## QQ plot

qnorm re
```

    
    Unknown #command
    


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;circle cx=&quot;666.52&quot; cy=&quot;2425.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;666.52&quot; cy=&quot;2425.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;809.70&quot; cy=&quot;2130.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;809.70&quot; cy=&quot;2130.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;899.41&quot; cy=&quot;2024.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;899.41&quot; cy=&quot;2024.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;966.24&quot; cy=&quot;1949.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;966.24&quot; cy=&quot;1949.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1020.07&quot; cy=&quot;1916.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1020.07&quot; cy=&quot;1916.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1065.61&quot; cy=&quot;1891.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1065.61&quot; cy=&quot;1891.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1105.21&quot; cy=&quot;1887.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1105.21&quot; cy=&quot;1887.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1140.48&quot; cy=&quot;1860.27&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1140.48&quot; cy=&quot;1860.27&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1172.28&quot; cy=&quot;1856.06&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1172.28&quot; cy=&quot;1856.06&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1201.37&quot; cy=&quot;1832.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1201.37&quot; cy=&quot;1832.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1228.34&quot; cy=&quot;1814.97&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1228.34&quot; cy=&quot;1814.97&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1253.34&quot; cy=&quot;1792.45&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1253.34&quot; cy=&quot;1792.45&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1276.73&quot; cy=&quot;1789.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1276.73&quot; cy=&quot;1789.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1298.88&quot; cy=&quot;1740.60&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1298.88&quot; cy=&quot;1740.60&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1319.79&quot; cy=&quot;1730.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1319.79&quot; cy=&quot;1730.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1339.72&quot; cy=&quot;1724.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1339.72&quot; cy=&quot;1724.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1358.65&quot; cy=&quot;1719.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1358.65&quot; cy=&quot;1719.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1376.84&quot; cy=&quot;1719.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1376.84&quot; cy=&quot;1719.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1394.29&quot; cy=&quot;1717.58&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1394.29&quot; cy=&quot;1717.58&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1411.00&quot; cy=&quot;1714.24&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1411.00&quot; cy=&quot;1714.24&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1427.21&quot; cy=&quot;1710.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1427.21&quot; cy=&quot;1710.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1442.92&quot; cy=&quot;1685.65&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1442.92&quot; cy=&quot;1685.65&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1458.02&quot; cy=&quot;1645.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1458.02&quot; cy=&quot;1645.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1472.75&quot; cy=&quot;1643.82&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1472.75&quot; cy=&quot;1643.82&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1486.98&quot; cy=&quot;1640.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1486.98&quot; cy=&quot;1640.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1500.84&quot; cy=&quot;1636.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1500.84&quot; cy=&quot;1636.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1514.33&quot; cy=&quot;1585.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1514.33&quot; cy=&quot;1585.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1527.57&quot; cy=&quot;1580.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1527.57&quot; cy=&quot;1580.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1540.44&quot; cy=&quot;1572.91&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1540.44&quot; cy=&quot;1572.91&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1552.94&quot; cy=&quot;1560.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1552.94&quot; cy=&quot;1560.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1565.31&quot; cy=&quot;1548.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1565.31&quot; cy=&quot;1548.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1577.32&quot; cy=&quot;1543.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1577.32&quot; cy=&quot;1543.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1589.20&quot; cy=&quot;1526.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1589.20&quot; cy=&quot;1526.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1600.71&quot; cy=&quot;1519.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1600.71&quot; cy=&quot;1519.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1612.09&quot; cy=&quot;1512.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1612.09&quot; cy=&quot;1512.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1623.23&quot; cy=&quot;1508.31&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1623.23&quot; cy=&quot;1508.31&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1634.24&quot; cy=&quot;1505.22&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1634.24&quot; cy=&quot;1505.22&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1645.13&quot; cy=&quot;1483.44&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1645.13&quot; cy=&quot;1483.44&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1655.65&quot; cy=&quot;1478.73&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1655.65&quot; cy=&quot;1478.73&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1666.17&quot; cy=&quot;1477.37&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1666.17&quot; cy=&quot;1477.37&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1676.44&quot; cy=&quot;1469.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1676.44&quot; cy=&quot;1469.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1686.71&quot; cy=&quot;1465.37&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1686.71&quot; cy=&quot;1465.37&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1696.74&quot; cy=&quot;1464.38&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1696.74&quot; cy=&quot;1464.38&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1706.64&quot; cy=&quot;1462.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1706.64&quot; cy=&quot;1462.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1716.41&quot; cy=&quot;1461.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1716.41&quot; cy=&quot;1461.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1726.07&quot; cy=&quot;1453.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1726.07&quot; cy=&quot;1453.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1735.59&quot; cy=&quot;1445.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1735.59&quot; cy=&quot;1445.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1745.12&quot; cy=&quot;1443.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1745.12&quot; cy=&quot;1443.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1754.40&quot; cy=&quot;1443.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1754.40&quot; cy=&quot;1443.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1763.68&quot; cy=&quot;1439.13&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1763.68&quot; cy=&quot;1439.13&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1772.84&quot; cy=&quot;1433.44&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1772.84&quot; cy=&quot;1433.44&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1781.88&quot; cy=&quot;1432.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1781.88&quot; cy=&quot;1432.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1790.79&quot; cy=&quot;1423.54&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1790.79&quot; cy=&quot;1423.54&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1799.70&quot; cy=&quot;1409.31&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1799.70&quot; cy=&quot;1409.31&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1808.48&quot; cy=&quot;1403.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1808.48&quot; cy=&quot;1403.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1817.27&quot; cy=&quot;1398.17&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1817.27&quot; cy=&quot;1398.17&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1825.93&quot; cy=&quot;1396.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1825.93&quot; cy=&quot;1396.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1834.47&quot; cy=&quot;1394.21&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1834.47&quot; cy=&quot;1394.21&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1843.01&quot; cy=&quot;1390.00&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1843.01&quot; cy=&quot;1390.00&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1851.55&quot; cy=&quot;1382.83&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1851.55&quot; cy=&quot;1382.83&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1859.96&quot; cy=&quot;1377.75&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1859.96&quot; cy=&quot;1377.75&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1868.25&quot; cy=&quot;1371.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1868.25&quot; cy=&quot;1371.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1876.55&quot; cy=&quot;1365.38&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1876.55&quot; cy=&quot;1365.38&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1884.84&quot; cy=&quot;1351.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1884.84&quot; cy=&quot;1351.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1893.00&quot; cy=&quot;1349.54&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1893.00&quot; cy=&quot;1349.54&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1901.17&quot; cy=&quot;1347.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1901.17&quot; cy=&quot;1347.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1909.21&quot; cy=&quot;1344.46&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1909.21&quot; cy=&quot;1344.46&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1917.26&quot; cy=&quot;1343.97&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1917.26&quot; cy=&quot;1343.97&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1925.30&quot; cy=&quot;1339.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1925.30&quot; cy=&quot;1339.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1933.35&quot; cy=&quot;1337.90&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1933.35&quot; cy=&quot;1337.90&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1941.27&quot; cy=&quot;1335.80&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1941.27&quot; cy=&quot;1335.80&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1949.19&quot; cy=&quot;1332.46&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1949.19&quot; cy=&quot;1332.46&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1956.98&quot; cy=&quot;1330.60&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1956.98&quot; cy=&quot;1330.60&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1964.90&quot; cy=&quot;1291.87&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1964.90&quot; cy=&quot;1291.87&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1972.70&quot; cy=&quot;1290.13&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1972.70&quot; cy=&quot;1290.13&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1980.49&quot; cy=&quot;1286.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1980.49&quot; cy=&quot;1286.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1988.17&quot; cy=&quot;1284.32&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1988.17&quot; cy=&quot;1284.32&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;1995.96&quot; cy=&quot;1278.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;1995.96&quot; cy=&quot;1278.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2003.64&quot; cy=&quot;1277.51&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2003.64&quot; cy=&quot;1277.51&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2011.31&quot; cy=&quot;1277.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2011.31&quot; cy=&quot;1277.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2018.98&quot; cy=&quot;1277.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2018.98&quot; cy=&quot;1277.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2026.65&quot; cy=&quot;1266.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2026.65&quot; cy=&quot;1266.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2034.20&quot; cy=&quot;1260.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2034.20&quot; cy=&quot;1260.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2041.88&quot; cy=&quot;1259.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2041.88&quot; cy=&quot;1259.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2049.42&quot; cy=&quot;1259.07&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2049.42&quot; cy=&quot;1259.07&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2056.97&quot; cy=&quot;1253.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2056.97&quot; cy=&quot;1253.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2064.64&quot; cy=&quot;1249.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2064.64&quot; cy=&quot;1249.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2072.19&quot; cy=&quot;1246.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2072.19&quot; cy=&quot;1246.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2079.74&quot; cy=&quot;1244.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2079.74&quot; cy=&quot;1244.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2087.29&quot; cy=&quot;1225.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2087.29&quot; cy=&quot;1225.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2094.72&quot; cy=&quot;1215.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2094.72&quot; cy=&quot;1215.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2102.26&quot; cy=&quot;1215.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2102.26&quot; cy=&quot;1215.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.81&quot; cy=&quot;1206.35&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.81&quot; cy=&quot;1206.35&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2117.36&quot; cy=&quot;1204.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2117.36&quot; cy=&quot;1204.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1196.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2124.91&quot; cy=&quot;1196.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2132.34&quot; cy=&quot;1190.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2132.34&quot; cy=&quot;1190.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2139.89&quot; cy=&quot;1189.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2139.89&quot; cy=&quot;1189.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2147.43&quot; cy=&quot;1186.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2147.43&quot; cy=&quot;1186.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2154.98&quot; cy=&quot;1186.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2154.98&quot; cy=&quot;1186.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2162.53&quot; cy=&quot;1179.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2162.53&quot; cy=&quot;1179.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2170.08&quot; cy=&quot;1169.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2170.08&quot; cy=&quot;1169.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2177.63&quot; cy=&quot;1168.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2177.63&quot; cy=&quot;1168.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2185.18&quot; cy=&quot;1167.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2185.18&quot; cy=&quot;1167.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2192.73&quot; cy=&quot;1165.51&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2192.73&quot; cy=&quot;1165.51&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2200.28&quot; cy=&quot;1154.50&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2200.28&quot; cy=&quot;1154.50&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2207.95&quot; cy=&quot;1145.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2207.95&quot; cy=&quot;1145.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2215.50&quot; cy=&quot;1143.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2215.50&quot; cy=&quot;1143.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2223.17&quot; cy=&quot;1142.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2223.17&quot; cy=&quot;1142.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2230.72&quot; cy=&quot;1118.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2230.72&quot; cy=&quot;1118.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2238.39&quot; cy=&quot;1116.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2238.39&quot; cy=&quot;1116.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2246.06&quot; cy=&quot;1116.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2246.06&quot; cy=&quot;1116.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2253.86&quot; cy=&quot;1097.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2253.86&quot; cy=&quot;1097.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2261.53&quot; cy=&quot;1094.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2261.53&quot; cy=&quot;1094.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2269.33&quot; cy=&quot;1091.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2269.33&quot; cy=&quot;1091.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2277.12&quot; cy=&quot;1091.14&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2277.12&quot; cy=&quot;1091.14&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2284.92&quot; cy=&quot;1083.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2284.92&quot; cy=&quot;1083.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2292.72&quot; cy=&quot;1077.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2292.72&quot; cy=&quot;1077.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2300.64&quot; cy=&quot;1063.79&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2300.64&quot; cy=&quot;1063.79&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2308.56&quot; cy=&quot;1061.68&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2308.56&quot; cy=&quot;1061.68&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2316.48&quot; cy=&quot;1053.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2316.48&quot; cy=&quot;1053.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2324.40&quot; cy=&quot;1049.93&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2324.40&quot; cy=&quot;1049.93&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2332.44&quot; cy=&quot;1049.93&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2332.44&quot; cy=&quot;1049.93&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2340.48&quot; cy=&quot;1049.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2340.48&quot; cy=&quot;1049.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2348.53&quot; cy=&quot;1049.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2348.53&quot; cy=&quot;1049.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2356.70&quot; cy=&quot;1039.41&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2356.70&quot; cy=&quot;1039.41&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2364.99&quot; cy=&quot;1028.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2364.99&quot; cy=&quot;1028.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2373.15&quot; cy=&quot;1027.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2373.15&quot; cy=&quot;1027.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2381.45&quot; cy=&quot;1025.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2381.45&quot; cy=&quot;1025.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2389.86&quot; cy=&quot;1024.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2389.86&quot; cy=&quot;1024.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2398.28&quot; cy=&quot;1024.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2398.28&quot; cy=&quot;1024.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2406.69&quot; cy=&quot;1006.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2406.69&quot; cy=&quot;1006.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2415.23&quot; cy=&quot;1004.14&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2415.23&quot; cy=&quot;1004.14&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2423.77&quot; cy=&quot;1002.28&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2423.77&quot; cy=&quot;1002.28&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2432.43&quot; cy=&quot;994.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2432.43&quot; cy=&quot;994.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2441.22&quot; cy=&quot;984.34&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2441.22&quot; cy=&quot;984.34&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2450.00&quot; cy=&quot;981.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2450.00&quot; cy=&quot;981.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2458.91&quot; cy=&quot;973.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2458.91&quot; cy=&quot;973.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2467.95&quot; cy=&quot;973.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2467.95&quot; cy=&quot;973.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2476.98&quot; cy=&quot;968.37&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2476.98&quot; cy=&quot;968.37&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2486.14&quot; cy=&quot;958.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2486.14&quot; cy=&quot;958.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2495.30&quot; cy=&quot;950.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2495.30&quot; cy=&quot;950.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2504.70&quot; cy=&quot;948.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2504.70&quot; cy=&quot;948.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2514.11&quot; cy=&quot;945.73&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2514.11&quot; cy=&quot;945.73&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2523.63&quot; cy=&quot;943.38&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2523.63&quot; cy=&quot;943.38&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2533.29&quot; cy=&quot;935.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2533.29&quot; cy=&quot;935.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2543.06&quot; cy=&quot;917.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2543.06&quot; cy=&quot;917.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2552.96&quot; cy=&quot;907.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2552.96&quot; cy=&quot;907.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2563.11&quot; cy=&quot;904.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2563.11&quot; cy=&quot;904.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2573.26&quot; cy=&quot;890.78&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2573.26&quot; cy=&quot;890.78&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2583.53&quot; cy=&quot;882.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2583.53&quot; cy=&quot;882.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2594.05&quot; cy=&quot;877.91&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2594.05&quot; cy=&quot;877.91&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2604.69&quot; cy=&quot;871.72&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2604.69&quot; cy=&quot;871.72&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2615.46&quot; cy=&quot;864.17&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2615.46&quot; cy=&quot;864.17&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2626.47&quot; cy=&quot;859.10&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2626.47&quot; cy=&quot;859.10&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2637.61&quot; cy=&quot;845.24&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2637.61&quot; cy=&quot;845.24&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2648.99&quot; cy=&quot;830.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2648.99&quot; cy=&quot;830.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2660.63&quot; cy=&quot;830.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2660.63&quot; cy=&quot;830.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2672.38&quot; cy=&quot;827.91&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2672.38&quot; cy=&quot;827.91&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2684.39&quot; cy=&quot;817.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2684.39&quot; cy=&quot;817.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2696.76&quot; cy=&quot;817.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2696.76&quot; cy=&quot;817.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2709.38&quot; cy=&quot;817.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2709.38&quot; cy=&quot;817.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2722.25&quot; cy=&quot;816.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2722.25&quot; cy=&quot;816.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2735.37&quot; cy=&quot;816.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2735.37&quot; cy=&quot;816.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2748.86&quot; cy=&quot;801.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2748.86&quot; cy=&quot;801.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2762.72&quot; cy=&quot;793.14&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2762.72&quot; cy=&quot;793.14&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2777.07&quot; cy=&quot;782.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2777.07&quot; cy=&quot;782.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2791.68&quot; cy=&quot;782.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2791.68&quot; cy=&quot;782.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2806.90&quot; cy=&quot;781.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2806.90&quot; cy=&quot;781.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2822.49&quot; cy=&quot;772.72&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2822.49&quot; cy=&quot;772.72&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2838.70&quot; cy=&quot;766.53&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2838.70&quot; cy=&quot;766.53&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2855.41&quot; cy=&quot;761.46&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2855.41&quot; cy=&quot;761.46&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2872.86&quot; cy=&quot;752.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2872.86&quot; cy=&quot;752.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2891.05&quot; cy=&quot;746.85&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2891.05&quot; cy=&quot;746.85&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2909.98&quot; cy=&quot;742.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2909.98&quot; cy=&quot;742.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2929.91&quot; cy=&quot;740.05&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2929.91&quot; cy=&quot;740.05&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2950.82&quot; cy=&quot;708.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2950.82&quot; cy=&quot;708.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2972.97&quot; cy=&quot;705.03&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2972.97&quot; cy=&quot;705.03&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2996.36&quot; cy=&quot;694.01&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2996.36&quot; cy=&quot;694.01&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3021.48&quot; cy=&quot;693.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3021.48&quot; cy=&quot;693.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3048.33&quot; cy=&quot;681.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3048.33&quot; cy=&quot;681.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3077.41&quot; cy=&quot;666.29&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3077.41&quot; cy=&quot;666.29&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3109.22&quot; cy=&quot;661.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3109.22&quot; cy=&quot;661.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3144.49&quot; cy=&quot;646.99&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3144.49&quot; cy=&quot;646.99&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3184.09&quot; cy=&quot;620.50&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3184.09&quot; cy=&quot;620.50&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3229.63&quot; cy=&quot;619.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3229.63&quot; cy=&quot;619.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3283.58&quot; cy=&quot;600.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3283.58&quot; cy=&quot;600.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3350.41&quot; cy=&quot;551.45&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3350.41&quot; cy=&quot;551.45&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3440.13&quot; cy=&quot;509.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3440.13&quot; cy=&quot;509.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3583.18&quot; cy=&quot;365.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3583.18&quot; cy=&quot;365.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;path d=&quot; M666.52 2124.36 L3583.18 297.38&quot; stroke-linejoin=&quot;round&quot; style=&quot;fill:none;stroke:#2D6D66;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2257.39&quot; x2=&quot;350.83&quot; y2=&quot;2257.39&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2257.39&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2257.39)&quot; text-anchor=&quot;middle&quot;&gt;-2000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1734.04&quot; x2=&quot;350.83&quot; y2=&quot;1734.04&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1734.04&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1734.04)&quot; text-anchor=&quot;middle&quot;&gt;-1000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1210.81&quot; x2=&quot;350.83&quot; y2=&quot;1210.81&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1210.81&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1210.81)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;687.45&quot; x2=&quot;350.83&quot; y2=&quot;687.45&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;687.45&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,687.45)&quot; text-anchor=&quot;middle&quot;&gt;1000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;350.83&quot; y2=&quot;164.22&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;164.22&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,164.22)&quot; text-anchor=&quot;middle&quot;&gt;2000&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Residuals&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2489.19&quot; x2=&quot;454.16&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-2000&lt;/text&gt;
	&lt;line x1=&quot;1289.47&quot; y1=&quot;2489.19&quot; x2=&quot;1289.47&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1289.47&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-1000&lt;/text&gt;
	&lt;line x1=&quot;2124.91&quot; y1=&quot;2489.19&quot; x2=&quot;2124.91&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;2960.35&quot; y1=&quot;2489.19&quot; x2=&quot;2960.35&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2960.35&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;1000&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2489.19&quot; x2=&quot;3795.66&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;2000&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Inverse Normal&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



    
    


```stata
# 統計学的検定

sktest re
```

    
    Unknown #command
    
    Skewness and kurtosis tests for normality
                                                             ----- Joint test -----
        Variable |       Obs   Pr(skewness)   Pr(kurtosis)   Adj chi2(2)  Prob>chi2
    -------------+-----------------------------------------------------------------
              re |       189         0.0555         0.9848          3.71     0.1564
    

帰無仮説は「残差に正規性がある」である。

棄却されると正規性がないことになる。

### 等分散性の確認


```stata
rvfplot, yline(0)
```


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2257.39&quot; x2=&quot;3859.02&quot; y2=&quot;2257.39&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1734.04&quot; x2=&quot;3859.02&quot; y2=&quot;1734.04&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1210.81&quot; x2=&quot;3859.02&quot; y2=&quot;1210.81&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;687.45&quot; x2=&quot;3859.02&quot; y2=&quot;687.45&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;3859.02&quot; y2=&quot;164.22&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1210.81&quot; x2=&quot;3859.02&quot; y2=&quot;1210.81&quot; style=&quot;stroke:#C10534;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2052.39&quot; cy=&quot;1396.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2052.39&quot; cy=&quot;1396.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2129.74&quot; cy=&quot;1394.21&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2129.74&quot; cy=&quot;1394.21&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;1390.00&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;1390.00&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2129.49&quot; cy=&quot;1371.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2129.49&quot; cy=&quot;1371.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1365.38&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1365.38&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;1344.46&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;1344.46&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1572.91&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1572.91&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2022.82&quot; cy=&quot;1332.46&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2022.82&quot; cy=&quot;1332.46&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2182.95&quot; cy=&quot;1343.97&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2182.95&quot; cy=&quot;1343.97&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2162.90&quot; cy=&quot;1339.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2162.90&quot; cy=&quot;1339.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1290.13&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1290.13&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1284.32&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1284.32&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2056.23&quot; cy=&quot;1278.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2056.23&quot; cy=&quot;1278.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.69&quot; cy=&quot;1286.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.69&quot; cy=&quot;1286.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1277.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1277.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1277.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1277.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2025.66&quot; cy=&quot;1259.07&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2025.66&quot; cy=&quot;1259.07&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2156.22&quot; cy=&quot;1277.51&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2156.22&quot; cy=&quot;1277.51&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1246.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1246.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2176.27&quot; cy=&quot;1260.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2176.27&quot; cy=&quot;1260.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2123.05&quot; cy=&quot;1244.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2123.05&quot; cy=&quot;1244.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3618.33&quot; cy=&quot;1478.73&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3618.33&quot; cy=&quot;1478.73&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3651.74&quot; cy=&quot;1483.44&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3651.74&quot; cy=&quot;1483.44&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2096.32&quot; cy=&quot;1225.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2096.32&quot; cy=&quot;1225.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1215.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1215.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3598.28&quot; cy=&quot;1453.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3598.28&quot; cy=&quot;1453.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2102.76&quot; cy=&quot;1204.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2102.76&quot; cy=&quot;1204.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;1432.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;1432.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;677.04&quot; cy=&quot;973.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;677.04&quot; cy=&quot;973.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1186.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1186.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1186.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1186.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2149.54&quot; cy=&quot;1189.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2149.54&quot; cy=&quot;1189.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;737.18&quot; cy=&quot;968.37&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;737.18&quot; cy=&quot;968.37&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;1398.17&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;1398.17&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2092.36&quot; cy=&quot;1165.51&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2092.36&quot; cy=&quot;1165.51&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2182.95&quot; cy=&quot;1179.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2182.95&quot; cy=&quot;1179.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;1169.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;1169.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2169.59&quot; cy=&quot;1206.35&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2169.59&quot; cy=&quot;1206.35&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2196.32&quot; cy=&quot;1167.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2196.32&quot; cy=&quot;1167.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2209.68&quot; cy=&quot;1154.50&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2209.68&quot; cy=&quot;1154.50&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;643.62&quot; cy=&quot;904.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;643.62&quot; cy=&quot;904.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3538.14&quot; cy=&quot;1347.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3538.14&quot; cy=&quot;1347.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2079.00&quot; cy=&quot;1118.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2079.00&quot; cy=&quot;1118.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3551.50&quot; cy=&quot;1349.54&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3551.50&quot; cy=&quot;1349.54&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1116.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1116.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1116.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;1116.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3625.01&quot; cy=&quot;1351.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3625.01&quot; cy=&quot;1351.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1097.57&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1097.57&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1337.90&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1337.90&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;634.22&quot; cy=&quot;877.91&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;634.22&quot; cy=&quot;877.91&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1330.60&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1330.60&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1094.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1094.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;1091.14&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;1091.14&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2189.63&quot; cy=&quot;1091.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2189.63&quot; cy=&quot;1091.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1053.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;1053.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2016.13&quot; cy=&quot;1049.93&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2016.13&quot; cy=&quot;1049.93&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;627.54&quot; cy=&quot;817.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;627.54&quot; cy=&quot;817.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.69&quot; cy=&quot;1049.93&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.69&quot; cy=&quot;1049.93&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1039.41&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1039.41&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2022.82&quot; cy=&quot;1024.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2022.82&quot; cy=&quot;1024.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2022.82&quot; cy=&quot;1024.81&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2022.82&quot; cy=&quot;1024.81&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1027.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1027.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;1028.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;1028.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3598.28&quot; cy=&quot;1266.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3598.28&quot; cy=&quot;1266.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;660.95&quot; cy=&quot;793.14&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;660.95&quot; cy=&quot;793.14&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1002.28&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1002.28&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;1006.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;1006.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;674.31&quot; cy=&quot;772.72&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;674.31&quot; cy=&quot;772.72&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3544.82&quot; cy=&quot;1215.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3544.82&quot; cy=&quot;1215.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2072.32&quot; cy=&quot;984.34&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2072.32&quot; cy=&quot;984.34&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;994.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;994.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;694.36&quot; cy=&quot;766.53&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;694.36&quot; cy=&quot;766.53&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;640.90&quot; cy=&quot;752.92&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;640.90&quot; cy=&quot;752.92&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2032.35&quot; cy=&quot;948.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2032.35&quot; cy=&quot;948.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2096.08&quot; cy=&quot;958.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2096.08&quot; cy=&quot;958.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2045.71&quot; cy=&quot;935.70&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2045.71&quot; cy=&quot;935.70&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;1168.36&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;1168.36&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2203.00&quot; cy=&quot;945.73&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2203.00&quot; cy=&quot;945.73&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;636.94&quot; cy=&quot;693.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;636.94&quot; cy=&quot;693.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3564.87&quot; cy=&quot;1143.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3564.87&quot; cy=&quot;1143.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1142.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1142.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3625.01&quot; cy=&quot;1145.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3625.01&quot; cy=&quot;1145.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.69&quot; cy=&quot;907.61&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.69&quot; cy=&quot;907.61&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;890.78&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;890.78&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;864.17&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;864.17&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;600.81&quot; cy=&quot;620.50&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;600.81&quot; cy=&quot;620.50&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;845.24&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2036.18&quot; cy=&quot;845.24&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3564.87&quot; cy=&quot;1077.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3564.87&quot; cy=&quot;1077.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3651.74&quot; cy=&quot;1083.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3651.74&quot; cy=&quot;1083.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1061.68&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;1061.68&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1063.79&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1063.79&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;817.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;817.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;827.91&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;827.91&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2156.22&quot; cy=&quot;830.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2156.22&quot; cy=&quot;830.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2096.08&quot; cy=&quot;817.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2096.08&quot; cy=&quot;817.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;1049.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;1049.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;1049.56&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;1049.56&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;816.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;816.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;816.40&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;816.40&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3611.64&quot; cy=&quot;1025.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3611.64&quot; cy=&quot;1025.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1004.14&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1004.14&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;761.46&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;761.46&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;746.85&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;746.85&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3564.87&quot; cy=&quot;981.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3564.87&quot; cy=&quot;981.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;742.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;742.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2092.36&quot; cy=&quot;740.05&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2092.36&quot; cy=&quot;740.05&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3611.64&quot; cy=&quot;973.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3611.64&quot; cy=&quot;973.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;950.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;950.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;708.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2109.44&quot; cy=&quot;708.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2032.35&quot; cy=&quot;694.01&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2032.35&quot; cy=&quot;694.01&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3625.01&quot; cy=&quot;943.38&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3625.01&quot; cy=&quot;943.38&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2029.50&quot; cy=&quot;681.02&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2029.50&quot; cy=&quot;681.02&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2182.95&quot; cy=&quot;705.03&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2182.95&quot; cy=&quot;705.03&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3631.69&quot; cy=&quot;917.26&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3631.69&quot; cy=&quot;917.26&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;666.29&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;666.29&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2096.32&quot; cy=&quot;661.59&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2096.32&quot; cy=&quot;661.59&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3504.72&quot; cy=&quot;882.12&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3504.72&quot; cy=&quot;882.12&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2096.32&quot; cy=&quot;646.99&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2096.32&quot; cy=&quot;646.99&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;871.72&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;871.72&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2016.13&quot; cy=&quot;619.76&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2016.13&quot; cy=&quot;619.76&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3544.82&quot; cy=&quot;859.10&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3544.82&quot; cy=&quot;859.10&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2082.96&quot; cy=&quot;600.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2082.96&quot; cy=&quot;600.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3551.50&quot; cy=&quot;830.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3551.50&quot; cy=&quot;830.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;801.55&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3558.18&quot; cy=&quot;801.55&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;782.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3578.23&quot; cy=&quot;782.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3618.33&quot; cy=&quot;781.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3618.33&quot; cy=&quot;781.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3645.06&quot; cy=&quot;782.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3645.06&quot; cy=&quot;782.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;509.25&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;509.25&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;551.45&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;551.45&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3711.88&quot; cy=&quot;365.69&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3711.88&quot; cy=&quot;365.69&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;674.31&quot; cy=&quot;2130.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;674.31&quot; cy=&quot;2130.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;2425.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3604.96&quot; cy=&quot;2425.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;730.50&quot; cy=&quot;1916.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;730.50&quot; cy=&quot;1916.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;2024.86&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;2024.86&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1949.49&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1949.49&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2089.64&quot; cy=&quot;1891.95&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2089.64&quot; cy=&quot;1891.95&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1887.74&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1887.74&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2085.68&quot; cy=&quot;1832.18&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2085.68&quot; cy=&quot;1832.18&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;1814.97&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2069.59&quot; cy=&quot;1814.97&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2129.49&quot; cy=&quot;1792.45&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2129.49&quot; cy=&quot;1792.45&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2203.00&quot; cy=&quot;1789.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2203.00&quot; cy=&quot;1789.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;1740.60&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;1740.60&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1730.20&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1730.20&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2016.13&quot; cy=&quot;1717.58&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2016.13&quot; cy=&quot;1717.58&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2156.22&quot; cy=&quot;1724.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2156.22&quot; cy=&quot;1724.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;1719.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;1719.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2065.64&quot; cy=&quot;1710.15&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2065.64&quot; cy=&quot;1710.15&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2149.54&quot; cy=&quot;1719.19&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2149.54&quot; cy=&quot;1719.19&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;1685.65&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;1685.65&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1640.23&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1640.23&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1645.43&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1645.43&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3538.14&quot; cy=&quot;1860.27&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3538.14&quot; cy=&quot;1860.27&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;1636.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2116.13&quot; cy=&quot;1636.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2162.90&quot; cy=&quot;1643.82&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2162.90&quot; cy=&quot;1643.82&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1856.06&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3571.55&quot; cy=&quot;1856.06&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;600.81&quot; cy=&quot;1377.75&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;600.81&quot; cy=&quot;1377.75&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;636.94&quot; cy=&quot;1382.83&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;636.94&quot; cy=&quot;1382.83&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;1585.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;1585.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2105.73&quot; cy=&quot;1580.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2105.73&quot; cy=&quot;1580.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;620.85&quot; cy=&quot;1335.80&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;620.85&quot; cy=&quot;1335.80&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2102.76&quot; cy=&quot;1560.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2102.76&quot; cy=&quot;1560.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1548.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2076.28&quot; cy=&quot;1548.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1543.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1543.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2029.50&quot; cy=&quot;1519.33&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2029.50&quot; cy=&quot;1519.33&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;623.58&quot; cy=&quot;1291.87&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;623.58&quot; cy=&quot;1291.87&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;1526.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2122.81&quot; cy=&quot;1526.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;1512.52&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2049.55&quot; cy=&quot;1512.52&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2082.96&quot; cy=&quot;1505.22&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2082.96&quot; cy=&quot;1505.22&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2196.32&quot; cy=&quot;1508.31&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2196.32&quot; cy=&quot;1508.31&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;3511.41&quot; cy=&quot;1714.24&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;3511.41&quot; cy=&quot;1714.24&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;656.99&quot; cy=&quot;1259.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;656.99&quot; cy=&quot;1259.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;636.94&quot; cy=&quot;1249.42&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;636.94&quot; cy=&quot;1249.42&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;663.67&quot; cy=&quot;1253.63&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;663.67&quot; cy=&quot;1253.63&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2009.45&quot; cy=&quot;1464.38&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2009.45&quot; cy=&quot;1464.38&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1465.37&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2062.91&quot; cy=&quot;1465.37&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2189.63&quot; cy=&quot;1477.37&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2189.63&quot; cy=&quot;1477.37&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;1469.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2136.17&quot; cy=&quot;1469.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2102.76&quot; cy=&quot;1461.66&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2102.76&quot; cy=&quot;1461.66&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2142.86&quot; cy=&quot;1462.77&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2142.86&quot; cy=&quot;1462.77&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1439.13&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1439.13&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2082.96&quot; cy=&quot;1443.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2082.96&quot; cy=&quot;1443.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1433.44&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2042.87&quot; cy=&quot;1433.44&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2162.90&quot; cy=&quot;1443.96&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2162.90&quot; cy=&quot;1443.96&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;580.76&quot; cy=&quot;1196.08&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;580.76&quot; cy=&quot;1196.08&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2176.27&quot; cy=&quot;1445.94&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2176.27&quot; cy=&quot;1445.94&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2002.77&quot; cy=&quot;1403.62&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2002.77&quot; cy=&quot;1403.62&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;640.90&quot; cy=&quot;1190.39&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;640.90&quot; cy=&quot;1190.39&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1409.31&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2039.03&quot; cy=&quot;1409.31&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;circle cx=&quot;2129.49&quot; cy=&quot;1423.54&quot; r=&quot;26.35&quot; style=&quot;fill:#1A476F&quot;/&gt;
	&lt;circle cx=&quot;2129.49&quot; cy=&quot;1423.54&quot; r=&quot;22.03&quot; style=&quot;fill:none;stroke:#1A476F;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2257.39&quot; x2=&quot;350.83&quot; y2=&quot;2257.39&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2257.39&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2257.39)&quot; text-anchor=&quot;middle&quot;&gt;-2000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1734.04&quot; x2=&quot;350.83&quot; y2=&quot;1734.04&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1734.04&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1734.04)&quot; text-anchor=&quot;middle&quot;&gt;-1000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1210.81&quot; x2=&quot;350.83&quot; y2=&quot;1210.81&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1210.81&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1210.81)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;687.45&quot; x2=&quot;350.83&quot; y2=&quot;687.45&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;687.45&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,687.45)&quot; text-anchor=&quot;middle&quot;&gt;1000&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;350.83&quot; y2=&quot;164.22&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;164.22&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,164.22)&quot; text-anchor=&quot;middle&quot;&gt;2000&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Residuals&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2489.19&quot; x2=&quot;454.16&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;2400&lt;/text&gt;
	&lt;line x1=&quot;1122.41&quot; y1=&quot;2489.19&quot; x2=&quot;1122.41&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1122.41&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;2600&lt;/text&gt;
	&lt;line x1=&quot;1790.79&quot; y1=&quot;2489.19&quot; x2=&quot;1790.79&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1790.79&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;2800&lt;/text&gt;
	&lt;line x1=&quot;2459.04&quot; y1=&quot;2489.19&quot; x2=&quot;2459.04&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2459.04&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;3000&lt;/text&gt;
	&lt;line x1=&quot;3127.41&quot; y1=&quot;2489.19&quot; x2=&quot;3127.41&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3127.41&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;3200&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2489.19&quot; x2=&quot;3795.66&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;3400&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Fitted values&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>




```stata
# 統計学的に確認

hettest
```

    
    Unknown #command
    
    Breusch-Pagan / Cook-Weisberg test for heteroskedasticity 
             Ho: Constant variance
             Variables: fitted values of bwt
    
             chi2(1)      =     0.00
             Prob > chi2  =   0.9961
    

棄却されないので、等分散性は少なくとも否定はされないことになる。

#### 参考文献

Stata公式資料

https://www.stata-press.com

講義プリント
