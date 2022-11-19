# Drug_Consumption
Group Project






----------------------------------------------------------------------------------------------------------------------------------------------------------
---
title: "Data Science as a Field Project"
author: "Kate Borden, Evan McCormick, Paige Phillips"
date: "11/17/2022"
format: html
toc: true
editor: visual
page-layout: full
---

{r Import Packages, include=FALSE}
library(tidyverse)
library(dplyr)
library(stringr)


Statement of question and interest

For our project we are interested in drug consumption and the association to personality and behavior. For instance, we want to further evaluate the different types of risk to be for each drug. We can split up the drugs based on how they are scheduled or classified in as controlled substances. ####TO BE CONTINUED- include something about which drug(s) we are looking at – we can compare based on drug class/schedule + drug usage + personality AND/OR based on educations level + drug usage + personality####

Question:

Question (optional):

Source & Dataset Description

We got our database from here, see Drug Consumptions (UCI)

You can access our data here, see GitHub

Contents of the Dataset:

Featured Attributes for Quantified Data

ID: total number of 1885 records in this database

Age: participant's age

Gender: binary of only fale or female

Education: participant's level of education

Country: participant's country of origin

Ethnicity: participant's ethnicity

Featured Attributes for Personality measurements (NEO-FFI-R)

Nscore: neuroticism

Escore: extraversion

Oscore: openness to experience.

Ascore: agreeableness.

Cscore: conscientiousness.

Impulsive: impulsiveness (measured by BIS-11)

SS: sensation seeing (measured by ImpSS)

Consumption usage of 18 legal and illegal drugs

Alcohol: alcohol consumption

Amphet: amphetamines consumption

Amyl: nitrite consumption

Benzos: benzodiazepine consumption

Caff: caffeine consumption

Cannabis: marijuana consumption

Choc: chocolate consumption

Coke: cocaine consumption

Crack: crack cocaine consumption

Ecstasy: ecstasy consumption

Heroin: heroin consumption

Ketamine: ketamine consumption

Legalh: legal highs consumption

LSD: LSD consumption

Meth: methadone consumption

Mushroom: magic mushroom consumption

Nicotine: nicotine consumption

Semer: class of fictitious drug Semeron consumption (i.e. control)

VSA: class of volatile substance abuse consumption

Rating's for Drug Use

CL0: Never Used

CL1: Used over a Decade Ago

CL2: Used in Last Decade

CL3: Used in Last Year 59

CL4: Used in Last Month

CL5: Used in Last Week

CL6: Used in Last Day

Dataframe

{r read in data, echo=FALSE, }
#| echo: false
# TODO - read in file directly from kaggle

df <- read.csv(url("https://raw.githubusercontent.com/paph3285/Drug_Consumption/main/Drug_Consumption.csv"))


{r data, echo=FALSE, }

knitr::kable(
  head(df[0:32])
  )

Subset dataframe

{r Subset Dataframe, echo=FALSE}
#| echo: false

Subset_Drugs <- df %>% select(-c("ID", "Ethnicity", "Country", "Education", "Age", "Gender", "Caff", "Choc", "Semer"))

head(Subset_Drugs)


Examine Data

{r Examine Data, echo=FALSE}
#| echo: false
# check for NAs

sum(is.na(df))


Clean data

{r Clean Data, echo=FALSE}
#| echo: false

categorical_columns = c('Alcohol', 'Amphet', 'Amyl', 'Benzos', 'Cannabis', 'Coke', 'Crack', 'Ecstasy', 'Heroin', 'Ketamine', 'Legalh', 'LSD', 'Meth', 'Mushrooms', 'Nicotine', 'Semer', 'VSA')

# TODO -Remove columns ID,Choc - chocolate consumption, Semer - class of fictitious drug Semeron consumption (i.e. control)

# We will drop overclaimers since, there answers might not truly be accurate

# We will also drop unnecesary columns

# convert drug columns to numeric
for (col in categorical_columns) {
  df[col] = as.numeric(gsub("CL", "", df[, col]))
  
  # categorize drug column as either 'used' (1) or 'never used' (0)
  # if Drugs[col] == 0 (never used) or Drugs[col] == 1 (used over a decade ago), cateogorize them as a non-user
  df[col] = ifelse((df[col] == 0 | df[col] == 1), 0, 1)
}


{r Combining Columns, echo=FALSE}
#| echo: false

# combine columns of similar drugs
# TODO - do we want to do this? How should we group them? 
Drugs_mutate <- transform(df, Psychedelics = pmax(LSD, Mushrooms), OTC = pmax(Alcohol, Cannabis, Nicotine), Illegal = pmax(Coke, Crack, Ecstasy, Heroin, Meth))

# drop redudent columns
# TODO - drop all columns that have been combined
Drugs_subset <- Drugs_mutate %>% select(-c('LSD', 'Mushrooms', 'Alcohol', 'Cannabis', 'Nicotine', 'Coke', 'Crack', 'Ecstasy', 'Heroin', 'Meth'))



Rename Columns

{r Rename Columns, echo=FALSE}
#| echo: false

# TODO - why are new column names from 'mutate' above wrong? this shouldn't be needed
Drugs_subset <- Drugs_subset %>% rename("Psychedelics" = "LSD.1", "OTC" = "Alcohol.1", "Illegal" = "Coke.1")

# regular print wasn't working for me, can remove
print.data.frame(head(Drugs_subset))

Correlation Matrix

{r Corr-Matrix, echo=FALSE}
#| echo: false

# TODO - can we gather any relevant info from this? doesn't seem like there's much correlation between personality traits and user/non-user

## res <- cor(Drugs_subset)
##res


Visualize Correlation #1

{r Graphing 1, echo=FALSE}
#| echo: false
# TODO - graph findings


Visualize Correlation #2

{r Graphing 2, echo=FALSE}
#| echo: false
# TODO - graph findings


Model/Analysis

{r Analysis, echo=FALSE}
#| echo: false


Conclusion

asdknflaslnksnfadnfkfllankldfnnvavlvnlnnvnldnvlnvslv

Possible Biases:

1. Kate's Bias -

sdnalsnsldnvslvn 

2. Evan's Bias- 

sdnalsnsldnvslvn 

3. Paige's Bias -

Some personal biases that I can acknowledge from this project and dataset are the "Drugs" listed. Personally, I would not classify a  handful of the options presented as drugs and would not have included them in the original dataset.  Precisely alcohol, caffeine, chocolate, and possibly marijuana. One possible explanation to my bias, is due to how prevalent or not these specifics drugs are in my life. I am more likely to see family, friends, peers consume alcohol, caffeine, chocolate, and marijuana (depending on where you live) than lets say consume meth, heroin, or benzos, and so forth. Another possible  explanation is that I am unfamiliar and unaware of anyone that has overdosed or become addicted. For those who have gone to rehab or know someone that has, they may view these drug categories very different than me. Lastly, the word "drug(s)" carries  negative connotation  behind it. More often than not my mind jumps to 1) illegal, 2) substance use disorder, 3) ruins lives. Again, a lot of these are associated with more illicits drugs like cocaine, meth, heroin, etc.,  not so much with alcohol, chocolate, caffeine, and  marijuana. How I perceive these drugs is a bias of itself. If I were to not include some of these drugs in the dataset, would be unethical of me as a data scientist.

SessionInfo

{r ,echo=FALSE}
#| echo: false

sessionInfo()

