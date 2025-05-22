# USDA Food Nutrient Classifier

## Abstract
The goal of our project will be to create a food classifier that when given a set of some of the most common nutrients will be able to identify the food category a food with such nutrient values belongs in according to the USDA foundation and branded datasets. In the process, we will see which nutrients are the most informative for food classification.

## Research questions
Which nutrients are most informative when classifying foods?

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
    | map(
        select(
            ((.servingSizeUnit // "") | ascii_downcase) as $unit
            | ($unit != "ml" and $unit != "kcal" and $unit != "kj" and $unit != "sp gr")
        )
        | {
            dataType,
            brandedFoodCategory,
            description,
            ingredients,
            servingSize,
            servingSizeUnit,
            foodNutrients: (.foodNutrients
                | map({
                    name: .nutrient.name,
                    unitName: .nutrient.unitName,
                    amount
                })
            )
        }
    )' FoodData_Central_branded_food_json_2025-04-24.json > brandedFoods.json
```
## A tentative list of milestones for the project
TBD

- 2025-04-25 Project update 1 - Abstract & Research Q
- 2025-05-09 Project update 2 - Dataset filtered, first model tests
- 2025-05-19 Presentation
- 2025-05-23 Final submission

## Tasks

- [ ] Filter JSON dataset (drop "ml" serving size in branded foods) @saspoto
- [ ] JSON to pandas DataFrame @ATmachines
- [ ] Standardize units in DataFrame @ATmachines
- [ ] Compare classifier methods, pick one.
- [ ] Validation on random samples of branded foods
- [ ] Find most informative nutrients
- [x] CANCELLED: Create category mapping between datasets @mfmcl + someone pls


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

## Preprocessing

### Category mapping

Foundation foods: 28 categories
Branded foods: 350 categories

Target: 7 (8?) categories
- Meat & Fish
- Vegetables
- Fruits
- Nuts
- Dairy
- Legumes
- Grains
- (Oils?)

### Nutrient mapping

nutrient names across the datasets are not identical.

### Filtering

- Removed irrelevant information about each food
- Removed foods with samples measured in ml
- Removed all foods that aren't within our set categories
- Removed "dubious foods" - avocados, tomatos, plant-based milks, ... - which we will test later

### Standardizing units

The foundation foods dataset provides nutritional values per serving, as well as the mass and unit of each serving.
The branded foods include 

Some nutritional values might be measured in g, mg or μg. We standardize the serving sizes to be per 100g across both datasets.

## Model architecture

In: dict: { nutrient: value }
Out: str: Category

Our initial testing includes random tree and k-nearest neighbor models. We will compare performance and decide which type of model is better suited for further training on different nutrient combinations.

We will move forward training models on different combinations of nutrients, with the goal of finding the subset of nutrients that is most informative for classification.

## Testing

@ATMachines