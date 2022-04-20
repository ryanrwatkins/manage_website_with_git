# manage_website_with_git
Manage the co-development of a website with Git. Use git to push website edits and to keep up-to-date version controls. This version sets up a development-site version of the website that gets co-edited first, and then later changes can be merged into the live website. You can just edit the main site by skipping the development site instructions (primiarly in Step 4).

![alt text](https://github.com/ryanrwatkins/manage_website_with_git/blob/main/manage_with_git_flow.png)


To get things set up, look at the flow image above and follow the instructions on this website: https://gist.github.com/Nilpo/8ed5e44be00d6cf21f22  As the website manage you will want to set up (a) a development site, (b) a live site, and (c) a "bare" git repo what will hold the "hook" which send files to the appropriate place.  You will also want to create a user group on the server, put your other users into the group, and then give that group read-write permissions on all the three directories.

Note that with Git installed on your local machine you should have a "remote repository pointing" to the Orgin on Github already, this is what allows you to push changes to Github, and now you are adding a second pointing script to push to your website's server.   You can list your "remote repository pointing" scripts by with 'git remote -v'.

Once you get it set up and working for you, then here are instructions for other users so they can also update the site and Github repo too.


**Instructions to send to other users:**

Step 1: Create SSH access without passwords (first time only). The best instructions I found were here: https://gist.github.com/Nilpo/8ed5e44be00d6cf21f22#su
Basically you have to create an .ssh directory in the home directory on your local computer (there is already one in your server home directory). In this .ssh directory you then have to create public/private keys. And then you will push the public key to the server .ssh directory.  Then when you log in to the server with SSH the next time, it should just let you  enter without a password (using the public/private key pairing).

Step 2: On your local machine download the private Git repository for the project (first time only).  You can create branches off the dev-site branch, and then we will update and test things on this branch first, and if we will like them then we can move them to the main (which links to the live serve).  As usual, you will want to give you local git up to date with the Github version each time you make edits.  

Step 3: Write code. Once you make changes locally you can either commit them Github or wait to commit after you have tested with the local dev-site branch (either way works since you will be updating the development site with the local dev-site files on your computer).

Step 4: Start development site in virtual environment. When you have changes you want to check out on then development site to see how they work, you first have to restart the virtual env so that the development site will be live again.

For my server, it is this process:<br/>
Log into the server with SSH<br/>
$  export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3 <br/>
$  cd /dev-site/directory &&  source `which virtualenvwrapper.sh`<br/>
$  python3 ./manage.py runserver 0.0.0.0:8080<br/>

This will start the development environment server and you can access the site at:  mywebsite.com:8080    It will keep running until you  terminate the session or hit CTRL-C.

Step 5: Add a Remote Repository Pointing to the Web Server. Once you have a working repository, you'll have to add a remote pointing on your local machine to the one you set up on your server.
On your local machine add...<br/>
$  git remote add <name_for_remote_connection_point> ssh://<your-login-name>@mywebsite.com/home/user/directory/with/bare.git<br/>
You can then use this to view all the remote connections point set for you git;<br/>
$ git remote -v 

Step 6: Push changes from local to development site. You can push you dev-site branch to the development site on the server using:  
On your local machine, go the directory with the website site.

$  git push <name_for_remote_connection_point> <name_of_branch_or_blank_for_main>

That should be it, your edits should show up on the development site for testing. Once you are happy, then you can push the new edits to GitHub and start a pull request.  When we all agree, then we can merge the dev-site branch with the main, and then update the  main version of the site.


**Also based on:<br/>**
https://www.sitepoint.com/deploying-from-github-to-a-server/

https://hackernoon.com/deploy-website-to-remote-server-using-git-da6048805637

https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps
