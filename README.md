# FEWNet: Filtered Ensemble Wavelet Neural Network

This repository serves as the source code for the research paper titled "Forecasting CPI Inflation under Economic Policy and Geo-political Uncertainties" (https://arxiv.org/abs/2401.00249). The paper introduces a new approach called the **Filtered Ensemble Wavelet Neural Network (FEWNet)**, which is capable of generating accurate long-term predictions for CPI inflation. The idea utilizes a maximum overlapping discrete wavelet transform on the CPI inflation data to extract high-frequency and low-frequency signals. The wavelet-transformed series and filtered exogenous variables are fed into autoregressive neural networks downstream to get the ultimate ensemble forecast. Our theoretical analysis demonstrates that FEWNet effectively minimizes the empirical risk in comparison to individual, fully connected neural networks. Furthermore, we provide evidence that the real-time forecasts generated by the suggested algorithm, using a rolling-window approach, are notably superior in accuracy compared to standard forecasting methods used for comparison. In addition, we utilize conformal prediction intervals to measure the level of uncertainty linked to the projections produced by the suggested method. The outstanding performance of FEWNet can be ascribed to its ability to efficiently capture non-linearities and long-range relationships in the data via its flexible architecture.

* FEWNet integrates the maximum overlapping discrete wavelet transformation (MODWT) approach, economic filtering methods including the Hodrick-Prescott (HP) filter and Christiano-Fitzgerald (CF) filter, and the autoregressive neural network with exogenous variables (ARNNx).

* The code module [economic filters](https://github.com/ctanujit/FEWNet/blob/main/code/data_analysis/economic_filters.py) consists of the generic code to derive the trend and cycle components using HP and CF economic filters.

* The global characteristics of the CPI Inflation, EPU, and GPRC can be derived using [global_characteristics](https://github.com/ctanujit/FEWNet/blob/main/code/data_analysis/Global_characteristics.R) code module. This R code module calculates key statistical properties like skewness, kurtosis, long-range dependency, seasonality, stationarity, and non-linearity of the above-mentioned time series.

* The application of Wavelet Coherence Analysis (WCA) for analyzing the spatio-temporal association among variables is available in [WCA](https://github.com/ctanujit/FEWNet/blob/main/code/Wavelet_Coherence_Analysis/WCA_BRIC.R). The WCA methodology offers a valuable method for examining the interconnection and simultaneous movement of two non-stationary signals in the time-frequency domain.

* The code module to perform MODWT (Maximal Overlap Discrete Wavelet Transform) - based decomposition of the CPI Inflation series for the BRIC countries is available in [MODWT](https://github.com/ctanujit/FEWNet/blob/main/code/MODWT_decomposition/MODWT_decomposition.R)

* The code modules for all the baseline models: DeepAR (python script), ARNNx (R script), NBeatsx (python script), ARFIMAx (R script), SARIMAx (python script), ARIMAx (python script), Lasso Regression (LR) (python script), ARDL (python script), XGBoost (python script), TFT (python script), WARIMAx (R script) are available in [baseline_models](/code/baseline_models). These code modules share a generic framework for all algorithms highlighted above. The specific model and its hyper-parameters may vary a bit for different countries and forecast horizons. Please take note of the fact that all the deep learning models like NBeats, and TFT are implemented using **Darts** package, SARIMAx/ARIMAx models are implemented using __statsmodels__ and __pmdarima__ packages in Python, and XGBoost models have been implemented using __skforecast__ and __xgboost__ packages in python. The relevant libraries/packages for different algorithms are highlighted in the subsequent R/Python scripts.
  
* The overall architectural design of the algorithm for generating long-term forecasts is shared below:
![architecture_FEWNet](https://github.com/ctanujit/FEWNet/blob/main/images/architecture_FEWNet.jpg)

* The following image illustrates the key steps for generating the forecasts using the FEWNet algorithm:
![Pseudo_Code_FEWNet](https://github.com/ctanujit/FEWNet/blob/main/images/PseudoCode_FEWNet.jpg)

* This repository contains the datasets for the CPI Inflation numbers along with the EPU and GPRC indices for the BRIC countries, and these datasets have been used in the downstream model development exercise. In order to access the dataset, please refer to the [dataset](https://github.com/ctanujit/FEWNet/tree/main/dataset) section in GitHub. For more details about the datasets, please refer to the paper [paper](ArXiv link). In our study, we consider the monthly CPI numbers for BRIC countries from 2003-01 to 2021-11 released by FRED (Federal Reserve Bank of St. Louis) [FRED - BRAZIL CPI](https://fred.stlouisfed.org/series/BRACPIALLMINMEI). 

* The source code for the proposed FEWNet model can be accessed through [FEWNet](https://github.com/ctanujit/FEWNet/blob/main/code/FEWNet/FEWNet_BRIC_12M_24M_with_ConformalPI_calc_V2.R). The proposed framework consists of two hyper-parameters $(p,k)$, where $p$ denotes the number of lagged input observations and $k$ indicates the number of nodes in the hidden layer of the proposed EWNet model. The hyper-parameter $p$ is tuned by minimizing the Symmetric Mean Absolute Percentage Error (SMAPE) metric on the validation set. The optimized values of these hyper-parameters for different economies are highlighted in the paper and can be accessed through the source code for the FEWNet. Moreover, we also share a **framework for Grid-search CV** to determine the optimal values of the hyper-parameters (HPs) at the end of the source code. Interested users can experiment with the code for different ranges of values for the HPs or for other data types.

* The source code [FEWNet](https://github.com/ctanujit/FEWNet/blob/main/code/FEWNet/FEWNet_BRIC_12M_24M_with_ConformalPI_calc_V2.R) also contains a code module to derive the **conformalised prediction intervals (CPI)** for the forecasts generated by the FEWNet Model. Similarly, for other models, the CPIs can be calculated using the same code module. The ground truth, forecasted values of CPI inflation series along with the CPI of the proposed FEWNet and forecasts from the __top two baseline models__ are shared below for 24M forecast horizon:
![24M_forecasts_CPI](https://github.com/ctanujit/FEWNet/blob/main/images/forecasts_CPI_24M.jpg)

* A generic Python code module performance evaluation and MCB test are available in [evaluation](https://github.com/ctanujit/FEWNet/blob/main/code/Performance_evaluation/evaluation.py) and  [MCB](https://github.com/ctanujit/FEWNet/blob/main/code/Performance_evaluation/MCB_test.R). The dataset used for the MCB test can be accessed in [performance_evaluation](https://github.com/ctanujit/FEWNet/blob/main/dataset/performance_evaluation/mcb_test_alternative_12M_24M_paper_data.xlsx).

* The code module, along with the datasets, can be referred to reproduce the tables, charts, and final performance evaluation.

* To cite this work, please use

  Sengupta, Shovon, Tanujit Chakraborty, and Sunny Kumar Singh. "Forecasting CPI inflation under economic policy and geo-political uncertainties." arXiv preprint arXiv:2401.00249 (2023).

  @article{sengupta2023forecasting,
  
  title={Forecasting CPI inflation under economic policy and geo-political uncertainties},
  
  author={Sengupta, Shovon and Chakraborty, Tanujit and Singh, Sunny Kumar},
  
  journal={arXiv preprint arXiv:2401.00249},
  
  year={2023},
  
  note=https://arxiv.org/pdf/2401.00249.pdf}
  
## References
* <a id="1">[1]</a> [CPI Data: Brazil](https://fred.stlouisfed.org/series/BRACPIALLMINMEI)
* <a id="2">[2]</a> [CPI Data: Russia](https://fred.stlouisfed.org/series/RUSCPIALLMINMEI)
* <a id="3">[3]</a> [CPI Data: India](https://fred.stlouisfed.org/series/INDCPIALLMINMEI)
* <a id="4">[4]</a> [CPI Data: China](https://fred.stlouisfed.org/series/CHNCPIALLMINMEI)
* <a id="5">[5]</a> [EPU Data](https://www.policyuncertainty.com/index.html)
* <a id="6">[6]</a> [GPRC Data](https://www.policyuncertainty.com/gpr.html)
* <a id="7">[7]</a> Pratap, Bhanu and Sengupta, Shovon. Rbi working paper series no. 04 Macroeconomic forecasting in India: Does machine learning hold the key to better forecasts? 2019.
* <a id="8">[8]</a> Panja, Madhurima, Chakraborty, Tanujit, Kumar, Uttam and Liu, Nan. Epicasting: An ensemble wavelet neural network for forecasting epidemics. Neural Networks, 2023.
* <a id="9">[9]</a> Herzen, Julien, et al. "Darts: User-friendly modern machine learning for time series." Journal of Machine Learning Research 23.124 (2022): 1-6.
* <a id="10">[10]</a> Garza, Federico et al. "StatsForecast: Lightning fast forecasting with statistical and econometric models." PyCon, Salt Lake City, Utah, US (2022).
* <a id="11">[11]</a> Hyndman, Rob J., and Yeasmin Khandakar. "Automatic time series forecasting: the forecast package for R." Journal of statistical software 27 (2008): 1-22.
* <a id="12">[12]</a> Di Narzo, A. F., Aznarte, J. L., & Stigler, M. (2022). Package ‘tsDyn’.

