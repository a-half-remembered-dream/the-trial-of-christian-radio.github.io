# The Slander

- Christian radio is bland and uncreative
- This story will be a fair judgment

The Christian radio station Air1 has been slandered. I know ... I also cried when I heard the news. The world's most credible news agency, the Babylon Bee (https://babylonbee.com), has charged Christian radio with playing bland, repetitive music. Not once (https://babylonbee.com/news/report-air1-still-top-alternative-good-music), not twice (https://babylonbee.com/news/close-call-this-christian-radio-dj-accidentally-played-the-same-song-for-3-weeks-straight-but-luckily-no-one-noticed), but three times (https://babylonbee.com/news/christian-radio-station-threatens-to-play-good-good-father-on-repeat-until-pledge-drive-goal-is-met). This is a serious accusation, and it deserves our attention - at least enough to read this webpage.

Accusations can be thrown around all day, but we need hard evidence to back them up. What if there is some truth to this slander? What if the Air1 that we all don't know and don't care about isn't what we think it is? 

We must perform an experiment to either clear the name of Christian radio, or run it through the dirt (the more likely option). Our experiment needs to compare Air1 to other Christian radio stations and also to secular radio stations. I chose KLOVE as a sample of Christian radio stations, and KISS FM - "Chicago's #1 Hit Music Station" - as a sample of meaningless garbage ... I mean hit radio. Our dependent variable will be various statistics on the songs they play, which means that we need to find a way to monitor these three stations at the same time. To test the slanderous accusation weighed against Air1, we need to find a way to gather some evidence. We need to turn to programming.

# The Site

We need to know what songs are played on the stations, when they are played, and how often they are played. The 20th century way to do this would be to tune into the radio every few minutes to record what song is currently playing. But that's what grandpa would do, and you, youngster, are the future. That means you need to do this the 21st century way. We need to turn to the time-saving wonder of automation.

Before we starting writing code, we need to ask the question: *what is a website made of anyway?* Mostly bits and pieces. The foundation of any website is HTML, the language that lays out the structure of the content we see on our screens. HTML includes links to other types of information, like CSS code to make the HTML pretty, Javascript to make the browser do tasks and make the HTML change, and embedded content like ads and YouTube videos that load content from other servers.

A task like reloading a website and reading a few words from the screen sounds easy to us humans, but that's because our brains have incredible visual processing abilities. Computers can't see like we can, but they are awesome at searching through text, and this is exactly what we need it to do in this situation. We aren't going to be feeding the computer an image of the website we see, but instead we are going to be feeding it the *underlying structure* of the webpage. We are going to be feeding it HTML.

To us, HTML looks large and ugly. Here's a section of HTML I took from the "Recently Played" page on Air1's website:

```html
<div class="recently-played-wrapper mb-5">
                    <div class="song-wrapper" style="margin-bottom: 20px;">
                        <div class="row shadow p-4 mb-5 m-2 bg-white rounded">
                            <div class="col-sm-4">
                                <div class="d-flex h-100">
                                    <div class="album-img w-100">
                                        <img src="//c.kloveair1.com/Common/Thumbnail.aspx?f=/art/albumart/1358.jpg&amp;s=250&amp;" class="img-responsive lazyload mh-75"  alt="Empty &amp; Beautiful">
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-8 text-container">
                                <div class="d-flex h-100">
                                    <div class="my-auto w-100">
                                        <h4>Your Grace Is Enough</h4>
                                        <div class="artist">
                                            Matt Maher
                                        </div>
                                        <div class="albumTitle">
                                            Empty &amp; Beautiful
                                        </div>
                                    </div>
                                </div>
                                
                            </div>
                        </div>
                    </div>
                    <div class="song-wrapper" style="margin-bottom: 20px;">
                        <div class="row shadow p-4 mb-5 m-2 bg-white rounded">
                            <div class="col-sm-4">
                                <div class="d-flex h-100">
                                    <div class="album-img w-100">
                                        <img src="//c.kloveair1.com/Common/Thumbnail.aspx?f=/art/albumart/2761.jpg&amp;s=250&amp;" class="img-responsive lazyload mh-75"  alt="Hallelujah Here Below">
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-8 text-container">
                                <div class="d-flex h-100">
                                    <div class="my-auto w-100">
                                        <h4>Won't Stop Now</h4>
                                        <div class="artist">
                                            Elevation Worship
                                        </div>
                                        <div class="albumTitle">
                                            Hallelujah Here Below
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

    				... more songs ...
</div>
```

