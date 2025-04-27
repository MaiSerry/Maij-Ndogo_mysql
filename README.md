# 💧 Water Access - SQL Analysis

The md_water_services database provides detailed information on water access and quality across various locations. It contains data collected by field employees through site visits, including water source types, quality assessments, pollution tests, and service coverage statistics.
This dataset supports analysis of water service delivery efficiency, community impact, and helps identify opportunities for improving access to clean water.

---

# 📚 Table of Contents

- [1. Generate Employee Emails](#1-generate-employee-emails)
- [2. Disable SQL Safe Updates](#2-disable-sql-safe-updates)
- [3. Update Employee Emails](#3-update-employee-emails)
- [4. Switch to md_water_services Database](#4-switch-to-md_water_services-database)
- [5. Compare Phone Number Lengths](#5-compare-phone-number-lengths)
- [6. View Trimmed Addresses](#6-view-trimmed-addresses)
- [7. Update Trimmed Phone Numbers](#7-update-trimmed-phone-numbers)
- [8. Count Employees per Town](#8-count-employees-per-town)
- [9. Find Employees with Fewest Visits](#9-find-employees-with-fewest-visits)
- [10. Count Locations per Town](#10-count-locations-per-town)
- [11. Count Locations by Province and Town](#11-count-locations-by-province-and-town)
- [12. Count Locations per Type](#12-count-locations-per-type)
- [13. Total People Served (Water Sources)](#13-total-people-served-water-sources)
- [14. Group Water Sources by Type](#14-group-water-sources-by-type)
- [15. Calculate Percentage of People Served](#15-calculate-percentage-of-people-served)
- [16. Rank Water Source Types](#16-rank-water-source-types)
- [17. Rank Water Sources within Type](#17-rank-water-sources-within-type)
- [18. View All Visits](#18-view-all-visits)
- [19. Survey Duration](#19-survey-duration)
- [20. Average Queue Time](#20-average-queue-time)
- [21. Average Queue Time by Hour](#21-average-queue-time-by-hour)
- [22. Average Queue Time by Day of the Week](#22-average-queue-time-by-day-of-the-week)
- [23. Formatted Visit Dates](#23-formatted-visit-dates)
- [24. Yearly Water Basin Access Changes](#24-yearly-water-basin-access-changes)
- [25. Running Average Queue Time](#25-running-average-queue-time)
- [26. View All Employees](#26-view-all-employees)
- [27. Count Employees in Dahabu](#27-count-employees-in-dahabu)
- [28. Count Employees in Harare (Kilimani)](#28-count-employees-in-harare-kilimani)
- [29. Use Database md_water_services](#29-use-database-md_water_services)
- [30. View Visits Table (First 5 Records)](#30-view-visits-table-first-5-records)
- [31. View Water Quality Table (First 5 Records)](#31-view-water-quality-table-first-5-records)
- [32. List Incorrect Water Quality Records](#32-list-incorrect-water-quality-records)
- [33. Count Mistakes per Employee](#33-count-mistakes-per-employee)
- [34. Identify Suspect Employees](#34-identify-suspect-employees)
- [35. Find Cash-Related Mistakes](#35-find-cash-related-mistakes)

---

# 📖 SQL Queries Description

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

---

# 📈 Summary of Key Insights
- Water Access Coverage:
The dataset tracks the number of people served by different water source types, helping to prioritize resources for the most impactful sources.

- Queue Time Analysis:
Analysis of visits data reveals the average time people spend waiting for water at different hours and days, identifying peak congestion periods.

- Survey Quality Control:
By comparing auditor evaluations with field surveyor scores, it’s possible to detect errors and spot employees who may need additional training.

- Geographical Trends:
Town and province-level breakdowns allow exploration of where water services are strongest and where improvement efforts should focus.

- Well Pollution Risks:
well_pollution data identifies wells with biological and chemical contamination, helping prioritize urgent interventions.

- Global Access Trends:
The global_water_access table shows national, rural, and urban trends in water service levels across multiple years.

- Employee Performance:
By analyzing visit counts and error rates, the organization can monitor employee productivity and data quality.



