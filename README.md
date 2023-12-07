# Junction Temperature Prediction for Photovoltaic Inverter Switches in Brazil using AWS environment
The objective of this GitHub is to present a solution developed in the Amazon environment to estimate the average temperature of the junction of electronic keys.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/4eb25168-161b-4193-a9ff-4b39f373adb3" width="100">
</p>

**Federal Institute of Education, Science and Technology of São Paulo - IFSP**, Campinas Campus

**Team**: \
Andrei Carvalho Ribeiro (CP3021912)\
Francisco Victor Esteves Lemos (CP3021769) \
João Lucas de Souza Silva (CP3021963)

## 1 - Introduction


The global output of energy from photovoltaic (PV) power systems has seen a substantial increase, soaring from 131.72 terawatt-hours (TWh) in 2013 to approximately 1,020.3 TWh in 2021, as reported by [1]. This surge has precipitated concerns regarding the reliability of PV inverters. Unreliable devices could contribute to the generation of electronic waste on a global scale—an especially undesired effect for what is considered a clean energy source. 
 
In the internal workings of inverters, it is predominantly the capacitors and semiconductor switches that exhibit the highest failure rates. The performance and longevity of these components are substantially influenced by two key thermal parameters: the junction temperature (T_j) and the hot-spot temperature (T_h). This correlation has been substantiated by various studies, including those by [2], [3], [4], and [5].


Concurrently, each country exhibits its own climatic peculiarities, leading to variations in environmental factors such as temperature based on location. Consequently, it becomes crucial to study and attempt to predict the temperature of key components like switches. By doing so, in conjunction with other research, one can estimate the reliability of PV inverters during operation in each specific country. In this project, two datasets of meteorological data from Brazil were utilized to forecast the junction temperature of switches within the Amazon environment.


## 2 - Description of Photovoltaic Systems connected to the electrical grid and PV Inverter

A grid-connected PV system comprises two main components: PV modules and a PV inverter. The photovoltaic modules convert solar energy into electrical energy, which is then processed by the inverter, as illustrated in Figure 1.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/84260ddc-b6b7-4b62-8f3c-68ca728ed774ouzasilva/IFSPproject/edit/main/README.md" width="400">
</p>

Figure 1 - Example of Components of a PV System with the Electricity Grid [6]


Generally, PV systems are designed to last for 25 years, during which time at least one inverter replacement is typically required. However, in Brazil, we do not yet have systems that have been operational for such an extended duration. Throughout this period, the study of reliability is essential to provide investors with greater confidence and assurance in the system's performance and longevity.

In Figure 2, we present the model of a typical PV inverter. The inverter usually consists of two stages. The first stage is a DC-DC converter (Direct Current to Direct Current) which aims to extract maximum power from the PV modules and adapt the energy for the second stage. In the second stage, there is a DC-AC converter (Direct Current to Alternating Current) that adjusts the standards to enable connection to the electrical grid and to supply the loads. In the first stage, there typically is an IGBT (Insulated Gate Bipolar Transistor) switch, and in the second stage, there are four IGBT switches [7]. Thus, switches are components that are also present in multiple units in the inverter as a whole, highlighting again the importance of evaluating the temperature of these switches.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/ca9758ec-210d-4adc-9d6b-8b5951f28b46" width="800">
</p>

Figure 2 - Example of a typical PV Inverter.

Key points highlighting the necessity of this study also include: 
- **Prevention of Premature Failures**: High temperatures can lead to physical damage in components, significantly reducing the lifespan of photovoltaic inverters.
- **Operational Efficiency**: Monitoring and controlling the junction temperature ensures that the IGBTs operate within their ideal efficiency ranges. This is essential for maximizing the overall efficiency of the photovoltaic system.
- **System Safety**: Elevated temperatures in IGBTs can pose safety risks, including the risk of fire.
- **Reduction in Maintenance Costs**: Closely tracking junction temperature can help predict and prevent failures, reducing costs associated with maintenance and component replacement.
- **Inverter Design Optimization**: Understanding the implications of junction temperature can lead to improvements in the design of photovoltaic inverters, resulting in more robust and efficient systems.

