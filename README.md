# Building-an-End-to-End-Scalable-Secure-and-Reliable-Data-Pipeline-in-Snowflake
Building an End-to-End Scalable, Secure, and Reliable Data Pipeline in Snowflake for performing Advanced Analytics on ODI cricket data

# End-to-End Data Engineering Project with Snowflake: Analyzing ODI Cricket Data :cricket: :snowflake:

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

## :sparkles: Overview

Welcome to the **End-to-End Data Engineering Project** repository! This project showcases a comprehensive data engineering solution built using Snowflake to analyze One Day International (ODI) cricket data. From data ingestion to insightful dashboards, this repository provides all the necessary resources to understand and implement a modern data pipeline.

This project is based on the [Data Engineering Simplified](https://data-engineering-simplified.medium.com/end-to-end-data-engineering-project-using-snowflake-8f8e4f0fd1d0) article by Raunak Malapure.

## :tv: Project Demo

Check out the [video tutorial](https://youtu.be/qDmqE89DSQQ) to get a walk-through of the project.

## :dart: Key Objectives

By exploring this project, you'll learn how to:

*   :arrow_right: Ingest and load JSON data from local machines into Snowflake stages.
*   :arrow_right: Handle both small and large JSON files efficiently.
*   :arrow_right: Implement delta data loading from local machines to internal stages.
*   :arrow_right: Use Snowflake's `$ notation` to query staged files.
*   :arrow_right: Execute `COPY` commands to load JSON files into tables.
*   :arrow_right: Design a multi-layered data architecture with fact and dimension tables.
*   :arrow_right: Automate data flow using Snowflake Tasks and Task Trees.
*   :arrow_right: Build interactive dashboards using Snowsight.

## :floppy_disk: Data Flow

The project follows a structured data flow:

1.  **Data Ingestion:** Loading raw JSON cricket data into Snowflake.
2.  **Staging:** Utilizing Snowflake stages for temporary data storage.
3.  **Raw Layer:** Creating raw tables to store the ingested JSON data.
4.  **Clean Layer:** Transforming and cleaning the raw data into structured tables.
5.  **Consumption Layer:** Designing fact and dimension tables for analysis.
6.  **Automation:** Implementing tasks to automate the data pipeline.
7.  **Visualization:** Building dashboards using Snowsight.

## :hammer_and_wrench: Architecture

![Architecture Diagram](images/architecture.png)
*A high-level architecture diagram of the data pipeline.*

## :bulb: Project Structure



## :rocket: Getting Started

Follow these steps to set up the project:

1.  **Prerequisites:**
    *   A Snowflake account with appropriate privileges.
    *   Basic understanding of SQL and data warehousing concepts.
2.  **Clone the Repository:**

    ```
    git clone https://github.com/<your-github-username>/end-to-end-snowflake-data-engineering.git
    cd end-to-end-snowflake-data-engineering
    ```
3.  **Set Up Snowflake:**
    *   Create the necessary database and schemas.
    ```
    CREATE DATABASE cricket;
    CREATE SCHEMA land;
    CREATE SCHEMA raw;
    CREATE SCHEMA clean;
    CREATE SCHEMA consumption;
    ```
4.  **Create Stage and File Format:**
    ```
    -- Create file format
    CREATE OR REPLACE FILE FORMAT my_json_format
    TYPE = json
    NULL_IF = ('\\n', 'null', '')
    STRIP_OUTER_ARRAY = true
    COMMENT = 'Json File Format with outer strip array flag true';

    -- Create internal stage
    CREATE OR REPLACE STAGE my_stg;
    ```
5.  **Data Loading:**
    *   If sample data is provided, load it to Snowflake. Otherwise, follow the instructions in the `data/` directory.  Consider joining the [Facebook private group](https://www.facebook.com/groups/your-facebook-group) to get more data files.
    ```
    -- Copy data into raw table
    COPY INTO cricket.raw.match_raw_tbl
    FROM (
        SELECT
            t.$1:meta::OBJECT AS meta,
            t.$1:info::VARIANT AS info,
            t.$1:innings::ARRAY AS innings,
            metadata$filename,
            metadata$file_row_number,
            metadata$file_content_key,
            metadata$file_last_modified
        FROM @cricket.land.my_stg/cricket/json (FILE_FORMAT => 'cricket.land.my_json_format') t
    )
    ON_ERROR = CONTINUE;
    ```
6.  **Run the SQL Scripts:**
    *   Execute the SQL scripts in the `scripts/` directory in the order of the layers (land, raw, clean, consumption, automation).
7.  **Explore the Data:**
    *   Use Snowsight to query the tables and build dashboards.
8.  **Automation:**
    *   Set up tasks to automate data loading and transformation.

## :open_file_folder: Data Model

![Data Model Diagram](images/data_model.png)
*The data model for the project, showing the relationships between tables.*

## :question: FAQs

*   **Where can I find more ODI cricket data?**
    *   Join my [Facebook private group](https://www.facebook.com/groups/your-facebook-group) to access a wealth of JSON files for your analysis.
*   **How do I contribute to this project?**
    *   Fork the repository, make your changes, and submit a pull request.
*   **I'm getting errors when running the SQL scripts. What should I do?**
    *   Double-check that you have the correct Snowflake account and permissions.
    *   Ensure that the data files are in the correct location and format.
    *   Refer to the Snowflake documentation for troubleshooting tips.

## :handshake: Contributing

Contributions are welcome! Feel free to open issues or submit pull requests to improve this project.

## :copyright: License

This project is licensed under the [MIT License](LICENSE).




