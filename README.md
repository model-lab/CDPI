# CDPI
Towards a Universal Approach for Predicting Variant Pathogenicity in Diverse Disease Landscapes
```R
# Loading necessary packages
library(caret)
library(dplyr)
library(randomForest)

```

    Loading required package: ggplot2
    
    Warning message:
    “package ‘ggplot2’ was built under R version 4.3.3”
    Loading required package: lattice
    
    Warning message:
    “package ‘lattice’ was built under R version 4.3.3”
    Warning message:
    “package ‘dplyr’ was built under R version 4.3.2”
    
    Attaching package: ‘dplyr’
    
    
    The following objects are masked from ‘package:stats’:
    
        filter, lag
    
    
    The following objects are masked from ‘package:base’:
    
        intersect, setdiff, setequal, union
    
    
    randomForest 4.7-1.1
    
    Type rfNews() to see new features/changes/bug fixes.
    
    
    Attaching package: ‘randomForest’
    
    
    The following object is masked from ‘package:dplyr’:
    
        combine
    
    
    The following object is masked from ‘package:ggplot2’:
    
        margin
    
    



```R
# function to load data from a specified file path
load_data <- function(file_path) {
        data <- read.csv(file_path,sep = "\t",header = TRUE)
        data[,2] <- as.factor(data[,2])
        return(data)
}

# Load the universal model trained with gnomAD v2.1.1 dataset
load("/path/to/file/random_forest_model.RData")

# Load test data, also see test dataset "test.dataset"
test_data <- load_data("/path/to/file/test.data")

# Columns V4 - V22 are "AF" features; Columns V23 - V41 are "DC" features
selected_features <- c("V13","V32","V35","V29","V26","V33","V38","V31","V30","V23","V34","V41","V36","V27","V28","V37","V39","V40","V24","V25","V12","V14","V15","V9","V18","V21","V10","V20","V8","V7","V19","V5","V4","V11","V6","V17","V22","V16")
test_data_x <- test_data[, selected_features]

```


```R
# Prediction
predictions <- predict(model, test_data_x)
predicted_prob <- predict(model, test_data_x, type="prob")

# Save the prediction results and comparison results to a file.
save_predictions_and_comparisons <- function(test_data, predictions, predicted_prob, output_file) {
        true_labels <- test_data[, 2]
        mutation_id <- test_data[, 1]
        comparison <- data.frame(Mutation_ID = mutation_id, True_Label = true_labels, Prediction = predictions, Prob = predicted_prob)
        # Calculate Accuracy
        accuracy <- sum(comparison$True_Label == comparison$Prediction) / nrow(comparison)
        print(paste("Test Accuracy:", accuracy * 100, "%"))

        # Save the results of the comparison to a file
        write.csv(comparison, file=output_file, row.names=FALSE)
}

save_predictions_and_comparisons(test_data, predictions, predicted_prob, "/path/to/file/test.data.predicted_results")
```

    [1] "Test Accuracy: 95.9183673469388 %"




# Cite
Chen X, Yu X. Toward a universal approach for predicting variant pathogenicity in diverse disease landscapes. J Genet Genomics. 2024 Jul 22:S1673-8527(24)00193-0. doi: 10.1016/j.jgg.2024.07.015