## 3 - Methodology

For the methodology, data was initially collected from one database, for Teresina-Piauí, with data collection every 5 minutes to be used in simulations and models. This database was later uploaded to the Amazon environment and processed within it. The processing mainly involved converting this data to an hourly time scale and obtaining the daily average, to be applied in a subsequent phase.

The next step involved simulating the inverter with switches in the PSIM software to estimate the temperature. PSIM software is one of the most robust power electronics software for this purpose, with real data on losses from the switches and other components. This simulation made it possible to observe the temperature behavior at the junction of the switches. In this simulation, an inverter topology with six switches (three-phase half-bridge converter) was used. Refer to Figure 3.


<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/b2bba95b-93a8-4ea6-88a8-5d793bec47ee" width="800">
</p>

Figure 3 - PV Inverter in PSIM [8].

Subsequently, with the characteristics of the switches obtained in the simulation and data from a city, it is possible to apply ML models that allow the prediction of the thermal behavior of these switches. Figure 4 illustrates the proposal.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/cffc3e5b-0971-4504-a09a-fa7e4c8d18e4" width="800">
</p>

Figure 4 - Flowchart for thermal analysis of a DC/AC converter. The inputs are the yearly mission profile and the component characteristics extracted from the PSIM’s device library or datasheet [8].


SOLCAST provides yearly data collected at 5-minute intervals spanning from December 30, 2017, to December 31, 2018, specifically for Teresina, PI. This comprehensive dataset encompasses 20 features (Table I), including parameters such as Direct Horizontal Irradiance (DHI), Air Temperature, and more, totaling a significant 105,676 samples. This detailed information offers valuable insights into the climatic and environmental conditions over this period, enabling comprehensive analyses and informed decision-making in various fields and industries.

<p align="center">
  Table I - Features description
</p>


| air_temp           | °C    | Air Temperature The air temperature at 2 meters above surface level.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| albedo             | 0 - 1 | Albedo Daily The average daytime surface reflectivity of visible light, expressed as a fractional value between 0 and 1. 0 represents complete absorption. 1 represents complete reflection. This is an interpolated daily average value, therefore does not capture any diurnal angular dependence of reflectivity.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| azimuth            | °     | Solar Azimuth Angle The angle between the horizontal direction of the sun, and due north, with negative angles eastwards and positive values westward . Varies from -180 to 180. A value of -90 means the sun is in the east, 0 means north, and 90 means west.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| dhi                | W/m2  | Clearsky Diffuse Horizontal Irradiance (DHI) The diffuse irradiance received on a horizontal surface (if there are no clouds). Also referred to as Diffuse Sky Radiation. The diffuse component is irradiance that is scattered by the atmosphere, in the clear sky scenario, (i.e. no water or ice clouds in the sky).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| cloudopacity       | %     | Cloud Opacity The attenuation of incoming sunlight due to cloud. Varies from 0% (no cloud) to 100% (full attenuation of incoming sunlight).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| dewpointtemp       | °C    | Dewpoint Temperature The dewpoint temperature at 2 meters above surface level.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| dhi                | W/m2  | Diffuse Horizontal Irradiance (DHI) The diffuse irradiance received on a horizontal surface. Also referred to as Diffuse Sky Radiation. The diffuse component is irradiance that is scattered by the atmosphere.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| dni                | W/m2  | Direct Normal Irradiance (DNI) Irradiance received from the direction of the sun (10th percentile clearness). Also referred to as beam radiation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ghi                | W/m2  | Global Horizontal Irradiance (GHI) Total irradiance on a horizontal surface. The sum of direct and diffuse irradiance components received on a horizontal surface.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| gti                | W/m2  | Global Tilted Irradiance (GTI) Total irradiance received on a surface with defined tilt and azimuth (sum of direct, diffuse and reflected components), fixed or tracking. Use Utility Scale Sites for the most accurate GTI, because it is based on detailed PV system specifications including detailed tracker behaviour. When requesting GTI by latitude and longitude only, you'll need to specify array_type ("fixed" or "horizontal_single_axis"). If you specify an array_type of "fixed", you can optionally specify a tilt and azimuth, or else we assume default values (equatorward-facing azimuth, latitude-dependent tilt). If you specify an array_type of "horizontal_single_axis", you can specify the tracker axis angle as azimuth, or else we assume a default value of zero (i.e. tracker axis runs north-south). Tilt is ignored if provided for an array_type of "horizontal_single_axis". |
| precipitable_water | kg/m2 | Precipitable Water Precipitable water of the entire atmospheric column.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| relative_humidity  | %     | Relative Humidity The relative humidity at 2 meters above ground level. Relative humidity is the amount of water vapour as a percentage of the amount needed for saturation at the same temperature. A value of 50% means the air is 50% saturated.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| surface_pressure   | hPa   | Surface Pressure The air pressure at surface level.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| snow_water         | cm    | Snow Water Equivalent The snow depth liquid water equivalent.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| wind_direction_10m | °     | Wind Direction 10m Wind direction at 10m above ground level. Zero means true north. Varies from 0 to 360. A value of 270 means the wind is coming from the west                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| wind_speed_10m     | m/s   | Wind Speed 10m Wind speed at 10m above ground level.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| zenith             | °     | Solar Zenith Angle The angle between the direction of the sun, and the zenith (directly overhead). The zenith angle is 90 degrees at sunrise and sunset, and 0 degrees when the sun is directly overhead.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

