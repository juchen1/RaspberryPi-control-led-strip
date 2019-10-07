# Raspberry Pi - Control Kitchen Cabinet lights

This project contains the tutorial files for EasyProgramming.net

## Contents

1. Front end - index.html, script.js, and style.css make up a very simple frontend UI for our app
2. led - this directory contains the Flask app that will act as our back end app

# Tutorial
This is currently in progress And it is a newer version of the following repo: https://github.com/naztronaut/RaspberryPi-browser-led-control
A full video tutorial can be found on YouTube at https://www.youtube.com/watch?v=c6FOpPXbLjs
<a href="https://www.youtube.com/watch?v=c6FOpPXbLjs" target="_blank"><img src="https://www.easyprogramming.net/img/ledBrowserControl.png" width="700px" alt="Control an LED from a browser"></a>

More information on the tutorial can be found at https://www.easyprogramming.net/raspberrypi/browser_control_led.php

## Prerequisites
Everything in this tutorial is the end product of what we've learned so far about the Raspberry Pi and some things we learned with JavaScript and jQuery. If you get stuck anywhere, take a look at these tutorials:

1. [Headless Raspberry Pi](https://www.easyprogramming.net/raspberrypi/headless_raspbery_pi.php)
2. [Using the RPi.GPIO library to turn on an LED](https://www.easyprogramming.net/raspberrypi/gpiozero_rpigpio_led.php)
3. [Run Apache on your Pi](https://www.easyprogramming.net/raspberrypi/pi_apache_web_server.php)
4. [Running a Flask App on your Pi](https://www.easyprogramming.net/raspberrypi/pi_flask_app_server.php)
5. [Run Flask behind Apache](https://www.easyprogramming.net/raspberrypi/pi_flask_apache.php)
6. [Simple AJAX with jQuery/JavaScript](https://www.easyprogramming.net/jQuery/get_data_ajax_method.php)

### Installation 

Follow the tutorial here to learn how to run a Flask app behind Apache: https://www.easyprogramming.net/raspberrypi/pi_flask_apache.php

Since we are running `RPi.GPIO` from a virtual environment, we need to take one extra step after activating venv and install the package:

```bash
pip3 install RPi.GPIO
```

We need to do this because our virtual environment can't access the globally installed RPi.GPIO package

### Configurations

The `script.js` has jQuery that calls the Flask app using simple AJAX calls. They assume that the path for the flask app is `/led` - 
if you use another path, change this in the JavaScript to avoid getting 404s on your AJAX calls. 

Place the `utils/apache-led.conf` configuration file in the appropriate Apache/sites-available directory and enable it with with:

```bash
sudo a2ensite apache-led.conf
```

The `led.wsgi` file should be placed in the same directory as `led.py` which contains your Flask controllers. If you rename the flask controller, you have edit the `wsgi` file to reflect the changes. 

If everything is set up correctly, the AJAX call will happen with the following url: `http://{{ip_addr}}/led?status=on`

Only a status of `on` or `off` are accepted. Anything else will return a simple error message. Open up the JavaScript console for more info.  

### Status.txt

Currently, the `status.txt` file is used to store the status of the lights so that we can detect it and store it in memory. Because of how flask and apache works,
the path in `led.py` for the txt file must be an absolute path. Currently it is the following:

```
/var/www/html/kitchenLights/led/status.txt
```

Edit it if your path differs from this repo. 