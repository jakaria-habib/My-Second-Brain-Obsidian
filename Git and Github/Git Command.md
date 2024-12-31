
/*git initialization and config*/
git --version
1. git init
2. git config --list
git config --global user.name "your user name"
git config --global user.email "your user email"

/* staging in git*/
ls
mkdir assets
cd assets
git status
git add index.html
3. git add --all

/* committing in git*/
4. git commit -m "first commit"

/* git push */
5. git branch -M main
6. git remote add origin https://github.com/Khanasif2642/test456.git
7. git push -u origin main

/* git pull */
git pull

# My documentation on Git and Github

1. git init
2. git config --list
3.  if no github account 
	git config --global user.name "your user name"
	git config --global user.email "your user email"
4. git add --all
5. git commit -m "first commit"
6. jodi new project github a rakha lage tahole ei code follow koro ,from now get code from github account 
	 git branch -M main
	 git remote add origin https://github.com/Khanasif2642/test456.git
	 git push -u origin main
7.  jodi project already thake and code again gitgub a rakha lage tahole etai final command
     `git push -u origin main`	

#### git init>git status>git add .>git status>git commit -m "commit message">git pull origin jakaria>git push origin jakaria
## pull system from github

1. go folder in computer where you want to pull the github project and write the below code  
git clone "copy project's url from github and paste in the middle of inverted comma"



## how to install or pull project from github on laravel phpstorm

1. clone or download project from github and keep it on computer folder. unzip the project if you download it.
2.  folder theke cmd on kore then write this code jekono akta dilei hobe ` composer update`
3. tarpor project on koro phpstorm theke and then `.env.example` file ta k rename koro likho  `.env`  jodi rename option refactor theke na ase tahole age db name connection agula on kore dao. 
4. tarpor db _connection= mysql, db_database= give db name, db_username = root , db_password =    (faka rakho)
5. phpmyadmin a giye database create koro
6. then cmd prompt theke command likho `php artisan migrate`
7. then cmd  a giye likho  `php artisan key:generate`

1. then cmd  a giye likho  `php artisan storage:link`
2. then cmd  a giye likho  `php artisan serve` finshed now