For instance, in Figure 5, there are "Plane of Array (POA) irradiance [W/m²]" and "Air temperature [ºC]" listed.



<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/7b3da74f-b434-468b-8abe-99f6bcf54cfe" width="600">
</p>

Figure 5 - Plane of Array (POA) irradiance [W/m²] and Air temperature [ºC]


Following that, in Figure 6, the two chosen days with the highest and lowest average temperatures display the simulated junction temperature behavior in PSIM. The graph illustrates that on the hottest day, the behavior differs throughout the hours of usage.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/c6fae9a9-4de3-4b7a-ace0-d08c56d7a6ae" width="800">
</p>
Figure 6 - Example of simulation of SOLCAST + PSIM’s Thermal Module.

## 4 - Methodology in AWS environment

In Figure 7, the proposed architecture implemented in the AWS environment is depicted, utilizing SOLCAST tables and PSIM simulations, culminating in prediction. Meanwhile, Table II showcases all the described steps.

The process involves eight pivotal steps to execute the project seamlessly. It commences by accessing the AWS Cloud through the Learner Lab, establishing an AWS Cloud9 environment for work, and creating an EC2 instance within it. Subsequently, three S3 buckets are set up in the us-east-1 region, designated for various data purposes. Original datasets find their place in the 'data-ifsp-1000' bucket, managed and stored via the Command Line Interface. AWS Glue initiates a database named 'ifsp_db' and configures a crawler 'ifspcrawler' to organize data, while Athena Query Editor refines queries and stores results in designated buckets. Visualizations using QuickSight are created, and refined files are prepared for utilization in PSIM software, with results subsequently uploaded to the appropriate bucket. Finally, a Sagemaker environment is crafted for in-depth data processing, leveraging Jupyter notebooks to apply Machine Learning techniques, making the results available for visualization in QuickSight.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/2029fe10-7d93-4bd2-9c2c-ff1a46859f8e" width="800">
</p>

Figure 7 - Proposed AWS Environment Architecture

<p></p>

<p align="center">
  Table II - Steps in the AWS Environment.
</p>

