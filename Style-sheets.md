### How to use Asset Pipeline to add stylesheets to our Rails project.

1. We need to write the stylesheets.
2. We need to add the stylesheet to the manifest file,
3. Add the manifest file to Rail's asset precompile list. That will ensure that Asset Pipeline is able to put together a stylesheet that's ready for serving to the browser.
4. Then go into our HTML and add a stylesheet link tag so that our page will know that that's the stylesheet that should be served up with the page.

### To write our stylesheets
 Stylesheets should be kept in
```
with asset pipe line: /app/assets/stylesheets
```
If for some reason you weren't using Asset Pipeline and you wanted to serve up static stylesheets instead,

```
without asset pipe line: /public/stylesheets
```
### File name.
Stylesheets should end their file names with **.css**, but **if we're using Sass, then you would end them in .css.scss.** It works the same way as the file endings we saw with templates, where we had .html.erb.

In this case, the file itself is going to be in Sass format, but the ending output is going to be CSS.
### What is Sass?
Sass is short for **Syntactically Awesome Stylesheets**. It's a scripting language which is then interpreted into CSS. And the language itself is very similar to CSS, it just includes lots of really nice extra features, such as **nested rules, variables, being able to define whole styles which can be used as mixins and other styles, and selector inheritance**.

If you wanna find out more information about Sass, you can go to their website, sass-lang.com, and they have some tutorials there. **Sass is not part of Ruby on Rails, but it is included in Rails and enabled by default**. That's because the **sass-rails gem is included in our gem file**. One really nice feature about Sass is that **you don't actually have to use any of the features at all. You can have a Sass stylesheet and use regular old CSS in it**. You don't have to learn any additional Sass, you can just have the file be a Sass file.

And then, as you'd start to learn more about Sass, you can start incorporating some of those features in your stylesheets. Let's take a look. You can see that on my desktop, I've got a couple of Sass files. You can tell because they say .css.scss at the end. If you have access to the exercise files, these will be included there. If not, we're gonna take a peak at them in just a moment. Let's add them to our project. They're gonna go inside app, assets, and then inside stylesheets. And to put them in there, I'm just gonna control-click on the directory and choose Show in Finder so that I have that directory right here.

I'm gonna take those files and just drag them inside. Now Rails also gives us a default stylesheet any time we generate a controller. So I have one for subjects, sections, pages, and demo. I don't need all of those separate ones, it's up to you. If you want to keep them grouped by controller, you can. For now, I'm just going to get rid of them. Now we can close up that directory and let's take a look at the contents of these files. First, let's look at primary.css.scss. This is a Sass file, you can tell because here, ***we're declaring some variables***.

```
$red: #993120;
$green: #94B26F;
$light_green: #B6D491;
$yellow: #F2DC87;
$dark_brown: #4C2516;
$white: #FFFFFF;

$sans_serif: Verdana,Lucida,Arial,Helvetica,sans-serif

html {
  height: 100%;
  width: 100%;
}

body {
  width: 100%;
  min-width: 980px;
  height: 100%;
  margin: 0;
  padding: 0;
  font: 14px, $sans_serif;
  color: $dark_brown;
}

p, td, ul, ol, li, dl, dt, dd {
  font-family: $sans-serif;
}

img { border: none;}

```

**This is how we declare a variable in Sass**. We have a **dollar sign in front of the name, a colon**, and **then the value that we want it to be**.

```
$red: #993120;
$green: #94B26F;
$light_green: #B6D491;
$yellow: #F2DC87;
$dark_brown: #4C2516;
$white: #FFFFFF;
```

Then later on, I have the ability to call those variables. Here's another one I'm doing for my font stack.
```
$sans_serif: Verdana,Lucida,Arial,Helvetica,sans-serif

```
So I've got all the fonts that I wanna use for sans-serif fonts. Notice down here in my body style, I'm using that variable.
```
body {
  width: 100%;
  min-width: 980px;
  height: 100%;
  margin: 0;
  padding: 0;
  font: 14px, $sans_serif;
  color: $dark_brown;
}
```
So it just drops that whole string right in there. And then here, I'm using the color, dark brown. Dark brown is equivalent to this number here. So that gives you an idea of how variables work.

Let's scroll down a bit more so we can see some other examples. Here inside the header tag,

```
#header {
  height: 4
  padding-top: 1em;
  text-align: center;
  background: $red;
  color: $white;

  h1 {margin: 0; padding: 0; }
}
```

you can see that I've got an h1 tag that's nested inside. That's the same as if I had written
```
#header h1 {}
```
header and then h1 and provided a style that way. ***I don't have to repeat myself by writing header over and over again. It knows that this is both header and h1,*** ***it's scoped inside of it***. So that gives you an idea of how this works. Let's just scroll down the page a bit more. Again, if you don't have access to the exercise files, you can pause the movie so that you can copy this down, or you can also feel free just to use your own styles.

