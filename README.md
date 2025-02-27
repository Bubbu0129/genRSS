# genRSS

### What is genRSS?
genRSS takes a directory hosted on your website and generates an RSS 2.0 feed for all media files within the directory. It can operate recursively and look for media files in subdirectories. Media files can also be restricted to a given set of extensions.

#### Media file duration
In order to have file duration in your feed (via `<itunes:duration>` tag), make sure that one of the following is installed on your machine.
- `mutagen`: preferred option because it's the fastest, can deal with audio and video files and it's a python module. For installation run:

```
pip install mutagen
```

- `sox`: can only handle audio files, runs faster than `ffprobe`.
- `ffprobe`: normally installed with `ffmpeg`, can deal with audio and video files but it the slowest of the three options.

In any case, if the program fails to get media duration with one tool, it'll fall back to the next one. If none if these is installed or if file duration could not be obtained, no `<itunes:duration>` tag will be inserted.


### How to use genRSS?
Suppose you have a web server and a website hosted on that server. genRSS can be run on a given directory on the website to generate a feed from media files in the directory so you can access them with a podcast client.

The following command launches an HTTP server that serves the current directory:

    python3 -m http.server

The server will be listening on port 8000 (default). You can also specify the port as an argument:

    python3 -m http.server 8080

Go to a web browser and type: http://localhost:8080/. You should get a web page listing of all elements in current directory.

Place the test media directory (contains fake media files) in the directory served by Python HTTP Server and refresh the web page. You should now see and be able to browse the media folder.

Now place genRSS.py into the same directory and try the following examples.

### Examples:

**_Generate a podcast from `mp3` files in "test/media"_**

The following command generates a feed for `mp3` files within `test/media` directory:

    python3 genRSS.py -d test/media -e mp3 -t "My Podcast" -p "My Podcast Description" -o feed.rss
 
feed.rss should now be visible on the web page. You can visit it or open it with a podcast client.

If no output file was given (option `-o`), the result would have been printed out on the standard output. It should look like:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0">
   <channel>
      <title>My Podcast</title>
      <description>My Podcast Description</description>
      <link>http://localhost:8080/</link>
      <item>
         <guid>http://localhost:8080/test/media/1.mp3</guid>
         <link>http://localhost:8080/test/media/1.mp3</link>
         <title>1.mp3</title>
         <description>1.mp3</description>
         <pubDate>Sat, 08 Apr 2017 21:19:52 +0000</pubDate>
         <enclosure url="http://localhost:8080/test/media/1.mp3" type="audio/mpeg" length="0"/>
      </item>
      <item>
         <guid>http://localhost:8080/test/media/2.MP3</guid>
         <link>http://localhost:8080/test/media/2.MP3</link>
         <title>2.MP3</title>
         <description>2.MP3</description>
         <pubDate>Fri, 07 Apr 2017 21:19:52 +0000</pubDate>
         <enclosure url="http://localhost:8080/test/media/2.MP3" type="audio/mpeg" length="0"/>
      </item>
   </channel>
</rss>
```

**_Generate a podcast from media files in "media" and its subdirectories_**

    python3 genRSS.py -r -d test/media -t "Podcast Title" -p "Podcast Description" -o feed.rss

**_Generate a podcast from `mp3` and `ogg` files in "media" and its subdirectories_**

    python3 genRSS.py -r -e mp3,ogg -d test/media -t "Podcast Title" -p "Podcast Description" -o feed.rss


### Access your podcast from another machine/device:

`localhost:8080` are your host name and your http server port respectively. This pair is automatically used by `genRSS` as a prefix for items in the generated podcast. Alternatively, you can use your machine's IP address instead of localhost. This is particularly useful if you want to access your podcast from another machine or a mobile device that share the same network.

**Example:**

    python3 genRSS.py -e "mp3,ogg" -d test/media -H 192.168.1.5:1234 -t "Podcast Title" -p "Podcast Description" -r -o feed.rss

### Tests

To run tests type:

    python -m doctest util.py

or in verbose mode:

    python -m doctest util.py -v

Wiki: https://github.com/amsehili/genRSS/wiki

### License
MIT: https://github.com/amsehili/genRSS/blob/master/LICENSE
