import pandas as pd
import numpy as np
from datetime import datetime,timedelta
import dash
from dash import dcc
from dash import html
import plotly.graph_objs as go
import dash_bootstrap_components as dbc
from dash.dependencies import Input, Output, State
import pickle
import plotly.express as px
import pycountry_convert as pc


df = pd.read_csv('outer.csv')
data = {'FIGI': len(df['Open_FIGIticker'].dropna()), 'IB': len(df['IB-ISIN'].dropna()),'EOD': len(df['EOD-Symbol'].dropna())}
dff = pd.DataFrame(data.items(), columns=['Source', 'Number'])
inds=df['IB-industry']
Industrial=[]
Communications = []
Consumer_Noncyclical =[]
Energy=[]
Financial=[]
Consumer_Cyclical=[]
Basic_Materials = []
Technology=[]
Diversified=[]
Utilities = []
Government = []
Empty = []
for i in range(len(inds)):
    if inds[i] == 'Industrial':
        Industrial.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Communications':
        Communications.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Consumer, Non-cyclical':
        Consumer_Noncyclical.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Energy':
        Energy.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Financial':
        Financial.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Consumer, Cyclical':
        Consumer_Cyclical.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Basic Materials':
        Basic_Materials.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Technology':
        Technology.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Diversified':
        Diversified.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Utilities':
        Utilities.append(inds[i])
for i in range(len(inds)):
    if inds[i] == 'Government':
        Government.append(inds[i])

data_ind={'Industrial':len(Industrial),
          'Communications':len(Communications),
          'Consumer_Noncyclical':len(Consumer_Noncyclical),
          'Energy':len(Energy),
          'Financial':len(Financial),
          'Consumer_Cyclical':len(Consumer_Cyclical),
          'Basic_Materials':len(Basic_Materials) ,
          'Technology':len(Technology),
          'Diversified':len(Diversified),
          'Utilities':len(Utilities),
          'Government':len(Government),
          'Empty':(len(df)-df['IB-industry'].count())}
        
dff1 = pd.DataFrame(data_ind.items(), columns=['Industry', 'Number'])
new = df[['Open_FIGIsecurityType2','EOD-Type']]
ty = new['Open_FIGIsecurityType2']
Common = []
Fund = []
Preferred = []

for i in range(len(ty)):
    if ty[i] == 'Common Stock':
        Common.append(ty[i])
    if ty[i] == 'Depositary Receipt':
        Common.append(ty[i])
    if ty[i] == 'REIT':
        Fund.append(ty[i])
    if ty[i] == 'Mutual Fund':
        Fund.append(ty[i])
    if ty[i] == 'Preferred Stock':
        Preferred.append(ty[i])
    if ty[i] == 'Partnership Shares':
        Common.append(ty[i])
    if ty[i] =='Unit':
        Common.append(ty[i])
    if ty[i] ==  'Warrant':
        Common.append(ty[i])
    if ty[i] == 'Equity':
        Common.append(ty[i])
    if ty[i] == 'Corp':
        Common.append(ty[i])
    if ty[i] ==  'Preference':
        Preferred.append(ty[i])
   
ty1 = new['EOD-Type']
Common1 = []
Fund1 = []
Preferred1 = []

for i in range(len(ty1)):
    if ty1[i] == 'Common Stock':
        Common1.append(ty[i])
    if ty1[i] == 'FUND':
        Fund1.append(ty[i])
    if ty1[i] == 'ETF':
        Fund1.append(ty[i])
    if ty1[i] == 'Preferred Stock':
        Preferred1.append(ty[i])
    if ty1[i] == 'USA':
        Common1.append(ty[i])
EODt = new['EOD-Type']
FIGIt=new['Open_FIGIsecurityType2']
typedata = {len(Common):len(Common1),len(Fund):len(Fund1),len(Preferred):len(Preferred1),(len(FIGIt)-FIGIt.count()):(len(EODt)-EODt.count())}
dff2 = pd.DataFrame(typedata.items(), columns=['FIGI', 'EOD'], index = ['Common Stock','Fund','Preferred Stock','Missing'])
dff2 = dff2.rename_axis('Asset type',axis=1).rename_axis('TYPE',axis=0)
dff3 = dff2.stack().reset_index()
dff3=dff3.rename(columns={0:'NUMBER'})

hhh = df['Open_FIGIexchCode']
hhh1 = df['EOD-Country'].dropna()
hhh2=pd.DataFrame(hhh1).rename(columns = {'index':'Country','Open_FIGIexchCode':'Amount'}).reset_index()
hhh3 = hhh2['EOD-Country']
cll1 = []
cll_wrong1=[]
for i in range(len(hhh3)):
    try:
        cll1.append(pc.country_name_to_country_alpha3(hhh3[i],cn_name_format="default"))
    except KeyError:
        if hhh3[i] == 'UK':
            cll1.append('GBR')
        if hhh3[i] == 'USD':
            cll1.append('USA')
cll1=pd.DataFrame(cll1,columns ={'Country'})
cll1 = cll1['Country'].value_counts()
cll1 = pd.DataFrame(cll1).reset_index()
fig = px.scatter_geo(cll1, locations = 'index',color = 'index',
                     size = 'Country',
                     projection="natural earth")

hist1 = px.histogram(dff, x='Source', y='Number')
hist2 = px.histogram(dff1,x='Industry',y='Number')


app = dash.Dash(__name__, external_stylesheets=[dbc.themes.DARKLY],
               meta_tags=[{'name':'viewport',
                         'content':'width=device-width, initial-scale=1.0'}])
server = app.server
app.layout = dbc.Container([
    dbc.Row([
        dbc.Col(html.H1("Type2 Dashboard",
                       className='text-center text-primary, mb-3'),
               width=12)
    ]),
    dbc.Row([
        dbc.Col([
            html.H1('Data Source Gap',
                   className='text-center text-primary, mb-3'),
            dcc.Graph(id='my-bar1', figure=hist1)
            
        ], width={'size':6,'offset':0}),
        dbc.Col([
            html.H1('Industry Distribution',
                  className='text-center text-primary, mb-3'),
            dcc.Graph(id='my-bar2', figure=hist2)
            ], width={'size':6,'offset':0}),
            
        ]),
    dbc.Row([
        dbc.Col([html.H1('Asste type',
                  className='text-center text-primary, mb-3'),
                 dcc.Graph(id='my-hist3', figure= px.bar(dff3, x='TYPE',y = "NUMBER",color = 'Asset type'))
                 ], width={'size': 12}),
            
        ]),
    
    dbc.Row([
        html.H1("EOD World Map"),
        dcc.Graph(figure=fig)
    ])
   
    ])



if __name__ == '__main__':
    app.run_server(debug=True, port=8000)