There's no reason you have to stick with mine. Let's take a look at admin_additions. So Admin Additional Styles is meant to load after that primary file, and it contains additions on top of that, that are suitable for our admin area.
```
#content h2 {
  padding-left: 100px;
  padding-right: 100px;
  h2 {
    width: 100%;
    text-align: center;
  }
}
```
So you can see once again, I've got some **nested styles in here**. We can scroll down, here's some more. Here's menu identity,
```
.menu {
  .identity {text-align: center;}
  ul {
    margin: 3em 250px;
    list-style: none;
    li { margin-bottom: 1em; }
  }
}
```

ul, li, and then notice here I'm using an **ampersand**.
```
a {
  text-decoration: none;
  &:hover { text-decoration: underline;}
}
```
**That's the same as if I'd actually had a separate style that just had the a in front of it**.
```
a:hover { text-decoration: underline;}
```
I'm saying basically **reuse this element right here in front of it**, that's kinda nice.

So again, I'll just scroll down, so if you need to pause the movie and copy these styles down, you can.


```
#content h2 {
  padding-left: 100px;
  padding-right: 100px;
  h2 {
    width: 100%;
    text-align: center;
  }
}

.login form {
  margin: 100px 250px
}

.menu {
  .identity {text-align: center;}
  ul {
    margin: 3em 250px;
    list-style: none;
    li { margin-bottom: 1em; }
  }
}

a {
  text-decoration: none;
  &:hover { text-decoration: underline;}
}

a.action.large {
  float: right;
  padding: 2px 5px;
  margin: 0 0 1em 1em;
  background: #94B26F;
  color: #4C2516;
  border: 1px solid #4C2516;
  text-decoration: none;
  font-size: 0.8em;
  text-align: center;
  &:hover { background: #B6D491; }
}

.actions {
  width: 200px;
  a.action {
    float: left;
    min-width: 3.5em;
    padding: 1px 2px;
    margin: 2px 5px;
    font-size: 0.7em;
  }
}

.notice {
  height: 1.25em;
  padding: 0.5em;
  text-align: center;
  background: #B6D491;
  color: #4C2516;
}

.back-line {
  font-size: 0.8em;
  margin-bottom: 1em;
}

.form_buttons {
  text-align: center;
  margin: 1em 0;
}

.index {
  table {
    clear: both; margin: 0 200px; width: 300px;
    &.wide { width: 600px; margin: 0 50px; }
    tr td {white-space: nowrap; padding-right: 5px; }
    tr td { border: 1px solid #666666; }
    }
  }

  .section + .section {
    padding-top: 0.5em;
    margin-top: 0.5em;
    border-top: 1px dotted #666666;
  }

  span.status {
    display: block;
    width: 10px; height: 10px;
    margin: 0.25em auto; padding: 0;
    border: 1px solid #000000;
    &.true { background: green; color: green; }
    &.false { background: red; color: red; }
  }

  .field-with-errors {
    label { color: #AA0000; }
    input { }
  }

#error-explanation {
  width: 400px;
  padding: 0; margin: 0 auto 1.5em;
  border: 1px solid #AA0000;
  background-color: #FFFFFF;
  font-size: 12px;
  h2{
    font-weight: bold;
    font-size: 12px;
    padding: 5px 5px 5px 15px; margin: 0;
    width: 380px;
    background-color: #AA0000; color: #FFFFFF;
  }
  p {
    color: #333333;
    margin: 0; padding: 3px 0.5em;
  }
  ul { margin-top: 0; }
  ul { list-style: square; }
}

```
And here's our status tag that we were working with earlier. Got a style defined for that. And then we've got some error styling down here. We'll talk about errors a little later, but I've gone ahead and added the styles for them. Alright, so now that we've written our stylesheets, step two was to add those to the manifest file. By default, ***we have a manifest file here called application.css***.

What's inside this file right now is just one big CSS comment.
```
/*
 * This is a manifest file that'll be compiled into application.css, which will include all the files
 * listed below.
 *
 * Any CSS and SCSS file within this directory, lib/assets/stylesheets, vendor/assets/stylesheets,
 * or any plugin's vendor/assets/stylesheets directory can be referenced here using a relative path.
 *
 * You're free to add application-wide styles to this file and they'll appear at the bottom of the
 * compiled file so the styles you add here take precedence over styles defined in any other CSS/SCSS
 * files in this directory. Styles in this file should be added after the last require_* statement.
 * It is generally better to create a new file per style scope.
 *
 *= require_tree .
 *= require_self
 */

```

However, some of these lines are special.
```
*= require_tree .
*= require_self
```
The lines that are down here that include an equal sign, those are the directives that tell the manifest file what it should load in. Notice that there are two directives already. The first is **require_tree and then a period after it**. That tells it to **load in all of the files that are included in the stylesheets directory**. The **period is the current directory, and tree means require all the files that are there**.

Now I don't want our application.css to do that. **I want it to just load in our primary style. So I'm going to remove that by just removing the equal sign in front of it**,
```
*require_tree .
```
and **that will turn it off so that it's no longer functional**. And then the other one is require_self.

```
*= require_self
```

