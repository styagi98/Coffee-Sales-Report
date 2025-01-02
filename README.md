# Coffee Sales Analysis with Power BI

## Problem Statement
This project aims to analyze a dataset containing coffee sales information. 
Using Power BI and DAX, the goal is to generate meaningful insights such as total sales, 
total quantity sold, monthly order trends, daily averages, and month-over-month growth.

## Steps Followed
1. **Imported the dataset** from a xlsx file containing sales data.
2. **Cleaned and transformed the data** in Power Query Editor.
3. **Created relationships** between tables and a Date Table for time-based analysis.
4. **Built DAX measures** for key metrics like total sales, total orders, daily averages, and MoM growth.
5. **Created visualizations** using bar charts, line graphs, and matrix.
6. **Added interactivity** through slicers and drill-down capabilities.

## Code Examples

### DAX Measures:
Here are the key DAX measures used in the Power BI report:

#### Total Sales:
    Total_Sales = SUM(Transactions[Sales])

#### Total Quantity Sold:
    Total_Qty_Sold = SUM(Transactions[transaction_qty])

#### Total Orders:
    Total_Orders = DISTINCTCOUNT(Transactions[transaction_id])

#### Current Month Sales:
    CM_Sales = VAR selected_month = SELECTEDVALUE(Date_Table[Month])
                RETURN
                TOTALMTD(CALCULATE([Total_Sales] , Date_Table[Month] = selected_month) , Date_Table[Date])

#### Current Month Orders:
    CM_Orders = VAR selected_month = SELECTEDVALUE(Date_Table[Month])
                RETURN
                TOTALMTD(CALCULATE([Total_Orders] , Date_Table[Month] = selected_month) , Date_Table[Date])

#### Current Month Quantity:
    CM_Qty = VAR selected_month = SELECTEDVALUE(Date_Table[Month])
                RETURN
                TOTALMTD(CALCULATE([Total_Qty_Sold] , Date_Table[Month] = selected_month) , Date_Table[Date])

#### Previous Month Sales:
    PM_Sales = CALCULATE([CM_Sales] , DATEADD(Date_Table[Date],-1,MONTH))

#### Previous Month Orders:
    PM_Orders = CALCULATE([CM_Orders] , DATEADD(Date_Table[Date],-1,MONTH))

#### Previous Month Quantity:
    PM_Qty = CALCULATE([CM_Qty] , DATEADD(Date_Table[Date],-1,MONTH))

#### MOM Sales Growth:
    MOM_Growth & Diff_Sales = 

        VAR month_diff = [CM_Sales] - [PM_Sales]
        VAR mom = ([CM_Sales] - [PM_Sales]) / [PM_Sales]
        VAR _sign = IF(month_diff > 0 ,"+", "")
        VAR _sign_trend = IF(month_diff > 0 , "▲" , "▼")
        RETURN
        _sign_trend & " " & _sign & FORMAT(mom , "#0.0%" & " | " & _sign & FORMAT(month_diff/1000, "0.0K")) & " " & "vs LM"

#### MOM Orders Growth:
    MOM_Growth & Diff_Orders = 

        VAR month_diff = [CM_Orders] - [PM_Orders]
        VAR mom = ([CM_Orders] - [PM_Orders]) / [PM_Orders]
        VAR _sign = IF(month_diff > 0 ,"+", "")
        VAR _sign_trend = IF(month_diff > 0 , "▲" , "▼")
        RETURN
        _sign_trend & " " & _sign & FORMAT(mom , "#0.0%" & " | " & _sign & FORMAT(month_diff/1000, "0.0K")) & " " & "vs LM" 

## Visualizations:

Created various visualizations in Power BI, including bar charts, line graphs, and tables, to display key metrics and insights.

A Total Sales chart showing monthly sales trends.
A Monthly Sales Growth chart displaying month-over-month growth percentages.

A Daily Average Sales line graph to show daily fluctuations in sales.
Used slicers to filter by months.


## Report Access
You can download the Power BI report file (.pbix) from the repository to view the analysis.
