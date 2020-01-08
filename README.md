# script-python Data Viz
dev Python
#!/usr/bin/env python
# coding: utf-8
from bqplot import pyplot as plt
import ipyvuetify as v
import ipywidgets as widgets
import numpy as np
import pandas as pd

df = pd.read_csv(r'C:\Users\khalil\Desktop\test\dashbording.csv', error_bad_lines=False,sep=';')
x=df.A
y=df.B

fig_hist = plt.figure(title='Histogram')

hist = plt.hist(y,colors=['green'] ,bins=25)

# slider#########################

slider = v.Slider(vertical=False,thumb_label='always',label ="Data zoom",ticks="always",step="10",tick_size="4",color="green" ,class_="px-4", v_model=30,append_icon="zoom_out",prepend_icon="zoom_in",track_color="red")

widgets.link((slider, 'v_model'), (hist, 'bins'))

fig_lines = plt.figure( title=' l`allure de courbe ')

lines = plt.plot(x, y,colors=['orange'])
# even handling

selector = plt.brush_int_selector()

def update_range(*ignore):

    if selector.selected is not None and len(selector.selected) == 2:

        xmin, xmax = selector.selected

        mask = (x > xmin) & (x < xmax)

        hist.sample = y[mask]

selector.observe(update_range, 'selected')        

# control for linestyle

line_styles = ['dashed', 'solid', 'dotted','dash_dotted']

widget_line_styles = v.Select(items=line_styles,chips=True,solo=True,attach=True ,label=' style de courbe', v_model=line_styles[0])

widgets.link((widget_line_styles, 'v_model'), (lines, 'line_style'));



display(

    v.Layout(pa_4=True, _metadata={'mount_id': 'content-nav'}, column=True, children=[slider, widget_line_styles])

)  # use display to support the default template 

fig_hist.layout.width = 'auto'

fig_hist.layout.height = 'auto'

fig_hist.layout.min_height = '300px' # so it still shows nicely in the notebook

fig_lines.layout.width = 'auto'

fig_lines.layout.height = 'auto'

fig_lines.layout.min_height = '300px' # so it still shows nicely in the notebook

content_main =  v.Layout(

                    _metadata={'mount_id': 'content-main'},

                    row=True, wrap=True, align_center=True, children=[

                    v.Flex(xs12=True, lg6=True, children=[

                        fig_hist

                    ]),

                    v.Flex(xs12=True, lg6=True, children=[

                        fig_lines

                    ]),

                ])

display(content_main)
