# 📊 **SO2 Concentration Analysis for Tehran: Pre and Post COVID-19** 📊

This project analyzes the **SO2 (Sulfur Dioxide) concentration** in **Tehran** before and after the COVID-19 pandemic, using data from the **Copernicus Sentinel-5P** satellite. The analysis compares the SO2 concentration levels between **April 2019** (before the pandemic) and **January to December 2021** (after the pandemic began). The project aims to visualize the impact of reduced human activities during the COVID-19 lockdown on air quality in Tehran.

---

## 🚀 **Key Features**

- **SO2 Concentration Analysis**: Visualize and compare the **SO2 concentration** in Tehran before and after the COVID-19 pandemic.
- **Data Source**: Utilizes **Copernicus Sentinel-5P** data to provide **SO2 column number density** values.
- **Comparison Periods**: 
  - **Before COVID-19**: April 2019
  - **After COVID-19**: January to December 2021
- **Visualization**: Display the SO2 concentration maps using a **color palette** to show varying levels of SO2.
- **Export Results**: Export the final processed SO2 image to Google Drive for further analysis.

---

## 🧑‍🔬 **Getting Started**

### 1. **Sign up for Google Earth Engine (GEE)**

If you don't already have a **Google Earth Engine (GEE)** account, you can sign up [here](https://signup.earthengine.google.com/).

### 2. **Run the Script**

1. **Open the [Google Earth Engine Code Editor](https://code.earthengine.google.com/)**.
2. Paste the provided script into the editor.
3. **Run** the script to visualize **SO2 concentration** levels before and after the COVID-19 pandemic in **Tehran**.

---

## 🔧 **How It Works**

### 1. **Define Region of Interest (ROI)**

The region of interest is defined as a polygon covering the city of **Tehran**, Iran. This is the area where SO2 concentration will be analyzed:

```javascript
var tehranGeometry = ee.Geometry.Polygon([
  [
    [51.2158203125, 35.7936099733036],
    [51.2158203125, 35.02999636902566],
    [52.381591796875, 35.02999636902566],
    [52.381591796875, 35.7936099733036]
  ]
]);
```

### 2. **Filter and Select SO2 Data**

The **Copernicus Sentinel-5P** dataset is used for **SO2 concentration** measurements. The script filters the dataset based on the defined region and two time periods:

- **Before COVID-19** (April 2019)
- **After COVID-19** (January to December 2021)

```javascript
var SO2_concentration = ee.ImageCollection('COPERNICUS/S5P/OFFL/L3_SO2')
  .filterBounds(tehranGeometry)
  .select('SO2_column_number_density');
  
var before_SO2 = SO2_concentration.filterDate(before_covid_start, before_covid_end);
var after_SO2 = SO2_concentration.filterDate(after_covid_start, after_covid_end);
```

### 3. **Calculate Median SO2 Values for Each Period**

The **median** of the SO2 data is calculated for both the **before** and **after** periods, providing a representative value for the concentration over the specified time frames:

```javascript
var beforeSO2 = before_SO2.median().clip(tehranGeometry);
var afterSO2 = after_SO2.median().clip(tehranGeometry);
```

### 4. **Visualization of SO2 Concentration**

The resulting **SO2 concentration** images for **2019** (before COVID-19) and **2021** (after COVID-19) are displayed using a color palette that represents varying levels of SO2:

- **Black**: No SO2
- **Red**: High SO2 concentration

```javascript
Map.addLayer(beforeSO2, {
  max: 0.0005,
  min: -0.0005,
  palette: ['black', 'purple', 'blue', 'green', 'yellow', 'orange', 'red']
}, 'SO2 Total - 2019', true, 0.6);

Map.addLayer(afterSO2, {
  max: 0.5,
  min: 0,
  palette: ['black', 'purple', 'blue', 'green', 'yellow', 'orange', 'red']
}, 'SO2 Total - 2020', true, 0.6);
```

### 5. **Time Series Chart for SO2 Concentration**

A time series chart is generated to show the **mean SO2 concentration** over the selected region (Tehran) for the **before COVID** period:

```javascript
print(ui.Chart.image.series(before_SO2, tehranGeometry, ee.Reducer.mean(), 100));
```

### 6. **Export Results**

The **after COVID-19** SO2 concentration image is exported as a **GeoTIFF** to **Google Drive** for further analysis:

```javascript
Export.image.toDrive({
  image: afterSO2,
  description: 'NDVI1',
  region: tehranGeometry,
  folder: 'earthengine',
  scale: 10,
  maxPixels: 1e10
});
```

---

## 📊 **Outputs**

### 1. **SO2 Concentration Map**

The script generates two maps:

- **Before COVID-19 (April 2019)**: Displays the SO2 concentration before the pandemic.
- **After COVID-19 (2021)**: Displays the SO2 concentration after the pandemic.

### 2. **Time Series Chart**

A **chart** that shows the **mean SO2 concentration** for **Tehran** before COVID-19.

### 3. **Exported Data**

- **SO2 concentration image** for **2021** is exported as a **GeoTIFF**.
- **Mean SO2 concentration** chart for **April 2019** is printed in the console.

---

## 📈 **Future Enhancements**

- **Extended Time Period**: Expand the analysis to include more years to understand long-term trends in air quality.
- **Other Pollutants**: Include other atmospheric pollutants such as **NO2** and **CO** to provide a more comprehensive air quality analysis.
- **Interactive Dashboard**: Build an interactive dashboard for users to explore air quality data for different regions and time periods.

---
![ee-chart (3)](https://github.com/user-attachments/assets/c8d61a25-5450-4231-824c-2f4eae2b83db)
![Snapshot_۲۵-۰۴-۲۸_۰۸-۲۰-۰۰](https://github.com/user-attachments/assets/6df9d2c2-f220-4c44-854e-d74fcc67fc1e)