|     Task      |                                                                                  Details                                                                                |
|:-------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|        1      |     Accessing   AWS Cloud via the Learner Lab and creating an AWS Cloud9 environment.                                                                                   |
|        2      |     Creating   a new EC2 environment with a t2.large instance.                                                                                                          |
|        3      |     Creating   three S3 buckets: data-ifsp-1000, query-ifsp-15020, and resultados-ifsp-1000.   All located in the us-east-1 region.                                     |
|        4      |     The   data-ifsp-1000 bucket will receive the original datasets.                                                                                                     |
|        5      |     Using   the Command Line Interface (CLI) to upload files and store them in   data-ifsp-1000.                                                                        |
|       6a      |     Creating   a database named ifsp_db in   AWS Glue. Setting up a crawler named ifspcrawler   using the IAM role LabRole.   Crawler results are stored in ifsp_db.    |
|       6b      |     Configuring   Athena Query Editor data outputs to the query-ifsp-15020 bucket. Performing   queries on the dataset and creating views.                              |
|       7a      |     Saving   the final table generated by Athena in the resultados-ifsp-1000 bucket.                                                                                    |
|       7b      |     Using   Athena results for QuickSight   visualization.                                                                                                              |
|       7c      |     Downloading   refined files for use in the PSIM software.                                                                                                           |
|       7d      |     Uploading   PSIM results to the resultados-ifsp-1000 bucket.                                                                                                        |
|       8a      |     Creation of an environment in Sagemaker to use the Jupyter notebook for reading, processing, and applying Machine Learning. The domain IFSP-SM was created with default settings and IAM role LabRole. Also, a user profile named lesfmv was created to use the Studio. In the SageMaker Studio environment, a JupyterLab environment called vianjo was created. It has 100GB of storage with the ml.t3.large instance.                                          |
|       8b      |     Making   results available for visualization in QuickSight.                                                                                                         |


During the work, several queries and manipulations were carried out in Athena and can be seen at: https://github.com/joaolucasdesouzasilva/IFSPproject/blob/main/SQL.md

Furthermore, with the completion of the project, two notebooks with codes also used for processing, manipulation and/or testing were added to this repository.

## 5 - Discussion and Results

### 5.1 Exploratory Analysis

The first step of the exploratory analysis involved generating boxplots (Figura 8 and 9), revealing the significance of data preprocessing. The removal of zeros notably enhanced the clarity of the boxplot. However, for temperature, the boxplot highlighted several outliers.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/e5552dee-aa69-4a25-b5c3-ed7886ed144c" width="400">
</p>
Figure 8 - Boxplots for irradiance (left), irradiance removing zero (center), and ambient temperature (right).

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/472c6864-d42a-48bb-8c53-7464d1937783" width="400">
</p>

Figure 9 - Boxplots for junction temperature (left) and its swing (right).


Later, the Pearson correlation matrix was constructed. The Pearson correlation matrix is crucial because it helps unveil relationships between variables. This analysis revealed that the junction temperature (Tj) had a stronger association with irradiance while also displaying a direct correlation with ambient temperature, as shown in Figure 10.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/68a99830-013b-4880-b42d-c5ee543e391e" width="500">
</p>
Figure 10 - Pearson correlation matrix.

### 5.2 Applying ML in SageMaker

Three distinct models were employed to forecast junction temperature in switches: Linear Regression, Decision Tree, and Multi-layer Perceptron (MLP).

Linear Regression, known for its simplicity and ease of application with new data points, provides equations for both target variables. Despite its user-friendly nature, its performance, measured by metrics like R2 and MSE, tends to be inferior to other models. This is particularly evident when attempting to capture the oscillations in junction temperature, as linear dynamics struggle to describe this phenomenon adequately.

The Decision Tree model offers insightful visualization capabilities, allowing us to comprehend the regression process by examining variables and their respective boundaries. However, as the depth of the tree increases beyond 10 levels, the model's performance diminishes notably, resulting in a decline in quality when validated against the test dataset.

Lastly, the Multi-layer Perceptron (MLP), a classical neural network, was explored. Various architectures were tested by manipulating the number of hidden layers and neurons distributed within them. Additionally, diverse activation functions such as ReLU and the sigmoid function were employed during experimentation. This approach aimed to leverage the flexibility of neural networks to capture the intricate patterns in junction temperature dynamics.


