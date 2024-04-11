# Attuned Voice
<sup><sub>The project structure is inspired by [VisiVox](https://github.com/GenderPerformance/gender-performance)</sub></sup><br/>
This is a web application for transgender individuals to provide helpful auditory feedback with machine learning during voice transition.

# Table of Contents
- [Attuned Voice](#attuned-voice)
- [Table of Contents](#table-of-contents)
  - [Technology Stack](#technology-stack)
  - [Install](#install)
    - [First time setup:](#first-time-setup)
  - [Verification](#verification)
  - [.env](#env)
  - [Starting on localhost 8080](#starting-on-localhost-8080)
    - [For macs:](#for-macs)
    - [For windows (in 2 terminals):](#for-windows-in-2-terminals)
  - [Deployment](#deployment)

## Technology Stack

| Frontend      | Backend       | Machine Learning     | Storage    | Authentication |
|---------------|---------------|----------------------|------------|----------------|
| React         | Node.JS       | Keras Tensorflow     | Amazon S3   | Google OAuth   |
| Redux         | PostgreSQL    | Librosa              |            |                |
| WaveSurver.JS | Python        | Pandas               |            |                |
|               |               | Numpy                |            |                |


## Install
For detailed steps, go to the First Time Setup section below. <br/>
```
npm i
npm run python-install
# create a postgres database called attuned 
npm run seed
```

### First time setup:
0. (Optional) Create venv as needed
```
python3.9 -m venv .venv
source .venv/bin/activate
``` 
1. Install/Switch to Node Version 12.11.1
```
nvm install 12.11.1
nvm use 12.11.1
node -v
```
2. Run ```npm install``` and ```npm run python-install``` in the root directory.
3. Install Postgres Version 14
```
brew install postgresql@14
export PATH="/usr/local/opt/postgresql@14/bin:$PATH"
```
3. Start Postgres, and after it starts, you should see a list of databases in psql.
```
brew services start postgresql@14
psql -d postgres
postgres=# \l
```
4. Create **```attuned```** database. You should see a response ```CREATE DATABASE``` after:<br/>
```postgres=# CREATE DATABASE attuned;```

5. Run ```npm seed``` in a separate terminal.

## Verification
1. whether the database was created successfully
In users database, you should see at least one user with their encrypted password. <br/>

```psql -d attuned``` (or if you kept the previous terminal open, you can run: ```\c attuned``` to switch to the database.)<br/>
Now run ```\dt``` and check the output:
```
attuned=# \dt
          List of relations
 Schema |    Name    | Type  | Owner 
--------+------------+-------+-------
 public | recordings | table | AC
 public | users      | table | AC
(2 rows)
```
- ```SELECT * FROM users;``` to see the users table<br/>
- ```\q``` to exit psql<br/>
- ```brew services stop postgresql``` to stop the postgres server<br/>

2. whether the python code is running correctly
In root dir, run:
```python gendervoicemodel/test.py --file "gendervoicemodel/test-samples/user-1-recording-7.wav"``
The output should be:
```{"mp": 2.39 , "fp": 97.61 }```

## .env
Create ```secrets.js``` file in the root directory and add the environment variables below:
```
process.env.GOOGLE_CLIENT_ID = 'super secret';
process.env.GOOGLE_CLIENT_SECRET = 'that you don't tell people';
process.env.AWS_ACCESS_KEY_ID = 'so keep it secret';
process.env.AWS_SECRET_ACCESS_KEY = 'do not push it to github';
process.env.S3_BUCKET = 'keep these secret';
```

## Starting on localhost 8080
**TEST USER: ```test@email.com``` password: ```123```**
### For macs:
```npm run start-dev```

### For windows (in 2 terminals):
```
npm run start-build-client-watch
npm run start-server
```

## Deployment
Update .travis.yml file with your encrypted heroku api key, your app name,
and which branch to deploy on.

Update your heroku config vars with the environment variables from above
if you want to deploy it.

Add Heroku buildpacks NodeJS, Python, https://github.com/heroku/heroku-buildpack-apt.
NodeJS has to be first




