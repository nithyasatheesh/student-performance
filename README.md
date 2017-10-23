# student-performance
Analysis of student performance of different subjects from different states
import pandas as pd
df1=pd.read_csv("c:\\users\\pc\\desktop\\nas-pupil-marks.csv")
df1.head()

STUID 	State 	District 	Gender 	Age 	Category 	Same language 	Siblings 	Handicap 	Father edu 	... 	Express science views 	Watch TV 	Read magazine 	Read a book 	Play games 	Help in household 	Maths % 	Reading % 	Science % 	Social %
0 	11011001001 	AP 	1 	1 	3 	3 	1 	5 	2 	1 	... 	3 	3 	4 	3 	4 	4 	20.37 	NaN 	27.78 	NaN
1 	11011001002 	AP 	1 	2 	3 	4 	2 	5 	2 	2 	... 	3 	4 	4 	3 	4 	4 	12.96 	NaN 	38.18 	NaN
2 	11011001003 	AP 	1 	2 	3 	4 	2 	5 	2 	1 	... 	3 	4 	3 	3 	4 	4 	27.78 	70.00 	NaN 	NaN
3 	11011001004 	AP 	1 	2 	3 	3 	2 	4 	2 	1 	... 	3 	4 	3 	3 	4 	4 	NaN 	56.67 	NaN 	36.00
4 	11011001005 	AP 	1 	2 	3 	3 	2 	5 	2 	1 	... 	3 	2 	3 	3 	4 	4 	NaN 	NaN 	14.55 	8.33

df1.info()
df1.describe()
list(df1.columns.values)

df1["Maths %"].fillna(0, inplace=True)
df1["Science %"].fillna(0, inplace=True)
df1["Social %"].fillna(0, inplace=True)
df1["Reading %"].fillna(0, inplace=True)
df1['Sci %'] = pd.cut(df1['Science %'], [0,20,40,60,80,100], labels=['0-20', '20-40','40-60','60-80','80-100'])
df1['Soc %'] = pd.cut(df1['Social %'], [0,20,40,60,80,100], labels=['0-20', '20-40','40-60','60-80','80-100'])
df1['Read %'] = pd.cut(df1['Reading %'], [0,20,40,60,80,100], labels=['0-20', '20-40','40-60','60-80','80-100'])
df1['Mat %'] = pd.cut(df1['Maths %'], [0,20,40,60,80,100], labels=['0-20', '20-40','40-60','60-80','80-100'])

import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid')
%matplotlib inline

# create state column :(south,North,East,West)
def value(x):
    if x=='AP'or x=='TN' or x=='KA'or x=='KL' or x=='PY'or x=='AN':
        return 'south'
    if x=='GA'or x=='GJ'or x=='MH'or x=='DN'or x=='DD':
        return 'west'
    if x=='DL'or x=='HP'or x=='JK'or x=='RJ'or x=='PB'or x=='UP'or x=='CH'or x=='UK'or x=='HR':
        return 'north'
    if x=='MG'or x=='MN'or x=='BR'or x=='MZ'or x=='OR'or x=='SK'or x=='WB'or x=='TR'or x=='AR'or x=='NG'or x=='JH':
        return 'east'

df1['state'] = df1['State'].apply(value)

# visualize Important factors of Students performance
fig, (axis1, axis2)  = plt.subplots(2, 1,figsize=(10,10))
sns.countplot(x='Father edu',hue="avg",data=df1,ax=axis1)
sns.countplot(x='Mother edu',hue='avg',data=df1,ax=axis2)

fig, (axis1, axis2)  = plt.subplots(2, 1,figsize=(10,10))
sns.countplot(x='Correct Math HW',hue='Mat %',data=df1,ax=axis2)
sns.countplot(x='Private tuition',hue="Mat %",data=df1,ax=axis1)

fig, (axis1, axis2)  = plt.subplots(2, 1,figsize=(10,10))
sns.countplot(x='Solve science problems',hue='Sci %',data=df1,ax=axis1)
sns.countplot(x='Conduct experiments',hue='Sci %',data=df1,ax=axis2)

