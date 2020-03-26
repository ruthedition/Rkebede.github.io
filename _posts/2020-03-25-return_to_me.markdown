---
layout: post
title:      "Return to me"
date:       2020-03-26 02:38:18 +0000
permalink:  return_to_me
---


You learn about `puts` and you learn about `prints`, and you kind of learn about `returns`... It's hard to say if you actually learn about `return` or experience `returns` when learning Procedural Ruby. 

Going through the lessons, several times you are asked to explicitly type out `puts` or `print`  and you very clearly know what is about to happen when you run that test. Well, no one tells you to explicitly type out `return`; sometimes you see it, sometimes you don't. 

#### Is that what I wanted... 

In Procedural Ruby, returns, as important as they are, can be a sutble concept that can often go over looked because of their sometimes implicent nature.

Sometimes, you get to a point where you're doubting the entire method you wrote because it keeps giving you `nil` only to realize that all this time Ruby has been looking out for you and always returning the last line that was executed unless you tell it otherwise. This may be a good thing...this may be a bad thing... 

It can be great because you always get something back, and on occasion that will save you. If the last line is what you wanted to be returned then everything is going according to plan. HOWEVER, if you needed to return something else...well then it's not so great, especially if you're the forgetful type. 

It took me a while to wrap my head around the idea, that returns are just the output of the method in the form of a data type. Once you know what you want from your method, understanding what is being returned makes so much more sense.

#### That's what I wanted! 

So what can we do to prevent these unfortunate instances when Ruby returns `nil` or  the last line instead of our intended line. You write that return, no one can stop you! 

Literally...type out `return` just like you would `puts` and `print`, then you always know when and what you're returning. This isn't a terrible habit to pick up since there are other languages that will require you to explicitly say what you are wanting to return. 

**As great of a habit as it may be, in bold print, I will warn you, when you explicity use return that is where your method will stop because it has returned what you asked for.**







