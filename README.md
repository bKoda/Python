# The purpose of this project was to determine the largest population size per Country as of 2020

# Import libaries
```
##Import libaries

#itables for interactive tables
from itables import init_notebook_mode
from itables import show
init_notebook_mode(all_interactive=True)

import numpy as np 
import pandas as pd 

#Library for visualizations
import plotly.express as px
from plotly.offline import iplot, init_notebook_mode
init_notebook_mode(connected=True)
```
# Import Dataset

```
df= pd.read_csv("world_population.csv", header=0)
```
# Clean column names

```
#Rename population columns from "Year Population" to  "Year"
for col in df.columns:
    if 'Population' and '0' in col:
        df = df.rename(columns={col: col.split(' ')[0]})   
df.head()
```
# Filter top 7 countries by population as of 2020

```
top7 = df.sort_values('2022', ascending=False).head(7)
top7 = top7.melt(id_vars=['Country'], value_vars=['2020', '2010', '2000', '1990', '1980', '1970'], var_name='Year', value_name='Population')
top7.head()
```
# Create line graph to visualize data

```
fig = px.line(top7, x='Year', y='Population', color='Country', markers=True, title='Top 7 Countries with Biggest Population Growth Throughout 1970-2020')
fig['layout']['xaxis']['autorange'] = "reversed"
fig.show()
```
![image](https://user-images.githubusercontent.com/75760072/190509158-bae36508-f68b-4557-8132-0f891d6408b6.png)
