
Problem statement: Scrap the data from Techolution Careers website and store the data according to the date of posting(Most old first) as a DataFrame in CSV.

Website URL: https://techolution.app.param.ai/jobs/

To solve this problem the steps were:<br>
1) Opened the website URL <br>
2)While inspecting the website I found out that there were three request sent by the website one of them was giving query on job type description and location<br> 
3)We copied this link it was https://techolution.app.param.ai/api/career/get_job/?query=&locations=&category=&job_types= we also got the content type it was json<br>
4)To extract information from this we used requests python package<br>
5)We loaded the information in json format using json.loads



```
import requests  
import json, sys, csv
import pandas as pd  

r = requests.get('https://techolution.app.param.ai/api/career/get_job/?query=&locations=&category=&job_types=')

j = json.loads(r.content.decode('UTF-8'))

```

We saved the json file as data_file.json


```
with open("data_file.json", "w") as write_file:
    json.dump(j, write_file)
```


```
for row in j:
    print(row)
```

    fil_locations
    data
    fil_job_types
    fil_category
    query_str
    total_jobs
    

In JSON file we found six keys, as we can see above, we observed the data and found that the data key will be sufficient to give all information related to jobs.<br>
The structure of data in json file is:<br>
data<br>
&nbsp;&nbsp;&nbsp;&nbsp;categories<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;jobs<br>
Inside the jobs we can find the informations related to the job we store this in a list arr and also the location is in array so we convert it into string<br>





```
arr=[]
locations = []
for i in j['data']:
    for obj in j['data'][i]['jobs']:
        locations = obj['locations']
        obj['locations'] = locations[0]
        arr.append(obj)
```

We found the columns and saved in obj list and created a dataframe using arr list and columns were in obj list


```
obj=[]
for col in j['data'][i]['jobs'][0]:
        obj.append(col)
df=pd.DataFrame(arr, columns=obj)

                
```

We need to sort the list using the posting date so first we convert the date created_at using pandas to_datetime and then sort the data frame on this column


```
df['created_at'] =  pd.to_datetime(df['created_at'])
```


```
df=df.sort_values('created_at')
```


```
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>title</th>
      <th>req_id</th>
      <th>slug</th>
      <th>created_at</th>
      <th>locations</th>
      <th>description</th>
      <th>job_type</th>
      <th>min_exp</th>
      <th>max_exp</th>
      <th>added_by</th>
      <th>added_by_email</th>
      <th>category</th>
      <th>business_unit_name</th>
      <th>organization_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20</th>
      <td>a933f8e2-dd5d-4a82-ab10-1209634d31c7</td>
      <td>Engineering Lead</td>
      <td>1861</td>
      <td>engineering-lead</td>
      <td>2019-02-08T13:11:49.966886Z</td>
      <td>mauritius</td>
      <td>&lt;p&gt;&lt;strong style="color: rgb(0, 0, 0); backgro...</td>
      <td>Full-time</td>
      <td>84</td>
      <td>216</td>
      <td>Rekha Allam</td>
      <td>rekha.allam@techolution.com</td>
      <td>Information Technology</td>
      <td>Cloud Automation - Mauritius</td>
      <td>Techolution Mauritius</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4e17f47b-7916-411a-b0cd-90d5eeb6346f</td>
      <td>DevOps Architect</td>
      <td>1873</td>
      <td>devops-architect</td>
      <td>2019-02-11T12:00:25.061831Z</td>
      <td>Hyderabad</td>
      <td>&lt;p&gt;&lt;span style="color: rgb(0, 0, 0); backgroun...</td>
      <td>Full-time</td>
      <td>60</td>
      <td>180</td>
      <td>Nikhil Shekhar</td>
      <td>nick.shekhar@techolution.com</td>
      <td>Information Technology</td>
      <td>Cloud Automation - India</td>
      <td>Techolution Pvt Ltd</td>
    </tr>
    <tr>
      <th>26</th>
      <td>4e641217-901a-4670-886e-dd2946bf5476</td>
      <td>Machine Learning Engineer</td>
      <td>1898</td>
      <td>machine-learning-engineer</td>
      <td>2019-02-14T16:13:38.000894Z</td>
      <td>Hyderabad</td>
      <td>&lt;p&gt;&lt;strong style="color: rgb(51, 51, 51);"&gt;Tit...</td>
      <td>Full-time</td>
      <td>36</td>
      <td>60</td>
      <td>Madhav Kommineni</td>
      <td>madhav@techolution.com</td>
      <td>Facial recognition</td>
      <td>FaceOpen</td>
      <td>Techolution LLC</td>
    </tr>
    <tr>
      <th>18</th>
      <td>d4847f54-7a0a-44dd-b3a6-b93c7fb3cb7d</td>
      <td>Sr SDET</td>
      <td>1903</td>
      <td>sr-sdet</td>
      <td>2019-02-14T16:38:50.411436Z</td>
      <td>New York</td>
      <td>&lt;p&gt;Techolution is a premier cloud, user interf...</td>
      <td>Full-time</td>
      <td>36</td>
      <td>120</td>
      <td>Satish Kumar</td>
      <td>satish.kumar@techolution.com</td>
      <td>Information Technology</td>
      <td>UI/UX Modernization - US</td>
      <td>Techolution LLC</td>
    </tr>
    <tr>
      <th>17</th>
      <td>c4daf0d7-f86f-4d86-b62e-ab9117ba2800</td>
      <td>OSS DevOps Engineer</td>
      <td>1905</td>
      <td>oss-devops-engineer</td>
      <td>2019-02-14T16:55:20.844881Z</td>
      <td>Hyderabad</td>
      <td>&lt;p&gt;&lt;strong&gt;Title&amp;nbsp;: OSS DevOps Engineer&lt;/s...</td>
      <td>Full-time</td>
      <td>72</td>
      <td>144</td>
      <td>Pavan Kumar</td>
      <td>pavan.thirunahari@techolution.com</td>
      <td>Information Technology</td>
      <td>Cloud Automation - India</td>
      <td>Techolution Pvt Ltd</td>
    </tr>
  </tbody>
</table>
</div>




```
df.to_csv("jobfile.csv",encoding='utf-8', index=False)
```

Saving the file as jobfile.csv. This is the required file
