# Heroku APP Deployment with Pytorch

In this Git we present how to deploy an app with [Heroku](https://www.heroku.com/)!
First of all you need to create and account at Heroku (we choosed the free version). 

### Project info

We are going to deploy an existing project: [Web Rest Api face detection with gender classification](https://github.com/MarioProjects/face_gender_detection). This project consists of an application in which we upload an image and detect the faces and their gender. For this it uses [face_recognition api](https://github.com/ageitgey/face_recognition) and a model trained to recognize the genre using Pytorch.

### Environment preparation

First of all we need to clone our project on a local folder. For this, execute where you want:

```sh
git clone https://github.com/MarioProjects/face_gender_detection.git
cd face_gender_detection
```

After that we will have cloned our project and we will be inside him. Now we can login into Heroku and initialize our work. Due to we need install opencv on Python we need use a buildpack because Heroku need some extra apt packages. Thanks to this [answer](https://stackoverflow.com/questions/49469764/how-to-use-opencv-with-heroku/51004957#51004957), at the app creation time we add a custom buildpack. Finally, as we will use python, add this to our Heroku app!

```sh
heroku login
# Your browser will request a confirmation
heroku create my-app --buildpack https://github.com/heroku/heroku-buildpack-apt.git
heroku buildpacks:add --index 2 heroku/python
```

With this we have almost everything. Only we need to define two files:

  - **Aptfile**: Without extension. Tell heroku which packages apt install. Will contain the necessary packages for opencv.
  ```
  libsm6
  libxrender1
  libfontconfig1
  libice6
  ```
  - **Procfile**: It indicates to Heroku which type of application we are deploying (a web application), and what have to run (our flask app).
```
web: python app.py
```

Heroku create my-app will tell you that this app name is on use, you only need to set your custom app name. Having everything configured, we can upload our app. Then we open it!

```sh
git add .
git commit -m "Heroku deployment!"
git push heroku master 
# This will take a time
heroku open
```

#### Common errors

If your application is not working you can see the logs with `heroku logs --tail`.

  - **Error R10 (Boot timeout) -> Web process failed to bind to $PORT within 60 seconds of launch**: This happens because you set Flask port automatically. Change your app.py port with:
  ```python
  port = int(os.environ.get('PORT', 5000))
  ```
  

License
----

MIT - **Free Software, Hell Yeah!**


