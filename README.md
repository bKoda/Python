# The purpose of this project was to determine the largest population size per Country and Continent as of 2020

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
![image](https://user-images.githubusercontent.com/75760072/190509284-e1adaa2c-be6f-4c51-81fb-c406526b3b40.png)

# Create line graph to visualize data

```
fig = px.line(top7, x='Year', y='Population', color='Country', markers=True, title='Top 7 Countries with Biggest Population Growth Throughout 1970-2020')
fig['layout']['xaxis']['autorange'] = "reversed"
fig.show()
```
![image](https://user-images.githubusercontent.com/75760072/190509158-bae36508-f68b-4557-8132-0f891d6408b6.png)


# Group population by continent and year excluding Antartica

```
continent = {'continent': [], 'sum': [], 'year': []}
years = ['1970', '1980', '1990', '2000', '2010', '2020']
continents = ['Asia', 'Europe', 'Africa', 'Oceania', 'North America', 'South America']

k = 0

for i in years:
    for j in continents:
        continent['sum'].append(df[df['Continent'] == j][i].sum())
        continent['year'].append(years[k])
    
    continent['continent'] += continents
    
    k+=1
continents = pd.DataFrame(continent).sort_values('year')
```

# Create visual for data

```
fig = px.bar(continents.sort_values(['year','sum'], ascending=[True, False]), x='continent', y='sum', color='continent', animation_frame="year")
fig.update_yaxes(range=[0, 4700000000])
fig.show()
```
![image](https://user-images.githubusercontent.com/75760072/190512713-6ea50b4d-3782-452f-9cb9-87d082c55fba.png)
