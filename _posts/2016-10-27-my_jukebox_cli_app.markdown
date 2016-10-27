---
layout: post
title:  " My Jukebox CLI app"
date:   2016-10-27 02:39:44 -0400
---

## Today I finally finished my CLI gem!
These past few months of balancing Learn, school, and a loss in the family have been pretty hard for me, so it feels incredible to finally make progress on something I love. I didn't really how much I missed coding until today when I buckled down on this gem and finished with a gem I am super happy about! So now let me tell you about it!

## The idea
It took me forever to come up with my idea for my gem. There are so many things out there that I could make a gem for, so how do I decide?! Finally I decided that something that I think would be useful is a gem that shows upcoming concerts. Usually concerts are pretty easy to miss and I often find myself hearing about a concert that I would have loved to have attended had I known about it. That's where my gem comes in! 

## How it works
1. Type "jukebox" and a list of concerts in Memphis will automatically be displayed.
2. Type the number associated with the concert you'd like more information on.
3. Type "list" to return to the list of upcoming concerts.
4. Type "exit" when you are done viewing concerts.

If you enter either a number not on the list or a word other than "list" or "exit," an error will appear and you can enter a new command.

## Behind the scenes
One of the things that stalled me in starting to develop this gem was how intimidated this project made me. I was very excited for it, but I had no idea where to start! Luckily, lots of great tutorials were there to help!

There were many issues along the way, but they all proved possible to tackle. My biggest issue was figuring out where to require everything. I ended up using my bin/jukebox file to call a new instance of the Jukebox::CLI and that was all!

So that left the Jukebox::CLI class to do all the heavy lifting! It has a method "call" that runs the whole program:

```
def call
    puts "Welcome to Jukebox!"
    list_concerts
    menu
    goodbye
  end
```

Call lists the concerts, takes the users input, and even says goodbye!


The other class that does a big job is Jukebox::Concert. This is where the information is scraped from a website and used to fill a concert object.

```
def self.scrape_concerts
    @concert_list = []

    #Opens the memphis eventful concert website and saves the HTML to doc
    doc = Nokogiri::HTML(open("http://memphis.eventful.com/events/categories/music"))

    #Saves the list of concerts
    concerts = doc.css("li.clearfix")

    #Scrapes information about a concert and creates a corresponding object
    concerts.each do |concert|
      new_concert = self.new
      new_concert.artist_name = concert.css("h4").text
      new_concert.date = concert.css(".event-meta strong").text
      new_concert.location = concert.css(".event-meta span").text
      new_concert.url = concert.css("h4 a").attr("href")
      #adds the object to the @concert_list array
      @concert_list << new_concert
    end

    #Returns the concert_list array
    @concert_list
  end
```


The hardest part of this project for me was during the making of this method. I had found where to access the information, but everytime that I tried to run the program, every single concert would be added to each instance of the object. **It was maddening!** It took **hours** to figure out why this was happening! Eventually I realized that I had made a really dumb mistake and in my loop, I kept accessing the information with "doc.css" instead of "concert.css."

Overall, this project was very fun and very rewarding. I'm really grateful that I had to do it. Even the maddening parts where I made really stupid mistakes were incredible because I finally started coding again!
