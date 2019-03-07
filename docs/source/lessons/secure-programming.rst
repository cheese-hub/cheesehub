Secure Programming 
==================

Several cybersecurity flaws can
ultimately be traced to a previously undetected vulnerability in computer
software. With the burgeoning use of computer software and libraries in every
conceivable domain, such vulnerabilities could be introduced from an imported
library, without a software developer’s knowledge. Thus, it is vital to instruct
both the current and future generation of developers (and not just cybersecurity
engineers) on all known vulnerabilities and their solutions. As a start,
CHEESEHub includes demonstrations of a few well-known and far-reaching software
vulnerabilities.  

SQL Injection 
-------------

The SQL (Structured Query Language)
Injection bug exploits the vulnerability in database client code that does not
properly validate user inputs. SQL is a computer language for performing query,
insertion, update, and deletion operations on the data stored in a database. The
SQL syntax also allows for several commands to be chained together, or to ignore
the rest of a command using a comment character. Database client code that
simply substitutes in a user’s input without validation can be exploited by a
malicious user to run arbitrary SQL commands. Since databases often store
privileged customer data in many applications, or implement access control for
applications, a malicious user can either retrieve the privileged data, or gain
access to secured resources.  

**Demonstration**

This demonstration has two parts,
first we use vulnerable client code and demonstrate how a malicious user with
knowledge of SQL syntax can retrieve, insert, or even delete data from a
database that contains user and password records. In the second part, we start
by making a minor fix to the database client code to get rid of the
vulnerability. We will then demonstrate how the malicious user can no longer
retrieve, insert, or delete data from the database after this fix. This
demonstration is not intended as a complete account of all possible SQL
injection attacks, neither does it provide the solution to all such attacks. It
primarily demonstrates one possible vulnerability in database client code that
developers need to be aware of, and, a specific solution that works for the
client code in question. 

A step-by-step account of the demonstration is as follows:

Step 1: After adding the SQL Injection application and starting the container,
launch it:


Step 2: Open the SQLInjection.ipynb Jupyter notebook for instructions on
following the demonstration:

 
Step 3: The notebook provides some introductory material as well as various
scenarios that exploit the vulnerability in the database client:


Step 4: All of the commands specified in the notebook need to be run in a
terminal. Start the terminal emulator included in Jupyter:

Step 5: The demonstration uses a simple program that queries a database for user
details when provided with the right username/password pair. To run the program,
type EmailCloud at the terminal prompt:


Step 6: When a valid username/password pair is provided, the program will return
details about that user from the database:


Step 7: When typing the malicious inputs, pay attention to the special
characters being used (single quotes, semicolons, etc.):


Step 8: You can always exit the program by typing ctrl-c at the Username:
prompt:


Step 9: To modify the client code to secure it against the vulnerability, go
back to the Jupyter file listing page in your browser and navigate to the app
folder, and click on the client.py file to open it:



Step 10: Look for the lines marked as unsafe in the client.py file; the safe
alternative is right below these lines:


Step 11: Comment out the unsafe lines (using #) and uncomment the safe lines, so
that the file now looks like this:


Step 12: Save the changes to the client.py file:


Step 13: Head back to the terminal, and restart the EmailCloud program. This
time, the malicious inputs should no longer work:

**More Information**

For more information on SQL Injection attacks, refer to ...


