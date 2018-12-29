
![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)


![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)


![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)





### _Jupyter Notebooks_

Jupyter Notebooks, formerly iPython, is typically referred to as Jupyter.  With the beta release of Jupyter Labs, an IDE version, I will refer to just the notebook as JN to avoid confusion for future readers.  

JN is a combination of Markdown with executable code in cells.  This example will repeat much of what was achieved above using AMLS. The text below is HTML, but the complete JN can be cloned from my GitHub repo.

For a good description of JN on Azure, Scott Hanselman has a nice YouTube video on JN many features - https://www.youtube.com/watch?v=JWEhns28Cr4

#### Azure Storage SDK
Surprisingly, the Azure Storage SDK was not pre-installed in Azure hosted JN.  Use pip or Annaconda (package managers) to install the azure storage sdk.  Once installed, this shell/bash command can be eliminated for futre runs in this project.


```python
!pip install azure-storage
```

#### Import Data
Azure Table data is imported using the Azure Storage /Table Storage SDK and the data is shaped using Pandas to match the AMLS dataset.  I would typically import my credentials which are needed for Azure access, but for this public notebook, I use a Shared Access Signature token to access the data in read/query only format.  The token has an expiration date which can be refresehed on request.  Alternatively, I have a small sample dataset, in my DuckpinA repo on GitHub. See below in _Data Alternatives_ on how to access web based data.
* Imports and constants


```python
# import credentials
from azure.storage.table import TableService
import numpy as np
import pandas as pd

container_name = 'duckpinjson'
account_name = container_name
# account_key = credentials.STORAGE_ACCOUNT_KEY
table_sas_token = 'sp=r&sv=2017-04-17&tn=pindata&sig=0vcZimSsLnyez5Kw5tOt6JASJ4MrVCnz13cOySaxtJQ%3D&se=2019-01-29T11%3A54%3A17Z'
table_name = 'pindata'
```

* Get Table Data and convert to Pandas Dataframe 


```python
# Two fancy Python functions to iterate Azure Tables and persist the result in a Pandas dataframe  
def get_dataframe_from_table_storage_table(table_service, filter_query):
    """ Create a dataframe from table storage data """
    return pd.DataFrame(get_data_from_table_storage_table(table_service,
                                                          filter_query))
def get_data_from_table_storage_table(table_service, filter_query):
    """ Retrieve data from Table Storage """
    for record in table_service.query_entities(
        table_name, filter=filter_query
    ):
        yield record
        
# table_service = TableService(
#     account_name=account_name, account_key=account_key)
table_service = TableService(
    account_name=account_name, sas_token=table_sas_token)
fq = "PartitionKey eq 'Lane 4'"
df = get_dataframe_from_table_storage_table(table_service=table_service,filter_query=fq)
df.head(5) #Show first five records in the dataframe
```




