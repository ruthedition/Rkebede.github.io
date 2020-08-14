---
layout: post
title:      "Just A Click - No JavaScript"
date:       2020-08-14 04:10:07 +0000
permalink:  just_a_click_-_no_javascript
---


When coming up with the concept for my Rails project my whole project relied on the ability of a user to click a button and get a response instantly. Obviously there were other components of my project that were essential, but this was the one thing I was excited about. 

Very quickly into my project I learned that Rails does not reload a page with something new on it, that's what JavaScript is for. So I needed to find a work around that would allow a user to click a button and immediately see a response on the same page without being redirected or noticing a new page being loaded. 

### A few things I had to take into consideration
1. be able to shuffle the restaurants before the redirect occurs so the user gets a random restaurant
2. only display one restaurant from the shuffled list
3. the list of restaurants being shuffled should only be taken from the current user's list
4. decide how the redirect will happen  
5. decide where the redirect will go to 
6. what the user should see after being redirected 
7. lastly, make it seemless!

I started with the easiest thing I could think of which was shuffling the restaurants, which I was able to do with a Ruby method `#shuffle` . I was able to take care of the next issue of only selecting one restaurant by chaining `#pop` to the shuffle method which returns that last element of an array. 

When it came time to figure out how the redirect will happen and where it will go, there were many options that came up after a quick Google search. Many of them mentioned creating a custom route and a new action for the button to go to then redirect the user to the page. I chose to keep this an option but I really wanted a seemless feel of clicking the button and seeing the result without "waiting for a page to refresh". After much digging I found that I was able to pass in an argument to the path for my link and specify the method. 

```
user_path(current_user, restaurant_id: @restaurants.shuffle.pop), method: :get
```

Wanting the user to feel like nothing changed I decided the best place for them to see the result was right below the button which was on the show page for the current user. `user_path(current_user)`

```user_path(current_user, restaurant_id: @restaurants.shuffle.pop```

I specified the route of the show page by passing in the current user, and I was able to pass in another argument to add a param of `restaurant_id` that will be recieved in the show action. The param being sent is the last restaurant from a shuffled list of restaurants that belong to the curent user. 

And lastly, I specified the method I wanted the link to be using, `get`. 

So my final line of code was the following:

```
<%= link_to "#{current_user.username}, I can help you pick where to eat!", user_path(current_user, restaurant_id: @restaurants.shuffle.pop), method: :get %>
```

Because I used a `link_to` I had it to wrap it in a `button` tag so it looked like a button!

In the controller, there was not much work that needed to be done. I set `@restaurants` to `@user.restaurants` so only the current user's list of restaurants were being randomized. Because I am sending my request to the show action there is no need for extra routes or an additional action. My if statement sets `@restaurant` to whatever restaurant is found in the Restaurant model matching the restaurant_id param that came in.

```
 Restaurant.find(params[:restaurant_id]) if params[:restaurant_id]
```

The final code in my `views/users/show` and `controllers/users_controller` is as follows: 

users/show.html.erb:
```
  <button class="button"> <%= link_to "#{current_user.username}, I can help you pick where to eat!", user_path(current_user, restaurant_id: @restaurants.shuffle.pop), method: :get %> </button><br><br>
```

user_controller.rb:
```
def show 
    @restaurants = @user.restaurants
    @restaurant = Restaurant.find(params[:restaurant_id]) if params[:restaurant_id]
  end
```

