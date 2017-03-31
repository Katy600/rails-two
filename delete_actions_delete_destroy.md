- [Instructor] We've now seen how to create, read, and update records from the controller. The last part of CRUD is delete and that's what we'll learn to do in this movie.

|**CRUD**| **Action**| **Description** | **REST Route**|
|:-------|:---------:|:---------------:| -----------------:|
| Create | new       |Display new record form|GET/subjects/new
|        |create     |Process new record form| POST/subjects/create
| Read   | index     |List records           |GET/subjects
|        | show      |Display a single record|GET/subjects/show/:id
| Update |edit       |Display edit record form|GET/subjects/edit/:id
|        |update     |Process edit record form|PATCH/subjects/update/:id
| Delete |delete     |Display delete record form|GET/subjects/delete/:id
|        |destroy    |Process delete record form|DELETE/subjects/destroy/:id


You'll remember, the delete can be defined by two standard rails actions. Delete and destroy. Like the other form actions that we've seen, delete displays a form and destroy processes that form. There are a few other key points to be aware of.

1. delete also wants an id, just like we did with update.
```
params[:id]
```

And it makes sense that **we can't destroy a record of the database if we don't have an id** so we can find it.
We need to have an id to reference the record that we want.
2. When it processes the form, **destroy finds the subject and then calls the destroy method on it.** Remember that there's also a method called delete. But we want to make sure that we're using destroy. You may be wondering, what does a delete form look like? I mean, we're not editing any values. **The delete page mostly serves as a confirmation page to make sure that we don't accidentally delete something that we didn't mean to.** It's up to you what you put on that form.

There could be warnings about deleting a record, or you could provide a summary of the record details so the user is very clear about what's about to be deleted. It give the user a chance to back out before they do something destructive. It's actually optional whether you have a delete page at all. Delete pages often just left out altogether. Don't be surprised if you see other Rails applications not using a delete page. Instead, clicking a link could just go straight to the destroy page. Or as sometimes java script will be used to display a simple confirmation dialogue box.

Something like, are you sure you want to delete this item? However, the way that we're going to do it is using two actions and with a delete page. Let's go to the application and add it. Let's start with adding a link which will take us to the delete page. So that would be the link that is right here on our index page. Right now that doesn't go anywhere. Let's enable that.
```
<div class="subjects index">
  <h2>Subject</h2>
  <%= link_to('Add new subject', new_subject_path, :class => 'action new')%>

  <table class='listing' summary='Subect list'>
    <tr class='header'>
      <th>#</th>
      <th>Subject</th>
      <th>Visable</th>
      <th>Pages</th>
      <th>Actions</th>
    </tr>
    <% @subjects.each do |subject| %>
      <tr>
        <td><%= subject.position %></td>
        <td><%= subject.name %></td>
        <td class="centre"><%= subject.visible ? 'Yes' : 'No' %></td>
        <td class="centre"><%= subject.pages.size %></td>
        <td class="actions">
          <%= link_to("Show", subject_path(subject), :class => "action show") %>
          <%= link_to("Edit", edit_subject_path(subject), :class => "action edit") %>
          <%= link_to("Delete", '#', :class => "action delete") %>
        </td>
      </tr>
     <% end %>
  </table>
</div>

```


Let's switch over to our text editor and let's go inside index.html.erb where we have delete. What we're gonna do is we wanna replace this placeholder with the route that we need to delete.

```
<%= link_to("Delete", '#', :class => "action delete") %>
```

```
<%= link_to("Delete", 'subjects/1/delete', :class => "action delete") %>
```

Now that's going to be subjects and then whatever id we want, delete.  **That's the string, based on our resourceful routing we came up with**. Now we could specify it as a string or we could use the hash notation and have action delete id and then have the subject id.
But **we're gonna use that restful route helper**,
```
<%= link_to("Delete", delete_subject_path(subject), :class => "action delete") %>
```
which is going to say delete underscore subject path and then subject. That will create the length that we need. Notice that it's very similar to what we had for both show and edit.

The only difference is that we have delete at the front of that. Let's save it. Let's go back to our page. Let's reload it. And you'll see now the bottom Firefox tells us that it's going to generate the correct URL for us for each one of these. So subject/5/delete.
#### Now let's go to the subject controller and write out delete action.
```
def delete
  @subject = Subject.find(params[:id])
end
```
If we're going to display information about the object being deleted, then we're going to need to find the object. So the process is going to look exactly like what we had for edit. We're going to present the object to the user. We're going to do that by signing the subject to the instance variable @subject.

