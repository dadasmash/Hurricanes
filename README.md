# Predicting Hurricanes
###Here's the code associated with my "Predicting Hurricanes" YouTube series. https://www.youtube.com/channel/UCPmLClJE0GmnZ4e7sW_Fu7A

Far too many tutorials on using Machine Learning out there make it seem like your whole job is just taking a nice pre-processed dataset that you were simply given and running it through the latest algorithm of the week. The reality of a true machine learning "engineer" however, is much dirtier, so to speak. Before a line of code ever gets written, there's a whole lot of work that has to get done, and a methodology we follow. Graduates of any of the top engineering schools all get this jammed down our throats through our whole college careers. It's called the "core engineering design process," and it applies to all disciplines of engineering, from mechanical and chemical to electrical and computer. Following this is how we figure out what precise problem needs to be solved, define our design requirements, research existing solutions to figure out what's been done and how we might be able to improve it or apply it in a different way, and then design a solution that will actually solve the problem. Randomly grabbing a dataset and trying a million things with it might get you a usable solution as well (and on the surface sounds like it fits the old "fail fast and fail often" philosophy of silicon valley startups), but following the engineering design process will ensure that you maintain a bigger picture strategy that will likely bring you to a more creative and interesting solution that works far better in the end. And unltimately, this is what many of those silicon valley tech startups are actually doing, at least the successful one. There's a lot of work that has to get done to set up the design/build phase, but once that whappens, we build prototype after prototype... iterating and honing in on the a final design until our performance metrics reach the desired levels. This process more than anything else is the reason why our technology has seen such a drastic increase in the rate of improvement over the past hundred years.

So that's what this series is all about. How do we solve real world problems using the latest technology? Well, we follow the engineering design process of course! If you're interested in getting your hands dirty with me as we apply this process to the problem of forecasting hurricanes using machine learning, strap in and subscribe. 

Just a couple notes up front: 
1. Instead of acting like a standard repository with the latest version being reflected by what you see here, I'm going to keep the code I use from each episode in its associated folder. So, "Episode_5" may have a newer version of the same file contained in "Episode_4" but that's just so you can follow along with the series. I'll include a readme in each episode folder to help aid you in using each file, but for the best experience, you should watch the associated episode.
1. Also, the datasets I use here are massive, and so I will not be posting any of that here. If you want to follow along, you'll have to download it yourself using the ftp sites and Python scripts I've written. Just a heads up though, I've basically filled up an entire 4TB hard drive, and I've only scratched the surface of the European data.

If you're still onboard, you'll need a few things.

## MySQL

Available here: https://www.mysql.com/downloads/

Make sure that you also install MySQL Workbench. Mine is installed in a Windows environment, but I think it should be pretty much the same if you install in Linux. I usually install the mysqlclient and mysql-connector-c python libraries via Conda as well. There's a lot of documentation available to help you get started, but it's very straightforward if you've ever used any type of SQL before.

## Anaconda (aka "Conda")

This is what you'll generally want to use to install Python and manage whatever environments and associated installed packages you want to set up. It's just so much easier to do this with Conda than any other distribution I've found. You won't be able to find every package in Conda, but the core ones, like numpy, Pandas, sqlalchemy, etc. are all there, and Conda makes sure that all the versions are compatible. There are almost always instances where you'll have to install additional packages. Just do those at the end, after everything that's in Conda. We'll be using PyGrib, for instance, which you'll need to install with pip. Installing these last doesn't guarantee you won't "break" anything in your environment, but it minimizes the risk. With that said, do make sure you set up a second environment for all of this. Never use just the base environment. It's very difficult to reset it if something goes wrong, whereas it's very easy to delete a broken environment and start over. 

One other thing I like about Conda is it comes with Spyder, which is a pretty decent IDE that's great for working with data. A problem with using just a text editor with a terminal / command prompt is that it's annoying to view large datasets or even samples. With Spyder, you can view all your active variables in the upper right corner, and then simply double click one, like a numpy array or Pandas DataFrame, and you'll be able to see the whole dataset, with numerical columns color coded by value to help you spot outliers. Also, you can easily create several iPython consoles to run numerous parallel instances of simple modules that you may have running. I use this when I set up clusters. You can alternatively use threading or multiprocessing to accomplish the same thing, but the problem there is printing output. There are ways to do that, but it's far easier to spot / debug issues that may only show up several hours into a run when each thread is running in its own iPython console, and if you code things up correctly, you won't necessarily have to start the whole experiment over again to fix the issue. Just fix it in each of the modules, and get them running again. As long as the central "learner" or whatever you want to call it hasn't choked on a bad input, you can keep running. I find this to be far safer when setting up a training run that could go for a week, and when you're working on something real-world and not just training a neural network to play a simple low resolution Atari game or learn on a cultivated, pre-processed sample dataset, you'll find this is really helpful.

## RabbitMQ and Pika

These are what I use for my cluster, at least as of now. I'm currently reading up on Spark / PySpark and thinking of ways of perhaps using those for a future iteration of this project, but for now, I'm sticking with these, mostly because I've been using them for years now and I have template code that I recycle to rapidly prototype systems. RabbitMQ is a simple queueing / messaging service. This lets numerous threads working on multiple computers on a LAN all send data in the form of numpy arrays or even Pandas DataFrames to a central queue. Pulling data out of that queue is very fast, especially compared with running a query on a SQL database. What this lets us do is to store our data in a central database and have multiple threads running queries on that database in parallel, pulling samples and putting them in the queue so that our central "learner" (i.e. our thread that's actually training a neural network or whatever machine learning algorithm we're working with) can pull out a batch without having to wait for the SQL database to do any matching and return results. This lets us more fully utilize both our CPU and GPU while training... enabling our CPU to continue preparing and loading data while our GPU works on training.

There are many ways of doing this. If you only have one computer, Keras offers a nice solution called "fit_generator" that will do pretty much the same thing (i.e. preparing the next sample with your extra CPU capacity while training with the GPU). However, be aware it only really works in Linux, so if you intend to go that route, get yourself a nice Linux distribution. Ubuntu is usually the best place for newbies to start because it's widely distributed and you can find answers to just about any issue on StackOverflow. I'm personally starting to eye the new Intel Clear distibution though, which has been making the news lately by putting out benchmark results that seem almost too good to be true. You may want to check that out.

And then there's Spark. This seems to be the most commonly used platform amongst the bigger companies working with Machine Learning these days. It's pretty compelling.. allowing you to store data across a cluster instead of simply using a cluster to increase processing capability the way I do. I'll certainly be looking into this more in the coming weeks... later episodes may make use of it if I can find an easy way to integrate it with this project. Otherwise, I'll probably feature it in the next season.