<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PartitionKey</th>
      <th>RowKey</th>
      <th>Timestamp</th>
      <th>beginingPinCount</th>
      <th>endingPinCount</th>
      <th>etag</th>
      <th>res</th>
      <th>x0</th>
      <th>x1</th>
      <th>x2</th>
      <th>x3</th>
      <th>x4</th>
      <th>x5</th>
      <th>y0</th>
      <th>y1</th>
      <th>y2</th>
      <th>y3</th>
      <th>y4</th>
      <th>y5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lane 4</td>
      <td>20181019044781</td>
      <td>2018-10-19 23:23:18.239444+00:00</td>
      <td>1023</td>
      <td>876</td>
      <td>W/"datetime'2018-10-19T23%3A23%3A18.2394448Z'"</td>
      <td>NaN</td>
      <td>763</td>
      <td>760</td>
      <td>754</td>
      <td>754</td>
      <td>755</td>
      <td>NaN</td>
      <td>388</td>
      <td>319</td>
      <td>229</td>
      <td>158</td>
      <td>97</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lane 4</td>
      <td>20181019047495</td>
      <td>2018-10-19 23:01:57.236804+00:00</td>
      <td>1023</td>
      <td>947</td>
      <td>W/"datetime'2018-10-19T23%3A01%3A57.2368045Z'"</td>
      <td>NaN</td>
      <td>225</td>
      <td>244</td>
      <td>259</td>
      <td>274</td>
      <td>290</td>
      <td>NaN</td>
      <td>397</td>
      <td>363</td>
      <td>305</td>
      <td>245</td>
      <td>186</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lane 4</td>
      <td>20181019054037</td>
      <td>2018-10-19 23:01:48.233574+00:00</td>
      <td>1023</td>
      <td>930</td>
      <td>W/"datetime'2018-10-19T23%3A01%3A48.2335741Z'"</td>
      <td>NaN</td>
      <td>1006</td>
      <td>974</td>
      <td>948</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>314</td>
      <td>173</td>
      <td>61</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lane 4</td>
      <td>20181019059314</td>
      <td>2018-10-19 22:59:22.238739+00:00</td>
      <td>1023</td>
      <td>1007</td>
      <td>W/"datetime'2018-10-19T22%3A59%3A22.2387392Z'"</td>
      <td>NaN</td>
      <td>870</td>
      <td>861</td>
      <td>853</td>
      <td>849</td>
      <td>840</td>
      <td>NaN</td>
      <td>384</td>
      <td>271</td>
      <td>166</td>
      <td>80</td>
      <td>15</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Lane 4</td>
      <td>20181019075651</td>
      <td>2018-10-19 23:00:39.253765+00:00</td>
      <td>1023</td>
      <td>661</td>
      <td>W/"datetime'2018-10-19T23%3A00%3A39.2537652Z'"</td>
      <td>NaN</td>
      <td>358</td>
      <td>373</td>
      <td>383</td>
      <td>392</td>
      <td>400</td>
      <td>NaN</td>
      <td>421</td>
      <td>389</td>
      <td>349</td>
      <td>289</td>
      <td>232</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail(5) #Show last five records in the dataframe
```




<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PartitionKey</th>
      <th>RowKey</th>
      <th>Timestamp</th>
      <th>beginingPinCount</th>
      <th>endingPinCount</th>
      <th>etag</th>
      <th>res</th>
      <th>x0</th>
      <th>x1</th>
      <th>x2</th>
      <th>x3</th>
      <th>x4</th>
      <th>x5</th>
      <th>y0</th>
      <th>y1</th>
      <th>y2</th>
      <th>y3</th>
      <th>y4</th>
      <th>y5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3652</th>
      <td>Lane 4</td>
      <td>20181221856778</td>
      <td>2018-12-22 03:48:29.726076+00:00</td>
      <td>1023</td>
      <td>643</td>
      <td>W/"datetime'2018-12-22T03%3A48%3A29.7260768Z'"</td>
      <td>1440</td>
      <td>399</td>
      <td>413</td>
      <td>425</td>
      <td>436</td>
      <td>446</td>
      <td>456</td>
      <td>392</td>
      <td>337</td>
      <td>256</td>
      <td>185</td>
      <td>122</td>
      <td>68</td>
    </tr>
    <tr>
      <th>3653</th>
      <td>Lane 4</td>
      <td>20181221916380</td>
      <td>2018-12-22 03:49:08.792957+00:00</td>
      <td>1023</td>
      <td>678</td>
      <td>W/"datetime'2018-12-22T03%3A49%3A08.7929576Z'"</td>
      <td>1440</td>
      <td>199</td>
      <td>240</td>
      <td>275</td>
      <td>306</td>
      <td>335</td>
      <td>360</td>
      <td>398</td>
      <td>330</td>
      <td>243</td>
      <td>167</td>
      <td>101</td>
      <td>41</td>
    </tr>
    <tr>
      <th>3654</th>
      <td>Lane 4</td>
      <td>20181221964908</td>
      <td>2018-12-22 03:46:42.825063+00:00</td>
      <td>1023</td>
      <td>1022</td>
      <td>W/"datetime'2018-12-22T03%3A46%3A42.8250631Z'"</td>
      <td>1440</td>
      <td>1136</td>
      <td>1102</td>
      <td>1075</td>
      <td>1049</td>
      <td>1030</td>
      <td>1016</td>
      <td>395</td>
      <td>325</td>
      <td>231</td>
      <td>154</td>
      <td>88</td>
      <td>31</td>
    </tr>
    <tr>
      <th>3655</th>
      <td>Lane 4</td>
      <td>20181221980028</td>
      <td>2018-12-22 03:51:20.847274+00:00</td>
      <td>1023</td>
      <td>959</td>
      <td>W/"datetime'2018-12-22T03%3A51%3A20.8472743Z'"</td>
      <td>1440</td>
      <td>128</td>
      <td>167</td>
      <td>199</td>
      <td>228</td>
      <td>253</td>
      <td>278</td>
      <td>391</td>
      <td>333</td>
      <td>264</td>
      <td>202</td>
      <td>143</td>
      <td>91</td>
    </tr>
    <tr>
      <th>3656</th>
      <td>Lane 4</td>
      <td>20181221996955</td>
      <td>2018-12-22 03:51:04.859007+00:00</td>
      <td>1023</td>
      <td>951</td>
      <td>W/"datetime'2018-12-22T03%3A51%3A04.8590072Z'"</td>
      <td>1440</td>
      <td>216</td>
      <td>250</td>
      <td>271</td>
      <td>291</td>
      <td>307</td>
      <td>325</td>
      <td>415</td>
      <td>370</td>
      <td>297</td>
      <td>225</td>
      <td>159</td>
      <td>104</td>
    </tr>
  </tbody>
</table>
</div>



* Compute the number of pins standing for each decimal value of the endingPinCount


```python
#Function to count the number of 1s in the binary equivalent of a decimal value
# Result returned in a dictionary for all pin configurations
def numUp():
    pcs = {}
    for pinCount in range(0, 1023):
        bits = bin(pinCount)
        count = 0
        # print(bits, str(bits))
        for bit in str(bits[2:len(str(bits))]):
            if bit == '1':
                count = count + 1
            # print(bit, count)
        pcs[pinCount] = count
    return pcs
