                                                      FINAL PROJECT

The covid-19 has brought a lot of changes to the world. The pandemic started at the end of 2019 and spread all across the world. Every country suffered economically and financially and continues to suffer as new variants of the virus emerge. USA
being the biggest country suffered heavily as people lost their lives, jobs, etc. New York City, being one of the biggest cities in the world, saw a lot of covid cases beginning from March 2020. So, my objective is to find the effect of covid-19
on the number of crimes in NYC. My assumption is that they are negatively correlated and hence, crimes went down during the pandemic in NYC. I believe as people were advised to stay inside their homes and in general, people were scared as the world was witnessing a major pandemic for the first time in almost a century, crimes declined as the covid-19 cases went up.

   For testing the correlation between crimes and covid-19 cases, I have used 3 datasets(.csv files)-"NYPD Complaint Data Historic","NYPD Complaint Data Current Year To Date (1)" and "COVID19_Daily_Counts_of_Cases__Hospitalizations__and_Deaths" from NYC Data website. The first dataset contains the record of all the crimes that happened in NYC till mid 2021. The second dataset has all the crimes recorded in NYC in 2021 till September and third dataset has the daily covid-19 cases, deaths, hospitalizations in NYC till October 2021. 
   
The first step was to extract all the data for crimes that happened in 2020 from the "NYPD Complaint Data Historic" and to eliminate some of the bad data using the the try and except method. I made two csv files for 2020. One having the whole data of 2020 and second one having data from march till december,to test the correlation with covid-19 cases, as covid-19 started in march in NYC. I also filtered data using the strptime method to eliminate rows that do not have a valid date format. I wrote the data row by row in a seperate csv file.

</br>
I used the the following code to achieve this. Row 1 had the date of the crime.
</br>

``` python

for row in reader:
    if n==0:
        writer.writerow(row)
    if n>0:
        date=None
        try:
            date=row[1]
        except:
            continue
        if date is not None:
             dto=None
            try:
                dto=datetime.datetime.strptime(date,"%m/%d/%Y")
            except:
                continue
            if dto is not None:
                if dto>datetime.datetime.strptime('02/29/2020',"%m/%d/%Y") and  dto<datetime.datetime.strptime('01/01/2021',"%m/%d/%Y"):                       
                    writer.writerow(row)
                    
```

Similarly, I created a seperate csv file for 2020 covid cases. The file has date, no. of cases, no. of deaths, no. of people being hospitalized and 7-day moving average of covid-cases for 2020 from march to december.

Next, as my aim is to find the correlation between covid-19 cases and number of crimes in NYC, I had to find the number of crimes for each day that happened in NYC in 2020. I made a seperate csv file having date and number of crimes on that day. I created dictionary and added unique dates as keys and their occurence as the respective value. 

</br>
I used the following code to do this-
</br>

```python

for row in reader:
    if n>0:
        date=row[1]
        if date in d.keys():
            d[date]+=1
        else:
            d[date]=1
    n+=1
    

l1=["Date","Number of crimes"]
writer.writerow(l1)

for k,v in sorted(d.items()):
    l=[k,v]
    writer.writerow(l)
    
```

After getting both, the covid data for 2020 and no. of crimes data for 2020, I combined them into one csv-"2020_Whole_Data"
and found the correlation of number of crimes with covid-19 cases, number of deaths, number of people hospitalized and 7-day moving average. Here is the result-

Type of Covid-19 data statistic |     Correlation coefficient      
-------------------------|--------------------------
Case count               |  -0.654
Hospitalized count       |  -0.670
Death count              |  -0.663
Case count 7-day moving avg.   |  -0.745



As we can see, crime is negatively correlated with all the covid-19 statistics. All have good correlation, with 7-day moving average having the strongest correlation with number of crimes(-.745).This proves that there is a moderate correlation between covid-19 cases and crimes. So, when the cases went up, crime decreased in NYC in 2020. 

Then, I wanted to verify my findings for 2021, as then I would have two covid year's data which will strengthen my claim. So I collated the data for 2021 using the same method that I used for 2020 data. I had 9 months of data for 2021 and combined data was stored in "2021_Whole_Data" and then I checked for correlation with the same statistics. Here is  the result_

Type of Covid-19 data statistic |     Correlation coefficient      
-------------------------|--------------------------
Case count               |  -0.455
Hospitalized count       |  -0.589
Death count              |  -0.654
Case count 7-day moving avg.   |  -0.577


We can see that again there is good correlation between covid-19 statistics and number of crimes in NYC. Crime has a moderate negative correlation with covid-19 cases and other statistics. This time death count had the strongest correlation with crimes in NYC. This further strengthens my assumption that crime is negatively correlated with covid-19 cases.

My next objective was to check how different types of crime behaved as the covid cases varied. There is a possibility that some crimes might have gone up during the covid. The 2020 dataset had 58 types of crimes and the 2021 dataset had 61 types of crimes. I wanted to get daily counts of different types of crimes to check their correlation with covid-19 cases. I created a different csv file called "Crime_Grouping" which contains crime-wise daily counts along with their dates. 

</br>
I used the following code for getting the crime-wise counts
</br>

```python

import csv, datetime

f=open("CovidDailyCases_2020.csv",'r')
reader1=csv.reader(f)

count_cases_per_day=[]
n=0
for row in reader1:
    if n>0:
        cases=row[1]
        count_cases_per_day.append(cases)
        
    n+=1

    
f1=open('Crime_complaints_2020.csv','r')
reader=csv.reader(f1)

f2=open('Crime_Grouping.csv','w')
writer=csv.writer(f2,delimiter=',',lineterminator='\n')

n=0
crimes={}
for row in reader:
    if n>0:
        date=row[1]
        crime=row[8]
        if crime!='':
            if crime not in crimes:
                crimes[crime]={}
            if date in crimes[crime]:
                crimes[crime][date]+=1
            else:
                crimes[crime][date]=1
    n+=1
    

base = datetime.datetime.strptime('03/01/2020','%m/%d/%Y')
date_list = [base + datetime.timedelta(days=x) for x in range(306)]
dates=[]
for i in date_list:
    dt=datetime.datetime.strftime(i,'%m/%d/%Y')
    dt1=dt.split
    dates.append(dt)
    
    
n=0
crime_list=[]

for i in crimes.keys():
    crime_list.append(i)
    
crime_list.insert(0,'Date')
crime_list.insert(1,'Total Covid Cases')

writer.writerow(crime_list)
list_final=[]
for date in dates:
    row=[]
    for crime in crimes.keys():
        if crimes[crime].get(date) is None:
            row.append(0)
        else:
            row.append(crimes[crime].get(date))
    
    row.insert(0,date)
    list_final.append(row)
    
    n+=1

m=0
for elements in list_final:
    elements.insert(1,count_cases_per_day[m])
    writer.writerow(elements)
    m+=1

    

f.close()           
f1.close() 
f2.close()
  
```

First I made a list called "count_cases_per_day" which has date-wise covid case count. Then I created a dictionary called "crimes" of dictionaries for the crime types and their counts for each day. The keys of outer dictionaries are the crime types and their value is a dictionary having keys as dates and value as the count of that crime on that day. The structure looks like this-


```JSONasPerl
Crimes
       -Crime1
              -Date1:count1
              -Date2:count2
       -Crime2
              -Date1:count1
              -Date2:count2
```


              
      




 
 
