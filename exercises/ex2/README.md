# Exercise 2 - Index Advisor/

In this exercise, you will create a dataset and generate index recommendations using **Recommendation App** in **HANA Cloud Central**. You will then manually apply these recommendations through the **SQL Console** in **HANA Cloud Central** and validate their performance benefits. 

## What is Index Advisor?

An index is a supplemental data structure that supports query processing. While it requires additional memory, an index can dramatically improve query performance when used correctly. As a result, indexes are considered one of the most critical factors in physical database design.

The current **Index Advisor** operates on two primary rules:
- Recommends creating an index on a column with a very high scan count
- Recommends dropping an index with a lookup count near zero

It also considers additional metadata to identify the most suitable candidates for index recommendations.
This initial release does not recommend creating indexes on columns used in join conditions, but future enhancements will extend its capabilities to handle more complex queries and additional features.

## Dataset Information and Preparation

For this hands-on exercise, two sample tables will be created and used. These are **artifically designed** to serve as candidates for index recommendations.

- **MOVIES**: A table with three columns (*ID*, *GENRE*, *STATUS*) and **60 million records**
- **MOVIES2**: A table with three columns (*ID*, *GENRE*, *STATUS*) and **100 records**

Open the **SQL Console** from HANA Cloud Central and connect using the **DBADMIN** user. Execute the SQL script provided below to generate a large, skewed dataset and create a query workload that is intentionally resource-intensive.