# Add up column to dataframe
df['up']= df['endingPinCount'].map(numUp())
df.head(5)
```




<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PartitionKey</th>
      <th>RowKey</th>
      <th>Timestamp</th>
      <th>beginingPinCount</th>
      <th>endingPinCount</th>
      <th>etag</th>
      <th>res</th>
      <th>x0</th>
      <th>x1</th>
      <th>x2</th>
      <th>x3</th>
      <th>x4</th>
      <th>x5</th>
      <th>y0</th>
      <th>y1</th>
      <th>y2</th>
      <th>y3</th>
      <th>y4</th>
      <th>y5</th>
      <th>up</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lane 4</td>
      <td>20181019044781</td>
      <td>2018-10-19 23:23:18.239444+00:00</td>
      <td>1023</td>
      <td>876</td>
      <td>W/"datetime'2018-10-19T23%3A23%3A18.2394448Z'"</td>
      <td>NaN</td>
      <td>763</td>
      <td>760</td>
      <td>754</td>
      <td>754</td>
      <td>755</td>
      <td>NaN</td>
      <td>388</td>
      <td>319</td>
      <td>229</td>
      <td>158</td>
      <td>97</td>
      <td>NaN</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lane 4</td>
      <td>20181019047495</td>
      <td>2018-10-19 23:01:57.236804+00:00</td>
      <td>1023</td>
      <td>947</td>
      <td>W/"datetime'2018-10-19T23%3A01%3A57.2368045Z'"</td>
      <td>NaN</td>
      <td>225</td>
      <td>244</td>
      <td>259</td>
      <td>274</td>
      <td>290</td>
      <td>NaN</td>
      <td>397</td>
      <td>363</td>
      <td>305</td>
      <td>245</td>
      <td>186</td>
      <td>NaN</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lane 4</td>
      <td>20181019054037</td>
      <td>2018-10-19 23:01:48.233574+00:00</td>
      <td>1023</td>
      <td>930</td>
      <td>W/"datetime'2018-10-19T23%3A01%3A48.2335741Z'"</td>
      <td>NaN</td>
      <td>1006</td>
      <td>974</td>
      <td>948</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>314</td>
      <td>173</td>
      <td>61</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lane 4</td>
      <td>20181019059314</td>
      <td>2018-10-19 22:59:22.238739+00:00</td>
      <td>1023</td>
      <td>1007</td>
      <td>W/"datetime'2018-10-19T22%3A59%3A22.2387392Z'"</td>
      <td>NaN</td>
      <td>870</td>
      <td>861</td>
      <td>853</td>
      <td>849</td>
      <td>840</td>
      <td>NaN</td>
      <td>384</td>
      <td>271</td>
      <td>166</td>
      <td>80</td>
      <td>15</td>
      <td>NaN</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Lane 4</td>
      <td>20181019075651</td>
      <td>2018-10-19 23:00:39.253765+00:00</td>
      <td>1023</td>
      <td>661</td>
      <td>W/"datetime'2018-10-19T23%3A00%3A39.2537652Z'"</td>
      <td>NaN</td>
      <td>358</td>
      <td>373</td>
      <td>383</td>
      <td>392</td>
      <td>400</td>
      <td>NaN</td>
      <td>421</td>
      <td>389</td>
      <td>349</td>
      <td>289</td>
      <td>232</td>
      <td>NaN</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



* Shape with Pandas and exclude unwanted columns.

Use Pandas to generate the dataset created in AMLS with SQLite.  The sql was:
```sql
select endingPinCount as epc,
        up,y1, 
        SQRT(SQUARE(x1-x0)+SQUARE(y1-y0)) as v1,
        SQRT(SQUARE(x2-x1)+SQUARE(y2-y1)) as v2,
        ATAN(CAST((x2-x1) as float)/CAST((y2-y1) as float)) as theta,
        CAST(x1 as float) as x
