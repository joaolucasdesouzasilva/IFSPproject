# Exemplos das consultas e manipulações no Athena e resultados no dataset Teresina_PI_-5.096124_-42.802307_Solcast_PT5M.csv
## 1 - Agrupamento por Mês com Média de Temperatura:
~~~sql
SELECT
  EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP)) AS month,
  AVG(CAST(airtemp AS DOUBLE)) AS avg_temperature
FROM teresina
WHERE airtemp IS NOT NULL
GROUP BY EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP))
ORDER BY month;
~~~
| #  | month | avg_temperature    |
|----|-------|--------------------|
| 1  | 1     | 25.487578405017995 |
| 2  | 2     | 24.8065724206348   |
| 3  | 3     | 25.57049731182782  |
| 4  | 4     | 25.280844907407346 |
| 5  | 5     | 26.206518817204287 |
| 6  | 6     | 27.280081018518526 |
| 7  | 7     | 28.05545474910397  |
| 8  | 8     | 28.681966845878037 |
| 9  | 9     | 30.63935185185183  |
| 10 | 10    | 29.91630824372764  |
| 11 | 11    | 29.49736111111106  |
| 12 | 12    | 26.38130535638966  |

## 2 - Análise de Correlação entre Temperatura e Irradiação Global:
~~~sql
SELECT
  CORR(airtemp, ghi) AS temperature_ghi_correlation
FROM teresina;
~~~
| # | temperature_ghi_correlation |
|---|-----------------------------|
|1	| 0.49306068203320863         |

## 3 - Ranking de Albedo Diário no Top 10:
~~~sql
SELECT
  periodstart,
  albedodaily,
  RANK() OVER (ORDER BY albedodaily DESC) AS albedo_rank
FROM teresina
ORDER BY albedo_rank
LIMIT 10;
~~~
| #  | periodstart          | albedodaily | albedo_rank |
|----|----------------------|-------------|-------------|
| 1  | 2018-01-27T23:05:00Z | 0.16        | 1           |
| 2  | 2018-01-27T23:20:00Z | 0.16        | 1           |
| 3  | 2018-01-27T22:50:00Z | 0.16        | 1           |
| 4  | 2018-01-27T23:00:00Z | 0.16        | 1           |
| 5  | 2018-01-27T23:10:00Z | 0.16        | 1           |
| 6  | 2018-01-27T23:15:00Z | 0.16        | 1           |
| 7  | 2018-01-27T22:40:00Z | 0.16        | 1           |
| 8  | 2018-01-27T22:45:00Z | 0.16        | 1           |
| 9  | 2018-01-27T22:55:00Z | 0.16        | 1           |
| 10 | 2018-01-27T22:35:00Z | 0.16        | 1           |

## 4 - Variação da Irradiação Global ao Longo do Dia:
~~~sql
SELECT
  EXTRACT(HOUR FROM CAST(FROM_ISO8601_TIMESTAMP(periodend) AS TIMESTAMP)) AS hour,
  AVG(ghi) AS avg_ghi
FROM teresina
WHERE periodend IS NOT NULL
GROUP BY EXTRACT(HOUR FROM CAST(FROM_ISO8601_TIMESTAMP(periodend) AS TIMESTAMP))
ORDER BY hour;
~~~
| #  | hour | avg_ghi            |
|----|------|--------------------|
| 1  | 0    | 0.0                |
| 2  | 1    | 0.0                |
| 3  | 2    | 0.0                |
| 4  | 3    | 0.0                |
| 5  | 4    | 0.0                |
| 6  | 5    | 0.0                |
| 7  | 6    | 0.0                |
| 8  | 7    | 0.0                |
| 9  | 8    | 1.806993642143506  |
| 10 | 9    | 63.87216167120799  |
| 11 | 10   | 231.877838328792   |
| 12 | 11   | 424.3905540417802  |
| 13 | 12   | 594.80608537693    |
| 14 | 13   | 724.5631244323342  |
| 15 | 14   | 775.1752951861944  |
| 16 | 15   | 752.8226612170754  |
| 17 | 16   | 665.1748410535877  |
| 18 | 17   | 534.5517711171663  |
| 19 | 18   | 387.67597638510443 |
| 20 | 19   | 224.60354223433242 |
| 21 | 20   | 56.67347865576748  |
| 22 | 21   | 0.5592643051771117 |
| 23 | 22   | 0.0                |
| 24 | 23   | 0.0                |

## 5 - Análise de Perda de Produção de PV devido à Chuva (20 primeiras medidas):
~~~sql
SELECT
  periodend,
  precipitablewater,
  RANK() OVER (ORDER BY precipitablewater DESC) AS rain_rank
