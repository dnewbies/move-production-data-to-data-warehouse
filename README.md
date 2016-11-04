# Install pgloader:
Installation guide:
https://github.com/dimitri/pgloader/blob/master/INSTALL.md

# Create new Postgres database and run command:
```
createdb pagila  
pgloader mysql://user@localhost/sakila postgresql:///pagila 
```
# A single command for VSell project 
(This command should run on Linux. A known issue has been reporting on Github relating to mySQL SSL when running on macOS)
```
pgloader mysql://user@localhost/sakila postgresql:///pagila
``` 
