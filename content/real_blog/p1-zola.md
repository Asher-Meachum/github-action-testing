+++
title = "Zola: an excellent tool with a maturing ecosystem"  
date = 2026-04-27  
+++

# Zola: an excellent tool with a maturing ecosystem

## Why Zola
Why did I choose Zola? I wanted the SSG I used to be written in Rust and in that space, there are really two options[^1]: Zola and Cobalt. Between these two, Zola seems to be more developed as a tool and has better ecosystem support.

## Setup
My setup, in brief, was the following. I installed Zola via Flatpak (this installed v0.22.1, which will be important later). I used `zola init` to create the project directory. Then came the long process of actually creating the website (caused mostly by the process of writing my own templates. Fun? Yes. Strictly necessary? Not at all.) For publishing, I had a Github repo setup for pages from actions, so that is where I sent my project. I should mention that I setup CD for the repository prior to publishing. If you want to see that, it is down [here](#a-bit-more-on-deployment)

## The Good, The Bad, The Ugly
Using Zola was a pleasure. It was simple to install and run; it was easy to get a project initialized; it was light, the tool compiled my website in less time than it took me to switch from my editor to where it was previewed on `127.0.0.1:1111`. If I have one complaint, it was that removing non-page files required `zola serve` to be restarted, but that was more a hit to polish than usability.  

That being said, there are some parts that could use improvement. The docs were one of the more difficult elements of Zola. The docs layout were much better for reference than learning, but in places lacked depth. To give an example of this, I wanted to create a list of subsections for one of my templates. I knew from the docs it was possible, but nowhere in the docs could I find the methods needed to do that. It eventually got to the point where I was going through the source, before I decided that maybe I didn't need that after all.  

Another issue with the docs, is that it sub-links to other docs too quickly, instead of including basic usage inside of its own documentation. This is understandable from a maintenance perspective, but is annoying for the end user.

And then there was continuous deployment.

## A Bit More on Deployment
For CD, I initially used the [official Zola Github Pages](https://github.com/getzola/github-pages) action: I created `.github/workflows/main.yml`, used the excerpt in the README of aformentioned action, and pushed my site. I navigated to the page and...404 with Github branding and an email shortly after telling me that my workflow had failed. Digging into the action log revealed this:  
```
Status: Downloaded newer image for ghcr.io/getzola/zola:v0.22.0
Building site...
ERROR config.toml not found in current directory or ancestors, current_dir is /workspace
Error: Process completed with exit code 1.
```
If you are sharp-eyed, you may have already caught it. Remember how I was running v0.22.1? Now, to be fair to Zola, there was this section I admittedly overlooked:
```
      - name: Build Zola + upload Pages artifact
        uses: getzola/github-pages@066755243e69f508fd1a74739fbf1a65f656c790
        with:
          zola_version: v0.22.0
```
To be fair to me, that wasn't the only problem, as you will see.
Not being sharp-eyed, I did the more obvious course of simply symlinking `config.toml` to `zola.toml`

I pushed this change, watched the Github Action dashboard and...success. I navigated to the page and there it was, a nice, proud, Github-themed 404. This second problem is much more difficult to trace, but it ultimately came down to a failure of the action (see it [here](https://github.com/getzola/github-pages/blob/v1/action.yml)) it was pulling from the getzola version to upload the site.



## Conclusion

[^1]: Technically, there is a third option: mdBook, but that seems to be aimed more at book-style docs, rather than more general websites.