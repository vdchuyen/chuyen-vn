---
layout: post
title: Build a Speedtest Page with python 
date: 2021-06-29 09:07:02 +0530
categories: build
---
This [page](https://sayno.cloud/s/) will show how speed improvement for my local ISP at home. The data began from 2019, missing some 2020 because of powerdown and continue in end of Jun 2021.

The code is rather simple, just a crontab traffic and exporting graph by wonderful matplotlib.

The traffic aggr crontab is something like this from reddit:
{% highlight python %}
#!/usr/bin/python3

import pandas as pd
import sqlite3
import speedtest
from lib.graph import graph_output

servers = []
# set this number to use threads
threads = None

s = speedtest.Speedtest()
s.get_best_server()
s.download(threads=threads)
s.upload(threads=threads,pre_allocate=False)
results = s.results.dict()

df = pd.DataFrame({'timestamp':[results['timestamp']],'download':[results['download']/1000000],'upload':[results['upload']/1000000],'ping':[results['ping']]})
con = sqlite3.connect('speed.db')
df.to_sql('speed',con,if_exists='append')
con.close()

graph_output()
{% endhighlight %}
And graph_output() function like this, put lib lib/graph.py, the query limit default is 10 but can be adjusted depend on your own.
{% highlight python %}
#!/usr/bin/python3

import sqlite3
import pandas as pd
import matplotlib
matplotlib.use('AGG')
import matplotlib.pyplot as plt

def graph_output():
        con = sqlite3.connect('speed.db')
        # per day
        df = pd.read_sql('SELECT * FROM (SELECT DATE(timestamp,"localtime") AS perday,download,upload,ping FROM tests  ORDER BY perday DESC limit 10) ORDER BY perday ASC', con)

        # set default plot size before savefig
        plt.rcParams['figure.figsize'] = [12, 9]
        ax1 = plt.subplots()

        ax1.plot_date(df['perday'],df['download'],label='download',linestyle = 'dotted')
        ax1.plot_date(df['perday'],df['upload'],label='upload',linestyle = 'dotted')
        ax1.plot_date(df['perday'],df['ping'],label='latency',linestyle = 'dotted',color='g')
        ax1.set_xlabel('Days')
        ax1.set_ylabel('Speed(Mbps)',color='b')
        ax1.legend(loc='best')

        ax2 = ax1.twinx()
        ax2.plot_date(df['perday'],df['ping'],label='latency',linestyle = 'dotted',color='g')
        ax2.set_ylabel('latency(ms)',color='g')

        plt.title('Viettel FTTH Internet speed test using speedtest')
        plt.savefig('speed.png',dpi=150)
{% endhighlight %}
This should be put in env and requires these modules in requirements.txt
{% highlight bash %}
cycler==0.10.0
kiwisolver==1.3.1
matplotlib==3.4.2
numpy==1.21.0
pandas==1.2.5
Pillow==8.2.0
pyparsing==2.4.7
python-dateutil==2.8.1
pytz==2021.1
six==1.16.0
speedtest-cli==2.1.3
{% endhighlight %}
