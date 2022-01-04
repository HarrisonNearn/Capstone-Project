# Nashville Crime: Pre-Covid, Covid, & Now
## *How has the emergence and continued presence of Covid-19 shaped crime statistics in Nashville-Davidson County?*
This is the foundational question for my Capstone Project.  Covid-19 has forced upon humanity an immeasurable paradigm shift; from the way people socially interact, the way business is conducted, how our children attend school, and how we make a living have all been altered.  I want to know how has that shift affected crime?

## My Motivation
Prior to enrolling in the full-time Data Analytics program at Nashville Software School, I served as an officer and detective for the Metropolitan Nashville Police Department (MNPD) from October 2009 to July of 2021.  In those nearly 12 years, I responded to thousands of calls for service and conducted hundreds of investigations into all manner of crime.  Also, I learned that crime, in general, followed certain patterns.  For example, when summer arrived and the days and nights were hot, work was going to get busy; by that I mean the amount of crime was going to increase and so to would calls for service.  Also, when kids were out of school for various breaks, certain crimes were going to spike.  It was just a foregone conclusion.

With the arrival of the virus, normal life was turned upside down.  Working from home was/is now the norm and via Zoom became the way children attended school. I became curious how crime adapted to the new way of life.  Although I was present for that change, I never looked into the actual numbers to see how crimes evolved.  There were certain assumptions I made, but those were based upon gut feelings and personal observations, not from diving into the data.  Given that more people are working from home, I assumed that the number of residential burglaries decreased; burglars don't want to encounter people when trying to break into a house.  Also, I assumed that more people working from home caused the number of domestic-related crimes to increase.  With constant interaction between spouses, family members, roommates, etc., it seemed only rational that friction between them would increase since they were not accustomed to being around each other all the time.  With that, I decided to dive into the Nashville-Davidson County crime statistics to see if my assumptions were correct, and what other changes (if any) arose.

## Acquiring The Data
I began by pulling the incident report data kept by the MNPD.  For the uninitiated, an incident report is a document filled out by a responding officer that lists the occurrence of the crime or incident to which the officer is responding.  This report contains all types of pertinent data to include, but not limited to time, place, victim and suspect demographic information, victim's relationship to the suspect (if any), was a weapon, and dozens of other criteria that may or may not be applicable to the event.  I decided to use the incident report data from 2019 to serve as my pre-Covid crime baseline, the 2020 data is obviously the during Covid-19 data, and the MNPD data for 2021 was used for the 'moving forward' data to see if and how crime shifts now that life with Covid-19 is becoming the "new normal".  

For Covid-19 statistics, I utilized the State of Tennessee data for all known Covid-19 cases beginning in March of 2020 and ending in early December of 2021.  The Covid-19 statistics tracked the emergence of new cases, recurring total cases, new and recurring hospitalizations, and deaths which were attributed to the virus.

**Metro Nashville Police Department Incidents**: https://data.nashville.gov/Police/Metro-Nashville-Police-Department-Incidents/2u6v-ujjs

**Covid-19 Data**: https://www.tn.gov/content/dam/tn/health/documents/cedep/novel-coronavirus/datasets/Public-Dataset-Daily-Case-Info.XLSX

## Data Cleaning, Manipulation, & Analysis
### *MNPD*
Upon acquiring the MNPD dataset, I discovered the CSV file contained nearly nine hundred thousand rows with each row representing an individual report taken, with some dating back to 2014.  The data frame consisted of 31 columns with each column representing a specific question answered by the incident report.  For data cleaning and manipulation, I utilized Python.  I initially discovered that the most commonly taken report was "Matter of Record".  From my previous experience, I knew these reports represented incidents that did not involve any crime taking place.  A matter of record report is used to document when two individuals may have been arguing, but no crime occurred; additionally it is used to document when a parent or guardian failed to abide by a civil court order (such as a child custody agreement).  This too is not a crime, however, parents/guardians routinely ask for such incidents to be documented so they can be presented during future court custody proceedings.  Additionally, reports like lost and found property were also present in the data frame.  Because I was solely focusing on crime statistics, I decided to remove any row that contained a report that was not specifically reporting the occurrence of a crime.

![original Python data snip](Snips/data_snip2.JPG)

