# CapstoneProject
con <- dbConnect(RSQLite::SQLite(),"RDB.sqlite")
con
worldcities <- read.csv("raw_worldcities.csv")
bike_sharing_systems <- read.csv("bike_sharing_systems.csv")
cities_weather_forecast <- read.csv("raw_cities_weather_forecast.csv")
seoul_bike_sharing <- read.csv("seoul_bike_sharing.csv")
dbWriteTable(con, "WORLD_CITIES", worldcities,overwrite=TRUE, header=TRUE)
dbWriteTable(con, "BIKE_SHARING_SYSTEMS", bike_sharing_systems,overwrite=TRUE, header=TRUE)
dbWriteTable(con, "CITIES_WEATHER_FORECAST", cities_weather_forecast,overwrite=TRUE, header=TRUE)
dbWriteTable(con, "SEOUL_BIKE_SHARING", seoul_bike_sharing,overwrite=TRUE, header=TRUE)
dbListTables(con)
dbGetQuery(con, "SELECT count(*) as Count_of_Records FROM seoul_bike_sharing")
dbGetQuery(con, "SELECT count(HOUR) as Numer_of_hours FROM seoul_bike_sharing
WHERE RENTED_BIKE_COUNT > 0")
dbGetQuery(con, "SELECT * FROM CITIES_WEATHER_FORECAST
WHERE CITY = 'Seoul'
Limit 1")
dbGetQuery(con, "SELECT DISTINCT(SEASONS) FROM seoul_bike_sharing")
dbGetQuery(con, "SELECT MIN(DATE) as Start_Date, MAX(DATE) as End_Date FROM seoul_bike_sharing")
dbGetQuery(con, "SELECT DATE, HOUR, RENTED_BIKE_COUNT as Maximum_COUNT FROM seoul_bike_sharing
WHERE RENTED_BIKE_COUNT = (SELECT MAX(RENTED_BIKE_COUNT) FROM seoul_bike_sharing)")
dbGetQuery(con, "SELECT SEASONS, HOUR, AVG(RENTED_BIKE_COUNT), AVG(TEMPERATURE) FROM seoul_bike_sharing GROUP BY SEASONS, HOUR ORDER BY AVG(RENTED_BIKE_COUNT) desc limit 10")
dbGetQuery(con, " SELECT SEASONS, AVG(RENTED_BIKE_COUNT) as AVG_S_COUNT, MIN(RENTED_BIKE_COUNT) as MIN_S_COUNT, MAX(RENTED_BIKE_COUNT) as MAX_S_COUNT FROM seoul_bike_sharing
group by (SEASONS)
Order by AVG_S_COUNT desc")
dbGetQuery(con," SELECT SEASONS, AVG(RENTED_BIKE_COUNT) as AVG_S_COUNT, AVG(TEMPERATURE) as AVG_S_TEMP, AVG(HUMIDITY) as AVG_S_HUMIDITY, AVG(WIND_SPEED) as AVG_WIND_SPEED, AVG(VISIBILITY) as AVG_VISIBILITY, AVG(DEW_POINT_TEMPERATURE) as AVG_DEW_POINT, AVG(SOLAR_RADIATION) as AVG_SOLAR_RADIATION, AVG(RAINFALL) as AVG_RAINFALL, AVG(SNOWFALL) as AVG_SNOWFALL   FROM seoul_bike_sharing
group by (SEASONS)
Order by AVG_S_COUNT desc")
dbGetQuery(con, "SELECT WORLD_CITIES.CITY_ASCII, WORLD_CITIES.POPULATION, WORLD_CITIES.LAT, WORLD_CITIES.LNG, SUM(BIKE_SHARING_SYSTEMS.BICYCLES) AS TOTAL_BICYCLES, BIKE_SHARING_SYSTEMS.COUNTRY
FROM WORLD_CITIES
INNER JOIN BIKE_SHARING_SYSTEMS ON WORLD_CITIES.CITY_ASCII = BIKE_SHARING_SYSTEMS.CITY
WHERE WORLD_CITIES.CITY_ASCII = 'Seoul'")
