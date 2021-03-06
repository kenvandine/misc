# How to Create Your Own Wallpaper Slideshow in GNOME

A very cool, lesser known, feature of GNOME is the ability to display a slideshow as your wallpaper.  You can select a wallpaper slideshow from the background settings panel in the GNOME Control Center, aka Settings.  Wallpaper slideshows can be distinguished from static wallpapers by a small clock emblem displayed at the lower right corner of the preview.

There are some pre-installed slideshow wallpapers in some distributions. For example, Ubuntu includes the stock GNOME timed wallpaper slideshow as well as a slideshow of the Ubuntu wallpaper contest winners.

What if you want to create your own custom slideshow to use as a wallpaper?  GNOME doesn't provide a user interface for creating such a slideshow, however if you aren't afraid to craft some simple XML files in your home directory, you'll find it quite simple to create your own.  Fortunately, the background selection in GNOME Control Center honors some common directory paths, which makes it pretty simple to create your own without the need to edit anything provided by your distribution.  Using your favorite text editor, create an XML file in `$HOME/.local/share/gnome-background-properties/`.  The file name isn't important, but the directory name matters (you'll probably have to create the directory).  For my example, I created `/home/ken/.local/share/gnome-background-properties/osdc-wallpapers.xml` with the following content:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE wallpapers SYSTEM "gnome-wp-list.dtd">
<wallpapers>
 <wallpaper deleted="false">
   <name>Opensource.com Wallpapers</name>
   <filename>/home/ken/Pictures/Wallpapers/osdc/osdc.xml</filename>
   <options>zoom</options>
 </wallpaper>
</wallpapers>
```

The above XML file needs a `<wallpaper>` stanza for each slideshow or static wallpaper you want to include in the backgrounds panel of the GNOME Control Center.

[![IMAGE ALT TEXT HERE](https://github.com/kenvandine/misc/raw/master/articles/osdc/gnome/slide-show-backgrounds/osdc/screenshot-osdc_wallpaper_500.png)]

In this example, `osdc.xml` file looks like this:

```
<?xml version="1.0" ?>
<background>
  <static>
    <!-- Duration in seconds to display the background -->
    <duration>30.0</duration>
    <file>/home/ken/Pictures/Wallpapers/osdc/osdc_2.png</file>
  </static>
  <transition>
    <!-- Duration of the transition in seconds, default is 2 seconds -->
    <duration>0.5</duration>
    <from>/home/ken/Pictures/Wallpapers/osdc/osdc_2.png</from>
    <to>/home/ken/Pictures/Wallpapers/osdc/osdc_1.png</to>
  </transition>
  <static>
    <duration>30.0</duration>
    <file>/home/ken/Pictures/Wallpapers/osdc/osdc_1.png</file>
  </static>
  <transition>
    <duration>0.5</duration>
    <from>/home/ken/Pictures/Wallpapers/osdc/osdc_1.png</from>
    <to>/home/ken/Pictures/Wallpapers/osdc/osdc_2.png</to>
  </transition>
</background>
```

There's a few important pieces in the above XML.  The background node in the XML is your outer node.  Each background supports multiple `<static>` and `<transition>` nodes.

The `<static>` node defines an image to be displayed and the duration to display it, with `<duration>` and `<file>` nodes, respectfully.

The `<transition>` node defines the `<duration>`, the `<from>` image and the `<to>` image for each transition.

### How about time based?

Another cool feature is time-based slideshows.  You can define the start time for the slideshow and GNOME will calculate the times based on the start time.  This is useful for changing the wallpaper based on the time of day.  For example, you could set the start time to 06:00 and display a wallpaper for the morning until maybe 12:00, change the wallpaper for the afternoon, then again at 18:00. In that way you are displaying different wallpapers for the current time of day.

This is accomplished by defining the `<startime>` in your XML like this:

```
  <starttime>
    <!-- A start time in the past is fine -->
    <year>2017</year>
    <month>11</month>
    <day>21</day>
    <hour>6</hour>
    <minute>00</minute>
    <second>00</second>
  </starttime>
```

The above XML will start the animation at 06:00 on November 21, 2017.  You could then set your morning wallpaper duration to 21600.0 (6 hours).  It will then be displayed until 12:00, then your next wallpaper will be displayed.  You can continue to do this to change the wallpaper at whatever intervals you'd like throughout the day.  You should ensure the total of all your durations equal 86400 seconds.

Of interest, GNOME will calculate the delta between the start time and the current time and display the correct wallpaper for the current time.  For example, if you select your new wallpaper at 16:00, GNOME will display the proper wallpaper for 36000 seconds past the start time of 06:00.

To see a complete example, see the adwaita-timed slideshow provided by the gnome-backgrounds package in most distributions.  Usually found in `/usr/share/backgrounds/gnome/adwaita-timed.xml`.

### Conclusion

Hopefully you have found this article interesting and have taken a dive into creating your own slideshow wallpapers.  If you would like to download complete versions of the files referenced in this article, they can be found on [github](https://github.com/kenvandine/misc/tree/master/articles/osdc/gnome/slide-show-backgrounds/osdc).

If you're interested in utility scripts for generating the XML files, do an internet search for gnome-background-generator.
