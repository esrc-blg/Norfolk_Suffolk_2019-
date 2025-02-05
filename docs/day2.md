# The Application of SQL 


## Query 1: Select the number of the distinct crime category of female victims in the age group 20-29 

```sql
SELECT 
    COUNT(DISTINCT `Overall Crime Category`)
FROM
    table_crimerecords
WHERE
    `Victim Gender` LIKE 'FEMALE'
        AND `Victim age band` = '20-29';
```

Output: 
```

+------------------------------------------+
| count(DISTINCT `Overall Crime Category`) |
+------------------------------------------+
|  8                                       |
+------------------------------------------+

```


## Query 2: Total number of crime by policing district

```sql
SELECT 
    `Policing_District`, SUM(CAST(`Count` AS UNSIGNED))
FROM
    table_monthly_crime_data
GROUP BY `Policing_District`;
```

Output: 
```
+-----------------------------------+--------------------------------+
| Policing_District                 | SUM(CAST(`Count` AS UNSIGNED)) |
+-----------------------------------+--------------------------------+
| Northern Irelan                   | 5155262                        |
+-----------------------------------+--------------------------------+
| Belfast City	                    | 1693062                        |
+-----------------------------------+--------------------------------+
| Lisburn & Castlereagh City        | 432843                         |
+-----------------------------------+--------------------------------+
| Ards & North Down                 | 468822                         |
+-----------------------------------+--------------------------------+
| Newry Mourne & Down               | 556075                         |
+-----------------------------------+--------------------------------+
| Armagh City Banbridge & Craigavon | 602688                         |
+-----------------------------------+--------------------------------+
| Mid Ulster                        | 416230                         |
+-----------------------------------+--------------------------------+
| Fermanagh & Omagh                 | 391802                         |
+-----------------------------------+--------------------------------+
| Derry City & Strabane             | 576819                         |
+-----------------------------------+--------------------------------+
| Causeway Coast & Glens            | 503548                         |
+-----------------------------------+--------------------------------+
| Mid & East Antrim                 | 457748                         |
+-----------------------------------+--------------------------------+
| Antrim & Newtownabbey             | 458676                         | 
+-----------------------------------+--------------------------------+
```

## Query 3: Total number of crime by policing district and show them in descending order.

```sql
SELECT 
    `Policing_District`, SUM(CAST(`Count` AS UNSIGNED))
FROM
    table_monthly_crime_data
GROUP BY `Policing_District`
ORDER BY SUM(CAST(`Count` AS UNSIGNED)) DESC;
```
Output:

```
+-----------------------------------+--------------------------------+
| Policing_District                 | SUM(CAST(`Count` AS UNSIGNED)) |
+-----------------------------------+--------------------------------+
| Northern Ireland                  | 5155262                        |
+-----------------------------------+--------------------------------+
| Belfast City	                    | 1693062                        |
+-----------------------------------+--------------------------------+
| Armagh City Banbridge & Craigavon | 602688                         |
+-----------------------------------+--------------------------------+
| Derry City & Strabane             | 576819                         |
+-----------------------------------+--------------------------------+
| Newry Mourne & Down	            | 556075                         |
+-----------------------------------+--------------------------------+
| Causeway Coast & Glens            | 503548                         |
+-----------------------------------+--------------------------------+
| Ards & North Down                 | 468822                         |
+-----------------------------------+--------------------------------+
| Antrim & Newtownabbey             | 458676                         |
+-----------------------------------+--------------------------------+
| Mid & East Antrim                 | 457748                         |
+-----------------------------------+--------------------------------+
| Lisburn & Castlereagh City        | 432843                         |
+-----------------------------------+--------------------------------+
| Mid Ulster	                    | 416230                         |
+-----------------------------------+--------------------------------+
| Fermanagh & Omagh                 | 391802                         |
+-----------------------------------+--------------------------------+
```

## Query 4: Retrieve areas that are most vulnerable for young male children.

```sql
SELECT DISTINCT
    `Area`
FROM
    table_incidents
WHERE
    `Incident ID` IN (SELECT 
            `Incident ID`
        FROM
            table_crimerecords
        WHERE
            `Victim age band` = '0-9'
                AND `Victim Gender` = 'MALE')
```

Output:
```
+---------------------
| Area               |
+---------------------
| Omagh              |            
+---------------------
| North Down         |               
+---------------------
| Lisburn            |               
+---------------------
| Down               |
+---------------------
| Antrim             |
+---------------------
| East Belfast       |
+---------------------
| Newtownabbey       |
+---------------------
| Moyle              |
+---------------------
| Castlereagh        |
+---------------------
| North Belfast      |
+---------------------
| Carrickfergus      |
+---------------------
| West Belfast       |
+---------------------
| Dungannon&S Tyrone |
+---------------------
| South Belfast      |
+---------------------
| Foyle              |
+---------------------
| Craigavon          |
+---------------------
| Ballymena          |
+---------------------
| Armagh             |
+---------------------
| Coleraine          |
+---------------------
| Fermanagh          |
+---------------------
| Ards               |
+---------------------
| Larne              |
+---------------------
| Newry & Mourne     |
+---------------------
| Limavady           |
+---------------------
| Ballymoney         |
+---------------------
| Cookstown          |
+---------------------
| Magherafel         |
+---------------------
| Banbridge          |
+---------------------
| Strabane           |
+---------------------
```
## Query 5: Create a view for 'Sexual offences’ crime category.
```sql
CREATE OR REPLACE VIEW victim_age_gender AS
    SELECT 
        `Incident ID`, `Victim age band`, `Victim Gender`
    FROM
        table_crimerecords
    WHERE
        `Overall Crime Category` LIKE 'Sexual offences'; 

```
## Query 6: Retrieve the records from the view for female victims
```sql
SELECT * FROM victim_age_gender WHERE `Victim Gender`='FEMALE';
``` 

Output (only showing part of the output):

```
+--------------+-----------------+---------------+
| Incident ID  | Victim age band | Victim Gender |
+--------------+-----------------+---------------+
| 30           | 0-9             | FEMALE        |
+--------------+-----------------+---------------+
| 73           | 30-39	         | FEMALE        |
+--------------+-----------------+---------------+
| 88           | 0-9	         | FEMALE        |
+--------------+-----------------+---------------+
| 114          | 10-19	         | FEMALE        |
+--------------+-----------------+---------------+
| 129          | 30-39	         | FEMALE        |
+--------------+-----------------+---------------+
| 236          | 10-19	         | FEMALE        |
+--------------+-----------------+---------------+
| 325          | 0-9	         | FEMALE        |
+--------------+-----------------+---------------+
| 335          | 40-49	         | FEMALE        |
+--------------+-----------------+---------------+
| 793          | 10-19	         | FEMALE        |
+--------------+-----------------+---------------+
| 27780        | 20-29	         | FEMALE        |
+--------------+-----------------+---------------+
| 27843        | 10-19	         | FEMALE        |
+--------------+-----------------+---------------+
| 27954        | 20-29	         | FEMALE        |
+--------------+-----------------+---------------+
| 27983        | 10-19	         | FEMALE        |
+--------------+-----------------+---------------+
| 28209        | 20-29	         | FEMALE        |
+--------------+-----------------+---------------+
| 28251        | 10-19	         | FEMALE        |
+--------------+-----------------+---------------+
```

## Query 7: Which female age group is most vulnerable in sexual offences? 

```sql
SELECT 
    `Victim age band`, COUNT(*)
FROM
    victim_age_gender
WHERE
    `Victim Gender` = 'FEMALE'
GROUP BY `Victim age band`
ORDER BY COUNT(*) DESC;
```

Ouptut:
```
+-----------------+-----------
| Victim age band | COUNT(*) |
+-----------------+-----------
| 10-19           | 114      |
+-----------------+-----------
| 0-9	          | 106      |
+-----------------+-----------
| 20-29	          | 73       |
+-----------------+-----------
| 30-39	          | 42       |
+-----------------+-----------
| 40-49	          | 32       |
+-----------------+-----------
| 50-59	          | 7        |
+-----------------+-----------
| 60+	          | 4        |
+-----------------+-----------
``` 
## Query 8: Retrieve all the crime categories from two tables given some conditions.

### Combine query results with UNION

```sql

SELECT 
    `Overall Crime Category`
FROM
    table_crimerecords 
UNION SELECT 
    `Crime_Type`
FROM
    table_monthly_crime_data
WHERE
    `Month` = 'May'
        AND `Calendar_Year` = '2014'
        OR `Calendar_Year` = '2015';

```
output: 

```
+--------------------------------------------------------------------------------------+
| Overall Crime Category                                                               |
+--------------------------------------------------------------------------------------+
| Violence against the person                                                          |
+--------------------------------------------------------------------------------------+
| Theft offences                                                                       |
+--------------------------------------------------------------------------------------+
| Public order offences                                                                |
+--------------------------------------------------------------------------------------+
| Sexual offences                                                                      |
+--------------------------------------------------------------------------------------+
| Criminal damage                                                                      |
+--------------------------------------------------------------------------------------+
| Miscellaneous crimes against society                                                 |
+--------------------------------------------------------------------------------------+
| Theft offences - Burglary                                                            |
+--------------------------------------------------------------------------------------+
| NFIB-related fraud                                                                   |
+--------------------------------------------------------------------------------------+
| Robbery                                                                              |
+--------------------------------------------------------------------------------------+
| Violence with injury (including homicide & death/serious injury by unlawful driving) |
+--------------------------------------------------------------------------------------+
| Violence without injury (including harassment)                                       |
+--------------------------------------------------------------------------------------+
| Theft - burglary residential                                                         |
+--------------------------------------------------------------------------------------+
| Theft - burglary business & community                                                |
+--------------------------------------------------------------------------------------+
| Theft - domestic burglary                                                            |
+--------------------------------------------------------------------------------------+
| Theft - non-domestic burglary                                                        |
+--------------------------------------------------------------------------------------+
| Theft from the person                                                                |
+--------------------------------------------------------------------------------------+
| Theft - vehicle offences                                                             |
+--------------------------------------------------------------------------------------+
| Bicycle theft                                                                        |
+--------------------------------------------------------------------------------------+
| Theft - shoplifting                                                                  |
+--------------------------------------------------------------------------------------+
| All other theft offences                                                             |
+--------------------------------------------------------------------------------------+
| Trafficking of drugs                                                                 |
+--------------------------------------------------------------------------------------+
| Possession of drugs                                                                  |
+--------------------------------------------------------------------------------------+
| Possession of weapons offences                                                       |
+--------------------------------------------------------------------------------------+
| Total police recorded crime                                                          |
+--------------------------------------------------------------------------------------+
```

