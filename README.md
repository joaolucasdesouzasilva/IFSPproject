# Junction Temperature Prediction for Photovoltaic Inverter Switches in Brazil using AWS environment
The objective of this GitHub is to present a solution developed in the Amazon environment to estimate the average temperature of the junction of electronic keys.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/4eb25168-161b-4193-a9ff-4b39f373adb3" width="100">
</p>

**Federal Institute of Education, Science and Technology of São Paulo - IFSP** , Campinas Campus

**Team**: \
Andrei \
Francisco Victor \
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

Figure 2 - Example of typical PV Inverter.

Key points highlighting the necessity of this study also include: 
- **Prevention of Premature Failures**: High temperatures can lead to physical damage in components, significantly reducing the lifespan of photovoltaic inverters.
- **Operational Efficiency**: Monitoring and controlling the junction temperature ensures that the IGBTs operate within their ideal efficiency ranges. This is essential for maximizing the overall efficiency of the photovoltaic system.
- **System Safety**: Elevated temperatures in IGBTs can pose safety risks, including the risk of fire.
- **Reduction in Maintenance Costs**: Closely tracking junction temperature can help predict and prevent failures, reducing costs associated with maintenance and component replacement.
- **Inverter Design Optimization**: Understanding the implications of junction temperature can lead to improvements in the design of photovoltaic inverters, resulting in more robust and efficient systems.

## 3 - Methodology

For the methodology, data was initially collected from two databases, one for Teresina-Piauí, and another for Curitiba-Paraná, with data collection every 5 minutes to be used in simulations and models. These databases were later uploaded to the Amazon environment and processed within it. The processing mainly involved converting this data to an hourly time scale and obtaining the daily average, to be applied in a subsequent phase.

The next step involved simulating the inverter with switches in the PSIM software to estimate the temperature. PSIM software is one of the most robust power electronics software for this purpose, with real data on losses from the switches and other components. This simulation made it possible to observe the temperature behavior at the junction of the switches. In this simulation, an inverter topology with six switches, a three-phase half-bridge converter, was used. Refer to Figure 3.


<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/b2bba95b-93a8-4ea6-88a8-5d793bec47ee" width="800">
</p>

Figure 3 - PV Inverter in PSIM.

Subsequently, with the characteristics of the switches obtained in the simulation and data from each city, it is possible to apply ML models that allow predicting the thermal behavior of these switches. Figure 4 illustrates the proposal.

<p align="center">
  <img src="https://github.com/joaolucasdesouzasilva/IFSPproject/assets/73505430/cffc3e5b-0971-4504-a09a-fa7e4c8d18e4" width="800">
</p>

Figure 4 - Flowchart for thermal analysis of a DC/AC converter. The inputs are the yearly mission profile and the component characteristics extracted from the PSIM’s device library or data shee.

## 4 - Methodology in AWS environment
<p align="center">
  <img src= "https://github.com/joaolucasdesouzasilva/IFSPproject/assets/48127394/3c4a9f1d-619c-4a0d-9c9d-f53ad0b80aee" width="800">
</p>

Figure 5 - Detailed workflow applied focusing in AWS Cloud Environment.
<p> </p>
<p align="center">
  Table I - Tasks performed and details.
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
|       8a      |     Creating   a Sagemaker   environment to use the Jupyter   notebook for reading, processing, and applying Machine Learning.                                          |
|       8b      |     Making   results available for visualization in QuickSight.                                                                                                         |

## 5 - Discussion and Results

## 6 - Final Considerations

## References

[1] IRENA (2023), “Renewable Energy Statistics 2023”, International Renewable Energy Agency, Abu Dhabi. [Online]. Available: https://www.irena.org/Publications/2023/Jul/Renewable-energy-statistics-2023\
[2] I. Vernica, “Model-based Reliability Analysis of Power Electronic Systems”, PhD thesis, Aalborg Universitetsforlag, 2019.\
[3] H. Wang, K. Ma, and F. Blaabjerg, “Design for Reliability of Power Electronic Systems”, in IECON 2012 - 38th Annual Conference on IEEE Industrial Electronics Society, 2012, pp. 33–44.\
[4] P. Hacke et al., “A status review of photovoltaic power conversion equipment reliability, safety, and quality assurance protocols”, Renewable and Sustainable Energy Reviews, vol. 82, pp. 1097-1112, Feb. 2018. doi: 10.1016/j.rser.2017.07.043.\
[5] A. Golnas, “PV system reliability: An operator’s perspective”, IEEE Journal of Photovoltaics, vol. 3, no. 1, pp. 416-421, 2013. doi: 10.1109/JPHOTOV.2012.2215015\
[6] J. L. de Souza Silva, M. M. Cavalcante, S. B. Martins, E. J. da Silva, T. A. dos S. Barros, and M. G. Villalva, "Data-Driven Analysis of Solar Photovoltaic Systems: Correlation and Distribution Patterns," presented at the Brazilian Congress on Electrical Power Systems (COBEP), Campinas, Brazil, 2023.
[7] J. L. de Souza Silva. (2020). Estudo e Desenvolvimento Experimental de Otimizadores de Potência para Sistemas Fotovoltaicos Conectados à Rede Elétrica. Masters dissertation, University of Campinas, Brazil.






