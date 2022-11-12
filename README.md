# SQL-Queries

SELECT *
FROM allergies

--Patients with specific allergies and counting patients based on various description

SELECT *
 FROM allergies
 WHERE Observation = 'Allergy to tree pollen'

SELECT DISTINCT Observation
  FROM allergies

SELECT COUNT(DISTINCT Observation )
  FROM allergies


## Query for diagnosis
SELECT start, 
        stop, 
		description as condition
  FROM conditions
  WHERE description =  'Diabetes' or stop IS NOT NULL

--To check if patient has any careplan

SELECT * 
 From careplans
 WHERE description = 'Respiratory therapy'\
 
--To know Immunization status and patients with immunizations
SELECT * 
 FROM immunizations

SELECT *
 FROM immunizations
 WHERE description = 'Influenza  seasonal  injectable  preservative free'

 SELECT TOP 10 *
  FROM immunizations

SELECT TOP 10 *
 FROM immunizations
 WHERE description = 'Influenza  seasonal  injectable  preservative free'


 SELECT DISTINCT description
  FROM immunizations

 SELECT COUNT(DISTINCT description)
  FROM immunizations


SELECT *
 FROM all_prevalences
 WHERE occurrences > 200 and population_type = 'LIVING'

SELECT item, occurrences, population_count
 FROM all_prevalences
 WHERE population_count <= 1000
 
SELECT item, occurrences, population_count
 FROM all_prevalences
 WHERE occurrences >= 200

SELECT * 
 FROM observations
 WHERE description = 'Body Height'
 ORDER BY value DESC

SELECT * 
 FROM observations
 WHERE description = 'Body Height'
 ORDER BY value ASC

SELECT patient, description, value, units 
 FROM observations
 WHERE description LIKE 'Body M%%'
 ORDER BY value ASC 

SELECT patient, code, description, reasoncode, reasondescription 
 FROM medications
 WHERE reasondescription = 'Diabetes' 
 AND description LIKE '24 HR %'
 ORDER BY code

SELECT patient, code, description
 FROM conditions
 WHERE description = 'Diabetes'
 OR description = 'Prediabetes'
 ORDER BY code
 
SELECT DISTINCT code
 FROM conditions

SELECT COUNT(DISTINCT code)
 FROM conditions

SELECT *
 FROM procedures

SELECT DISTINCT reasondescription
 FROM procedures

SELECT patient, description, code, reasondescription
 FROM procedures
 WHERE reasondescription = 'Fracture of rib'
       AND NOT description LIKE 'Bone i%'

SELECT patient,
       description,
	   value
	FROM observations
 WHERE description = 'Body Weight'

SELECT patient, description, reasondescription
 FROM medications
 WHERE reasondescription IS NOT NULL

SELECT count(patient)
 FROM medications
 WHERE description LIKE 'Pen%'

SELECT * 
 FROM all_prevalences




SELECT item, population_type, occurrences
 FROM all_prevalences
 WHERE population_type = 'LIVING'
 AND occurrences BETWEEN 200 AND 500
 

SELECT DISTINCT population_type
  FROM all_prevalences
 
SELECT patients.patient,
       patients.marital,
	   patients.race,
	   patients.drivers,
	   patients.gender,
	   patients.birthplace,
	   medications.description AS Prescrption,
	   medications.reasondescription AS Diagnosis
 FROM patients, medications
 WHERE patients.patient = medications.patient


SELECT patients.patient,
       patients.marital,
	   patients.race,
	   patients.gender,
	   patients.birthplace,
	   medications.description AS Prescrption,
	   medications.reasondescription AS Diagnosis
  FROM patients
       JOIN medications
	   ON medications.patient = patients.patient
	WHERE medications.reasondescription IS NOT NULL


SELECT * 
 FROM procedures

SELECT DISTINCT race 
 FROM patients

SELECT patients.patient,
       patients.marital,
	   patients.race,
	   patients.gender,
	   patients.birthplace,
	   procedures.description AS Prescrption,
	   procedures.reasondescription AS Diagnosis
  FROM patients
       LEFT OUTER JOIN procedures
	   ON patients.patient = procedures.patient
	WHERE patients.race = 'hispanic'
	      AND patients.gender = 'M'

SELECT patient,
       description,
	   CAST(value AS DECIMAL)Value,
	   units
 FROM observations
 WHERE observations.description = 'Body Height'

SELECT *
 FROM allergies


SELECT COUNT(Observation)
    FROM allergies
    WHERE Observation = 'Allergy to fish'


SELECT patient, 
       COUNT(Observation)No_obser
	FROM allergies
	GROUP BY patient
	ORDER BY COUNT(Observation) DESC

SELECT patient,
       birthdate,
	   deathdate,
	   DATEDIFF(year, birthdate, deathdate)AS 'Age at Death'
  FROM patients
  WHERE deathdate IS NOT NULL
  ORDER BY 'Age at Death' DESC
  

 FROM all_prevalences



SELECT patient,
       Observation
 From allergies
 Where Observation 
 IN ('Allegry to tree pollen', 'Allergy to mould', 'House dust mite allergy')


SELECT COUNT(patient)Total_Patient,
       COUNT(Observation)Total_Observation
 From allergies
 Where Observation 
 IN ('Allegry to tree pollen', 'Allergy to mould', 'House dust mite allergy')
 Order BY COUNT(Observation) DESC

SELECT * 
 FROM observations_cleaned

SELECT patient,
       ROUND(AVG (value), 2) AS 'BMI',
	   CASE
	     WHEN value < 20
		 THEN 'Under Weight'
		 WHEN value >= 20 AND value < 25
		 THEN 'Healthy'
		 WHEN value >= 25 AND value < 32
		 THEN 'Over Weight'
		 WHEN value >32
		 THEN 'Obese'
       END AS 'BMI Chart'
   From observations_cleaned
   WHERE description = 'Body Mass Index'
   GROUP BY patient, value

SELECT patient,
       AVG(value) AS 'BMI',
	   COUNT(value) AS 'Reading No',
	   MAX(value)
	   FILTER( WHERE value >32) AS 'Max Obese BMI'
 FROM observations_cleaned
 GROUP BY patient
 HAVING AVG(value) > 32
 ORDER BY MAX(value) ASC

 SELECT patient,
       ROUND(AVG(value), 2) AS 'BMI',
	   COUNT(value) AS 'Number of Readings',
	   ROUND(MAX(value), 2) AS 'Max Obese'
  FROM observations_cleaned
  WHERE description = 'Body Mass Index'
  GROUP BY patient
  HAVING AVG(value) > 32
  ORDER BY MAX(value) DESC

 
