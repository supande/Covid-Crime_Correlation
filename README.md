                                                      FINAL PROJECT

The covid-19 has brought a lot of changes to the world. The pandemic started at the end of 2019 and spread all across the world. Every country suffered economically and financially and continues to suffer as new variants of the virus emerge. USA
being the biggest country suffered heavily as people lost their lives, jobs, etc. New York City, being one of the biggest cities in the world, saw a lot of covid cases beginning from March 2020. So, my objective is to find the effect of covid-19
on the number of crimes in NYC. My assumption is that they are negatively correlated and hence, crimes went down during the pandemic in NYC. I believe as people were advised to stay inside their homes and in general, people were scared as the world was witnessing a major pandemic for the first time in almost a century, crimes declined as the covid-19 cases went up.

   For testing the correlation between crimes and covid-19 cases, I have used 3 datasets(.csv files)-"NYPD Complaint Data Historic","NYPD Complaint Data Current Year To Date (1)" and "COVID19_Daily_Counts_of_Cases__Hospitalizations__and_Deaths" from NYC Data website. The first dataset contains the record of all the crimes that happened in NYC till mid 2021. The second dataset has all the crimes recorded in NYC in 2021 till September and third dataset has the daily covid-19 cases, deaths, hospitalizations in NYC till October 2021. 
   
The first step was to extract all the data for crimes that happened in 2020 from the "NYPD Complaint Data Historic" and to eliminate some of the bad data using the the try and except method. I made two csv files for 2020. One having the whole data of 2020 and second one having data from march till december,to test the correlation with covid-19 cases, as covid-19 started in march in NYC.

</br>
I used the the following code to achieve this. Row 1 had the date of the crime.
</br>

''' python

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
                if dto>datetime.datetime.strptime('02/29/2020',"%m/%d/%Y") and                       
                    writer.writerow(row)
                    
'''

Similarly, I created a seperate csv file for 2020 covid cases.




 
 