from t1
WHERE y2 IS NOT NULL;
```

If you are following closely, you will see that I combined AMLS sequences to get theta and x to absolute values abst and absx, respectively.


```python
df1 = df
df1['v1'] = np.sqrt((df['x1'].astype(float)-df['x0'].astype(float))**2 + (df['y1'].astype(float)-df['y0'].astype(float))**2)
df1['v2'] = np.sqrt((df['x2'].astype(float)-df['x1'].astype(float))**2 + (df['y2'].astype(float)-df['y1'].astype(float))**2)
df1['abst'] = np.arctan((df['x2'].astype(float)-df['x1'].astype(float))/ (df['y2'].astype(float)-df['y1'].astype(float))).abs()
df1 = df1[df1["y1"].notnull()]
df1['absx'] = (df1['x1'].astype(float)-562).abs()
df1= df1.drop(['PartitionKey','RowKey','Timestamp','beginingPinCount','etag', 'res',
               'x0','y0','x1','y1','x2','y2','x3','y3','x4','y4','x5','y5'], axis=1)
df1.head(5)
```




<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>endingPinCount</th>
      <th>up</th>
      <th>v1</th>
      <th>v2</th>
      <th>abst</th>
      <th>absx</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>876</td>
      <td>6</td>
      <td>69.065187</td>
      <td>90.199778</td>
      <td>0.066568</td>
      <td>198.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>947</td>
      <td>7</td>
      <td>38.948684</td>
      <td>59.908263</td>
      <td>0.253076</td>
      <td>318.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>930</td>
      <td>5</td>
      <td>144.585615</td>
      <td>114.978259</td>
      <td>0.228103</td>
      <td>412.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1007</td>
      <td>9</td>
      <td>113.357840</td>
      <td>105.304321</td>
      <td>0.076044</td>
      <td>299.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>661</td>
      <td>5</td>
      <td>35.341194</td>
      <td>41.231056</td>
      <td>0.244979</td>
      <td>189.0</td>
    </tr>
  </tbody>
</table>
</div>



Similar to AMLS, I plot the data but this time will use theta to see if ball angle and presumably spin affect pin results.  A shell/bash command is needed to redirect the plot output to JN.


```python
%matplotlib inline
```


```python
import matplotlib.pyplot as plt

fig =plt.figure() 
ax = fig.gca()
ax.cla()
ax.set_xlim(-1, 10)
ax.set_ylim(-1,1)
ax.set_title('Does ball angle/spin matter?')
plt.ylabel('mean angley and deviation')
plt.xlabel('# of pins remaining after roll')
for x in range(0,10):
    #pandas syntax to group data by the number of up pins.
    endcountGroup = df1.loc[df1['up'] == x]
    #pandas describe()generates a table of stats for the dataframe
    g = endcountGroup.describe()
    ax.errorbar(x, g.iloc[1][4], g.iloc[2][4], marker='^')
# fig.savefig ("duckpin.png")


