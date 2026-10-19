# Daily Pandas Practice with ChatGPT, Claude, Meta AI (WhatsApp) 19th October 2026 Part 1
<b>Round 2: Environmental Monitoring Analysis</b></br>
<b>Step 1 - Create the datasets</b></br>
```Python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "site_id": [201, 202, 203, 204, 205, 206, 207, 208],
    "state": ["WB", "WB", "AS", "AS", "OD", "OD", "WB", "AS"],
    "landuse": [
        "Urban", "Forest", "Forest", "Urban",
        "Agriculture", "Forest", "Agriculture", "Urban"
    ],
    "rainfall": [420, 180, 650, 320, 520, 720, 360, 460],
    "elevation": [15, 125, 55, 9, 45, 80, 35, 65],
    "ndvi": [0.28, 0.78, 0.88, 0.32, 0.42, 0.82, 0.58, 0.68]
})

backup_df = pd.DataFrame({
    "site_id": [201, 202, 203, 204, 205, 206, 207, 208],
    "rainfall_backup": [410, 190, 640, 310, 510, 710, 355, 455],
    "temperature": [32, 28, 30, 34, 33, 27, 31, 29]
})
```
<b>Step 2 - Your 8 tasks</b></br>
<b>Challenge tracker</b></br>
<ul>
  <li><b>1. Create two working DataFrame</b></li>
    Create `working_df= df.copy(` and a separate high_rainfall` DataFrame where rainfall >= 400 AND elevation < 100.
  <li><b>2. Classify rainfall</b></li>
    Using a function and .apply()`, add `rainfall_class`: below 300 - Low`; 300 to below 500 _, `Moderate"; 500 or above - `High'
  <li><b>3. Calculate vulnerability</b></li>
    Use a function with`.apply(axis=1)". Return `High` if rainfall >= 500 AND elevation < 100 AND NDVI < 0. 5; otherwise return `Low'
  <li><b>4. Create a grouped summary</b></li>
    Create summary` by grouping on both `state` and 'landuse' calculating mean rainfall and mean NDVI using `. agg()
  <li><b>5. Use transform</b></li>
    Add a `state_mean_rainfall column to working_df using groupby(state)[rainfall].transform(mean).
  <li><b>6. Merge the backup data</b></li>
    Merge `backup_df into `working_df` using site_id`. Keep the main dataset's rows.
  <li><b>7. Fill missing rainfall</b></li>
    Imagine some `rainfall` values are missing. Create rainfall_final using the original rainfall as the priority and `rainfall_backup` as the fallback with `.combine_first()
  <li><b>8. Build a pivot table</b></li>
    Create `rainfall_pivot`: states as rows, landuse as columns, mean `rainfall_final` as values
</ul>

# Solution (Script)
```Python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "site_id": [201, 202, 203, 204, 205, 206, 207, 208],
    "state": ["WB", "WB", "AS", "AS", "OD", "OD", "WB", "AS"],
    "landuse": [
        "Urban", "Forest", "Forest", "Urban",
        "Agriculture", "Forest", "Agriculture", "Urban"
    ],
    "rainfall": [420, 180, 650, 320, 520, 720, 360, 460],
    "elevation": [15, 125, 55, 9, 45, 80, 35, 65],
    "ndvi": [0.28, 0.78, 0.88, 0.32, 0.42, 0.82, 0.58, 0.68]
})

backup_df = pd.DataFrame({
    "site_id": [201, 202, 203, 204, 205, 206, 207, 208],
    "rainfall_backup": [410, 190, 640, 310, 510, 710, 355, 455],
    "temperature": [32, 28, 30, 34, 33, 27, 31, 29]
})
working_df_1 = df.copy()
high_rainfall_df = df.copy()

high_rainfall_df = high_rainfall_df[(high_rainfall_df["rainfall"]>= 400) & (high_rainfall_df["elevation"] <100 )]

def myfunction1 (rain):
    if rain < 300:
        return "Low"
    elif rain < 500:
        return "Moderate"
    else:
        return "High"

working_df_1["rainfall_class"] = working_df_1["rainfall"].apply(myfunction1)

def myfunction2 (row):
    rain = row["rainfall"]
    height = row["elevation"]
    NDVI = row["ndvi"]
    if rain >= 500 and height < 100 and NDVI < 0.5:
        return "High"
    else:
        return "Low"
working_df_1["vulnerability"] = working_df_1.apply(myfunction2,axis=1)

working_df_1["state_mean_rainfall"] = working_df_1.groupby("state")["rainfall"].transform("mean")
grouped_summary_1 = working_df_1.groupby(["state","landuse"])[["rainfall","ndvi"]].agg("mean").reset_index()
working_df_1_updated = working_df_1.merge(backup_df,on="site_id")
working_df_1_updated["rainfall_final"] = working_df_1_updated["rainfall"].combine_first(working_df_1_updated["rainfall_backup"])
the_p_t = working_df_1_updated.pivot_table(index="state",
                                           columns="landuse",
                                           values="rainfall_final",
                                           aggfunc="mean").reset_index()
print()
print(high_rainfall_df)
print()
print(working_df_1_updated)
print()
print(grouped_summary_1)
print()
print(the_p_t)
print()
```

# Solution (Output)
```
PS C:\Users\hp\Downloads\sql_practice(0)> & C:\Users\hp\AppData\Local\Programs\Python\Python313\python.exe "c:/Users/hp/Downloads/sql_practice(0)/test2.py"

   site_id state      landuse  rainfall  elevation  ndvi
0      201    WB        Urban       420         15  0.28
2      203    AS       Forest       650         55  0.88
4      205    OD  Agriculture       520         45  0.42
5      206    OD       Forest       720         80  0.82
7      208    AS        Urban       460         65  0.68

   site_id state      landuse  rainfall  elevation  ndvi rainfall_class vulnerability  state_mean_rainfall  rainfall_backup  temperature  rainfall_final
0      201    WB        Urban       420         15  0.28       Moderate           Low           320.000000              410           32             420
1      202    WB       Forest       180        125  0.78            Low           Low           320.000000              190           28             180
2      203    AS       Forest       650         55  0.88           High           Low           476.666667              640           30             650
3      204    AS        Urban       320          9  0.32       Moderate           Low           476.666667              310           34             320
4      205    OD  Agriculture       520         45  0.42           High          High           620.000000              510           33             520
5      206    OD       Forest       720         80  0.82           High           Low           620.000000              710           27             720
6      207    WB  Agriculture       360         35  0.58       Moderate           Low           320.000000              355           31             360
7      208    AS        Urban       460         65  0.68       Moderate           Low           476.666667              455           29             460

  state      landuse  rainfall  ndvi
0    AS       Forest     650.0  0.88
1    AS        Urban     390.0  0.50
2    OD  Agriculture     520.0  0.42
3    OD       Forest     720.0  0.82
4    WB  Agriculture     360.0  0.58
5    WB       Forest     180.0  0.78
6    WB        Urban     420.0  0.28

landuse state  Agriculture  Forest  Urban
0          AS          NaN   650.0  390.0
1          OD        520.0   720.0    NaN
2          WB        360.0   180.0  420.0

PS C:\Users\hp\Downloads\sql_practice(0)> 
```
<ul>
    <li>
        <b>hu</b>
        <p>jj</p>
    </li>
</ul>
