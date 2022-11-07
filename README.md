# Drilling-
A model to predict drill hole length, given other drilling parameters 
this model will be updated shortly to solve little challenges and make it efficient enough

import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import seaborn as sns
import matplotlib.pyplot as plt

drill_data = pd.read_csv('test.csv')

# Data Exploration
drill_data

drill_data = pd.get_dummies(drill_data, columns=['Rig', 'Shift'])
drill_data

# Exploratory data analysis: correlation and heatmap
correlation = drill_data.corr()
sns.heatmap(correlation,annot=True,cmap='cool-warm')

# Exploratory data analysis: pair plot
sns.pairplot(drill_data)

correlation = drill_data.corr()
correlation

X = drill_data.drop('Depth_To',axis=1)
y = drill_data['Depth_To']

# Split data into test/train set (70/30 split) and shuffle
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size =0.2, shuffle=True)

# Assign algorithm
model = LinearRegression()

# Link algorithm to X and y variables
model.fit(X_train, y_train)

#Find y-intercept
model.intercept_

# # Find x coefficients
model.coef_

# Data point to predict
drill = [
	40, #Depth_From
	65, #Dip
	120, #Azimuth
	16, #Time(mins)
	22, #Penetration Rate
	0, #Rig_M3-BH29
	1, #Rig_M3-BH30
	1, #Shift_Day
	1, #Shift_Night
]

# Make prediction
drill = model.predict([drill])

drill

# Target Values
target_depth = 300.0
target_dip = 65.0
target_azimuth = 120.0
av_penetration_rate = 25.0
drilling_rate = 22.0
Y='Yes'
N='No'

intro = input("Are you ready to input today's values?\nYes/No: ")
if intro.title()==Y:
    depth = input("Depth: ")
    dip = input("Dip: ")
    azimuth = input("Azimuth: ")
    penetration = input('Penetration: ')


    diff_depth=target_depth-float(depth)
    diff_dip=target_dip-float(dip)
    diff_azimuth=target_azimuth-float(azimuth)
    diff_penetration=av_penetration_rate-float(penetration)

    dip_allowance=range(1,21)
    azimuth_allowance=range(1,51)

    if diff_depth>0.0:
        print(f'{diff_depth} meters left to be drilled')
    elif diff_depth==0.0:
        print('Depth target reached')
    else:
        print(f'Drill depth exceeded by {abs(diff_depth)} meters' )
    
    
    if int(abs(diff_dip)*100) in dip_allowance:
            print(f'{abs(diff_dip)} dip differnce. You are on track.')
    elif int(diff_dip*100)>dip_allowance[-1]:
            print(f'DEVIATION FROM DIP TARGET: \n\tYou have deviated {abs(diff_dip)} less than the target. If feasible, correct the dip and keep on track.')
    elif int(diff_dip*100) < -abs(dip_allowance[-1]):
            print(f'DEVIATION FROM DIP TARGET: \n\tYou have deviated {abs(diff_dip)} above the target. If feasible, correct the dip and keep on track')       
    else :
            print("Dip target achieved") 

    if int(abs(diff_azimuth)*100) in azimuth_allowance:
         print(f'{abs(diff_azimuth)} azimuth difference. You are on track.')
    elif int(diff_azimuth*100)>azimuth_allowance[-1]:
        print(f'DEVIATION FROM AZIMUTH TARGET:\n\tYou have deviated {abs(diff_azimuth)} less than the target. If feasible, correct the azimuth and keep on track.')
    elif int(diff_azimuth*100)<-abs(azimuth_allowance[-1]):
        print(f'DEVIATION FROM AZIMUTH TARGET:\n\tYou have deviated {abs(diff_azimuth)} above the target. If feasible, correct the azimuth and keep on track')
    else:
        print("Azimuth target achieved")

    if diff_penetration>0.0:
        print(f'increase your penetration by {diff_penetration} m/h')
    elif diff_penetration==0.0:
        print('penetration rate achieved')
    else:
        print(f"you're doing a great job with the penetration rate")
elif intro.title()==N:
    "break"
else :
    print('Oops! please type Yes or No')