```

![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)

### _Alternative Data Souces_
#### Github Repo
In my GitHub repo, I have a dataset (pindatacsv.csv) that can be used for this notebook in lieu of the full dataset using the SAS token above.

I also have a dataset (results01.csv) used for testing.  It is 3400+ records of csv data in the format:
`epc,up,y1,v1,v2,theta,x`

To access that data, use curl and the url for the RAW option for that file in my DuckpinA repo.

```python
!curl https://raw.githubusercontent.com/cliffeby/DuckpinA/master/pindatacsv.csv -o pincsv.csv
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  323k  100  323k    0     0  1403k      0 --:--:-- --:--:-- --:--:-- 1411k


Use numpy or pandas to read the file.  In this case, I used numpy and sorted the records prior to creating the dataframe 


```python
csv = np.recfromcsv('pincsv.csv')
csv = np.sort(csv)
df3 = pd.DataFrame(csv)
df3.head(5)
```




<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>partitionkey</th>
      <th>rowkey</th>
      <th>timestamp</th>
      <th>beginingpincount</th>
      <th>endingpincount</th>
      <th>x0</th>
      <th>y0</th>
      <th>x1</th>
      <th>y1</th>
      <th>x2</th>
      <th>y2</th>
      <th>x3</th>
      <th>y3</th>
      <th>x4</th>
      <th>y4</th>
      <th>up</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b'Lane 4'</td>
      <td>20181019044781</td>
      <td>b'10/19/2018 11:23:18 PM'</td>
      <td>1023</td>
      <td>876</td>
      <td>763</td>
      <td>388</td>
      <td>b'760'</td>
      <td>b'319'</td>
      <td>b'754'</td>
      <td>b'229'</td>
      <td>b'754'</td>
      <td>b'158'</td>
      <td>b'755'</td>
      <td>b'97'</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b'Lane 4'</td>
      <td>20181019047495</td>
      <td>b'10/19/2018 11:01:57 PM'</td>
      <td>1023</td>
      <td>947</td>
      <td>225</td>
      <td>397</td>
      <td>b'244'</td>
      <td>b'363'</td>
      <td>b'259'</td>
      <td>b'305'</td>
      <td>b'274'</td>
      <td>b'245'</td>
      <td>b'290'</td>
      <td>b'186'</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>b'Lane 4'</td>
      <td>20181019054037</td>
      <td>b'10/19/2018 11:01:48 PM'</td>
      <td>1023</td>
      <td>930</td>
      <td>1006</td>
      <td>314</td>
      <td>b'974'</td>
      <td>b'173'</td>
      <td>b'948'</td>
      <td>b'61'</td>
      <td>b'""'</td>
      <td>b'""'</td>
      <td>b'""'</td>
      <td>b'""'</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b'Lane 4'</td>
      <td>20181019059314</td>
      <td>b'10/19/2018 10:59:22 PM'</td>
      <td>1023</td>
      <td>1007</td>
      <td>870</td>
      <td>384</td>
      <td>b'861'</td>
      <td>b'271'</td>
      <td>b'853'</td>
      <td>b'166'</td>
      <td>b'849'</td>
      <td>b'80'</td>
      <td>b'840'</td>
      <td>b'15'</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b'Lane 4'</td>
      <td>20181019075651</td>
      <td>b'10/19/2018 11:00:39 PM'</td>
      <td>1023</td>
      <td>661</td>
      <td>358</td>
      <td>421</td>
      <td>b'373'</td>
      <td>b'389'</td>
      <td>b'383'</td>
      <td>b'349'</td>
      <td>b'392'</td>
      <td>b'289'</td>
      <td>b'400'</td>
      <td>b'232'</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



### Creating the SAS token
The following shows how to create  a _Shared Access Signature_ to access the Azure table data in read/query only format.  Since the my credentials are needed to create the token, I have moved the code to a Markdown cell. To create the sas_token:
```python
from azure.storage.table.models import TablePermissions
from datetime import datetime, timedelta

table_service = TableService(
    account_name=account_name, account_key = account_key)
table_sas_token = table_service.generate_table_shared_access_signature(
    table_name, 
    TablePermissions.QUERY, 
    datetime.utcnow() + timedelta(hours=800))

table_sas_token
```