**SQL Script** (Estimated execution time: approx. 3 min)
```SQL
-- Expensive Statement Collection Setting
ALTER SYSTEM ALTER CONFIGURATION ('global.ini', 'SYSTEM') SET ('expensive_statement', 'enable') = 'true' WITH RECONFIGURE;
ALTER SYSTEM ALTER CONFIGURATION ('global.ini', 'SYSTEM') SET ('expensive_statement', 'threshold_duration') = '1000000' WITH RECONFIGURE;

-- Step 1: Create the tables needed by the procedure.
CREATE TABLE GENRE_LIST (ID INT PRIMARY KEY, ITEM VARCHAR(100));
INSERT INTO GENRE_LIST VALUES (0,'Action');
INSERT INTO GENRE_LIST VALUES (1,'Comedy');
INSERT INTO GENRE_LIST VALUES (2,'Drama');
INSERT INTO GENRE_LIST VALUES (3,'Sci-Fi');
INSERT INTO GENRE_LIST VALUES (4,'Thriller');
INSERT INTO GENRE_LIST VALUES (5,'Romance');
INSERT INTO GENRE_LIST VALUES (6,'Animation');
INSERT INTO GENRE_LIST VALUES (7,'Horror');
INSERT INTO GENRE_LIST VALUES (8,'Documentary');
INSERT INTO GENRE_LIST VALUES (9,'Fantasy');

CREATE TABLE STATUS_LIST (ID INT PRIMARY KEY, ITEM VARCHAR(100));
INSERT INTO STATUS_LIST VALUES (0, 'Coming Soon');
INSERT INTO STATUS_LIST VALUES (1, 'Premiere Screening');
INSERT INTO STATUS_LIST VALUES (2, 'Rating Pending');
INSERT INTO STATUS_LIST VALUES (3, 'Trailer Released');
INSERT INTO STATUS_LIST VALUES (4, 'Poster Released');
INSERT INTO STATUS_LIST VALUES (5, 'Now Playing');
INSERT INTO STATUS_LIST VALUES (6, 'IMAX Screening');
INSERT INTO STATUS_LIST VALUES (7, '4DX Screening');
INSERT INTO STATUS_LIST VALUES (8, 'Early Bird Discount');
INSERT INTO STATUS_LIST VALUES (9, 'Late Night Show');
INSERT INTO STATUS_LIST VALUES (10, 'Stage Greeting');
INSERT INTO STATUS_LIST VALUES (11, 'Guest Visit (GV)');
INSERT INTO STATUS_LIST VALUES (12, 'Merchandise Event');
INSERT INTO STATUS_LIST VALUES (13, '1+1 Ticket Event');
INSERT INTO STATUS_LIST VALUES (14, 'Re-release');
INSERT INTO STATUS_LIST VALUES (15, 'Screening Ended');
INSERT INTO STATUS_LIST VALUES (16, 'VOD Available');
INSERT INTO STATUS_LIST VALUES (17, 'OTT Release');
INSERT INTO STATUS_LIST VALUES (18, 'Director''s Cut');
INSERT INTO STATUS_LIST VALUES (19, 'Film Festival Entry');

-- Create a MOVIES table with three columns
CREATE TABLE MOVIES (ID INTEGER, GENRE VARCHAR(100), STATUS VARCHAR(100));

-- Insert the data in MOVIES table
INSERT INTO MOVIES (ID, GENRE, STATUS)
SELECT
    -- 1. Generate 60M sequential IDs.
    T.GENERATED_PERIOD_START AS ID,

    -- 2. Generate Genre with skewed distribution.
    CASE
        WHEN MOD(T.GENERATED_PERIOD_START, 10000000) = 0 THEN 'Action' -- Every 10M row gets 'Action'.
        ELSE GL.ITEM
    END AS GENRE,

    -- 3. Generate Status with skewed distribution.
    CASE
        WHEN MOD(T.GENERATED_PERIOD_START, 10000000) = 0 THEN 'Coming Soon' -- Every 10M row gets 'Coming Soon'.
        ELSE SL.ITEM
    END AS STATUS

FROM
    -- Generate a virtual table with 60M numbers using the HANA series generator.
    SERIES_GENERATE_INTEGER(1, 1, 60000001) AS T
    -- JOIN with the remaining values for the round-robin logic.
    LEFT JOIN GENRE_LIST AS GL ON GL.ID = MOD(T.GENERATED_PERIOD_START - 1, 9) + 1
    LEFT JOIN STATUS_LIST AS SL ON SL.ID = MOD(T.GENERATED_PERIOD_START - 1, 19) + 1;


-- Create a MOVIES2 table with three columns
CREATE TABLE MOVIES2 (ID INTEGER, GENRE VARCHAR(100), STATUS VARCHAR(100));

-- Insert only 100 rows with repeated patterns
INSERT INTO MOVIES2 (ID, GENRE, STATUS)
SELECT
    ROW_NUMBER() OVER() AS ID,
    CASE MOD(ROW_NUMBER() OVER(), 4)
        WHEN 0 THEN 'Action'
        WHEN 1 THEN 'Comedy' 
        WHEN 2 THEN 'Drama'
        ELSE 'Thriller'
    END AS GENRE,
    CASE MOD(ROW_NUMBER() OVER(), 3)
        WHEN 0 THEN 'Coming Soon'
        WHEN 1 THEN 'Now Playing'
        ELSE 'Rating Pending'Continue to - [Exercise 3 - Elastic Compute Node Advisor and SAP Automation Pilot](../ex2/README.md)
    END AS STATUS
FROM SERIES_GENERATE_INTEGER(1, 1, 101);

-- Create indexes that will not be used
CREATE INDEX MOVIES2_GENRE ON MOVIES2(GENRE);
CREATE INDEX MOVIES2_STATUS ON MOVIES2(STATUS);

-- Delta Merge
MERGE DELTA OF MOVIES;
 
-- Off Auto Merge
ALTER TABLE MOVIES DISABLE AUTOMERGE;
ALTER TABLE MOVIES2 DISABLE AUTOMERGE;

-- Plan Cache Clear
ALTER SYSTEM CLEAR SQL PLAN CACHE;

-- A procedure that generates a workload by repeatedly executing two SELECT queries for a specified number of iterations.
CREATE OR REPLACE PROCEDURE GENERATE_SELECT_WORKLOAD(IN ITERATION_COUNT INT)
LANGUAGE SQLSCRIPT AS
BEGIN
    DECLARE dummy_count INT;
    DECLARE i INT;

    FOR i IN 1..ITERATION_COUNT DO
        -- 1. Execute query for GENRE = 'Action'
        SELECT COUNT(GENRE) INTO dummy_count FROM MOVIES WHERE GENRE = 'Action';

        -- 2. Execute query for STATUS = 'Coming Soon'
        SELECT COUNT(STATUS) INTO dummy_count FROM MOVIES WHERE STATUS = 'Coming Soon';
    END FOR;
END;

-- Call the workload generation procedure (repeating each query 10,000 times)
CALL GENERATE_SELECT_WORKLOAD(10000);
```

Once you see it's successfully executed, you are ready to start Exercise 2.1.

Continue to - [Exercise 2.1 - Generate index recommendations with Index Advisor](ex2.1/README.md)





