Title: HTML5 and "Protected" iTunes Content
Author: Nathan Rajlich
Date: Mon Sep 06 2010 17:18:52 GMT-0700 (PDT)


While developing __[nTunes][]__, I came to a particularly interesting road block:

An iTunes library may consist of "protected" content that has been purchased
through the iTunes Store. "protected" really means encrypted with Apple's
[FairPlay](http://en.wikipedia.org/wiki/FairPlay) DRM encryption technology.

This is a problem for __[nTunes][]__ because an HTTP client has the ability to
request the "raw" contents of a media file in your iTunes library. This
immediately becomes a problem for playback since the file itself is encoded
by Apple, and only knows how to be played by qualified Apple products.

## But what about Safari?

But wait, Safari is an Apple product. Shouldn't it be able to playback these
"protected" files? Interestingly enough, _YES_ is the answer!

I'm now including an example page with __[nTunes][]__ demonstrating this behavior.
To start it, first install __[nTunes][]__, either with __[npm](http://github.com/isaacs/npm)__:

    npm install nTunes

Or you can checkout the repo with Git. Once that's ready, __nTunes__ comes
with a demo server that can be launched by simply invoking:

    nTunes

That should start an HTTP server listening on port _8888_.

Now ___WITH SAFARI___, either desktop or mobile (_iOS_) version, browse to the
IP address of the computer running __nTunes__ at port _8888_. You should see
a link for "Protected Content with Safari", so go ahead and click (tap) on it.

A random "protected" track should begin to autoplay. This even works on
iPad/iPhone as long as the device has been synced with the computer __after__
the "protected" content has been downloaded.

Why does this work? Both iTunes and Safari use the same QuickTime decoder to
playback the file. That being said, ANY file that iTunes can play, theoretically
Safari can play as well.

__UPDATE:__ `jsjohnst` in the comments has posted that this doesn't work with
_video_ files on _desktop_ Safari for some reason. The proper dimensions are
detected and you get a grey screen the size of the video, no audio is played
either. What's also strange is that desktop Safari still acts like it's playing
the video; the `currentTime` JavaScript property moves along at the expected
rate, the `duration` is properly calculated, and the `play()` and `pause()`
functions work, but you never get anything but a grey screen. I wonder if this
was intentional by Apple?

## But what about everyone else?

So the above news might be good enough if you're only intending on building an
iOS WebApp to interface with your iTunes library, but if you're going for
cross-browser compatibility that what is one to do?

There is one solution... sort of.. [Requiem][] is a tool to remove the DRM
encryption from the "protected" media files purchased through iTunes.

I say sort of because __Requiem__ is not a perfect solution, in that the
author of the software is always playing catchup with Apple to get a release
that works with the current version of iTunes. For example, the Requiem 1.9.7
got released a few days ago, and it supports up to iTunes 9.2.1! However, only
a few hours later, Apple released iTunes 10, which now breaks __Requiem__ and
we have to wait once more.

__UPDATE:__ So as of 9/12/2010, Requiem is caught back up to the current
iTunes version (10). So this is good news; at least for nTunes!

What's nice about __Requiem__ is that it removes the DRM in a lossless way,
so that the produced file doesn't need to be "encoded" in any form. Even the
ID3 tags identifying the original purchaser of the content is still there. The
author of __Requiem__ has stated that it is NOT meant to be used for piracy. He
only started on it so he could play his purchased content on his PS3 and other
non-DRM hardware.

I feel like __nTunes__ is a quite legitimate use for the software, as I intend
for it to be locked down to pretty much yourself (maybe your family...), to
give access to your iTunes library on other devices like an iPhone, etc.
What do you think?

So in nTunes, I am able to detect when the request track is a "protected" one,
and then I can slip in Requiem's `decrypt` command to remove the DRM when the
browser is something like Chrome, Opera, Firefox, or even a Safari that isn't
authorized to play the protected track.


[nTunes]: http://github.com/TooTallNate/nTunes
[Requiem]: http://tag3ulp55xczs3pn.tor2web.com