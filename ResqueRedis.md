Running Background Processes with Resque and Redis
==================================================

With Rails 4 we began using [Active Job](http://edgeguides.rubyonrails.org/active_job_basics.html) to run tasks in the background that take a long time.

Active Job provides the framework, and we use [Resque](https://github.com/resque/resque) and [Redis](http://redis.io) to implement the queuing and running of background tasks.

##Running resque and redis in the development environment

1. Download and install [Redis](http://redis.io/topics/quickstart)
(I did this on the vagrant vm in the home directory of the vagrant user.)

    ```
    wget http://download.redis.io/redis-stable.tar.gz
    tar xvzf redis-stable.tar.gz
    cd redis-stable
    make
    sudo make install
    ```
The output recommends that you run `make test`, but it requires a newer version of `tcl` so I didn't bother.

3. Start `redis-server` in one shell by simply running the executable.  This is the memory store for keeping track of jobs in the queue.

2. Resque is a gem and should have been installed when you ran `bundle install.`

To start up resque use the following in another shell window.

```
 VVERBOSE=1 QUEUE=* bundle exec rake environment resque:work
```

Note: This should be fixed because VERBOSE is deprecated.