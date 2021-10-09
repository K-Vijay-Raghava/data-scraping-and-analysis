import bs4
import requests
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from IPython.display import display

def get_basketball_stats(link='https://en.wikipedia.org/wiki/Michael_Jordan'):
    response = requests.get(link)
    soup = bs4.BeautifulSoup(response.text, 'html.parser')
    table = soup.find(class_='wikitable sortable')
    headers = table.tr
    titles = headers.find_all('abbr')
    data = {title['title']: [] for title in titles}
    for row in table.find_all('tr')[1:]:
        for key, a in zip(data.keys(),row.find_all('td')[2:]):
            data[key].append(''.join(c for c in a.text if (c.isdigit() or c == '.')))
        Min = min([len(x) for x in data.values()])
        for key in data.keys():
            data[key] = list(map(lambda x: float(x), data[key][:Min]))
    return data
links = ['https://en.wikipedia.org/wiki/Michael_Jordan'\
       ,'https://en.wikipedia.org/wiki/Kobe_Bryant'\
      ,'https://en.wikipedia.org/wiki/LeBron_James'\,'https://en.wikipedia.org/wiki/Stephen_Curry','https://en.wikipedia.org/wiki/Pau_Gasol','https://en.wikipedia.org/wiki/Kevin_Durant','https://en.wikipedia.org/wiki/Chris_Paul','https://en.wikipedia.org/wiki/Kyrie_Irving','https://en.wikipedia.org/wiki/Tim_Duncan','https://en.wikipedia.org/wiki/Kawhi_Leonard']
names = ['Michael Jordan','Kobe Bryant','Lebron James','Stephen Curry','Pau Gasol','Kevin durant','chris paul','Kyrie Irving','Tim Duncan','khavi leonard']

michael_jordan_dict = get_basketball_stats(links[0])
kobe_bryant_dict = get_basketball_stats(links[1])
lebron_james_dict = get_basketball_stats(links[2])
stephen_curry_dict = get_basketball_stats(links[3])
Pau_Gasol_dict=get_basketball_stats(links[4])
kevin_durant_dict=get_basketball_stats(links[5])
chris_paul_dict=get_basketball_stats(links[6])
kyrie_irving_dict=get_basketball_stats(links[7])
tim_duncan_dict=get_basketball_stats(links[8])
khavi_leonard_dict=get_basketball_stats(links[9])

mj_table = pd.DataFrame(michael_jordan_dict)
kb_table = pd.DataFrame(kobe_bryant_dict)
lj_table = pd.DataFrame(lebron_james_dict)
sc_table = pd.DataFrame(stephen_curry_dict)
pg_table = pd.DataFrame(Pau_Gasol_dict)
kd_table=pd.DataFrame(kevin_durant_dict)
cp_table=pd.DataFrame(chris_paul_dict)
ki_table=pd.DataFrame(kyrie_irving_dict)
td_table=pd.DataFrame(tim_duncan_dict)
kl_table=pd.DataFrame(khavi_leonard_dict)
list_table =[mj_table, kb_table, lj_table, sc_table,pg_table,kd_table,cp_table,ki_table,td_table,kl_table]

i = 0
for name in names:
    print(name)
    display(list_table[i].head())
    i += 1

j = 0
for name in names:
    plt.plot(list_table[j][['Points per game']],label=name)
    plt.legend()
    plt.xlabel('years')
    plt.ylabel('Points per game')

    j += 1

csv_name = 'MJ1.csv'
mj_table.to_csv(csv_name)
