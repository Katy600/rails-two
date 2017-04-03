A better user interface would be to display a status message that says something like subject added successfully, or subject updated successfully right after the redirect once the user lands on the new page. But as we saw earlier when we were **examining the request response cycle, a redirect is actually a new request from the browser**. It's not dissimilar from typing in a url directly. **Any variables which were set in the first request response cycle, before the redirect, won't be available in the second request response cycle after the redirect.**

### HTML: Stateless
This is a classic problem with HTMl because **HTML is a stateless environment**. **It doesn't keep track of information from previous pages**. **It submits a fresh request for each page**. But ***we can keep track of data between requests by using cookies and sessions.*** The basic idea is that **when your browser goes to a website for the first time, the site gives your browser a bit of data known as a browser cookie**. And then **with each future request your browser makes to that same site, it will send that cookie data along with the request**.

That allows the website to recognize your browser as the same browser who visited previously. Sessions use a similar idea.

### The flash hash
- **stores a message in the session data**. When a user makes another request, the site will recognise them based on their cookie, and the message will be available for them to view.
- It also **clears old messages after every request** to make sure that old messages don't stick around.
- **Every message you put into the flash hash will stick around for one additional request response cycle**. Usually, that means **through one redirect.**
- The way that we assign values to the flash hash is like this,
```
flash[:notice] = "The Subject was created successfully"
```
you see I have flash, then it's square brackets, and then **I use a key notation**. So I have notice. I **set it equal to whatever string I want it to be**. You can have other keys besides notice if you'd like.
```
flash[:error] = "Not enough access privileges "
```
It's completely up to you. **Notice and error are probably the most common used in rail applications**, but **any key value pair that you assign in the flash hash is going to be available in the next request**.

Most of the time the reason you use a different key name is just so that you can tell them apart, maybe you want to apply different CSS styles in different cases. Notice in that each of these examples, I'm assigning a string to the flash hash. You could assign whole Ruby objects to the flash, but it's not a good practice and I do not recommend it. Objects can be large, take up lots of memory, and if they aren't being taken directly from the database, they can be out of date or stale. If you need access to a lot of data, then you should be storing it in the database or in the session file, not in the flash hash.

The flash hash is really designed for short informational text messages that you want to save between requests.

### Adding Flash messages to our applications
```
def create
  @subject = Subject.new(subject_params)
    if @subject.save
      flash[:notice] = "Subject created successfully."
      redirect_to(subjects_path)
    else
      render('new')
    end
  end
```
So let's start by going into our **subjects controller**. Let's go up to the top and what we want to look for are **any places where we perform an action and then redirect the user**. The first of those you can see is inside the **create action**. **I'm saving the subject. I'm modifying the resource, and then I'm doing a redirect**. This is exactly the kind of place we would want to have a flash hash.
```
def update
    @subject = Subject.find(params[:id])
    if @subject.update_attributes(subject_params)
      flash[:notice] = "Subject updated successfully."
      redirect_to(subject_path(@subject))
    else
      render('edit')
    end
  end
```
So right in front of it, let's ***add flash and then notice, equals, and let's change it to subject created successfully***. Alright, now let's take that line and I'm just going to copy it. I'm going to drop down to the next place that we have a redirect right after we've updated the resource, and right above it, I'm going to add in flash notice subject updated successfully. And then let's scroll on down. And once we destroy it, we can do the same thing here.


```
def destroy
    @subject = Subject.find(params[:id])
    @subject.destroy
    flash[:notice] = "Subject '#{@subject.name}' destroyed successfully."
    redirect_to(subjects_path)
  end
```
We can do subject destroyed successfully. Now, remember, **when we destroy an object, we still have access to that object***, and that's important for messages like this. Perhaps we want to actually say what subject it is. We want to say, put it in single quotes, and let's put the subject dot name. Alright, so now it's going to say subject and then in quotes it will have the name of the subject destroyed successfully. Even though we've destroyed it and it's not in the database, we still have a frozen Ruby object that's there, that we can access for exactly this kind of case.

Okay, so now that we've set those messages, before all three of the redirects in our controller.

### Now let's read them in in our templates.
 You'll notice that **the create and destroy actions redirect to the index template**. The ***update is going to redirect to the show template***. So let's go to those templates now. Let's start with the ***index template***,

```
<% if !flash[:notice].blank? %>
  <div class='notice'>
    <%= flash[:notice]%>
  </div>
  <% end %>
```
and let's go right up here to the very top before anything else and let's paste in this bit of code. **If flash notice is not blank**, that's what the exclamation point here means, it means the opposite, so it's not blank.

So if it's not blank, **then display it**. So we're going to display a div with class notice on it. And then we're just going to **output the content of flash notice**. That's all there is. **We don't actually need to find this value of flash notice inside our subjects controller**. We're just gonna **call the flash hash directly**. It's ***always available to us in our templates. We don't need to set it equal to an instance variable*** or anything like that. Let's take this same code and let's switch over to our show template and let's do the same thing. At the very top of this page, let's put in if flash notice is not equal to blank, then display the flash notice.

Incidentally, **blank is a handy Rails method. It's a lot like checking to see if a value is equal to nil**, but it also allows for a few more options, like **checking for empty strings**. Okay, once we have that defined, let's try it out. Let's save it, let's go back to our application. And let's start out by adding a new subject. So let's create a new subject. I'm going to call it flash hash test, give it a position of four, and visible false. Create the subject, there were are. Flash hash test was created. But notice up here at the top, subject created successfully.

It tells me that. It actually set the value, put it in my session, and then when it came back for the redirect, it made a new request, the browser recognized that this was the same computer, the same browser, making an additional request, because it sent along some cookie information at the same time. It found that value in the session data, displayed it to the user, and then it cleared it. If we actually reload this page now, the message goes away. It's only there for the next redirect.

One time and then it gets cleared automatically. Alright, so now let's go and let's edit that flash hash test. Just change position to be five. Click update subject. Subject updated successfully. Again, if we reload the page, it's not there anymore. The message gets cleared. Go back to list, and now let's try deleting that last one. Here's our delete page. Delete the subject. Subject flash has test destroyed successfully. So it destroyed it, it set the message, and it was able to use the name while it was doing that.

You can see how the flash hash makes the user experience much nicer. Now that we've added the flash hash, we've completed adding the CRUD for our subjects.
