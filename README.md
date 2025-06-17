## Guide: How to Upload an Excel File, Analyze Data, and Create Graphs in R

This is a simple, step-by-step guide for beginners on how to upload an Excel file, perform basic analysis, and create graphs in R.



## 1. Install and Loading Packages

```r
install.packages("readxl")
install.packages("dplyr")
install.packages("ggplot2")

library(readxl)
library(dplyr)
library(ggplot2)
````

## 2. Import Your Excel File

Save your `.xls` or `.xlsx` file in your **working directory** first.

```r
# Import the first sheet by default
df <- read_excel("clinical_data.xlsx")

# Alternatively, you can select a sheet by name
df <- read_excel("clinical_data.xlsx", sheet = "Sheet1")
```

## 3. Inspect and Clean Your Data

```r
# View first few rows
head(df)

# Summary of the data
summary(df)

# Clean: for example, rename columns and filter missing values
df <- df %>%
  rename(SBP = BP_sys, DBP = BP_dia) %>%  # Renaming
  filter(!is.na(SBP), !is.na(DBP))         # Remove NAs
```

## 4. Descriptive Statistics

```r
# Overall summary
overall <- df %>%
  summarise(
    n = n(), 
    mean_sbp = mean(SBP), 
    sd_sbp = sd(SBP),
    mean_age = mean(Age),
    sd_age = sd(Age)
  )
print(overall)

# Summary by gender
by_gender <- df %>%
  group_by(Gender) %>%
  summarise(
    n = n(), 
    mean_sbp = mean(SBP), 
    sd_sbp = sd(SBP)
  )
print(by_gender)
```

## 5. Visualizations

### Histogram:

```r
ggplot(df, aes(x = SBP)) + 
  geom_histogram(binwidth = 5, fill = "lightblue", color = "black") + 
  labs(title = "Distribution of Systolic BP", x = "SBP (mmHg)", y = "Count")
```

### Boxplot:

```r
ggplot(df, aes(x = Gender, y = SBP, fill = Gender)) + 
  geom_boxplot() + 
  labs(title = "Systolic BP by Gender")
```

### Scatter Plot:

```r
ggplot(df, aes(x = DBP, y = SBP, color = Gender)) + 
  geom_point() + 
  labs(title = "Systolic vs Diastolic BP")
```

## 6. Quick Analyses (Optional)

### Pearson’s Correlation:

```r
correlation <- cor(df$DBP, df$SBP, use = "complete.obs")
print(paste("Correlation between Diastolic and SystolicBP :", round(correlation, 2)))
```

### T-test (BP by gender)

```r
t_result <- t.test(SBP ~ Gender, data = df)
print(t_result)
```

## 7. (Optional) Export Cleaned Data

```r
write.csv(df, "clinical_data_cleaned.csv", row.names = FALSE)
```

## Now you know how to:

* Import data from an Excel file
* Perform descriptive statistics
* Visualize your data with histograms, boxplots, and scatter plots
* Perform simple statistical tests
* Export cleaned data back to CSV
