# snowflake_cicd
CI/CD setup using snowflake schema change database change management


GitHub Actions: 

 

GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub. Make code reviews, branch management, and many more things the way you want. 

 

SchemaChange Overview: 

 

Database Change Management (DCM) refers to a set of processes and tools used to manage objects within a database. SchemaChange is a lightweight Python-based tool to manage all your Snowflake objects. It follows an imperative-style approach to database change management (DCM) and was inspired by the Flyway database migration tool. When schemachange is combined with a version control tool and a CI/CD tool, database changes can be approved and deployed through a pipeline using modern software delivery practices. As such schemachange plays a critical role in enabling Database (or Data) DevOps. 

 

Folder Structure 

schemachange expects a directory structure like the following to exist: 

(Project root) 

| 

|-- folder_1 

    |-- V1.1.1__first_change.sql 

    |-- V1.1.2__second_change.sql 

    |-- R__sp_add_sales.sql 

    |-- R__fn_get_timezone.sql 

|-- folder_2 

    |-- folder_3 

        |-- V1.1.3__third_change.sql 

        |-- R__fn_sort_ascii.sql 

The schemachange folder structure is very flexible. The project_root folder is specified with the -f or -root-folder argument. Under the project_root folder you are free to arrange the change scripts any way you see fit. You can have as many subfolders (and nested subfolders) as you would like. 

 

Change Scripts 

Versioned Script Naming 

With the following rules for each part of the filename: 

Prefix: The letter 'V' for versioned change 

Version: A unique version number with dots or underscores separating as many number parts as you like 

Separator: __ (two underscores) 

Description: An arbitrary description with words separated by underscores or spaces (cannot include two underscores) 

Suffix: .sql or .sql.jinja 

For example, a script name that follows this convention is: V1.1.1__first_change.sql. As with Flyway, the unique version string is very flexible. You just need to be consistent and always use the same convention, like 3 sets of numbers separated by periods. Here are a few valid version strings: 

1.1 

1_1 

1.2.3 

1_2_3 

Every script within a database folder must have a unique version number. schemachange will check for duplicate version numbers and throw an error if it finds any. This helps to ensure that developers who are working in parallel do not accidentally (re-)use the same version number. 

 

Repeatable Script Naming 

Repeatable change scripts follow a similar naming convention to that used by Flyway Versioned Migrations. The script name must follow this pattern (image taken from Flyway docs: 

e.g.: 

R__sp_add_sales.sql 

R__fn_get_timezone.sql 

R__fn_sort_ascii.sql 

All repeatable change scripts are applied each time the utility is run if there is a change in the file. Repeatable scripts could be used for maintaining code that always needs to be applied in its entirety. e.g., stores procedures, functions, and view definitions etc. 

Just like Flyway, within a single migration run, repeatable scripts are always applied pending versioned scripts have been executed. Repeatable scripts are applied in the order of their description. 

 

Always Script Naming 

Changing scripts are executed with every run of schemachange. This is an addition to the implementation of Flyway Versioned Migrations. The script name must follow pattern: 

A__Some_description.sql 

e.g. 

A__add_user.sql 

A__assign_roles.sql 

This type of change script is useful for an environment set up after cloning. Always scripts are applied always last. 

 
