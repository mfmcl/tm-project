# USDA Food Nutrient Classifier

## Abstract
The goal of our project will be to create a food classifier that when given a set of some of the most common nutrients will be able to identify the food category a food with such nutrient values belongs in according to the USDA foundation and branded datasets. In the process, we will see which nutrients are the most informative for food classification.

## Research questions
What nutrients are needed in order to perform the best classification, as in what nutrients are the most informative?

## Dataset
https://test.fdc.inonde.io/download-datasets

The foundational foods dataset will be used to train the classifier. We will need to filter it so we remove less informative nutrient values (there are a lot). Also, remove all of the information regarding the sampling (there are 1000 lines solely dedicated to samples of hummus). Can be downloaded as JSON or CSV files, 6MB.

**Command to parse data set**:
```
jq '.FoundationFoods
    | map({
        foodClass,
        description,
        foodNutrients: (.foodNutrients
            | map({
                name: .nutrient.name,
                unitName: .nutrient.unitName,
                max,
                min,
                median,
                amount
            })
        )
    })' FoodData_Central_foundation_food_json_2025-04-24.json > foundationFoods.json
```

The branded foods dataset will tentatively be used to get testing data for the classifier (check out a brand of hummus' nutrient values, plug it into the classifier and see if it predicts the category as Legumes and Legume Products, and then see if it predicts it correctly as hummus). Can be downloaded as JSON or CSV files, 3GB.

**Command to parse data set**:
```
jq '.BrandedFoods
    | map({
        foodClass,
        description,
        foodNutrients: (.foodNutrients
            | map({
                name: .nutrient.name,
                unitName: .nutrient.unitName,
                max,
                min,
                median,
                amount
            })
        ),
        ingredients,
        servingSize,
        servingSizeUnit
    })' FoodData_Central_branded_food_json_2025-04-24.json > brandedFoods.json

```
## A tentative list of milestones for the project
TBD

- 2025-04-25 Project update 1 - Abstract & Research Q
- 2025-05-09 Project update 2 - Dataset filtered, first model tests
- 2025-05-19 Presentation
- 2025-05-23 Final submission

## Tasks

- [ ] Filter JSON dataset
- [ ] JSON to pandas DataFrame
- [ ] Standardize units in DataFrame
- [ ] Try models (individually) (fuck around and find out)
- [ ] 
- [ ] 


## Documentation
> This can be added as the project unfolds. You should describe, in particular, what your repo contains and how to reproduce your results.

### Repo structure
```
data/ - not tracked, place to store datasets
notebooks/ - Jupyter notebooks
src/ - pure python scripts
```

### Conventions

Python 3.10
Files and function names: snake_case
Indentation: tabs (1 tab = 4 spaces)