FROM teresina
WHERE precipitablewater IS NOT NULL
ORDER BY rain_rank
LIMIT 20;
~~~
| #  | periodend            | precipitablewater | rain_rank |
|----|----------------------|-------------------|-----------|
| 1  | 2018-04-05T01:00:00Z | 64.0              | 1         |
| 2  | 2018-04-05T01:20:00Z | 64.0              | 1         |
| 3  | 2018-04-05T00:10:00Z | 64.0              | 1         |
| 4  | 2018-04-05T01:05:00Z | 64.0              | 1         |
| 5  | 2018-04-05T00:25:00Z | 64.0              | 1         |
| 6  | 2018-04-05T00:55:00Z | 64.0              | 1         |
| 7  | 2018-04-05T00:40:00Z | 64.0              | 1         |
| 8  | 2018-04-05T01:25:00Z | 64.0              | 1         |
| 9  | 2018-04-05T01:15:00Z | 64.0              | 1         |
| 10 | 2018-04-05T01:10:00Z | 64.0              | 1         |
| 11 | 2018-04-05T00:35:00Z | 64.0              | 1         |
| 12 | 2018-04-05T00:30:00Z | 64.0              | 1         |
| 13 | 2018-04-05T00:20:00Z | 64.0              | 1         |
| 14 | 2018-04-05T00:15:00Z | 64.0              | 1         |
| 15 | 2018-04-05T00:50:00Z | 64.0              | 1         |
| 16 | 2018-04-05T00:45:00Z | 64.0              | 1         |
| 17 | 2018-04-05T00:05:00Z | 64.0              | 1         |
| 18 | 2018-04-05T01:30:00Z | 64.0              | 1         |
| 19 | 2018-04-05T02:05:00Z | 63.9              | 19        |
| 20 | 2018-04-05T02:00:00Z | 63.9              | 19        |

## 6 - Média de Precipitação por Mês:
~~~sql
SELECT
  EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP)) AS month,
  AVG(precipitablewater) AS avg_precipitation
FROM teresina
GROUP BY EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP))
ORDER BY month;
~~~
| #  | month | avg_precipitation  |
|----|-------|--------------------|
| 1  | 1     | 47.10203853046579  |
| 2  | 2     | 52.958816964285695 |
| 3  | 3     | 49.76325044802876  |
| 4  | 4     | 50.67291666666726  |
| 5  | 5     | 45.6000784050178   |
| 6  | 6     | 38.06394675925935  |
| 7  | 7     | 36.389964157706345 |
| 8  | 8     | 34.18428539426533  |
| 9  | 9     | 35.43755787037036  |
| 10 | 10    | 40.18611111111128  |
| 11 | 11    | 45.372488425926065 |
| 12 | 12    | 47.399683677773176 |

## 7 - Análise de Velocidade e Direção do Vento:
~~~sql
SELECT
  winddirection10m,
  AVG(windspeed10m) AS avg_wind_speed
FROM teresina
GROUP BY winddirection10m
ORDER BY avg_wind_speed
limit 10;
~~~
| #  | winddirection10m | avg_wind_speed     |
|----|------------------|--------------------|
| 1  | 241              | 0.7179487179487178 |
| 2  | 242              | 0.7636363636363636 |
| 3  | 235              | 0.776923076923077  |
| 4  | 230              | 0.7854838709677415 |
| 5  | 225              | 0.7979591836734693 |
| 6  | 229              | 0.8074074074074071 |
| 7  | 228              | 0.8088888888888889 |
| 8  | 246              | 0.8111111111111111 |
| 9  | 220              | 0.8142857142857142 |
| 10 | 244              | 0.8357142857142857 |

## 8 - Análise da Tendência da Irradiação Global mensal:
~~~sql
WITH daily_ghi AS (
  SELECT
    EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP)) AS month,
    AVG(ghi) AS avg_ghi
  FROM teresina
  WHERE periodstart IS NOT NULL
GROUP BY EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP))
)
SELECT
  month,
  avg_ghi,
  COALESCE(avg_ghi - LAG(avg_ghi) OVER (ORDER BY MONTH), 0) AS ghi_variation
FROM daily_ghi
ORDER BY month
~~~
| #  | month | avg_ghi            | ghi_variation       |
|----|-------|--------------------|---------------------|
| 1  | 1     | 212.77576164874552 | 0.0                 |
| 2  | 2     | 193.56795634920636 | -19.20780529953916  |
| 3  | 3     | 208.8462141577061  | 15.278257808499745  |
| 4  | 4     | 211.80555555555554 | 2.9593413978494425  |
| 5  | 5     | 210.86962365591398 | -0.9359318996415595 |
| 6  | 6     | 222.87037037037038 | 12.000746714456398  |
| 7  | 7     | 233.93525985663084 | 11.064889486260455  |
| 8  | 8     | 252.22423835125448 | 18.28897849462365   |
| 9  | 9     | 267.9347222222222  | 15.710483870967721  |
| 10 | 10    | 262.4379480286738  | -5.49677419354839   |
| 11 | 11    | 228.91805555555555 | -33.51989247311826  |
| 12 | 12    | 212.11377056094474 | -16.80428499461081  |

