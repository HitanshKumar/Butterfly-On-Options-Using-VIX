
# Butterfly On Options using India VIX🔗

Combining the two❗

🥇 The butterfly strategy is a very common strategy used for options and is widely used, but to increase its accuracy and to get the perfect strike levels we can use india vix calculation and here is how.

🥈 Divide the vix value by the root of 365 for a day, 52 for a wek or 12 for a month

🥉 The number that you received will be the percentage by which the index is expected to go up or down for a particular time period

✨ For this project i specifically took the data for jan2024 as it showed exactly how this butterfly strategy would work.
## DEPLOYMENT⚒️

Following are the imp code snipits⬇️

1️⃣ Importing historical options data using ICICI Breeze API⤵️
```bash
month = input('enter the month')
a = int(np.round(strike_up,-2))
b = int(np.round(strike_down,-2))
strikes = [a,a+200,b,b-200]

to_expiry = "2024-01-25"
To = []
Type = ['call','put']
data = {}
for i in strikes:
    for j in Type:
        key = f'{month}_{i}_{j}'
        data[key] = breeze.get_historical_data(interval="1day",
                            from_date = "2023-12-29",
                            to_date= to_expiry,
                            stock_code="NIFTY",
                            exchange_code="NFO",
                            product_type="options",
                            expiry_date= to_expiry,
                            right=str(j),
                            strike_price= str(i))
```
2️⃣ Forming the Data Frame⤵️
```bash
df = pd.DataFrame()
for i in data.keys():
    if 'call' in i:
        a = pd.DataFrame(data[i]['Success'])
        df[i] = pd.DataFrame(a['close'])
    else:
        b = pd.DataFrame(data[i]['Success'])
        df[i] = pd.DataFrame(b['close'])

```
3️⃣ Forming the positions column⤵️
```bash
for i in strikes:
    if i == int(np.round(strike_up,-2)):
        merged_df[f'{i}_position_call'] = -1
        
    elif i == int(np.round(strike_down,-2)):
        merged_df[f'{i}_position_put'] = -1
        
    elif i == int(np.round(strike_up,-2)) + 200:
        merged_df[f'{i}_position_call'] = 1
        
    elif i == int(np.round(strike_down,-2)) - 200:
        merged_df[f'{i}_position_put'] = 1
    else:
        pass

```
4️⃣ Calculating Returns for the strategy⤵️
```bash
position_columns = {
    'jan_20800_put': '20800_position_put',
    'jan_22600_call': '22600_position_call',
    'jan_20600_put': '20600_position_put',
    'jan_22800_call': '22800_position_call'
}

for strike in all_strikes:
    position_col = position_columns.get(strike)
    
    if position_col in merged_df.columns and f'{strike}_log_return' in merged_df.columns:
        merged_df[f'{strike}_s_returns'] = merged_df[position_col] * merged_df[f'{strike}_log_return']

print(merged_df.head())
 
```

## Conclusion🔚

➡️ The strategy returns that we got were not expected, for jan2024 the market was in a range but still the strategy returns were not good, that is because there were 2 major spikes in that time period which tells us that there can still be drawbacks to such a strategy even in a range bound market
## 🔗 Links
[![ICICI BREEZE](https://api.icicidirect.com/apiuser/home)](https://api.icicidirect.com/)


## Percentage calculation for every time frame

| Time⌚        | Percentage%                                                |
| ----------------- | ------------------------------------------------------------------ |
| 1DAY | vix/(root(365)) |
| 1WEEK | vix/(root(52)) |
| 1MONTH | vix/(root(12)) |
| 1YEAR | vix/(root(1)) |



