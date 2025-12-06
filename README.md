# thegeodatascientist

Code for plotting a shooting star out of the Schiller*innen in this Advent time. Enjoy!
The code is optimized for Jupyter Notebook but it can be adapted to any standalone format. Please adapt the paths to files. 

[Open HTML visualization](https://github.com/yzut-ydv/thegeodatascientist/blob/main/schillerinnen_blume_all.html)


```
import plotly
import plotly.graph_objects as go
import pandas as pd

df_persons = pd.read_csv("./adventskalender_schiller/schillerinnen_persons.csv", sep=",")
df_persons.head()

df_routes = pd.read_csv("./adventskalender_schiller/schillerinnen_routes.csv", sep=",")
df_routes.head()

fig1 = go.Figure()

fig1.add_trace(go.Scattergeo(
    lon = df_persons['long'],
    lat = df_persons['lat'],
    hoverinfo = 'text',
    text = df_persons['person'],
    mode = 'markers+text',
    textposition="top center",   # or "middle right", etc.
    textfont=dict(
        family="Calibri",      # any font name
        size=10,             # font size
        color="white"        # text color
    ),
    marker = dict(
        size = 3,
        color = 'rgb(245, 185, 36)',
        line = dict(width = 3, color = 'rgba(245, 185, 36, 0)')
    )
))

for i in range(len(df_routes)):
    fig1.add_trace(
        go.Scattergeo(
            lon = [df_routes['start_lon'][i], df_routes['end_lon'][i]],
            lat = [df_routes['start_lat'][i], df_routes['end_lat'][i]],
            mode = 'lines',
            line = dict(width=2, color='rgb(245, 185, 36)'),
        )
    )

fig1.update_layout(
    title=dict(
        text='Schiller*innen Sternschnuppe im Advent',
        x=0.5,       # center horizontally
        y=0.1,      # move to bottom (0 = very bottom, 0.02 = slight margin)
        xanchor='center',
        yanchor='middle',
        font=dict(
            family="Calibri",      # any system or web-safe font
            size=16,             # font size in px
            color="black"        # any CSS color
        )
    ),
    showlegend=False,
    geo=dict(
        scope='world',                         # show whole world
        projection_type='natural earth',       # works well for Europe+Africa
        showland=True,
        landcolor='rgb(0, 121, 107)',
        # ---- Optional: zoom into Europe + Africa ----
        lonaxis=dict(range=[-20, 20]),         # west to east
        lataxis=dict(range=[0, 60]),         # south to north
    ),
)



# Save the plot as HTML and PNG
fig1.write_html(f"adventskalender_schiller/schillerinnen_blume_all.html")
fig1.write_image(f"adventskalender_schiller/schillerinnen_blume_all.png", scale=4)

fig1.show()

fig2 = go.Figure()

positions = ["top center", "bottom center", "middle left", "middle right"]

fig2.add_trace(go.Scattergeo(
    lon = df_persons['long'],
    lat = df_persons['lat'],
    hoverinfo = 'text',
    text = df_persons['person'],
    mode = 'markers+text',  
    textposition=[positions[i % len(positions)] for i in range(len(df_persons))],
    textfont=dict(
        family="Calibri",      # any font name
        size=10,             # font size
        color="white"        # text color
    ),
    marker = dict(
        size = 3,
        color = 'rgb(245, 185, 36)',
        line = dict(width = 2, color = 'rgba(245, 185, 36, 0)')
    )
))

for i in range(len(df_routes)):
    fig2.add_trace(
        go.Scattergeo(
            lon = [df_routes['start_lon'][i], df_routes['end_lon'][i]],
            lat = [df_routes['start_lat'][i], df_routes['end_lat'][i]],
            mode = 'lines',
            line = dict(width=2, color='rgb(245, 185, 36)'),
        )
    )

fig2.update_layout(
    title=dict(
        text='Schiller*innen Sternschnuppe im Advent',
        x=0.5,       # center horizontally
        y=0.1,      # move to bottom (0 = very bottom, 0.02 = slight margin)
        xanchor='center',
        yanchor='middle',
        font=dict(
            family="Calibri",      # any system or web-safe font
            size=16,             # font size in px
            color="black"        # any CSS color
        )),
    showlegend=False,
    geo=dict(
        scope='world',                         # show whole world
        projection_type='natural earth',       # works well for Europe+Africa
        showland=True,
        landcolor='rgb(0, 121, 107)',
        showocean=True,
        oceancolor='rgb(178, 235, 242)',
                # ---- Optional: zoom into Europe + Africa ----
        lonaxis=dict(range=[0, 20]),         # west to east
        lataxis=dict(range=[45, 60]),         # south to north
)


fig2.update_geos(fitbounds=False) # prevent Plotly from auto-resizing the map

# Save the plot as HTML and PNG
fig2.write_html(f"adventskalender_schiller/schillerinnen_blume_europe.html")
fig2.write_image(f"adventskalender_schiller/schillerinnen_blume_europe.png", scale=4)

fig2.show()
