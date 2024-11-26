# SQL-Data-exploration-Project

Covid data from ourworldindata.org was used to create tables in snowflake.  

Link to Dataset: https://ourworldindata.org/covid-deaths

**Skills used: Joins, CTE's, Temp Tables, Windows Functions, Aggregate Functions, Creating Views, Converting Data Types**


SELECT * from covid_deaths limit 100;

SELECT * from covid_vacc limit 100;

**--Select data that we are going to use**

![image](https://github.com/user-attachments/assets/b2d4c56d-87b1-4b1b-a8ff-c150d10e8efd)


**--Total cases vs total deaths**

Select "LOCATION", "DATE","TOTAL_CASES","TOTAL_DEATHS",("TOTAL_DEATHS"/"TOTAL_CASES")*100 as "Death Percentage"
FROM COVID_DEATHS ORDER BY 1,2;

**--Total cases vs total deaths filter by country**

Select "LOCATION", "DATE","TOTAL_CASES","TOTAL_DEATHS",("TOTAL_DEATHS"/"TOTAL_CASES")*100 as "Death Percentage"
FROM COVID_DEATHS Where "LOCATION" like 'India';


**--Total cases vs Population (Shows what percetage of population got covid)**

Select "LOCATION", "DATE","TOTAL_CASES","POPULATION" ,("TOTAL_CASES"/"POPULATION")*100 as "TCases v Population"
FROM COVID_DEATHS Where "LOCATION" like 'India';

**--Total Deaths vs Population (Shows what percentage of population died with Covid)**

Select "LOCATION", "DATE","TOTAL_DEATHS","POPULATION" ,("TOTAL_DEATHS"/"POPULATION")*100 as "Death vs Population" FROM COVID_DEATHS Where "LOCATION" like 'India';

**--Find countries with highest infection rate**

Select "LOCATION", "POPULATION", MAX("TOTAL_CASES")as "Highest Infection Count", MAX("TOTAL_CASES"/"POPULATION")*100 as "TCases v Population"
FROM COVID_DEATHS 
Group BY "LOCATION", "POPULATION"
ORDER BY "TCases v Population" DESC;

**--Showing countries with highest death count**

SELECT "LOCATION", max("TOTAL_DEATHS") from COVID_DEATHS
where "CONTINENT" is not NULL
GROUP BY "LOCATION"
ORDER BY max("TOTAL_DEATHS") DESC;

**--Breaking down with continents with highest death count**

SELECT "CONTINENT", max("TOTAL_DEATHS") from COVID_DEATHS
where "CONTINENT" is not NULL
GROUP BY "CONTINENT"
ORDER BY max("TOTAL_DEATHS") DESC;

**-- Getting Global death percentage by date**

SELECT "DATE", SUM("NEW_CASES")as "Total Cases",SUM("TOTAL_DEATHS") as "Total Deaths",(SUM("NEW_DEATHS")/SUM("TOTAL_DEATHS"))*100 as "Death Percentage" FROM COVID_DEATHS
WHERE "CONTINENT" is not null
GROUP BY "DATE";

**--Joining of 2 tables Covid daths and Covid vaccinations**

SELECT * from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date;

**--Getting data for Total vaccinations vs Populations**

SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date
where covid_deaths.continent is not null
order by 2,3;

**--Getting data for Total vaccinations vs Populations for specific location**

SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date
where covid_deaths.LOCATION like 'Canada'
order by 2,3;

**--Rolling total of new vaccinations by location and date**

SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS,
Sum(covid_vacc.new_vaccinations) OVER (Partition by covid_deaths.LOCATION ORDER BY covid_deaths.LOCATION,covid_deaths.DATE ) as "Rolling total"
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date
where covid_deaths.continent is not null
order by 2,3;





**--Rolling total of new vaccinations by location and date**

SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS as "N_Vaccs",
Sum(covid_vacc.new_vaccinations) OVER (Partition by covid_deaths.LOCATION ORDER BY covid_deaths.LOCATION,covid_deaths.DATE ) as "Rolling total"
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date
where covid_deaths.LOCATION like 'Canada'
order by 2,3;

**--Common Table Expression (CTE)
-- Goal is the get the percentage of the people getting vaccinated. As we are joining 2 tables we need to create a CTE first and then the calculation**

With PopvsVac (Continent, Location, Date, Population, New_Vaccinations, Rolling_Total)
as
(SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS,
Sum(covid_vacc.new_vaccinations) OVER (Partition by covid_deaths.LOCATION ORDER BY covid_deaths.LOCATION,covid_deaths.DATE ) as "Rolling_total"
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date
where covid_deaths.LOCATION like 'Canada'
)
Select * ,(Rolling_Total/Population)*100 as "Vacc%" from PopvsVac;


**--Creating Temp table to get percentage of people vaccinated**

DROP Table if exists EDP_GTMA.GBS_SCHEMA_EXTERNAL.PercentPeopleVaccinated;
CREATE TABLE EDP_GTMA.GBS_SCHEMA_EXTERNAL.PercentPeopleVaccinated(
Continent nvarchar(255),
Location nvarchar(255),
Date datetime,
Population numeric,
New_vaccinations numeric,
Rolling_Total numeric
)

INSERT INTO PercentPeopleVaccinated
SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS,
Sum(covid_vacc.new_vaccinations) OVER (Partition by covid_deaths.LOCATION ORDER BY covid_deaths.LOCATION,covid_deaths.DATE ) as "Rolling_total"
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date;
--where covid_deaths.LOCATION like 'Canada'

Select * ,(Rolling_Total/Population)*100 as "Vacc%" from PercentPeopleVaccinated;

**--Creating a view**

Create VIEW EDP_GTMA.GBS_SCHEMA_EXTERNAL.PercentageofVaccPopulation as

SELECT covid_deaths.CONTINENT,covid_deaths.LOCATION,covid_deaths.DATE,covid_deaths.POPULATION,covid_vacc.NEW_VACCINATIONS,
Sum(covid_vacc.new_vaccinations) OVER (Partition by covid_deaths.LOCATION ORDER BY covid_deaths.LOCATION,covid_deaths.DATE ) as "Rolling_total"
from covid_deaths
join covid_vacc 
on covid_deaths.location = covid_vacc. location
AND
covid_deaths.date = covid_vacc.date;
