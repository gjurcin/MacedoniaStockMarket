# MSE dataset column description

## The following information is obtained from MSE

- Date - Trade Date
- stock_id - Unique ID of the traded company
- Open - Opening price (same as closing price of the previous day)
- High - Highest price during the day
- Low - Lowest price during the day
- Close - The price of the last transaction in the day
- Volume - Total amount in MKD of all transactions in the day through the BEST trading system of MSE
- Adj Close - Same as Close (ignore it)
- Quantity - Total quantity of traded unints in all transaction in the day
- Average - Average price considering all transactions in the day
- Change % - Change of average price compared to the last day on which there was a transaction
- Volume Total - Total amount in MKD of all transactions in the day through the BEST trading system of MSE and Block transactions (which are not processed through BEST)

## The following columns are derived and can be ignored or recomputed in a different way if you need

- Ratio - The average price during the day divided by the maxium average price since beggining of time till today (smoothed by Exponential Moving Average(5))
- Ratio 1m - The Average price during the day divided by the maxium average price in the last 30 days relative to the day when the data was collected (i.e., MAX(Date))
- Ratio 3m - The Average price during the day divided by the maxium average price in the last 90 days relative to the day when the data was collected (i.e., MAX(Date))
- Ratio 6m - The Average price during the day divided by the maxium average price in the last 180 days relative to the day when the data was collected (i.e., MAX(Date))
- Ratio 1y - The Average price during the day divided by the maxium average price in the last 365 days relative to the day when the data was collected (i.e., MAX(Date))
- Ratio 2y - The Average price during the day divided by the maxium average price in the last 730 days relative to the day when the data was collected (i.e., MAX(Date))
- Ratio 3y - The Average price during the day divided by the maxium average price in the last 1095 days relative to the day when the data was collected (i.e., MAX(Date))
