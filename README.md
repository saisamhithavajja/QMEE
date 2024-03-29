This study explores the intricate interplay of sexual selection, allometry, and intralocus sexual conflict in shaping the evolution of sexual dimorphism in *Drosophila* wing shape and size. Contradictory findings regarding the stability of sexual shape dimorphism in *Drosophila* prompt a closer examination of allometry's contribution to the evolution of wing shape dimorphism. The study delves into the complex relationship between intersexual genetic correlation, intralocus sexual conflict, and the constraints they impose on the evolution of sexual dimorphism. The evidence suggests that shared genetic architecture between males and females limits the evolutionary response of wing shape dimorphism, highlighting the role of intralocus sexual conflict. Furthermore, the study explores the potential resolution of such conflicts by evolving condition-dependent traits.
The concept of condition dependence is investigated in the context of Drosophila wing size and shape, revealing a link between nutrition, mating success, and the observed sexual dimorphism. The study extends its focus to interspecific patterns, exploring how the degree of sexual shape and size dimorphism correlates with condition dependence across a phylogeny of flies. The findings suggest that species with more pronounced sexual dimorphism exhibit stronger condition dependence for sexually selected traits.

The objective of this study will be to look at the cell counts in the wings of different *Drosophila* species and how sexually dimorphic the cell counts are in the species of Drosophila. The biological questions we would like to answer are as follows;

1) Condition dependence difference in the cell counts in the wings of different Drosophila species. 
2) Within the same species, how dimorphic is the condition dependent cell count in the wings.


Following below, I have written some initial analysis as well as explaination of what the code is doing. 

1) Load Packages:

```r
library(tidyverse)
library(ggplot2) ## don't need to load this separately, comes with tidyverse
```

The code loads the tidyverse package, which includes a collection of packages for data manipulation (dplyr), data visualization (ggplot2), and other useful tools.

2) Load Data:
```
data <- read.csv("data/MP_SpeciesStarvation_Cell_count_data.csv")
```
Reads a CSV file ("MP_SpeciesStarvation_Cell_count_data.csv") into a dataframe named data.

3) Explore Data Structure:

```{r}
str(data)
```
Prints the structure of the data, showing the types and first few values of each column.

4) Check for Missing Values:

```{r}
missing_values <- data %>%
  summarise_all(~ sum(is.na(.)))
```
Computes the sum of missing values in each column and stores the result in the missing_values dataframe.

5) Drop Rows with Missing Values:
```ruby
data <- data %>%
  drop_na()
```
Removes rows with missing values from the dataframe.

6) Extract Sex Information:
```{r}
data <- data %>%
  mutate(Sex = ifelse(str_detect(Label, "_F_"), "Female", "Male"))
```
Creates a new column Gender based on whether the "Label" column contains "F" (Female) or not.

7) Show Unique Entries in Columns:
```{r}
unique_entries <- data %>%
  distinct(Label, Wing_Area)
print(unique_entries)
```
Prints unique entries in the "Label" and "Wing_Area" columns.

8) Create a New Column "Species":
```{r}
data <- data %>%
  mutate(Species = case_when(
    ... # species mapping rules
  ))
```
Creates a new column "Species" based on patterns in the "Label" column using the case_when function.

9) Print Updated Data:
```{r}
print(data)
```
Prints the updated dataframe with the new "Species" column.

10) Filter Data and Create Plots:
```ruby
# ... (code for creating boxplot, violin plot, t-test, etc.)
```
Filters the data, creates boxplots, violin plots, and performs a t-test for a comparative analysis.

10) Basic Summary Statistics:
``r
summary_stats <- data %>%
  group_by(Species, Gender) %>%
  summarise(
    mean_cell_count = mean(Count),
    sd_cell_count = sd(Count),
    min_cell_count = min(Count),
    max_cell_count = max(Count),
    total_count = n()
)
```
Computes basic summary statistics by grouping data based on "Species" and "Gender."

11) Visualize Mean Cell Count:
```r
ggplot(summary_stats, aes(x = Species, y = mean_cell_count, fill = Gender)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Mean Cell Count by Species and Gender",
       x = "Species",
       y = "Mean Cell Count")
```
Creates a bar plot to visualize the mean cell count by species and gender.

12) Boxplot for Cell Counts by Species and Gender:
```r
ggplot(data, aes(x = Species, y = Count, fill = Gender)) +
  geom_boxplot() +
  labs(title = "Cell Counts by Species and Gender",
       x = "Species",
       y = "Cell Count")
```
Creates a boxplot to compare cell counts by species and gender.

13) Scatter Plot: Cell Count vs. Wing Disc:

```{r}
ggplot(data, aes(x = Wing_Area, y = Count, color = Species)) +
  geom_point() +
  labs(title = "Cell Count vs. Wing Disc",
       x = "Wing Disc",
       y = "Cell Count",
       color = "Species")
```
Creates a scatter plot to visualize the relationship between cell count and wing disc size.

14) Correlation between Cell Count and Wing Disc:
```r
correlation_result <- data %>%
  group_by(Wing_Area) %>%
  summarise(correlation = cor(Count))
