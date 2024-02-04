---
name: The World Bank's Aid in Sri Lanka's Economic Meltdown
tools: [MySQL, Data Analysis, Article Writing]
image: [https://i.postimg.cc/qqTfqxz6/crisismain.jpg]
description: Conducted comprehensive financial analysis for the World Bank's aid during Sri Lanka's economic crisis, & identified the financial & accounting landscape during the period.
---

![Preview](https://i.postimg.cc/qqTfqxz6/crisismain.jpg)

# The World Bank's Aid in Sri Lanka's Economic Meltdown

Probably one of the worst economical crisis any country could face in the 21st century, it happened in Sri Lanka. Like a fusillade towards a building, the economy of Sri Lanka was taking heavy damage from debts and consequences of bad decisions. A price paid by the citizens of Sri Lanka.

This didn't strongly seem to happen back in 2019 when Gotabaya Rajapaksha won the election with majority and became the President of Sri Lanka. People were celebrating and no one thought of an incoming wrath called 'consequences' and to push even further down into the abyss, a natural disaster.

Imagine a ticking clock in seconds, inside the back of your mind.

## 2019

While the country was still grieving from its loss of 250 citizens from terrorists attacks in mid April 2019, elections took place and Gotabaya Rajapaksha became The President of Sri Lanka while he was facing allegations of corruption, human rights violation and torture, who also happens to end the 26 year old civil war of Sri Lanka and pushed the country into heavy debt to China. That polarized result divided Sinhala buddhist communities and other minorities in Sri Lanka. President Gotabaya during the election win-day said, "I would like to inform you that I will execute everything you trusted in me to do". In his first cabinet meeting, he reduced tax cuts with low interest rates and pursued to become the most influential family in Sri Lanka. The taxpayers took a massive hit when the GDP of Sri Lanka fell down (the Tax-GDP ratio didn't support) and country's debt to China's hegemony was too much to bear. In the end, GDP-debt ratio was unbearable.

## 2020

Amidst this debt payments and tax cuts, the government faced shortages of money and soon consequences followed in the form of shortage of fuel and then came COVID. Tourism got shattered, imports and exports entirely came to a halt, this made the lives of citizens much worse and COVID spread increased health costs. Sri Lanka was running out of federal reserves very quickly. Let me remind you that, Sri lankan rupee is pegged to US dollar. This COVID and forced closure of borders further plunged the economy down. Soon after the lift of first lockdown, Rajapaksha's government decided to cut down costs by initiating a ban on the import of chemical fertilizers and to become an organic farming country, which he did promise during his campaign. This decision was an unimaginable blow to the country. There wasn't enough money to feed his citizens let alone buy and provide the technology to farmers to begin organic farming. The ban of chemical fertlizers from abroad lowered the rice production and then came shortage of food. Farmers started using local fertilizers. By this point the inflation started to increase, reaching 6.15% in 2020. Further COVID lockdowns hampered the flow of currency and drained the federal reserves. Parts of Sri Lanka then faced immense flooding for weeks in the end of the year destorying homes, agricultural fields and infrastructure. Power Outages became more frequent.

## 2021

By April 2021, president Rajapaksha entirely banned use of chemical/synthetic fertilizers and forced farmers to use organic means of farming, this worsened the crop yield and crippled the rice production. Rajapaksha's government failed to supply subsidies and money to farmers. Meeting food shortage demands from abroad costed them too much. The foreign debts of Sri Lanka in 2021 summed upto staggering ~$56.5 billion USD. One more severe flood struck in 2021 stranding many citizens and loss of life. Shortage of food and fuel became a reality.

## 2022

The nightmare, Sri Lanka announced it has defaulted in April 2022 on foreign trade with immense debt of $86.6 billion of which $4.2 billion is to China while its foreign reserves were mere US$1.9 billion. There is now no money. Mass resignations happened within his administration and there were no ministers to run the government or country. Civil unrest took place and the crisis became world issue. Sri Lanka sought out help from foreign banks and neighbouring countries like India and China. India disbursed US$ 1 billion to help Sri Lanka fight its crisis. World Bank and IMF said yes to help Sri Lanka fight the economic crisis. By this time in July 2022 Rajapaksha fled the country and resigned. Governements changed after political and citizens' anger. 

## World Bank's Role

I read a news flash around June 2022, world bank disbursed first aid to Sri Lanka. Now, when I look back and after finding the dataset through Avery's bootcamp. I thought let me look into the data. I went to World Bank's official website and downloaded the updated dataset that includes the history of all credits and grants in world bank. One can find the dataset [here](https://finances.worldbank.org/Loans-and-Credits/IDA-Statement-Of-Credits-and-Grants-Historical-Dat/tdwh-3krx/about_data).

### The Analysis

I had lot of questions regarding this data. The data actually has ~1.25 million rows and about 30 columns. Each row is either a credit or grant, in other words a transaction. Importing this data into excel will fail. I used MySQL. I used the follwoing code to load the dataset into my MySQL server:

```
# ---------------------------------------------------------------------------------
SHOW VARIABLES LIKE 'secure_file_priv';
GRANT FILE ON *.* TO root@localhost;
# ---------------------------------------------------------------------------------
USE worldbankdata;
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/IDA_Statement.csv' INTO TABLE ida_statements
  FIELDS TERMINATED BY ','
  ENCLOSED BY '"'   -- Enclosed by double quotes
  LINES TERMINATED BY '\r\n'
  IGNORE 1 LINES;

# Making changes to EOP column: Formatting as Datetime  
#-- Step 1: Add a new column to temporarily store the converted datetime values
ALTER TABLE ida_statements
ADD COLUMN EOP_temp DATETIME;

#-- Step 2: Update the new column with the converted datetime values
UPDATE ida_statements
SET EOP_temp = STR_TO_DATE(EOP, '%m/%d/%Y %H:%i');

#-- Step 3: Drop the original varchar column
ALTER TABLE ida_statements
DROP COLUMN EOP;

#-- Step 4: Rename the temporary column to the original column name
ALTER TABLE ida_statements
CHANGE COLUMN EOP_temp EOP DATETIME;

# Update Tables with Double in required columns
ALTER TABLE ida_statements
MODIFY COLUMN Original_P_amt DOUBLE;
```









