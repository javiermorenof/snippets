# Overview

```python
import altair as alt
```

## Bar Chart
```python
alt.Chart(brain).mark_bar().encode( #Being brain the name of the dataset
                                    x = 'Brain Weight',
                                    y = "count()"
                                    ).properties(
                                    width=300,
                                    height=150,
                                    title="Relación peso del cerebro y del cuerpo"
                                    ).interactive()
```

## Bar Chart selecting maximum of bins
```python
alt.Chart(brain).mark_bar().encode(
                                    x = alt.X('Brain Weight',bin = alt.Bin(maxbins=100)), #naming altair to indicate which is X and select number of Bins
                                    y = "count()"
                                    ).properties(
                                    width=300,
                                    height=150,
                                    title="Relación peso del cerebro y del cuerpo"
                                    ).interactive()
```

## Bar Chart (more variations)
```python
bar_trends = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term" #Adding color to the graph based on search_term column
).properties(width=200 #Add width to the graph
).interactive()

```

## Line Chart
```python
alt.Chart(trends).mark_line().encode(
    x="date",
    y="value",
    color="search_term"
)
```

## Line Chart (If doesn't recognize date as a date time format)
```python
alt.Chart(trends).mark_line().encode(
    x="date:T", #Using :T to indicate it's a datetime
    y="value",
    color="search_term"
).interactive()
```

## Line Chart (other variations)
```python
line_trends = alt.Chart(trends).mark_line().encode(
    x="yearmonth(date):T",
    y="mean(value)",
    color="search_term"
)
line_trends
```

## Combined charts, filtering by clicking
```python
select_term = alt.selection(type="single", encodings = ["x"]) #Encodings sirve para decir qué se filtra

trend_line = alt.Chart(trends).mark_line().encode(
    x="yearmonth(date):T",
    y="mean(value)",
    color="search_term"
).transform_filter( #transform_filter es el filtrado
    select_term
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term",
    tooltip="value" #tooltip sirve para que aparezca un cartelito si dejas el ratón por encima
).properties(
    selection=select_term #Y con selection le dices que quieres que filtre este gráfico en base a lo indicado en select_terms
)

trend_bar|trend_line  #Used to show in the same line both charts using "|". If used (x|y)&z, x and y will be in the same line and z in the next one.
```

## Combined charts, filtering by intervals
```python
select_date = alt.selection(type="interval",encodings=["x"]) #con interval puedes filtrar por intervalos arrastrando

trend_line = alt.Chart(trends).mark_line().encode(
    x="yearmonth(date):T",
    y="mean(value)",
    color="search_term"
).properties(
    selection=select_date
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="value",
    color="search_term",
    tooltip="search_term"
).transform_filter(
    select_date
)

trend_bar|trend_line
```

## Scatter plot
```python
scatter_GDP = alt.Chart(lifecountries).mark_circle().encode(
                                                            x='Life Expectancy',
                                                            y='Country GDP',
                                                            color = 'Continent',
                                                            size = 'size' #size used to give size to the bublles based on frequency
                        ).properties(
                        width=700,
                        height=300,
                        title="Life Expectancie"
                        ).interactive()
```

## Map plot
```python
temp = world_temp.groupby("city").mean().reset_index()

points = alt.Chart(temp).mark_circle().encode(
    latitude="lat",
    longitude="lon",
    color=alt.Color("mean(temp)",scale=alt.Scale(domain=[-30,10,40],range=["lightblue","orange","red"]))#Meter rango y colores
)

# remote geojson data object
url = "https://raw.githubusercontent.com/datasets/geo-countries/master/data/countries.geojson"
data_geojson_remote = alt.Data(url=url, format=alt.DataFormat(property='features',type='json'))

# chart object
background = alt.Chart(data_geojson_remote).mark_geoshape(
    fill="lightgrey",
    stroke="white"
).encode(
    color="properties.name:N"
).properties(
    projection={'type': 'identity', 'reflectY': True} #Es el tipo de proyección geográfica que puedes hacer
)

(background+points)
```

## Global Options
```python
config = {'arc': {'fill': '#30a2da'},
 'area': {'fill': 'black'},
 'axisBand': {'grid': False},
 'axisBottom': {'domain': False,
  'domainColor': '#999',
  'domainWidth': 3,
  'grid': False,
  'gridColor': '#cbcbcb',
  'gridWidth': 1,
  'labelColor': '#999',
  'labelFontSize': 14,
  'labelPadding': 4,
  'tickColor': 'white',
  'tickSize': 10,
  'titleFontSize': 14,
  'titlePadding': 10},
 'axisLeft': {'domainColor': 'white',
  'domainWidth': 1,
  'grid': False,
  'gridColor': 'white',
  'gridWidth': 1,
  'labelColor': 'white',
  'labelFontSize': 10,
  'labelPadding': 4,
  'tickColor': 'white',
  'tickSize': 10,
  'ticks': True,
  'titleFontSize': 14,
  'titlePadding': 10},
 'axisRight': {'domainColor': '#333',
  'domainWidth': 1,
  'grid': True,
  'gridColor': '#cbcbcb',
  'gridWidth': 1,
  'labelColor': 'white',
  'labelFontSize': 10,
  'labelPadding': 4,
  'tickColor': 'white',
  'tickSize': 10,
  'ticks': True,
  'titleFontSize': 14,
  'titlePadding': 10},
 'axisTop': {'domain': False,
  'domainColor': '#333',
  'domainWidth': 3,
  'grid': False,
  'gridColor': '#cbcbcb',
  'gridWidth': 1,
  'labelColor': '#999',
  'labelFontSize': 10,
  'labelPadding': 4,
  'tickColor': '#cbcbcb',
  'tickSize': 10,
  'titleFontSize': 14,
  'titlePadding': 10},
 'background': 'white',
 'group': {'fill': 'black',"stroke":"white"},
 'legend': {'labelColor': '#333',
  'labelFontSize': 14,
  'padding': 1,
  'symbolSize': 30,
  'symbolType': 'square',
  'titleColor': '#333',
  'titleFontSize': 14,
  'titlePadding': 10},
 'line': {'stroke': '#30a2da', 'strokeWidth': 2},
 'path': {'stroke': '#30a2da', 'strokeWidth': 0.5},
 'rect': {'fill': '#30a2da'},
 'range': {'category': ['#30a2da',
   '#fc4f30',
   '#e5ae38',
   '#6d904f',
   '#8b8b8b',
   '#b96db8',
   '#ff9e27',
   '#56cc60',
   '#52d2ca',
   '#52689e',
   '#545454',
   '#9fe4f8'],
  'diverging': ['#cc0020',
   '#e77866',
   '#f6e7e1',
   '#d6e8ed',
   '#91bfd9',
   '#1d78b5'],
  'heatmap': ['#d6e8ed', '#cee0e5', '#91bfd9', '#549cc6', '#1d78b5']},
 'symbol': {'filled': True, 'shape': 'circle'},
 'shape': {'stroke': 'white'},
 'style': {'bar': {'binSpacing': 2, 'fill': '#30a2da', 'stroke': None}},
 'title': {'anchor': 'start', 'fontSize': 24, 'fontWeight': 600, 'offset': 20},
         "view": {
     "stroke": "transparent"
  }}

```