print(correlation_result)
```
Computes the correlation between cell count and wing disc size, grouping by "Wing_Area."

This code is a comprehensive analysis pipeline for exploring and visualizing cell count data in Drosophila species. It includes data preprocessing, visualization, statistical testing, and summary statistics. The specific details of the analysis depend on the content of the dataset and the research questions being addressed.

Some of the Results: 

1) Visualize mean cell count by Species and Gender
   ![image](https://github.com/saisamhithavajja/QMEE/assets/96578069/9c6eea9a-78f2-4869-adf3-5c00c49a9ae0)

2) Box plots for cell counts by species and Gender
   ![image](https://github.com/saisamhithavajja/QMEE/assets/96578069/c0ab432f-44d7-4460-9293-056ee822b39a)

3) Scatter Plot: Cell Count vs. Wing Disc
   ![image](https://github.com/saisamhithavajja/QMEE/assets/96578069/fdaefc1c-4485-486d-8195-a887ccf19936)

Use the saveRDS function in R to save a clean (or clean-ish) version of your data and Write a separate script that reads in your .rds file and does something with it: either a calculation or a plot.

```{r}
#save the data in an RDS file
saveRDS(data, file = "Assignment1code.rds")

# Load necessary libraries
library(ggplot2)

# Read the .rds file
data <- readRDS("Assignment1code.rds")

# Display the structure of the loaded data
str(data)

# Create a scatter plot
ggplot(data, aes(x = Count , y = Species)) +
  geom_point() +
  labs(title = "Scatter Plot of X vs. Y",
       x = "Cell Count",
       y = "Different Species of Drosophila")
```
Result: ![image](https://github.com/saisamhithavajja/QMEE/assets/96578069/c08d817d-2e76-444d-9ca1-bd820e874503)

Further investigations that I plan to make in this study:

Given that we have cell count data for the wings of different species of Drosophila, and the data is split into males and females, we can perform more advanced investigations and statistical analyses to gain deeper insights. Here are some suggestions:

1. Multivariate Analysis of Variance (MANOVA):
Use MANOVA to analyze the differences in cell counts across different species while considering both males and females. This allows you to test whether significant differences exist in the combination of dependent variables (cell counts) among groups (species and gender).

2. Principal Component Analysis (PCA):
Conduct PCA to reduce the dimensionality of the data and visualize patterns of variation. This can help us understand whether there are distinct patterns or clusters based on cell count data.

3. Cluster Analysis:
Use cluster analysis to identify groups of species or individuals with similar cell count patterns. This can help reveal natural groupings in the data.

ASSIGNMENT 2 (26TH JANUARY 2023)
I have provided the code for the assignment 2 and I am also attaching the results for the assignments:

```ruby
##Assignment 2

##some more ggplots to visualize the data
#1) First one
##load all the required packages
library(dplyr)
library(ggplot2)

mean_cell_count_by_species <- data %>%
  group_by(Species) %>%
  summarise(mean_cell_count = mean(Count))

# Print the result
print(mean_cell_count_by_species)

# Calculate mean and standard deviation by species
summary_stats <- data %>%
  group_by(Species) %>%
  summarise(mean_cell_count = mean(Count),
            sd_cell_count = sd(Count))

# Plot the mean and standard deviation
ggplot(summary_stats, aes(x = Species, y = mean_cell_count)) +
  geom_bar(stat = "identity", fill = "skyblue", color = "black") +
  geom_errorbar(aes(ymin = mean_cell_count - sd_cell_count,
                    ymax = mean_cell_count + sd_cell_count),
                width = 0.2, position = position_dodge(0.9), color = "black") +
  labs(title = "Mean and Standard Deviation of Cell Count by Drosophila Species",
       x = "Species",
       y = "Cell Count") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

```ruby
##2) Second one

library(dplyr)

# Function to calculate z-scores for a vector
calculate_z_scores <- function(x) {
  mean_x <- mean(x)
  sd_x <- sd(x)
  z_scores <- (x - mean_x) / sd_x
  return(z_scores)
}

# Calculate z-scores for cell counts within each species
data_filtered <- data %>%
  group_by(Species) %>%
  filter(between(Count, quantile(Count, 0.05), quantile(Count, 0.95))) %>%
  mutate(z_score = calculate_z_scores(Count))

# Plot the updated boxplot without outliers
if (nrow(data_filtered) > 0) {
  library(ggplot2)
  
  ggplot(data_filtered, aes(x = Species, y = Count, fill = Gender)) +
    geom_boxplot() +
    labs(title = "Cell Counts by Drosophila Species and Gender (Outliers Removed)",
         x = "Species",
         y = "Cell Count",
         fill = "Gender") +
    theme_minimal() +
    theme(axis.text.x = element_text(angle = 45, hjust = 1))
} else {
  print("No data points remaining after outlier removal.")
}

```
RESULTS

1) ![image](https://github.com/saisamhithavajja/QMEE/assets/96578069/4cbf052a-9d00-4d5c-a64d-04e533835809)
2) ![image](https://github.com/saisamhithavajja/QMEE/assets/96578069/50fc0619-92d4-4f1b-afb9-9fbad211b00b)