*To better understand the columns from the original MNPD CSV file and what each column represents, I recommend following the link*: [Explanation Columns in Dataset](https://data.nashville.gov/api/views/2u6v-ujjs/files/4537ce42-a1d7-4157-be45-6ab2f22c15ef?download=true&filename=Metro-Nashville-Police-Department-Incidents-Metadata-v2.pdf).

As I dove deeper into the cleaning and manipulation phase, I began sub-setting my data into the years in which I was interested in exploring - 2019, 2020, and 2021; respectively.  This allowed me to create data frames which I would utilize for the baseline, during Covid, and moving on with Covid portions of my analysis.  

I also wanted to be able to analyze the frequency of crimes throughout each of the years; however, I discovered the crimes were listed into two categories: the TIBRS code and the sub-classification of each crime.  A TIBRS (Tennessee Incident Based Reporting System) code is a list of crimes within the State of Tennessee with an associated numeric or alpha-numeric code for each one.  This allows the TBI to track statewide crime statistics.  Similarly, there is NIBRS (National Incident Based Reporting System) code which is handled by the FBI to allow for national tracking of crime statistics; and MNPD RMS (Report Management System) Codes which handle Metro-specific report codes.  As an example, the TIBRS/NIBRS code for an aggravated assault is a 13A; the MNPD RMS code for a matter of record report is a 740.  As I stated previously, in addition to those codes there was also incident sub-classifications.  An example of a sub-classification would be Theft of Vehicle - $25,000 to $50,000.  This tells me that a vehicle theft occurred, but also tells me how much the vehicle was worth.  For the purposes of this project, I wanted to know solely know that a theft of vehicle occurred; the value of the vehicle was unnecessary.  To remedy this, I made a dictionary within Python that defined each of the TIBRS, NIBRS, and MNPD RMS codes so I could track the occurrence of each crime by its name, not by its code or sub-classification.  Doing this allowed me to then aggregate the occurrence of each crime in 2019, 2020, and 2021; and then analyze them as necessary.

*To create the dictionary I used the following TIBRS, NIBRS, and MNPD report codes*:
[TIBRS](Other/TIBRSCodes.pdf),
[NIBRS](Other/NIBRS_Offense_Codes.pdf),
[MNPD Side 1](Other/MNPD_Offense_Report_Codes.jpeg), and [MNPD Side 2](Other/MNPD_Offense_Report_Codes2.jpeg).

### *Top 5 Crimes*
![2019 top 5 crimes](Snips/19_top_5.JPG)
![2020 top 5 crimes](Snips/20_top_5.JPG)
![2021 top 5 crimes](Snips/21_top_5.JPG)

### *Covid-19*
The State of Tennessee Covid-19 dataset needed very little cleaning to make the data useable.  The majority of the work was manipulating the data by removing and creating columns so I could aggregate Covid-19 cases by month and year; and also more easily join the MNPD and Covid-19 data frames together.  Also, during the analysis I wanted to see how Covid-19 numbers changed from 2020 to 2021.  Thinking the two years would have similar monthly trends, I was surprised by the findings.

![Covid data frame snip](Snips/covid_snip.JPG)

### *Covid-19 Month-To-Month Comparison*
![2020-2021 Covid Numbers](Snips/covid_monthly.JPG)

### *Changes in Overall Crime Numbers From 2019-2021*
![2019-2021 Crime Change](Snips/monthly_crime_snip.JPG)


## Summary
Throughout this process, I found myself constantly amazed at both the variation and lack of variation in Nashville's crime trends since 2019.  Although there seems to be some consistency in which crimes are most often perpetrated, there appears to have been a significant increase in the amount of violent crime that has occurred.  Additionally, an unintended discovery was how the fluctuation of Covid-19 cases appear to have no discernable pattern from 2020 to 2021.  Overall, I found thoroughly enjoyed my this project and the analysis of these two sets of data and am curious to see how the two continue to evolve as the time passes.

## Acknowledgements
I would be remiss if did not mention all the people that made journey possible.  

First, my family.  Without the constant, unending support from wife, Chelsey, and our beautiful daughters, Quinn and Ella; none of this would have been possible.  Although this class and project have kept me busy, I know my girls are happier to have me home.  

Second, to my NSS instructors: Chris, Josh, Chip, and Toni.  Their knowledge, guidance, and patience is without equal.  Looking back, it is truly amazing to see how far they can bring a blue collar street cop with no coding knowledge and no idea what he is doing.  They are truly masters of their craft.

Lastly, the classmates and friends from DDA5.  In three short months, we have transformed from a group of 19 individuals into one cohesive unit.  Its been incredible to have met you all and share this adventure with you.  I wish you all the best!