### let's switch over to the delete template.
```
<%= link_to("<< Back to Link", subjects_path, :class => 'back-link') %>

<div class="subjects delete">
  <h2>Delete Subject</h2>

  <%= form_for(@subject, :url => subjects_path(@subject), :method => 'delete') do |f| %>

  <p>Are you sure you want to permanently delete this subject?</p>

  <p class='reference-name'><%= f.object.name %></p>

  <div class="form-buttons">
    <%= f.submit("Delete Subject") %>
  </div>

  <% end %>
</div

```

I've got my back to list link here at the top, just like we've had before. It's got a title that says delete subject up at the top and then it's got a form_for. **Form_for the subject**, just like we had with edit but you can see I've also **added extra options indicating the URL that we want it to use and the method**. And remember **when we're using rest, the method that we want is delete**.

Now **the URL is the exact same URL we would get automatically by having an existing object in the database**. It's the same URL, whether we're using patch or whether we're using delete. **The difference is just the method**. So therefore, I can actually leave this portion out.
```
  <%= form_for(@subject, :method => 'delete') do |f| %>
```
 **I don't have to specify the URL. But I do have to specify the method as being delete**

. If I leave that out, then Rails is going to think that this is a form for updating. It's going to see this as a saved object. It's going to say, "Oh, you must want to update that object".

So I must specify that the method is delete. All the other defaults I can use, but that one I must give. And then inside my code block, you can see I have text that says, "Are you sure you want to permanently delete this subject?" And then notice here that I am asking for the subject's name. And instead of doing that by asking for @subect name, which would absolutely work. Right, the subject is instantiated, we have access to that instance variable. But instead, I'm doing it another way. I'm telling the form builder to get the the object and the object is the subject.

It's just another way of specifying the same thing. It's saying, "Hey, form builder, Whatever object you were given, bring that up now. I need to use that". So just wanted to show you that, that if you need to actually find the attributes of something that your form_for is creating, you can just use f.object and then work with it in it's normal way. And then we've got a button here that says delete subject at the bottom. So let's save it and let's take a look at it. Let's go back to this page and we already have it loaded up. Let's click delete, and there's our form. Delete subject, are you sure you want to delete this subject? And here it is, test subject.

And it's ready for us delete.
### Let's also go into those web developer tools to page source.
```
<form class="edit_subject" id="edit_subject_2" action="/subjects/2" accept-charset="UTF-8" method="post"><input name="utf8" type="hidden" value="&#x2713;" /><input type="hidden" name="_method" value="delete" /><input type="hidden" name="authenticity_token" value="mMAPPUpkCGf9YfFNTmXih98TSf3j3v1GQcuwIuyPbGnzy7XTj9kriNmm9B3/acqk6zAgTG3vSAR10UWfIHeQMA==" />

```
And now we can take a look here. Form, and you can see **the action here is subject/5.** That's **exactly as what we had with edit and it's also going to be method post**. But, if we look over here at the **hidden value**, you'll see that **now it has a hidden method of delete.** And **that's going to tell Rails to use the restful design pattern and to treat this as if it is deleting from a resource.** Okay, so let's close that up.

Now after we actually submit that, we want it to be handled in a controller by our destroy action.
```
def destroy

end
```
So let's come down here in to destroy, and how do we go about destroying it? Well, first we just want to find it.
```
def destroy
  @subject = Subject.find(params[:id])
  @subject.destroy
  redirect_to(subjects_path)
end
```

We can just copy that. And then **after we find it, we'll just destroy it. Subject.destroy** And then once we're done destroying it, we need to do something. We need to **either render a template or we need to redirect**. We don't have a destroy template. Probably don't need one.

Probably just need to **go back to another page**, like our **index page. Redirect_to, and then what's our path for that? Subjects_path And that will take us back to our index page**. Let's save it and let's try out the destroy action. Over here and we'll just click delete subject, and now it's gone. It was successfully deleted. Hopefully now you've begun to put together the pieces of how CRUD works, how rest and resource for routing works, and how you can work with these objects inside your controllers. There is something we can add that can make this process a lot nicer.

Notice that when I deleted a subject, it took be to the index page and the subject was gone. But the application didn't actually tell me what it did. A more user friendly design would give a result message. Something that says subject deleted successfully. In the next movie, we'll learn how to do that using the flash hash.
