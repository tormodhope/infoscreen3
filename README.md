# Info screen<sup>3</sup>

When you need to display same content to multiple locations or need to remotely control a slide show.

## Features
* Displays slide show in browser
* Uses Full HD 1920x1080 resolution internally
* Easy-to-use editor for slides. Supports adding easily text and image content
* The output resolution does not matter, all contents are automatically scaled from the Full HD source
* Finnish and English language support, sets automatically from browser language.
* Multiple renderers to choose from:
  1. WebGL (with 60+ changeable transitions)
  2. CSS3 (only blinds3D)
  3. Lite (only cross fade)
       *  in case your viewer laptop is totally a potato
* Content
  * Multiple screen locations, for example big screen may have different slide shows than kiosk -screen etc.
  * Webpages as slide + custom scaling of the content
    * can be useful for dynamic content like timetables, brackets and so on
  * Now with embedded Youtube video support (to have a 10h nyancat)
  * Supports mp4 video as a slide, in case you wish to run ads or democompo entry as a content slide.
  * Optional support for local RTMP Live streams, buffer delay is about 2 seconds.

## Setup
1. run `npm install`
2. copy `config-default.js` to `config.js`
3. run `npm start`
   - optionally you can start as a background task: `npm run-script daemon`, it will output just a pid for the new process and you find new files: `output.log` and `errors.log` at the `data` directory.

## Configuration
> The default `config.js` file serves you well only if you wish to run infoscreen at localhost only!

To access the infoscreen server outside from the localhost, like at LAN network, the config must be bind to external IP your server machine has.

Example:
```javascript
module.exports = {
    "serverListenPort": 8000,
    "serverHost": "192.168.56.100",
    // ...config file continues from here...
```
If you wish to have live stream support at local network using OBS, set `mediaServer` to `true` and set desired streamKey
You can run the server listen port as well the default http port `80`, but in that case you have to run the app with privileges at linux systems: `sudo npm start`

## Plugins

List of available plugins

| Plugin   | Description                                                                    |
| :------- | :----------------------------------------------------------------------------- |
| profiler | Outputs memory usage statistics at console                                     |
| overlay  | Change infoscreen to work as overlay to stream, by disabling background layers |
| ping     | Example plugin to display ping for local network machine.                      |

## Usage
Viewer is located at: http://localhost:8000<br>
Admin interface is located at: http://localhost:8000/admin<br/>

> for production it is highly encouraged to change admin credentials from the default!

| Username | default password |
| :------- | :--------------- |
| admin    | admin            |
| view     | view             |

## Local stream support for OBS
> Works only when config has `mediaServer` set to `true`

at OBS go to `Settings` -> `Stream`

| Setting            | Value                                              |
| :----------------- | :------------------------------------------------- |
| Service            | Custom...                                          |
| URL                | `rtmp://localhost/live`                            |
| Stream key         | config.streamKey value, defaults to: `INFOSCREEN3` |
| Use Authentication | *leave un-ticked*                                  |

## Mediaserver admin panel
> if you have changed the default port from 8000 to something else, the correct port to access this feature is (config.serverListenPort+1)

Admin interface is located at: http://localhost:8001/admin
It accepts the same crendetials as configured at the main app.

## Environment variables
| ENV         | default   | Usage                                                 |
| :---------- | :-------- | :---------------------------------------------------- |
| PORT        | 8000      | Server listen port                                    |
| HOST        | localhost | Host or ip, where the server is externally accessible |
| ADMIN_USER  | admin     | Username to access admin interface                    |
| ADMIN_PASS  | admin     | Password for the admin interface                      |
| USER        | view      | Username to access viewer                             |
| PASS        | view      | Password to access viewer                             |
| FRONT_PROXY | false     | Tell software that it's behind a front proxy          |

## Dockerfile

Dockerfile is provided for building a docker container. Docker container accepts the same ENV variables as the standard setup (see them above).

```bash
docker build -t reaby/infoscreen .
docker run -p 8000:8000 -e HOST=infoscreen.lan -e PORT=8000 -e ADMIN_USER=admin -e ADMIN_PASS=secret --name infoscreen reaby/infoscreen
```

## Linux service

Service file for sysctl, provided by Hartsa (many thanks!)
Just edit the working directory of the script and change the user from root to yours, if needed.

```bash
sudo cp infoscreen.service /etc/systemd/system/
sudo systemctl start infoscreen.service
sudo systemctl enable infoscreen.service
```

If you need to see status of the service:

```bash
sudo systemctl status infoscreen.service
```

# Thanks
WebGL renderer bases upon initial working of [Creative WebGL Image Transitions](https://github.com/akella/webGLImageTransitions) repository here at github. Thanks for your awesome article.