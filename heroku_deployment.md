# Deploy Project - Heroku

### Documentation
https://devcenter.heroku.com/articles/heroku-cli

install heroku cli
heroku apps:create snacks-api
Check remote with git remote -v
If heroku remote doesn't show then heroku git:remote -a snacks-api
Create heroku.yml in root folder.
Add below text to heroku.yml
build:
  docker:
    web: Dockerfile
release:
  image: web
run:
  web: gunicorn project.wsgi
IMPORTANT STEP : DO NOT SKIP THIS
heroku stack:set container
Add/Commit
git push heroku main
Go to heroku
Login takes you to dashboard
Select your app
Go to settings
Click reveal config vars button
Add config vars to match .env file
ALLOWED_HOSTS should match the heroku URL for your app.
Click Open app button to see it
Leave out the https:// and trailing slash.
E.g. snacks-api.herokuapp.com
It can take a minute for the environment variable changes to take effect
heroku logs --tail will show you progress in terminal

Once site is ready then see if you can log in, create snacks, etc.
Bittersweet success!
It will be ugly because the styling was lost.
This is due to the Heroku file system's "ephemeral" nature
One way to handle issue is to run collectstatic locally then ACP to heroku.
We can also use API Tester to see if API working.
Edit api_tester.py to use the new api's url.
Update the user and password as needed
python manage.py api_tester.py create_snack --name=victory

python manage.py api_tester.py get_snacks

