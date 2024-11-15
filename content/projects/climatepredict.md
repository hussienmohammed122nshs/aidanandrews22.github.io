This project is a Rust-based application that uses machine learning techniques to predict global temperature changes based on CO2 emissions. It implements three regression models:

1. **Linear Regression**
2. **Polynomial Regression**
3. **Random Forest Regression**

The project leverages Rust’s efficiency to process large datasets and includes CUDA support for GPU acceleration.

---

## Technical Implementation

### Regression Models

#### 1. Linear Regression

Linear regression models the relationship between CO2 emissions and temperature change using a simple linear relationship. It computes the slope (β) and intercept (α) of the regression line.

```rust
fn linear_regression(x: &Tensor, y: &Tensor) -> Result<(f64, f64), Box<dyn Error>> {
    // Core computation of linear regression
    let x_mean = x.mean(0)?.mean(0)?.to_scalar::<f64>()?;
    let y_mean = y.mean(0)?.mean(0)?.to_scalar::<f64>()?;

    let x_diff = x - x_mean;
    let y_diff = y - y_mean;

    let numerator = (&x_diff * &y_diff)?.sum_all()?.to_scalar::<f64>()?;
    let denominator = (&x_diff * &x_diff)?.sum_all()?.to_scalar::<f64>()?;

    let slope = numerator / denominator;
    let intercept = y_mean - slope * x_mean;

    Ok((slope, intercept))
}
```

---

#### 2. Polynomial Regression

Polynomial regression extends linear regression by capturing nonlinear relationships. The model fits a polynomial curve of a specified degree to the data.

```rust
fn polynomial_regression(x: &DMatrix<f64>, y: &DMatrix<f64>, degree: usize) -> Result<Vec<f64>, Box<dyn Error>> {
    let x_poly = build_polynomial_features(x, degree);
    let xt = x_poly.transpose();
    let xt_x = &xt * &x_poly;
    let xt_y = xt * y;

    let chol = nalgebra::linalg::Cholesky::new(xt_x).ok_or("Cholesky decomposition failed")?;
    let beta = chol.solve(&xt_y);

    Ok(beta.column(0).iter().cloned().collect())
}
```

---

#### 3. Random Forest Regression

Random forest regression uses ensemble learning to make predictions by training multiple decision trees.

```rust
let rf_params = RandomForestRegressorParameters::default();
let rf = RandomForestRegressor::fit(&x_train, &y_train, rf_params)?;

let preds = rf.predict(&x_test)?;
let mse = mean_squared_error(&y_test, &preds);
let rmse = mse.sqrt();
```

---

### Data Processing

The dataset is processed from a CSV file containing emissions and temperature data.

```rust
fn process_data(data: &str) -> Result<((Tensor, Tensor), (Vec<f64>, Vec<f64>)), Box<dyn Error>> {
    let file = std::fs::File::open(data)?;
    let mut reader = ReaderBuilder::new().has_headers(true).from_reader(file);

    let mut emissions = Vec::new();
    let mut temps = Vec::new();

    for result in reader.records() {
        let record = result?;
        emissions.push(record[1].parse::<f64>()?);
        temps.push(record[2].parse::<f64>()?);
    }

    let device = Device::new_cuda(0)?;
    let emissions_tensor = Tensor::from_slice(&emissions, (emissions.len(), 1), &device)?;
    let temps_tensor = Tensor::from_slice(&temps, (temps.len(), 1), &device)?;

    Ok(((emissions_tensor, temps_tensor), (emissions, temps)))
}
```

---

### Evaluation Metrics

The models are evaluated using:

- **Mean Squared Error (MSE)**: Average squared differences between predictions and actual values.
- **Root Mean Squared Error (RMSE)**: Square root of MSE for interpretability.

---

### Interactive Mode

The application includes an interactive mode where users can input CO2 emissions or years to predict temperature changes.

```rust
loop {
    println!("Enter emission value (g CO2/kWh) or type 'exit' to quit:");
    let mut emission_input = String::new();
    io::stdin().read_line(&mut emission_input)?;
    if emission_input.trim().eq("exit") {
        break;
    }
    let emission_value: f64 = emission_input.trim().parse()?;
    let predicted_temp = slope * emission_value + intercept;
    println!("Predicted temperature change: {:.2} °C", predicted_temp);
}
```

---

### Results

#### Visualizations

![Global Temperature](/assets/Global_Temperature.png)

1. **Linear Regression Fit**
   - A scatter plot of the data points with the regression line overlay.
   
2. **Polynomial Regression Curve**
   - Visualization of the polynomial fit for nonlinear relationships.

3. **Random Forest Predictions**
   - Actual vs. Predicted scatter plot.

#### Performance

- **Linear Regression**: Fast but struggles with nonlinear relationships.
- **Polynomial Regression**: Effective for curves but may overfit for high-degree polynomials.
- **Random Forest Regression**: Handles complex patterns but requires tuning.

---

### Libraries and Tools

- **Rust**: Core language for implementation.
- **Candle**: Tensor computation library.
- **NALgebra**: Linear algebra operations.
- **SmartCore**: Random forest regression.
- **CUDA**: GPU acceleration.

---

### Datasets
**Carbon Emission Datasets:** \
Dataset 1: https://www.kaggle.com/datasets/patricklford/global-co-emissions \
Dataset 2: https://www.kaggle.com/datasets/soheiltehranipour/co2-dataset-in-usa \
\
**Temperature Datasets:** \
Dataset 1: https://climate.nasa.gov/vital-signs/global-temperature/?intent=121 \
Dataset 2: https://www.kaggle.com/code/ash316/is-the-mercury-rising/input \
\
**Carbon Behavior Dataset:** \
https://www.kaggle.com/datasets/dumanmesut/individual-carbon-footprint-calculation?resource=download   
