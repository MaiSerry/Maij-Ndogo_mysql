# Maij-Ndogo_mysql

# SQL Queries Description

| **#** | **Query** | **Purpose** |
|:---|:---|:---|
| 1 | `SELECT CONCAT(...) FROM employee;` | Generate employee emails by replacing spaces with dots, converting to lowercase, and appending "@nodgowater.gov". |
| 2 | `SET sql_safe_updates=0;` | Disable SQL safe updates to allow updates without a WHERE clause. |
| 3 | `UPDATE employee SET email = CONCAT(...);` | Update the `email` field in the `employee` table with the new format. |
| 4 | `USE md_water_services;` | Switch to the database `md_water_services`. |
| 5 | `SELECT LENGTH(phone_number), LENGTH(TRIM(phone_number)) FROM employee;` | Compare original and trimmed phone number lengths. |
| 6 | `SELECT TRIM(address) FROM employee;` | View addresses with leading/trailing spaces removed. |
| 7 | `UPDATE employee SET phone_number = TRIM(phone_number);` | Save trimmed phone numbers back into the database. |
| 8 | `SELECT town_name, COUNT(*) FROM employee GROUP BY town_name;` | Count number of employees per town. |
| 9 | `SELECT assigned_employee_id, COUNT(visit_count) FROM visits ...` | Find the 3 employees with the fewest number of visits. |
| 10 | `SELECT town_name, COUNT(*) FROM location GROUP BY town_name;` | Count number of locations per town. |
| 11 | `SELECT province_name, town_name, COUNT(*) FROM location ...` | Count locations grouped by province and town, sorted by province descending. |
| 12 | `SELECT location_type, COUNT(*) FROM location GROUP BY location_type;` | Count number of locations per location type. |
| 13 | `SELECT SUM(number_of_people_served) FROM water_source;` | Get total number of people served by all water sources. |
| 14 | `SELECT type_of_water_source, SUM(...), AVG(...) FROM water_source GROUP BY type_of_water_source;` | Show total served, average served, and count of each water source type. |
| 15 | `SELECT type_of_water_source, SUM(...), ROUND(SUM(...) / 27628140 * 100, 0) ...` | Same as #14, but also calculates % of total people served. |
| 16 | `SELECT type_of_water_source, SUM(...), RANK() OVER (...) FROM water_source;` | Rank water source types by total people served. |
| 17 | `SELECT *, RANK() OVER (PARTITION BY type_of_water_source ORDER BY number_of_people_served DESC) FROM water_source;` | Rank individual water sources within each type. |
| 18 | `SELECT * FROM visits;` | View all visits data. |
| 19 | `SELECT DATEDIFF(MAX(time_of_record), MIN(time_of_record)) FROM visits;` | Calculate how many days the full survey took. |
| 20 | `SELECT AVG(IFNULL(time_in_queue,0)) FROM visits;` | Calculate average queue time for water (treat nulls as 0). |
| 21 | `SELECT AVG(time_in_queue), TIME_FORMAT(...) FROM visits GROUP BY hour_of_day;` | Average queue time for each hour of the day. |
| 22 | `SELECT TIME_FORMAT(...), AVG(CASE WHEN DAYNAME(...) THEN ...) ...` | Average queue time by day of the week and hour. |
| 23 | `SELECT CONCAT(DAY(time_of_record), ' ', MONTHNAME(time_of_record), ' ', YEAR(time_of_record)) FROM visits;` | Display formatted visit dates like "27 April 2025". |
| 24 | `SELECT name, wat_bas_r - LAG(wat_bas_r) OVER (PARTITION BY name ORDER BY year) FROM global_water_access;` | Calculate year-to-year changes in water basin access. |
| 25 | `SELECT location_id, time_in_queue, AVG(time_in_queue) OVER (...) FROM visits;` | Find running average queue time for each location visited more than once. |
| 26 | `SELECT * FROM employee;` | Show all employee records. |
| 27 | `SELECT COUNT(*) FROM employee WHERE town_name LIKE '%Dahabu%';` | Count employees working in towns like Dahabu. |
| 28 | `SELECT COUNT(*) FROM employee WHERE province_name='Kilimani' AND town_name='Harare';` | Count employees in Harare town within Kilimani province. |
| 29 | `USE md_water_services;` | Switch to the database `md_water_services`. |
| 30 | `SELECT * FROM visits LIMIT 5;` | View first 5 records from the visits table. |
| 31 | `SELECT * FROM water_quality LIMIT 5;` | View first 5 records from the water_quality table. |
| 32 | `WITH incorrect_records AS (...)` | List incorrect records where auditor and surveyor water scores don't match, for first visits. |
| 33 | `WITH error_count AS (...)` | Count number of mistakes per employee. |
| 34 | `WITH suspect_list AS (...)` | Identify employees with mistake counts above the average. |
| 35 | `SELECT location_id, employee_name, statements FROM incorrect_records WHERE employee_name IN (...) AND statements LIKE '%cash%';` | Find records by suspect employees mentioning "cash" in auditor's notes. |

