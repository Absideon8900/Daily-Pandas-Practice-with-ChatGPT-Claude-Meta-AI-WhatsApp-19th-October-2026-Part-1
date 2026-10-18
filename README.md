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
  <li><b>2. Classify rainfall</b></li>
  <li><b>3. Calculate vulnerability</b></li>
  <li><b>4. Create a grouped summary</b></li>
  <li><b>5. Use transform</b></li>
  <li><b>6. Merge the backup data</b></li>
  <li><b>7. Fill missing rainfall</b></li>
  <li><b>8. Build a pivot table</b></li>
</ul>
