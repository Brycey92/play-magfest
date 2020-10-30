## Mark's notes

Basically there is an openvidu server running in the background on Ubuntu to distribute webrtc video streams. I have one VM per game which runs the emulator, and has a browser tab open to https://play.magfest.org/stream which captures the  screen and sends it to the server. For the controls, I send the current state of your inputs to a crossbar server 50 times/second, and a script averages the inputs and sends them back to the crossbar. Each emulator VM has a script that emulates a joystick and listens for the final input commands.

In this repo, backend is a tiny server that gives out stream keys to your browser to control access and also stores game metadata (name, console, which controller to use, etc. All the stuff you define in the stream interface) Runs next to openvidu.

button_server is an electron app that receives all the client inputs and outputs the controller commands. This part needs work. I had a much better version for the magstock stream, but apparently lost that code...

frontend is the website for both streaming and watching.

joystick is another electron app that receives controller commands and simulates a hardware joystick.

I'm using proxmox to run VMs, and just plain old windows 10. It shouldn't matter which version, you just need a version where chrome's desktop capture works.

I don't like how javascript heavy it all is, but js is the happiest language to interface with crossbar, and everything is running on windows to make the browser streaming easier. Given more time, I want to figure out how to stream from a gstreamer pipeline to webrtc and will rewrite everything probably in python, though I might do rust since I've been enjoying it recently and it's easier to package.


## Required Software

The softwares running on each Ubuntu system can be combined onto fewer Ubuntu systems. The split is documented here to show the portability of each set of softwares/servers.

### Linux:
- Docker
- Docker compose (minimum 2.14 for OpenVidu)
- [OpenVidu](https://docs.openvidu.io/en/latest/deployment/deploying-on-premises/)
- [backend](backend)

### Linux:
- [Node.js](https://nodejs.org/en/)
- [Python 3](https://www.python.org/downloads/)
- Electron
- [button_server](buton_server)

### Linux, Mac, or Windows
- Docker
- [Crossbar](https://crossbar.io/docs/Installation/)

### Linux, Mac, or Windows:
- Docker
- [frontend](frontend)

### Windows:
- Docker
- [Node.js](https://nodejs.org/en/)
- Chrome browser
- Emulator software
- ROM file
- [joystick](joystick)