fig, (axis1, axis2)  = plt.subplots(2, 1,figsize=(10,10))
sns.countplot(x='Historical excursions',hue="Soc %",data=df1,ax=axis1)
sns.countplot(x='Small groups in SocSci',hue='Soc %',data=df1,ax=axis2)
fig, (axis1, axis2)  = plt.subplots(2, 1,figsize=(10,10))
sns.countplot(x='Dictionary to learn',hue="Read %",data=df1,ax=axis1)
sns.countplot(x='Read other books',hue="Read %",data=df1,ax=axis2)

#Correlation (to find important factors)
read=pd.DataFrame(train,columns=['Social %','Private tuition','Use computer','Use Internet','Father edu','Mother edu','Read magazine','Answer English WB','Answer English aloud','Read other books','Dictionary to learn','English is difficult','Read English','Read a book','Like school','Library use','Help in Study','Play games','Watch TV'])
read.corr()
soc=pd.DataFrame(train,columns=['Social %','Private tuition','Use computer','Use Internet','Father edu','SocSci is difficult','Participate in SocSci','Express SocSci views','Historical excursions', 'Small groups in SocSci','Mother edu','Like school','Library use','Help in Study','Play games','Watch TV'])
soc.corr()
science=pd.DataFrame(train,columns=['Science %','Private tuition','Use computer','Use Internet','Father edu','Science is difficult ','Conduct experiments','Solve science problems','Mother edu','Like school','Library use','Help in Study','Play games','Watch TV'])
science.corr()
maths=pd.DataFrame(train,columns=['Maths %','Private tuition','Use computer','Use Internet','Library use','Help in Study','Play games','Watch TV','Draw geometry','Solve Maths in groups','Solve Maths','Maths is difficult','Correct Math HW','Give Math HW','Father edu','Mother edu','Like school','Use calculator','Same language'])
maths.corr()

# state wise counting of boys and girls:
fig, (axis1, axis2)  = plt.subplots(2, 1,figsize=(10,10))
sns.countplot(x='State',hue='gender',data=df1,ax=axis1)

#gender wise performance: split  up boys and girls
boys=df1[df1.Gender == 1]
(new.pivot_table(index='State',columns='Mat %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))
(new.pivot_table(index='State',columns='Sci %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))
(new.pivot_table(index='State',columns='Soc %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))
(new.pivot_table(index='State',columns='Read %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))

#counting state wise and gender wise performance:
pd.crosstab([df1['state'], df1['Mat %']],df1['gender'],  margins=True)
pd.crosstab([df1['state'], df1['Sci  %']],df1['gender'],  margins=True)
pd.crosstab([df1['state'], df1['Soc  %']],df1['gender'],  margins=True)
pd.crosstab([df1['state'], df1['Read  %']],df1['gender'],  margins=True)
new1=df1[df1.Gender == 2]
(new1.pivot_table(index='state',columns='Mat %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))
(new1.pivot_table(index='state',columns='Sci  %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))
(new1.pivot_table(index='state',columns='Soc  %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))
(new1.pivot_table(index='state',columns='Read  %',values='Gender',aggfunc='sum',fill_value=0).plot.bar(stacked=True))

# COMPARISON OF SOUTH STATE WITH OTHER STATES:
pd.crosstab(df1['Mat %'],df1['state'],  margins=True)
pd.crosstab(df1['Sci %'],df1['state'],  margins=True)
#visualization
from scipy.stats.stats import pearsonr
from functools import partial
sns.countplot(x="state",hue='Sci %',data=df1, palette="muted");
sns.countplot(x="state",hue='Mat %',data=df1, palette="muted");
sns.countplot(x="state",hue='Soc %',data=df1, palette="muted"); 
sns.countplot(x="state",hue='Read  %',data=df1, palette="muted");

# subject wise comparison
sns.countplot(x="Subjects", data=df1, palette="muted");

# comparison with avg (mat.sci,soc,read) and state:
 df1['Avg']=df1[["Maths %",'Science %','Social %','Reading %']].mean(axis=1)
df1['avg'] = pd.cut(df1['Avg'], [0,20,40,60,80,100], labels=['0-20', '20-40','40-60','60-80','80-100'])
sns.countplot(x="avg",hue='State',data=df1, palette="muted");
sns.countplot(x="avg",hue='gender',data=df1, palette="muted");

