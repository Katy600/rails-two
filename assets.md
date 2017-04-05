## Asset Pipeline

Helps to **manage the CSS, JavaScript, and images used by your application**. In this movie we'll get an overview of how it works. Asset Pipeline offers a few benefits. It **concatenates together separate CSS and JavaScript files into single files**. That **reduces the number of browser requests needed to render a page, and allows caching of these files**. It also **compresses and minifies the code.** It **removes white space and comments** and **compresses them using gzip for web servers that support that**.

It doesn't do all of this with each and every request. Instead, **it does it ahead of time**. It **precompiles the CSS and the JavaScript**. Because it's being precompiled, that also gives us the opportunity to write our CSS and JavaScript using other languages, which can then be compiled into regular old CSS and JavaScript. CoffeeScript is a popular example for writing code for JavaScript, and SAS is a popular way to write CSS. We can even use ERB to have dynamic content in our assets. It also adds asset fingerprinting.

**Asset fingerprinting is a clever way to force browsers to keep cached assets up-to-date**. What happens is that **Asset Pipeline looks at each asset and it develops an MD5 Hash for that asset**. That is, **it takes the entire contents of the asset and from that distills it down to a single string**. And then it **pins that string to the end of the file name**. And it's called a **fingerprint**. The reason it does that is because the **asset file is gonna be cached by the user's web browser**. But **whenever something about the file changes, the MD5 Hash will change, and that means that the file name will change**.

So now the HTML page will be telling the browser to go find a different asset file, **because the name is different and that forces the browser to get a fresh copy of the asset**. In early versions of Rails, all assets were located in sub-directories of public,
```
public/images

public/javascripts

public/stylesheets
```

With the Asset Pipeline, **the preferred location for these assets in now in the app/assets directory.**

```
app/assets/images

app/assets/javascripts

app/assets/stylesheets
```
### Files in Public Directory
Assets can still be placed in the public hierarchy, but if you do, **they will be served as static files by the web server exactly as-is.**

And you'll give up all of those advantages that we just talked about with using Asset Pipeline.
```
no concatenation, compression, or minification
```
The way that Asset Pipeline knows which files to concatenate together is through the use of something called the manifest file.

### The manifest file
- ***contains directives telling Asset Pipeline which assets should be included and in what order.*** Asset **Pipeline loads the files in the manifest**, **processes them** if necessary, **concatenates them into a single file**, and then **compresses that file into one**. This creates a **single smaller stylesheet**, which can be cached by a user's browser, and which can contain styles from many parts of the site.

It's generally better to serve one file to the browser instead of many files. But at the same time, we have the ability to develop using many files. We can separate our CSS up into 20 different files if we want, and they'll all get compressed into one file to serve to the user. So there is an important distinction about the way that Asset Pipeline works
## Development VS. Production.
When we are in **development, Asset Pipeline skips the concatenation, the compression and the fingerprinting and only does the file processing**. If we're using SAS, CoffeeScript or ERB, those will still be interpreted.

But it won't do all the other steps. There are a couple of reasons why that's true. The main one being, that files are changing frequently during development, and so they need reprocessing but it's not worth spinning the extra energy on concatenating, compressing and fingerprinting. **When we're in production, though, Asset Pipeline actually skips all of those steps, and it doesn't do any asset processing**. That may be surprising, but the reason why is because **Rails assumes that those assets have already been precompiled for it**. And that's a necessary step when we're deploying using Asset Pipeline, is we have to actually create those files.

We have to take the manifest file, and create those fingerprinted files for it. Those typically **reside in public/assets**. So this would be an example of how application dot css would be turned into public/assets/application with a fingerprint after it dot css. And then the gzipped version as well.
### How do we do this precompilation?
Well, you wanna make sure that your Rails environment is set to production,
```
export RAILS_ENV=production
```
... and then from the root of your Rails application, on the Command line, you could type...

```
 bundle exec rails assets:precompile
```

Now in truth, **most deployment systems used for production will perform this process for you**. So if you're using something like **Capistrano to deploy your code, it'll handle precompiling your assets at the same time as it does the deployment**. If you're not using a tool to help you deploy, then you would just have to do it yourself. In the next three movies, we'll take a closer look at stylesheets, JavaScripts, and images.