## 9 - Média mensal da temperatura e irradiancia no ano de 2018
~~~sql
SELECT
  EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP)) AS month,
  AVG(airtemp) AS avg_temperature,
  AVG(gtitracking) AS avg_irradiance
FROM teresina
WHERE EXTRACT(YEAR FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP)) = 2018
GROUP BY EXTRACT(MONTH FROM CAST(FROM_ISO8601_TIMESTAMP(periodstart) AS TIMESTAMP))
ORDER BY month;
~~~
| #  | month | avg_temperature    | avg_irradiance     |
|----|-------|--------------------|--------------------|
| 1  | 1     | 25.487578405017995 | 240.79861111111111 |
| 2  | 2     | 24.8065724206348   | 216.57440476190476 |
| 3  | 3     | 25.57049731182782  | 243.26232078853047 |
| 4  | 4     | 25.280844907407346 | 247.3896990740741  |
| 5  | 5     | 26.206518817204287 | 249.3412858422939  |
| 6  | 6     | 27.280081018518526 | 276.2980324074074  |
| 7  | 7     | 28.05545474910397  | 292.32459677419354 |
| 8  | 8     | 28.681966845878037 | 314.7170698924731  |
| 9  | 9     | 30.63935185185183  | 333.06944444444446 |
| 10 | 10    | 29.91630824372764  | 315.51556899641577 |
| 11 | 11    | 29.49736111111106  | 267.1516203703704  |
| 12 | 12    | 26.255134529147917 | 240.43060538116592 |

## 10 - Criação do dataset com a média por hora para o ano de 2018:
~~~sql
CREATE TABLE IF NOT EXISTS media_hora_2018_teresina AS
WITH filtered_data AS (
  SELECT
    CAST(FROM_ISO8601_TIMESTAMP(periodend) AS TIMESTAMP) AS period_end,
    airtemp,
    azimuth,
    cloudopacity,
    dewpointtemp,
    dhi,
    dni,
    ebh,
    ghi,
    gtifixedtilt,
    gtitracking,
    precipitablewater,
    relativehumidity,
    snowwater,
    surfacepressure,
    winddirection10m,
    windspeed10m,
    zenith,
    albedodaily
  FROM teresina
  WHERE EXTRACT(YEAR FROM CAST(FROM_ISO8601_TIMESTAMP(periodend) AS TIMESTAMP)) = 2018
),
numbered_intervals AS (
  SELECT
    period_start,
    FLOOR(DATEDIFF(minute, MIN(period_end), period_end) / 60) AS interval_number
  FROM filtered_data
)
SELECT
  TIMESTAMPADD(hour, interval_number, TIMESTAMP '2018-01-01 00:00:00') AS periodend,
  AVG(airtemp) AS avg_airtemp,
  AVG(azimuth) AS avg_azimuth,
  AVG(cloudopacity) AS avg_cloudopacity,
  AVG(dewpointtemp) AS avg_dewpointtemp,
  AVG(dhi) AS avg_dhi,
  AVG(dni) AS avg_dni,
  AVG(ebh) AS avg_ebh,
  AVG(ghi) AS avg_ghi,
  AVG(gtifixedtilt) AS avg_gtifixedtilt,
  AVG(gtitracking) AS avg_gtitracking,
  AVG(precipitablewater) AS avg_precipitablewater,
  AVG(relativehumidity) AS avg_relativehumidity,
  AVG(snowwater) AS avg_snowwater,
  AVG(surfacepressure) AS avg_surfacepressure,
  AVG(winddirection10m) AS avg_winddirection10m,
  AVG(windspeed10m) AS avg_windspeed10m,
  AVG(zenith) AS avg_zenith,
  AVG(albedodaily) AS avg_albedodaily
FROM
  filtered_data
JOIN
  numbered_intervals
ON
  FLOOR(DATEDIFF(minute, MIN(filtered_data.periodend), filtered_data.periodend) / 60 / 12) = numbered_intervals.interval_number
GROUP BY
  numbered_intervals.interval_number;
~~~

~~~sql
SELECT periodend, airtemp, gtotracking
FROM media_hora_2018_teresina
limit 5;
~~~
| # | periodend             | airtemp  | gtitracking |
|---|-----------------------|----------|-------------|
| 1 | 2018-01-01 01:00:00   | 27.90208 | 289.58681   |
| 2 | 2018-01-01 02:00:00   | 26.43299 | 232.63194   |
| 3 | 2018-01-01 03:00:00   | 25.03889 | 250.94444   |
| 4 | 2018-01-01 04:00:00   | 25.4316  | 254.875     |
| 5 | 2018-01-01 05:00:00   | 25.33715 | 140.43056   |