This is a directive that says that it should load in any styles that we might happen to have in this file so that those get included as well. In general, that's not a great practice. It's much better to put your styles in separate stylesheets and not in the manifest file directly. So I'm gonna leave that directive in there, but I'm gonna add another one right before it.
```
 *require_tree .
 *=require primary
 *= require_self
```

It's gonna say require, and then a space. Not an underscore, but a space. And then we're gonna require primary **so that application.css is going to require our primary Sass file**, it's right here. So that'll get compiled together, and the end result will be a file called application.css. Notice it doesn't have .scss at the end of it, it's just CSS by itself. Alright, let's save this file and let's add another manifest file. I'll control-click on stylesheets and choose New File, and then let's call this one admin.css.

And let's just take everything that was in application, we'll copy it, and let's paste it in there. So now we have another manifest file, but this time, instead of requiring tree, requiring primary, or requiring self, we wanna add one more,
```
*require_tree .
 *=require primary
 *=require admin_additions
 *= require_self
```
 which is gonna be the directive to require admin_additions, so now it'll load in primary and then load in admin_additions as well. So now I have two different manifest files. That's gonna result in two different CSS files, and I can choose which one I want to load in.

So now we have our manifest, the next step is to ***make sure that Rails knows to precompile these manifest files for us***. The way we do that is we go into config.
```
app/config/initialise/assets.rb
```

Inside initializers, there's a file called assets.rb,
```
# Be sure to restart your server when you modify this file.

# Version of your assets, change this if you want to expire all your assets.
Rails.application.config.assets.version = '1.0'

# Add additional assets to the asset load path
# Rails.application.config.assets.paths << Emoji.images_path

# Precompile additional assets.
# application.js, application.css, and all non-JS/CSS in app/assets folder are already added.
# Rails.application.config.assets.precompile += %w( search.js )

```
and the part that we care about, it's right down here, where we precompile additional assets.
```
# Precompile additional assets.
```
Now by default, it's already going to compile application.css for us, so we don't need to do anything additionally if we're just using application.css, but we're not. Remember, we created a second manifest file.

We need to tell it about that one. So what we need to do is uncomment this line,
```
Rails.application.config.assets.precompile += %w( search.js )
```

and we need to add to its precompile list our new manifest, which is admin.css. So we'll just put admin.css here, and it'll make sure that it gets precompiled.
```
Rails.application.config.assets.precompile += %w( admin.css )

```
If we had additional ones, you would just list them out here without any commas or anything, main.css, et cetera. Well we only have one additional one. Now we can save that file and close it up. And now Asset Pipeline will know to precompile our manifest file, and it know what our manifest file is composed of.

#### Tell our HTML about this stylesheet.
The way that **we declare a stylesheet in HTML is by using a link tag**, so we use link href, and then we provide the location of the stylesheet. For Asset Pipeline, those are gonna be stored in /assets/stylesheets, and then the name of the manifest file. application.css, for example. And then it would have rel stylesheet, type, media, et cetera. That's what an HTML stylesheet link tag looks like. Rails gives us a nice convenient helper that makes that process much easier.

We just output stylesheet_link_tag, and then provide the name of the manifest file. You don't need to put .css or anything after it. It knows that that's what it's going to be. What's great about this helper method is it does more than just write the HTML code you see above. It actually knows whether or not we're in development or in production, and based on our environment, it can adjust the behavior and the path to find our assets. There's one addition that you probably wanna make to this file. By default, it's going to default to media screen for a stylesheet link tag, and you may wanna have that be media all.

And you can just add that as an option if that's the case. Let's add this to the layout file we created earlier. So if you remember, we created a layout inside views, inside layouts, and we've got one for application.html.erb and another one for admin. So what we wanna add is at the top of this file, let's just remove the existing style that we have, and instead, let's paste in admin. stylesheet_link_tag, we wanna bring in the admin stylesheet, media all.

We wanna make a couple of other additions to this layout while we've got it open, just so we'll have something more to style. Let's just move this over, let's add this inside a div id equals main, and let's put this also down here. div, that will be the main div, and let's add another div, we'll remove this before yield, we don't need that anymore, let's add div id equals content, and after yield, we'll just close the div tag.

And last of all, let's add in a footer at the very bottom. Okay, so now we got a style here, let's take a look at it. Let's open up the command line, and from the root of our Rails project, we'll launch the server, and then let's load it up. Localhost:3000, and let's just go look at subjects. There we go. We have our header, we have our footer, we have the content in the middle. All of that got styled according to our stylesheets. If we surf around now, 'cause it's in our layouts, it's added to all of our pages.

So that's what our admin area is gonna look like. Let's just quickly go and look at what we've got for the application.html.erb. You can see that by default, they've already added a stylesheet link tag here. They left out the parentheses around it, but this is the stylesheet link tag that'll bring in application. For now, don't worry about the addition that they've made here for Turbolinks to it. So now we followed all the steps. We wrote our stylesheets, we added the new manifest, we told Rails to precompile them, and we added them to our html. In the next movie, let's talk about how Asset Pipeline can help us with JavaScript.
