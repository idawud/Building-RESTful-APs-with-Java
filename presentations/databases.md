# Working Databases (Postgres) 

## Notes

* [Relational Database](https://www.ibm.com/cloud/learn/relational-databases)
* [SQL](https://www.w3schools.com/sql/default.asp)
* [Postgresql Tutorials](https://www.postgresqltutorial.com/)

#### What is a relational database?
A relational database organizes data into tables which can be linked—or related—based on data common to each. This capability enables you to retrieve an entirely new table from data in one or more tables with a single query. It also allows you and your business to better understand the relationships among all available data and gain new insights for making better decisions or identifying new opportunities.

For example, imagine your company maintains a customer table that contains company data about each customer account and one or more transaction tables that contain data describing individual transactions.

The columns (or fields) for the customer table might be Customer ID, Company Name, Company Address, etc.; the columns for a transaction table might be Transaction Date, Customer ID, Transaction Amount, Payment Method, etc. The tables can be related based on the common Customer ID field. You can, therefore, query the table to produce valuable reports, such as a consolidated customer statement.

Report generators take these queries and run them on demand to create formal reports. Many of the documents businesses run to track inventory, sales, finance, or even perform financial projections come from a relational database operating behind the scenes.

#### What is SQL?
You can communicate with relational databases using Structured Query Language (SQL), the standard language for interacting with management systems. SQL allows the joining of tables using a few lines of code, with a structure most nontechnical employees can learn quickly.

With SQL, analysts do not need to know where the order table resides on disk, how to perform the lookup to find a specific order, or how to connect the order and customer tables. The database compiles the query and figures out the correct data points.

### 1. Introduction

-----------------------------------------------------------------------------------------------
 
## Lab 1 Databases
Since have our deployment on Heroku, we can take advantage of their free Postgres database add-on.

### Step 1 - Add Heroku Postgres
1. Click on the Resources Tab and Search for `Heroku Postgres` and select it.
![Heroku Postgres Addon](/presentations/images/DBpostgres-Addon.png)

2. Navigate to the Settings Tab of the just added addon and click on View credentials. It contains the database name, user, password and url which we'll need to connect to it from our app. 
![Heroku Postgres Credentials](/presentations/images/DBviewcredentials.png)

3. Now open you app again in another browswer Tab. In the settings of the app click on `Reveal Config Vars` to view your Environment Variables.
![Heroku Postgres Config](/presentations/images/DBherokuConfig.png)

4. By Default the `DATABASE_URL` variable is provided but we'll have to create variables for `DATABASE_USERNAME` and `DATABASE_PASSWORD` and set their values to the User and Password from the Heroku Postgres Addon.
![App Postgres Config](/presentations/images/DBSetConfigFromAddon.png)


### Step 2 - Add JDBC Dependencies
To work with postgre with need 2 dependencies postgres and JDBC to execute the queries.
``` build.gradle
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'org.postgresql:postgresql'
```

![JDBC Deps](/presentations/images/DBlibs.png) 

### Step 3 - application.properties config
The application.properties keeps config which spring boot uses mostly for dependency Injections. Here to want provide the values in the Environment Variables we setted on heroku. The `${}` means its reading from env or we can set the values here direct but it isnt adviceasble.
``` application.properties
spring.datasource.url=${ DATABASE_URL}
spring.datasource.username=${ DATABASE_USERNAME}
spring.datasource.password=${ DATABASE_PASSWORD}
spring.datasource.driver-class-name=org.postgresql.Driver
```

### Step 4 - DataSource Configuration
Next we create a class with Configuration anotation which reads the env values and creates a datasource for us i.e. the database connection.
```Configuration
@Configuration
public class DatabaseConfig {
    @Autowired
    private Environment env;

    @Bean
    public DataSource dataSource(){
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(Objects.requireNonNull(env.getProperty("spring.datasource.driver-class-name")));
        dataSource.setUrl( env.getProperty("spring.datasource.url"));
        dataSource.setUsername( env.getProperty("spring.datasource.username") );
        dataSource.setPassword( env.getProperty("spring.datasource.password") );
        return dataSource;
    }
}
```

### Step 5 - JdbcTemplate and SQL Queries
Next is autowiring the JdbcTemplate to the TodoStage class which will automatically work with our DataSource Bean and allow to execute the queries we want. 
```
@Autowired
    private JdbcTemplate jdbcTemplate;
```
![JDBCTemplate](/presentations/images/jdbcTemplate.png.png)

We'll refactor all the function to work with the db instead of the Map variable.
