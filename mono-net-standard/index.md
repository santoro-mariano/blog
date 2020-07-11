# Mono and .Net Standard

## TL;DR

If you get the following error when you are running a .Net application on linux using mono:

```
Could not load file or assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken...
```

Make sure you have installed the complete mono package (apt install mono-complete) and not just the runtime.

---

Today I was ready to deploy a .Net application I had been working for the last few months on my raspberry pi zero w so, after flashing the Raspberry Pi OS (ex Raspbian) image on the microSD card and connecting the Raspberry Pi to the power source I connected my computer to the raspberry pi using an SSH client:

```
ssh pi@raspberrypi.local
```

So far so good, everything was as expected. Now it's time to install mono! :blush:

To do that I followed the proper instructions in [mono website](https://www.mono-project.com/download/stable/#download-lin-raspbian):
```bash
sudo apt install apt-transport-https dirmngr gnupg ca-certificates

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF

echo "deb https://download.mono-project.com/repo/debian stable-raspbianbuster main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list

sudo apt update

sudo apt install mono-runtime
```

Once the runtime was installed I proceed to run the application. I wasn't completely sure if it was goint to work properly because my app was basically a [GRPC Server](https://grpc.io/) and I needed to compile the GRPC core library from the source code using cross-compilation for ARM architecture to make it work on the Raspberry Pi.

As I suspect, the application didn't work, but the error wasn't something I was expecting:

```
Could not load file or assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken...
```
Wait... WHAT?? .Net Standard?

I thought mono could run .Net Standard libraries...

![confusion meme](https://github.com/santoro-mariano/blog/raw/master/mono-net-standard/assets/confusedmeme.jpg "Confused")

As every single software developer in this world, the first thing I did was ask to my friend google "What the heck is happening???" and, of course, it already had the answer (sort of):

![github issue](https://github.com/santoro-mariano/blog/raw/master/mono-net-standard/assets/github.jpg "Github Issue")

More precisely, then solution was here:

![github issue](https://github.com/santoro-mariano/blog/raw/master/mono-net-standard/assets/answer.jpg "Github Issue")

Once I installed mono completely and not just the runtime, it worked like a charm.

Cheers! :beers: