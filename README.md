# SQL-Data-exploration-Project

Covid data from ourworldindata.org was used to create tables in snowflake.  

Link to Dataset: https://ourworldindata.org/covid-deaths

**Skills used: Joins, CTE's, Temp Tables, Windows Functions, Aggregate Functions, Creating Views, Converting Data Types**


SELECT * from covid_deaths limit 100;

SELECT * from covid_vacc limit 100;

**Select data that we are going to use**

![image](https://github.com/user-attachments/assets/a6ff120a-9567-42bd-a3e2-abae4c9239a0)



**Total cases vs Total deaths**

![image](https://github.com/user-attachments/assets/6ec4bdb2-bcb0-47de-a473-b3229903006f)


**Total cases vs otal deaths filtered by country**

![image](https://github.com/user-attachments/assets/834e7e82-9f8c-4a5b-b2fd-29cda6ad5969)


**Total cases vs Population (Shows what percetage of population got covid)**

![image](https://github.com/user-attachments/assets/5687a6b1-ae77-4d5f-a56f-909cd480e04d)

**Total Deaths vs Population (Shows what percentage of population died with Covid)**

![image](https://github.com/user-attachments/assets/8130109f-b646-4e39-9a56-a3d6a06519e2)

**Find countries with highest infection rate**

![image](https://github.com/user-attachments/assets/5a936d3f-edea-49aa-828e-b0ae95cecd4f)


**Showing countries with highest death count**

![image](https://github.com/user-attachments/assets/98e26dd6-8173-46eb-9809-e470f5de2e3f)


**Breaking down with continents with highest death count**

![image](https://github.com/user-attachments/assets/307c27fc-f45c-4b19-bc8e-cc1873d81cf6)

**Getting Global death percentage by date**

![image](https://github.com/user-attachments/assets/46fa4f6f-debd-4ac4-8590-ca90baa088ab)


**Joining of 2 tables Covid daths and Covid vaccinations**

![image](https://github.com/user-attachments/assets/0036c889-9270-47a4-bdae-dd38da0314fb)


**Getting data for Total vaccinations vs Populations**

![image](https://github.com/user-attachments/assets/63c96fe3-cfbc-41f7-aa4c-4c7e6d84f25c)


**Getting data for Total vaccinations vs Populations for specific location**

![image](https://github.com/user-attachments/assets/be1cb47d-f5a2-44ac-8b1f-75f48edf6344)


**--Rolling total of new vaccinations by location and date**

![image](https://github.com/user-attachments/assets/4cd39d95-5430-46d3-bc6a-d36565aea0bd)


**Rolling total of new vaccinations by location and date**

![image](https://github.com/user-attachments/assets/5d94c5a3-fae0-4cff-903b-9f3a0ab6d2d9)


**Common Table Expression (CTE)
Goal is the get the percentage of the people getting vaccinated. As we are joining 2 tables we need to create a CTE first and then the calculation**

![image](https://github.com/user-attachments/assets/90af3a1e-875b-4d50-9b10-f8a85f04bc60)

**Creating Temp table to get percentage of people vaccinated**

![image](https://github.com/user-attachments/assets/9ec1f73e-d7dd-48b2-ba21-5f37bbb08405)


**Creating a view**

![image](https://github.com/user-attachments/assets/d8dddb39-399b-42dc-b6bc-dcb82dc51929)
