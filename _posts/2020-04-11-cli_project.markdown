---
layout: post
title:      "CLI Project"
date:       2020-04-11 01:26:44 -0400
permalink:  cli_project
---

As I write this blog post, I have confidently pushed up the last bit of edits for my very first CLI Project. It's crazy to think that just 6 weeks ago CLI was just a combination of letters and meant nothing to me. Now I'm building tic tac toe games, calling APIs, and giving users options to use something I built!

This project gave me a way to tie together everything I have learned, and more importantly realize that I know WAAAAYYY more than the Ruth from six weeks ago. 

### Using an API

For my CLI I used the [Open Weather API](https://openweathermap.org/api) to allow users to enter their zip code and get back the weather forecast for the day. Getting the API key and navigating the Open Weather website was not as terrifying as I expected, especially with how detailed docs on each of their API options are. I did have to learn how to hide my API key, but again, not as bad as it seems.

### Preparation  
I didn’t realize how much preparation is needed for all of the components of coding that happen behind the scene. You need to install gems, edit your file tree, and make sure everything is installed and in its place before you even write your code. Thankfully the videos we were given provided a lot of guidance, and bundle install is a life saver.

Once my environment was ready and I had my API, I thought I would be able to dive right in and just write code and bam lab complete; well that is definitely not how it went. 

### Revelations 
For starters, I will never take tests for granted again! When I realized that the only way to know if my program was working would be to keep running it over and over again or use IRB, I decided to learn how to write tests. Once my awkwardly written tests provided me some guidance on what my application needed to do, I was able to start making some progress. 

Something that I had not taken into much consideration when doing labs in previous lessons, was the placement of my `require` files. I quickly learned to prefer having my `require` and `require_relative` in the file that was doing the requiring, so I know what I am referencing. However, it was pointed out to me, that while this is fine for a small CLI project, when a project gets big and there are several folders and several files that may need to be required it would be best to have them all in an environment folder where they can all be required at once. 

#### Working with JSON

I created my base URL which is the first part of my path.

`BASE_URL = "https://api.openweathermap.org/data/2.5/weather"`

Then I went on to call my API at which point I immediately realized I won't be able to get everything the way I wanted it.  The JSON response looked like the one below.

` {"coord": { "lon": 139,"lat": 35},
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "clear sky",
      "icon": "01n"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 281.52,
    "feels_like": 278.99,
    "temp_min": 280.15,
    "temp_max": 283.71,
    "pressure": 1016,
    "humidity": 93
  }`

It used coordinates and gave me a forecast in celcius, so I needed to add in parameters for the API to know what I needed from the call, such as: searching by the zip code, the unit of measurement for the weather, and of course my API key. 

`params = {zip: zipcode, appid: ENV["API_KEY"], units: "imperial"}`

To change that I need to describe what I am requesting in addition to my base url which will be a `query:`.  At this point my response variable should equal my url with the conditions of my parameters that have been set. 

`response = HTTParty.get(BASE_URL, query: params)`

I think the most difficult thing I came across was wrapping my head around the idea that even though the JSON response looks like it comes in as a hash (snippet above), it actually comes in as a string which needs to be parsed and returned as a hash. So, I used `JSON.parse` to get the job done. I was able to call `.body` to retrieve the contents from the response and turn the keys into symbols with `symbolize_names:` true which ultimately gives me a hash I can use.

`JSON.parse(response.body, symbolize_names: true)`

Once I came to that understanding by using my ` binding.pry ` and checking what each return was, I was able to put together my CLI and add in features such as a menu selection for what kind of forecast the user wants to see, and the option to exit after seeing the forecast.

Which brings us to the code for calling my API and receiving the information in hashes.

```
def self.call_api(zipcode)
    params = {zip: zipcode, appid: ENV["API_KEY"], units: "imperial"}
    response = HTTParty.get(BASE_URL, query: params)
    JSON.parse(response.body, symbolize_names: true)
 end 
```

### Refactoring
Once I was done with the functionality of my app, then came refactoring and cleaning things up. Thinking it was a requirement, I put together a flow diagram. It was one of the last things I did, and I'm glad I did. While working through how to draw out the flow of my app, I quickly became frustrated by all of my overlapping arrows. There was no clean flow to how my app functioned; it was practically a rollercoaster. So I began to move  things around in my flow diagram and as I continued to shift excess arrows began disappearing and my flow started to simplify. As I adjusted my flow I adjusted my code to follow and I began seeing my code shrink. All the repetitive calls on methods were streamlined into other methods. By the time I finished my flow diagram I realized I had basically been refactoring just not in the traditional sense of going in and cleaning up code.

![Refactored Weather CLI Flow diagram](https://i.ibb.co/HF157d4/Untitled-Diagram.png)

### Submitting 
It felt very strange getting ready to submit my project. I did not know if my CLI was truly done. There is always the question of could I have done more, or is there a better way I could have written what I have done. Honestly, there probably is, but isn’t that the beauty of being able to create something from scratch?



