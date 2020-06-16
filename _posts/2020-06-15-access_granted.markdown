---
layout: post
title:      "Access Granted"
date:       2020-06-15 18:06:51 -0400
permalink:  access_granted
---


The Sinatra Mod seemed to go by in a blink of an eye. I wasn't sure if any of the information I was learning was sticking or if I was just faking it until I made it. I did all the things, and wrote all the code and it got me through the lessons, but having to create a website from scratch seemed to bring it all together. Somehow I began putting a project together that in fact incorporated Active Record, and Object Oriented Ruby, and Rake and Sinatra. It was like all the things that didn't make sense independently were now working together harmoniously to put together a website. 

For my project I built a website using a Sinatra framework to allow users the ability to create an account they can manage to keep a record of wines they want to keep track of. The website allows users to create a new bottle of wine, update, and delete it. Users are also able to edit their account information in the account settings. Information such as the name of the wine, country, type, price, and the year sealed are information that the user is able to record for their collection.

Conceptualizing how to authenticate and authorize a user, was probably one of my biggest struggles throughout the project. A lot of it isn't just very obviously in the code, it spans over different files and folders, and requires a network of things to work right. 

The very first thing that needs to go right is creating a session hash, which is what is used to identify that the current user is in fact the logged in user. When a user signs up they are given a user id which is set to the session hash that will be continute to be used until they logout. Using this session hash not only are we are able to access the current user in different controllers, but we are also able to authenticate that the person logged in does have permission to go to the requested page. 

From here we need to authorize any changes that are being made by the current user. To do so we need to check that whatever is being changed, in my case information on a saved bottle of wine, is linked to the user id in Active Record. When the user id matches the user id associated with the wine bottle the changes are able to be saved and the record for that wine is offically edited. However, if the wine bottle being changed did not belong to the save user id it was linked to in the database, the change does not get authorized and the user is not able to change the information about the wine or even delete it, because it does not belong to them. 


```
 delete '/wines/:id' do 
    @wine = Wine.find_by(id: params[:id])
    authorize(@wine.user)
    @wine.destroy
    redirect "users/#{current_user.slug}"
 end 
	
```

And that is pretty much the difference between authenticating and authorizing, but behind the scenes there are helper methods in the application controller that are making this all possible. There is a helper method whose sole purpose is to find the current user based on the id in the session hash.

```
 def current_user
      User.find_by(id: session[:user_id])
 end 

```

Another is to check if there is a current user logged in. 

```
 def logged_in?
      !!current_user
 end 

```

Then regularly used helper methods that also incorporate the first two are the authenticate and authorize. The authenticate checks if the user is logged in, and if they are not it redirects them to the login page. 

```
 def authenticate
      redirect '/login' if !logged_in?
 end 

```
This ensures security and makes sure that people can't just jump to different pages on the website that they do not have permisson to access. The authorize takes in an argument of a user and will authenticate first then check if the user is the current user before allowing any changes to be made.  

```
 def authorize(user)
      authenticate
      redirect "/users/#{user.slug}" if user != current_user
 end

```
