offline-pages
=============

Save a bunch of web pages as a self-contained, compressed archive file for offline storage and sharing.

Installation
------------

    git clone git://github.com/iandennismiller/offline-pages.git
    cd offline-pages
    sudo make install

What is it?
-----------

Offline-pages lets you save an entire website to a file, along with all the required media you'll need to view the pages offline.  It's like your browser's "Save Page As" feature, except it isn't limited to one page so it can handle entire websites.  The archive can be browsed offline, and all the inter-links will point within the archive (i.e. it is self-contained).

Usage
-----

Let's say you want to mirror the wikipedia article for "Webarchive".  

### 1. Create a file containing target URLs

This file can contain as many URLs as you want; they will all be added to the same archive.

```
echo http://en.wikimedia.org/wikipedia/en/wiki/Webarchive > urls.txt
```

### 2. Use this file as input to the `offline-create` program:

```
offline-create ./urls.txt wikipage
```

### 3. View the results

```
offline-browse wikipage.archive.tgz
```

Use Cases
---------

### Archiving a forum thread

So there is a forum thread consisting of hundreds of posts, and it spans dozens of pages.  Offline-pages can create a fully self-contained mirror of all these pages, such that the offline version can be navigated much like the online version. In this case, just create a URLs file containing each of the pages you want to include in the archive.

If you were archiving a vBulletin forum, you might have a base URL like this:

http://www.example.com/vb/threads/1234-this-thread

Subsequent pages of the forum thread simply append "pageX" to the URL, like this:

http://www.example.com/vb/threads/1234-this-thread/page2

### 1. identify target URLs

Begin with the stable portion of the URL and write all URLs to a file at once:

```
export BASE_URL=http://www.example.com/vb/threads/1234-this-thread/page
for i in $(seq 1 40); do echo ${BASE_URL}${i}; done > urls.txt
```

### 2. fix the first URL

If you look at the file, everything looks great except the first URL.  You will notice that the first line now says:

http://www.example.com/vb/threads/1234-this-thread/page1

We don't want "page1" there (because that's not how vBulletin works), so edit `urls.txt` and change the first line to read:

```
http://www.example.com/vb/threads/1234-this-thread?s=1
```

### 3. archive the thread

Now you are ready to archive the thread.  

```
offline-create ./urls.txt vbulletin-thread
```

See also
--------

### Webarchive

[http://en.wikipedia.org/wiki/Web_ARChive](http://en.wikipedia.org/wiki/Web_ARChive)

The Internet Archive created an awesome file format called WARC for storing their web crawls.  This seems to offer a great toolkit for very serious work, but it's not easy to use at all.  I never figured out how to simply "browse" a web archive using my browser, so even though it might be able to get the job done, it is overly complex for simple tasks.

### HTTrack

[https://secure.wikimedia.org/wikipedia/en/wiki/Httrack](https://secure.wikimedia.org/wikipedia/en/wiki/Httrack)

HTTrack looks like a full-featured and relatively simple site mirroring tool.  Unfortunately, it does not compile under OSX Mountain Lion, and I was unable to evaluate how easy it was to use its offline archive file format.

### MHTML

[http://en.wikipedia.org/wiki/MHTML](http://en.wikipedia.org/wiki/MHTML)

It's possible to save an entire web page's assets (images, css, whatever) into a single .html file the same way email messages can include images and attachments.  If you click around, you'll get kicked out of the archive and back onto the live Internet.  This becomes a drawback since this works for a single file rather than for multiple files or an entire website.

### MAF

[http://en.wikipedia.org/wiki/Mozilla_Archive_Format](http://en.wikipedia.org/wiki/Mozilla_Archive_Format)

Mozilla Archive Format might have provisions for saving multiple files into an archive.  It seems like this is integrated with Firefox, and I haven't played with it much to test its capabilities.

### Webarchive

[http://en.wikipedia.org/wiki/Webarchive](http://en.wikipedia.org/wiki/Webarchive)

Safari can save pages for offline use, but the end product behaves a lot like a .PDF because it is sortof like a static page of text.  Webarchive has the same single-page drawbacks as MHTML.