<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/b057ce23-66db-4f23-91d9-cb62761cc447" width="400">
</p>
Figure 11 - Method results with a specific seed (42).

## 6 - Final Considerations and Future Works

In the exploratory analysis, it was observed that junction temperature is linked to irradiance and ambient temperature. We applied three models: (i) linear regression, (ii) decision tree, and (iii) multilayer perceptron. Linear regression is simple and easy to use with new samples because it provides equations for both targets. Its performance (R2 and MSE) was lower compared to other models. It makes sense since the junction temperature oscillation is not well described by linear dynamics. The decision tree permits us to interpret the regression process by examining the variables and their threshold. An interesting observation for this model is that as the depth increases, the model becomes more flexible, but when depth exceeds 10, the decision tree starts to lose quality on the validation set (overfitting). The MLP is a classical neural network that permits us to perform several tests considering different architectures by varying the number of hidden layers and the number of neurons distributed among them. We also conducted tests with different activation functions such as ReLU and the sigmoid function. 

Our approach to big data did not primarily focus on cost reduction or the optimal selection of services. Instead, our decisions regarding services, features, and configurations were heavily influenced and backed by the insights gained from laboratory experiments and the capstone project. However, in the realm of corporate or industrial projects, establishing clearer governance and defining access to various project stages is crucial to mitigate conflicts and role shadowing. Additionally, robust measures need implementation to safeguard data access and maintain security.

In future activities, Amazon Kinesis will be employed for real-time data ingestion, allowing the collection and processing of diverse data sets, such as inverter readings and plant monitoring data. AWS IoT Core will be utilized to gather and manage sensors within PV plants. AWS Lambda functions will serve as a key component for processing real-time data, enabling tasks such as computing performance metrics and identifying anomalies. Moreover, Amazon DynamoDB will be implemented as a fully managed, scalable NoSQL database, adept at real-time structured data storage and efficiently managing high write and read rates.

## References

[1] IRENA (2023), “Renewable Energy Statistics 2023”, International Renewable Energy Agency, Abu Dhabi. [Online]. Available: https://www.irena.org/Publications/2023/Jul/Renewable-energy-statistics-2023\
[2] I. Vernica, “Model-based Reliability Analysis of Power Electronic Systems”, PhD thesis, Aalborg Universitetsforlag, 2019.\
[3] H. Wang, K. Ma, and F. Blaabjerg, “Design for Reliability of Power Electronic Systems”, in IECON 2012 - 38th Annual Conference on IEEE Industrial Electronics Society, 2012, pp. 33–44.\
[4] P. Hacke et al., “A status review of photovoltaic power conversion equipment reliability, safety, and quality assurance protocols”, Renewable and Sustainable Energy Reviews, vol. 82, pp. 1097-1112, Feb. 2018. doi: 10.1016/j.rser.2017.07.043.\
[5] A. Golnas, “PV system reliability: An operator’s perspective”, IEEE Journal of Photovoltaics, vol. 3, no. 1, pp. 416-421, 2013. doi: 10.1109/JPHOTOV.2012.2215015\
[6] J. L. de Souza Silva, M. M. Cavalcante, S. B. Martins, E. J. da Silva, T. A. dos S. Barros, and M. G. Villalva, "Data-Driven Analysis of Solar Photovoltaic Systems: Correlation and Distribution Patterns," presented at the Brazilian Congress on Electrical Power Systems (COBEP), Campinas, Brazil, 2023. \
[7] J. L. de Souza Silva. (2020). Estudo e Desenvolvimento Experimental de Otimizadores de Potência para Sistemas Fotovoltaicos Conectados à Rede Elétrica. Masters dissertation, University of Campinas, Brazil.\
[8] A. C. Ribeiro, et al. “Junction Temperature Prediction: A Multi-Featured Dataset Approach”, in 8th IEEE International Conference on Southern Power Electronics Conference (SPEC), 2023, Florianopolis, Brazil. 