### MYSQL does not support INTERSECT, so we can simulate the function using subquery.

```sql
SELECT DISTINCT
    `Overall Crime Category`
FROM
    table_crimerecords
WHERE
    `Overall Crime Category` IN (SELECT 
            `Crime_Type`
        FROM
            table_monthly_crime_data
        WHERE
            `Month` = 'May'
                AND `Calendar_Year` = '2014'
                OR `Calendar_Year` = '2015') ;
```

Output:

```
+--------------------------------------+
| Overall Crime Category               |
+--------------------------------------+
| Sexual offences                      |
+--------------------------------------+
| Robbery                              |
+--------------------------------------+
| Criminal damage                      |
+--------------------------------------+
| Public order offences                |
+--------------------------------------+
| Miscellaneous crimes against society |
+--------------------------------------+
```

## Query 9: Write a Stored procedure for selecting monthly crimes.

```sql
DELIMITER //
DROP PROCEDURE IF EXISTS SelectMonthyCrime;
CREATE PROCEDURE SelectMonthyCrime 
(IN District text, Month text)
BEGIN  	
SELECT DISTINCT `Crime_Type` FROM table_monthly_crime_data  WHERE `Policing_District` = District and `Month` = Month;
END //
DELIMITER ;

```
### Calling the stored procedure 

```sql
CALL SelectMonthyCrime ('Northern Ireland', 'Apr');
```

Output: 

```
+--------------------------------------------------------------------------------------+
| Crime_Type                                                                           |
+--------------------------------------------------------------------------------------+
| Violence with injury (including homicide & death/serious injury by unlawful driving) |
+--------------------------------------------------------------------------------------+
| Violence without injury (including harassment)                                       |
+--------------------------------------------------------------------------------------+
| Sexual offences                                                                      |
+--------------------------------------------------------------------------------------+
| Robbery                                                                              |
+--------------------------------------------------------------------------------------+
| Theft - burglary residential                                                         |
+--------------------------------------------------------------------------------------+
| Theft - burglary business & community                                                |
+--------------------------------------------------------------------------------------+
| Theft - domestic burglary                                                            |
+--------------------------------------------------------------------------------------+
| Theft - non-domestic burglary                                                        |
+--------------------------------------------------------------------------------------+
| Theft from the person                                                                |
+--------------------------------------------------------------------------------------+
| Theft - vehicle offences                                                             |
+--------------------------------------------------------------------------------------+
| Bicycle theft                                                                        |
+--------------------------------------------------------------------------------------+
| Theft - shoplifting                                                                  |
+--------------------------------------------------------------------------------------+
| All other theft offences                                                             |
+--------------------------------------------------------------------------------------+
| Criminal damage                                                                      |
+--------------------------------------------------------------------------------------+
| Trafficking of drugs                                                                 |
+--------------------------------------------------------------------------------------+
| Possession of drugs                                                                  |
+--------------------------------------------------------------------------------------+
| Possession of weapons offences                                                       |
+--------------------------------------------------------------------------------------+
| Public order offences                                                                |
+--------------------------------------------------------------------------------------+
| Miscellaneous crimes against society                                                 |
+--------------------------------------------------------------------------------------+
| Total police recorded crime                                                          |
+--------------------------------------------------------------------------------------+

```


## Query 10: Using programming construct in the stored procedure

```sql
DELIMITER $$ 
DROP PROCEDURE IF EXISTS test_mysql_while_loop$$ 
CREATE PROCEDURE test_mysql_while_loop(IN count INT) BEGIN 
DECLARE x  INT;                 # Declare a variable  
SET x = 1;                      # Initialise the variable 
WHILE x  <= count DO            # Repeat the same add operation	
SET  x = x + 1;                 # Add operation 
END WHILE;               

SELECT x;                       # Print the final value in x    
END$$
DELIMITER ;
```

### Calling the stored procedure 

```sql 
CALL test_mysql_while_loop(5);
```

Output:
```
+---+
| x |
+---+
| 6 |
+---+
```
