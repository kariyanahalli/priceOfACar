# priceOfACar
What drives the price of a car? The target audience for this model is - a used car dealership. 

## Problem Definition
For a used car dealership, our analysis focuses on identifying the key factors that drive vehicle resale prices. 
By examining attributes such as age, mileage, manufacturer, condition etc.,. We use data-driven modeling to 
reveal which features most impact pricing. These insights will help the dealership for inventory management, 
set competitive prices, and target the vehicles consumers value most.

## Data Understanding
First find how much data are we dealing with and what are the features that could potentially influence the "Target Variable" (Price)

### Key Predictors

- **id**
- **region**
- **price**
- **year**
- **manufacturer**
- **model**
- **condition**
- **cylinders**
- **fuel**
- **odometer**
- **title_status**
- **transmission**
- **VIN**
- **drive**
- **size**
- **type**
- **paint_color**
- **state**

### Missing and Bad Values

Using IQR did not give the expected cleanup. So I decided to chose hardoced ranges, using my domain knowledge as to what values are acceptable and resonable (As IQR is still having some unrealistic values like negative price and odometer readings).

Firstly lets plot the quality of the data.

![Screenshot of Missing values per Column](images/missingValues.png)

#### IQR reduction yields:

```
price: Keeping values between -24978.62 and 57364.38
year: Keeping values between 1994.50 and 2030.50
odometer: Keeping values between -106053.75 and 277300.25
Removed VIN duplicates: 282445
Original rows: 426880
Rows after IQR cleaning: 111861
Kept 26.2% of rows
```

#### Hardcoded ranges yielded better

```
Total rows: 426880
Flagged by price   : 46439
Flagged by year    : 9618
Flagged by odometer: 6102
Missing required   : 5536
Removed VIN duplicates: 268299
Rows after cleaning: 99930
Kept 23.4% of rows
```

## Data Preparation

After ColumnTransformer(s) we can get replace the missing Numeric values with its "median" and Categorical features with "OHE". 


## Evaluation and Deployment

### More fine tuning
Needs more fine tuning. 

#### LassoCV model
With OHE did NOT converge. Gave up!!

#### SGDRegressor model
Gave somewhat better results after playing with 
```
    l1_ratio=0.1,          
    alpha=3e-3,
```
Yielded ..
```
RMSE: 18,342.62
R2:   -1.8105
```
Makes sure 
### Deployment
#### Vehicle Age and Mileage

Strong negative correlation with price. Older, high-mileage vehicles see steep depreciation.
![Age to Price correlation](images/CarPriceToAge.png)


#### Brand and Model Effects

Certain brands (e.g., Toyota, Honda) retain value better than others. Luxury brands show sharp initial depreciation but hold appeal in the secondary market if condition is good.

Value to dealers: Prioritize brands with strong resale value; selectively buy luxury vehicles that can be priced below new market alternatives.

![Brand and Model Effect](images/brand.png)

#### Engine Size, Fuel Type, and Efficiency

Larger engines often increase price, but rising fuel costs dampen this effect.

#### Condition and Features

Trucks and SUVs command higher prices in rural or colder regions. Convertibles peak in summer demand.

Value to dealers: Time purchases and advertising campaigns to match seasonal demand.


