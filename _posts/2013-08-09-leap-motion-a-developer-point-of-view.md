---
layout: post
title: "Leap Motion : A developer's point of view"
category: english
comments: true
---

Last week, the company [Optik 360](http://www.optik360.com/), specialized in 360 degrees pictures, asked me to test Leap Motion by creating an application to view panoramic images. The image would move according to the movements made by the user.

For those who don't know yet Leap Motion, it is a small motion sensor about eight centimeters. For more information, visit [their official website](https://www.leapmotion.com/).

Please note *I'm not a native english-speaker*. I probably did a lot of mistakes in this article. If you want to correct my mistakes, you can change the text [here](https://github.com/GCorbel/blog/blob/gh-pages/_posts/2013-08-09-leap-motion-a-developer-point-of-view.md) and do a pull request. Thanks!

Step 1 : Installation
---------------------

On Windows, installing takes less than 5 minutes.
I went to the specified page to download the application, installed it, and my movements were detected by the Leap Motion sensor.

I expected that my CPU would be overloaded: actually it's not! When I move in front of the sensor, Leap Motion uses 7% of my CPU (my Windows Media Player uses 8%).
However, I chose maximum accuracy at the expense of speed, in the Control Panel. So, there is no problem with speed.

After moving my hands in all directions by testing the provided applications (my dog think I'm crazy), I wanted to install it on Ubuntu 12.04.
It is possible to download a Linux-compatible version of the SDK on their site. Installing was also very fast.
The sensor and the configuration panel are launched properly. The only problem I encountered: it does not work.
The controller does not recognize my movements. I sent [a message on their forum](https://developer.leapmotion.com/forums/forums/10/topics/1811).
For now, I have no response. We'll see for the future.

Step 2 : First development
-------------------------------

Then I went back to Windows to start developing. Leap Motion has an official SDK in multiple languages.
Unfortunately, my favorite language, Ruby, is not officially supported. I think it's great to have chosen to be compatible with multiple languages. It gives a lot of opportunities for developers.

For my first project, I choose to develop in JavaScript. Leap Motion allows to do a client-side application. The application listens a port to receive data from the sensor. I choose JavaScript because there is no need to do an installation on a PC. A simple (modern) web-browser allow to have an application that works with Leap. Awesome!

The library provided by Leap is impressive. It's super simple to do an application. Only few lines of code are enough. Here is an example:

{% highlight coffeescript %}
Leap.loop(function(frame) {
  console.log(frame.fingers.length)
});
{% endhighlight %}

This snippet logs the number of fingers detected by the controller. Any developer can do that.

Step 3 : Search for a JavaScript library
-------------------------------------------------

After some tests, I stated to looking for any existing code to display panoramic images. I found [this project](http://mrdoob.github.io/three.js/examples/webgl_panorama_equirectangular.html). A big advantage of this code is that it uses [Three.js](http://threejs.org/). Three.js is a library which allows to do 3D animations in JavaScript. There is a [a lot of example provided by Three.js's developers](http://mrdoob.github.io/three.js/). With hard work, we can develop a Leap compatible version of all these examples.

So, I downloaded the code locally and I started to edit it to customize it as I want. I changed it to be more object-oriented and I used CoffeeScript.


Phase 4 : Integration of the Leap library
-----------------------------------------------

I studied the behavior of the mouse and I wanted to replicate it. First, I extracted the code for the mouse in a separate class. To test the movement with Leap Motion, my first instinct was to do this kind of code :

{% highlight coffeescript %}
class LeapPano.LeapMotion
  constructor: (view) ->
    @view = view

  init: =>
    Leap.loop((frame) =>
      @view.setLon(@view.getLon() + .1)
    )
{% endhighlight %}

This code allows, for each iteration, to move the camera horizontally.
A problem with the code is that I do not manage the iterations speed.
I didn't found a way to make it on the Leap's documentation. I choose to do it like this :

{% highlight coffeescript %}
class LeapPano.LeapMotion
  constructor: (view) ->
    @view = view

  setFrame: (frame) ->
    @frame = frame

  init: ->
    setTimeout(@checkMotion, 10)

  checkMotion: =>
    if @frame?
      @view.setLon(@view.getLon() + .1)

    setTimeout(@checkMotion, 10)
{% endhighlight %}

The call to the function `Leap.loop` is extracted from this object and the result is passed via a setter at each iteration. At the initialization, the `setTimeout` function that allows to execute the movements recognation function every ten milliseconds, is launched. This code allow also allows to not have multiple calls to the function `Leap.loop`. The loop is started only one time, in the main page. The date recognized are sent to different objects in need. I think this is a better architecture.

Now, it works as I want. I can take care of the motion recognition. I changed the function `checkMotion` like this :

{% highlight coffeescript %}
checkMotion: =>
  if @frame?
    finger = @frame.fingers[0]
    if finger?
      x = finger.tipPosition[0]
      y = finger.tipPosition[1]
      @view.setLon(@view.getLon() + (x/320))
      @view.setLat(@view.getLat() + ((y-160)/320))

  setTimeout(@checkMotion, 10)
{% endhighlight %}

`@frame.fingers[0]` retried the first finger detected. `finger.tipPosition` returns an array containing the position in X, Y and Z. Finally, I change the position of the camera in the 3D scene. In this code, a ratio is calculated to move faster if the finger is far of the center of the sensor.

The last feature I wanted to add was to change the image when the finger do a circle. So, I added this :

{% highlight coffeescript %}
if(@frame.gestures.length > 0)
  gesture = @frame.gestures[0]
  if gesture.type == "circle"
    @view.switchFile()
{% endhighlight %}

Again, it's very simple. `@frame.gestures` returns a list of detected movements. Thereafter, the code checks if the type of movement is a circle. If this is the case, the image is changed.

To recognized the movements, it is necessary to call the function `Leap.loop` like this :

{% highlight coffeescript %}
Leap.loop({enableGestures: true}, function(frame) {
  pano.setFrame(frame)
})
{% endhighlight %}

And voila! Everything works. As a good Open-Source contributor, I posted the code here : [https://github.com/GCorbel/LeapPano](https://github.com/GCorbel/LeapPano). I also created a page with [GitHub-Pages](http://pages.github.com/) visible here : [http://gcorbel.github.io/LeapPano/](http://gcorbel.github.io/LeapPano/).

What I like Leap Motion
-----------------------

First, I loved the ease of installation. It's really Plug and Play. The development is extremely easy. The API is provided as simple as possible. The availability in few languages is awesome. I'm really impressed with the opportunities provided and simplicity.

Where I have some doubts
------------------------

Small flat for documentation. I think the documentation is not very detailed. There is a little code displayed. To see the actual code, it's necessary to look the examples available on GitHub.

At the motion recognition, there are three basic movements, Circle, Swipe and Tap. To add movement is much complex. I cannot imagine a game like [Black & White](http://en.wikipedia.org/wiki/Black_%26_White_%28video_game%29) for the moment.

The weak point is the accuracy. The number of detected fingers is not always real and the position is approximate. When you think about it, this is normal. If you point an image to the screen, your finger is shaking. Targeting a small icon can be very long. Leap Motion is not ready to replace the mouse.

In addition, I find the tool detection is very hazardous. A pencil pass from detected to undetected without reason. Finger detection is much more stable.

Actually, the Leap Motion's version is 0.8. This is a brand new system. We just imagine that the system will be improved over time.

Conclusion
----------

I love Leap Motion!

The future as imagined in the 80s/90s is our worn. We are not yet able to make screen as in Minority Report, but you get approach. Technologies such as Leap, Google Voice, Google glaces, home automation, etc. are dreaming. The future is open to any developers. We can do impressive things.