This looks a little confusing to us, but to a computer this is as clear as language gets. This might be long, but remember that this is only a *part* of the HTML of this page. This is only 55 lines out of 481 lines!

### Locating the data

The first step to scraping data off of a website is actually locating the data we want to extract in the HTML code. This is pretty easy; we only need to take a quick glance at this section of code and we can spot two song names: "Your Grace Is Enough" and "Won't Stop Now". We can also find the artist field and the album field, two important pieces of information that we can also extract. Now we know the items we need to pull out of this bulky mass of text.

### Find the containers

Now that we have found our data, we need to find what *contains* our data. In HTML, the most common container is a `div` element. `div` just stands for divider. We can find the `div` element that encapsulates our song data by looking for a starting tag (`<div`) that is less indented than our song data.

```HTML
<div class="song-wrapper" style="margin-bottom: 20px;">
                        <div class="row shadow p-4 mb-5 m-2 bg-white rounded">
                            <div class="col-sm-4">
                                <div class="d-flex h-100">
                                    <div class="album-img w-100">
                                        <img src="//c.kloveair1.com/Common/Thumbnail.aspx?f=/art/albumart/1358.jpg&amp;s=250&amp;" class="img-responsive lazyload mh-75"  alt="Empty &amp; Beautiful">
                                    </div>
                                </div>
                            </div>
                            <div class="col-sm-8 text-container">
                                <div class="d-flex h-100">
                                    <div class="my-auto w-100">
                                        <h4>Your Grace Is Enough</h4>
                                        <div class="artist">
                                            Matt Maher
                                        </div>
                                        <div class="albumTitle">
                                            Empty & Beautiful
                                        </div>
                                    </div>
                                </div>
                                
                            </div>
                        </div>
</div>
```

### Show HTML and Image of rendered HTML beside it

Here is the HTML that shows one song. You can see that there are a lot of layers to show just one small part of a website. There layers have to be there to style the image and the song details, but for this project we don't care about those. We just need the identifiers of the outer-most layer of the song's HTML. This is the opening tag of the divider:

```html
<div class="song-wrapper" style="margin-bottom: 20px;">
```

Let's take note of how `class` equals `song-wrapper`. This is important because we can program our data-collecting program to "zoom in" on each song using this information. This is like a bookmark for each song.

## The Script

Now, let's look through the program I made to gather these songs:

```python
def retrieveAir1Songs() -> List[PlayedSong]:
	"""Get a list of the last 5 songs played on Air1."""

    # Output
    processedSongs = []

    # Get page
    try:
        page = requests.get("https://www.air1.com/listen/recently-played", timeout=5)
    except requests.exceptions.Timeout:
        print('[-] Timeout getting Air1 songs.')
        return []

    # Parse HTML
    try:
        parser = BeautifulSoup(page.text, 'html.parser')
        recently_played = parser.find('div', attrs={'class': 'recently-played-wrapper'})
        songsRaw = recently_played.findAll('div', attrs={'class': 'song-wrapper'})
    except AttributeError:
        return []

    # Iterate through the extracted HTML sections
    titles = []
    for song in songsRaw:
        data = [field.strip() for field in song.text.replace('\r', '').split('\n') if field.strip()]
        title, artist, album = data
        if title not in titles:
            # print('PlayedSong:\n\ttitle: %s\n\tartist: %s\n\talbum: %s\n' % (title, artist, album))
            titles.append(title)
            processedSongs.append(PlayedSong(title, artist, album, 'Air1'))

    return processedSongs
```

This is the block of code that loads the Air1 website, pulls our bookmarked data from it, and puts it into nice containers for us to process later.

```python
def retrieveAir1Songs() -> List[PlayedSong]:
	"""Get a list of the last 5 songs played on Air1."""
```

The first thing this block of code does is define a function using the `def` keyword. On these lines we define a function that accepts no parameters and outputs a list of songs. `PlayedSong` is called an object in Python which stores in a structured way the information about each song that is played on the station. (I'm not going to show the definition of this object because it's 44 lines long!) In total, this definition means that this block of code, this function, will output a list of songs.

```python
    # Output
    processedSongs = []

    # Get page
    try:
        page = requests.get("https://www.air1.com/listen/recently-played", timeout=5)
    except requests.exceptions.Timeout:
        print('[-] Timeout getting Air1 songs.')
        return []
```

Now let's get to the meat of the function - what it actually does. Notice that each of these lines are indented with one tab. This is because they are contained inside the function definition. This is just like how in the function definition of `f(x) = 2x + 9`, `2x + 9` is contained within the symbol `f`. In our case, `retrieveAir1Songs` is the name of our function and all the indented lines of code underneath are all the steps needed to find its value. Essentially, programming is just math and logic, but on more than one line.

The first thing we need to do is make a place to put the songs that we have scraped from the website. Let's call this place `processedSongs`. To do this we simply make an empty list using two brackets with nothing inside of them (`[]`). This is the variable that will store our 5 songs. Next, we send a web request to Air1 with a timeout of 5 seconds. If we send a request to Air1 and do not get a reply within 5 seconds, the `except` block is triggered and the function gives up.

This is important to add because we want to be courteous to Air1's servers. If the program kept trying the load the page until it got a response, that would put a burden on the responding server. That's why we say that you "load" a website, because a load is put on the responding server. It has to reply with all the images and text that is seen on that page, so I kept my web requests to a minimum.

```python
# Parse HTML
    try:
        parser = BeautifulSoup(page.text, 'html.parser')
        recently_played = parser.find('div', attrs={'class': 'recently-played-wrapper'})
        songsRaw = recently_played.findAll('div', attrs={'class': 'song-wrapper'})
    except AttributeError:
        return []
```

This block of code is encapsulated in a `try/except` container, which basically means that if something goes wrong in this section, the program won't crash, but the function will just give up and say that no songs can be retrieved. This is important, because if the program does crash, it may be in the middle of the night where I cannot start it again. The `try/except` block allows the program to keep going even though there might be some problems, making sure that it collects each one of the songs played on the station. 

Inside the error-catching block the program assigns the variable `parser` to act as an HTML parser. This is where our HTML "bookmarks" come into play. From here, we can search for our bookmarks to narrow down the data that we want to extract. In the next line, we narrow down our search to all the recently-played songs on the page. After that, we collect all the HTML pieces that are `div` elements (dividers) that have a `song-wrapper` class. Obviously, the only pieces of HTML that have a `song-wrapper` class are songs. Now we can successfully narrowed down the HTML that holds our song data.

```python
# Iterate through the extracted HTML sections
    titles = []
    for song in songsRaw:
        data = [field.strip() for field in song.text.replace('\r', '').split('\n') if field.strip()]
        title, artist, album = data
        if title not in titles:
            print('PlayedSong:\n\ttitle: %s\n\tartist: %s\n\talbum: %s\n' % (title, artist, album))
            titles.append(title)
            processedSongs.append(PlayedSong(title, artist, album, 'Air1'))

    return processedSongs
```

This next snippet of code refines the HTML further. It puts the title, name of the artist, and name of the album into a simple container for us to use later. Then it puts that container into the list we made at the very beginning of the function, `processedSongs`. Each container that it makes it appends to that list, just like adding another task to your todo list. Finally, it "returns" the list, which means that the rest of our program can now use this information.

Now, all we need to do is turn on our program and wait.

And wait.

For 10 days.

# The Stats

- times that "Symphony" was played
- 10 top song frequency stat
  - Little overlap between stations
- frequency graph comparing three stations
  - Size of music collection
- TODO - comparison with KISS

After about 10 straight days of scraping songs from the websites, I had my data. I could tell you anything and everything about what was played on those stations during that time.

Now let's dive in to Exhibit A in the trial against Christian radio. Here is a graph of the times where the song "Symphony" by the artist "Switch" was played on Air1:

### Figure 1 - Symphony play graph

As you can see, Air1 *loves* to play this song - to my pain. This graph confirms my theory that Air1 plays this song way too much. It's not that I have chosen unlucky times to tune in, but that Air1 has a problem. (It's not me, it's you, Air1!)

(There are a few points on the graph where the song is "played" almost back-to-back. These are errors in my song-stitching algorithm. Sometimes when the script checks the Air1 recently-added page, it adds a duplicate song. However, these errors are easy to spot.)

This pattern would be understandable if this song was recently released, but Air1 started playing this song *at least two weeks* before I started collecting this data. Only recently has the station started playing it this much. When the song was first played, it was my favorite on Air1, but it was not actually played very frequently, even though it deserved to be played more. Air1 mostly ignored the song for the first two weeks. When Air1 actually heard that people enjoyed the song, the station started playing it on repeat, as the graph shows. The problem is that they delayed the "launch" of the song - the boost in play frequency - by at least two weeks. This made listeners who liked the song at the beginning become frustrated that the song was not played enough, and listeners at the end become frustrated that the song was being played *too much*. They missed the best opportunity to play the song the most, which was at the *very beginning*. They should have realized the song was going to be popular, sooner.

The song "Symphony" is a good example of a top 10 song on Air1. As they should, Air1 wants to play the most popular songs the most ... but how often are we talking about? Well, it turns out that the top 10 songs constitute **26.6% of all songs played** on the station at any given time! This means that if you listen to Air1 for 30 minutes, you will listen to an average of 7.6 songs during that time, so that means that the probability that you will not hear one song from the top 10 is **11.3%**. I hope you're not sick of any of those songs!

Air1 has obviously put a priority on playing the newest, most popular songs and has neglected to play lesser-known songs. Interesting business choice. There's nothing wrong with that, except for that Air1's most regular listeners have got to be bored out of their minds or have just given up on caring. Now, I'm not a businessman, but I do know that's is not a great idea for a business to prioritize its least loyal customers at the expense of its most loyal customers. This is exactly what Air1 is doing by playing their top songs this often.

Now, when I saw these results I admit that I thought the verdict was made. After all, this is egregious repetition! But sometimes it is best to step back and look at the big picture, and this is what you see:

### Figure 2 - Frequency comparison graph

These are what I call the "frequency curves" of each station. These graphs show us the big picture of how often a song of a certain popularity is played on the station.

The X axis represents each distinct song played on the station sorted from most popular to least popular. So when x is 1, y is the percentage of times the most popular song was played on the station (out of all the songs played). When x is 2, y is the percentage of times the *second most popular* song was played on the station. As it should be, there is a negative correlation between the popularity ranking of a song (the x axis) and the times it is played (the y axis). 

### Images of PL phenomena

But the slope of these curves are just ridiculous! This is a type of **power-law distribution**, a pattern that arises when a small minority of subjects hold the majority of a certain quantity. This pattern is seen everywhere in our world and it pops us in the most random places. Here are a few examples of phenomena that follow the power-law pattern: the distribution of wealth, the distribution of the mass of stars in the universe, the size of cities, the frequency of words in text, the magnitude of wars and terrorist attacks, and even the diameters of dust devils! Most of the experiences in life we consider "random" are really just the result of complex interactions that actually form regular patterns. Our brains are usually just too distracted by the present moment to notice the bigger picture.

Back to the song frequency distribution. These stations aren't wrong to use a power-law distribution; the problem is the **bend** in the curve. This is the curve that I would like to see radio stations use.

### Repeat image of frequency graph with better curve

I believe that almost all radio stations should adopt this curve. From a business standpoint, this is optimal, because it gives listeners what they want. New listeners still get a good taste of the most popular music, because the curve still peaks at the y-axis, while regular listeners get a good taste of variety because the curve's rise is not too steep. If only radio stations could just use math to make their decisions! The world would be a better place!

You can also infer the size of the song collection of each station from how long the "tails" of each line extend. I collected data from KISS's website for about a third of the time as Air1 and KLOVE, so if I wanted to quantitatively compare the size of each station's music collection, I should collect data on KISS for a little longer. All I really care about, however, is a qualititive comparison of which station plays *more* songs, and it looks settled that the song collection of KISS is at least the same as or smaller than Air1's collection. Christian radio deserves some points for this, because there are many more secular pop songs than mainstream Christian contemporary songs, so we would expect KISS to have a bigger collection.

These results are surprising! Christian radio might deserve some due criticism about its boring repetition, but if we are being fair, secular stations like KISS deserve that same complaint too. I think this trial is settled.

# The Sentence

- judgment - condemn them to the 7th level of hell

In a normal trial we would gather together a jury of these radio station's peers, but that's hard, so for simplicity's sake I'm just going to adopt the role of judge, jury, and executioner. I formally sentence all radio stations that play popular songs too often to the seventh layer of Hell. Yes, that includes Christian radio and many pop stations.

The court is adjourned.

And as always, thanks for reading. 



If you want to see more of my code you can visit my Github repository for this project.