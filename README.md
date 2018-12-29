
![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)


![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)


![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)


![png](https://user-images.githubusercontent.com/1431998/50466200-17ae6800-096a-11e9-8460-1997debbc14b.png)

### _Alternative Data Souces_
#### Github Repo
In my GitHub repo, I have a dataset (results01.csv) used for testing.  It is 3400+ records of csv data in the format:
`epc,up,y1,v1,v2,theta,x`

To access that data, use curl and the url for the RAW option for that file in repo DuckpinA.

```python
!curl https://raw.githubusercontent.com/cliffeby/DuckpinA/master/results01.csv -o pincsv.csv
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  206k  100  206k    0     0   889k      0 --:--:-- --:--:-- --:--:--  889k


Use numpy or pandas to read the file.  In this case, I used numpy and sorted the records before creating the pandas dataframe. 


```python
csv = np.recfromcsv('pincsv.csv')
csv = np.sort(csv)
df = pd.DataFrame(csv)
df = df[df["v1"].notnull()]
df = df.fillna(0)
df = df.drop(['y1'], axis = 1)
df.head(5)
```




<div>
<table class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>epc</th>
      <th>up</th>
      <th>v1</th>
      <th>v2</th>
      <th>theta</th>
      <th>x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>0</td>
      <td>0</td>
      <td>169.705627</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>545</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0</td>
      <td>0</td>
      <td>171.143215</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>538</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0</td>
      <td>0</td>
      <td>186.077941</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>550</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0</td>
      <td>0</td>
      <td>162.098735</td>
      <td>112.178429</td>
      <td>-0.197396</td>
      <td>501</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0</td>
      <td>0</td>
      <td>165.737745</td>
      <td>114.236597</td>
      <td>-0.238609</td>
      <td>529</td>
    </tr>
  </tbody>
</table>
</div>



The second option is to use a _Shared Access Signature_ to access the data in read only format.  To create the sas_token:


```python
from azure.storage.table.models import TablePermissions
from datetime import datetime, timedelta

table_sas_token = table_service.generate_table_shared_access_signature(
    table_name, 
    TablePermissions.QUERY, 
    datetime.utcnow() + timedelta(hours=800))

table_sas_token
```




    'sp=r&sv=2017-04-17&tn=pindata&sig=0vcZimSsLnyez5Kw5tOt6JASJ4MrVCnz13cOySaxtJQ%3D&se=2019-01-29T11%3A54%3A17Z'



And to use the token


```python
# Create a service and use the SAS 
table_service = TableService(
    account_name=account_name, sas_token=table_sas_token)
fq = "PartitionKey eq 'Lane 4'"
df2 = get_dataframe_from_table_storage_table(table_service=table_service,
                                                 filter_query=fq)
df2.head(3)
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
  </tbody>
</table>
</div>


