# Amazon start and stop

This is a sample "script" to schedule start and stop [Amazon EC2 instances](https://aws.amazon.com/) based on a date time range, i.e. start at 6AM and stop at 0AM.

Have a look at [AWS Calculator](https://calculator.s3.amazonaws.com/calc5.html) to see how much you can save by stopping unsed instances from 0AM to 6AM (save 25%) or other date time ranges.


## Usage

### Checkout amazon_start_stop.

```bash
$ git clone git@github.com:phstc/amazon_start_stop.git
$ cd amazon_start_stop
```

Edit [bin/amazon_start_stop](https://github.com/phstc/amazon_start_stop/blob/master/bin/amazon_start_stop) and set the constants `START_HOUR` and `STOP_HOUR` as you need.

```ruby
# bin/amazon_start_stop
START_HOUR = 6 # Start 6AM
STOP_HOUR  = 0 # Stop  0AM (midnight)
```

### Heroku

1. Create a [Heroku account](https://www.heroku.com/) if don't have one and install the [Heroku Toolbelt](https://toolbelt.heroku.com/).
2. Create a [new app](https://devcenter.heroku.com/articles/creating-apps).

```bash
$ heroku apps:create
```

### Deploy

```bash
$ git push heroku master
```

### AWS Credentials

Configure your [AWS Credentials](http://aws.amazon.com/iam/) using [Heroku config vars](https://devcenter.heroku.com/articles/config-vars).

```bash
$ heroku config:set AWS_ACCESS_KEY_ID=
$ heroku config:set AWS_SECRET_ACCESS_KEY=

# The instance id to start and stop i.e. i-1a1aa111
$ heroku config:set AWS_INSTANCE_ID=

# Leave blank if you don't need to associate an Elastic IP
# (everytime you stop an instance Amazon dissociates Elastic IPs)
$ heroku config:set AWS_ELASTIC_IP=
```

In development create an `.env`.

```bash
# .env
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_INSTANCE_ID=
AWS_ELASTIC_IP=
```

### Heroku Scheduler

Add and configure [Heroku Scheduler add-on standard](https://devcenter.heroku.com/articles/scheduler) (free add-on).

```bash
$ heroku addons:add scheduler:standard
$ heroku addons:open scheduler
```

In the Scheduler Dashboard add a new Scheduler.

| TASK                    | DYNO SIZE  | FREQUENCY        |
| ----------------------- |:----------:| ----------------:|
| bin/amazon_start_stop   | 1x         | Every 10 minutes |

### Manual execution

In production.

```bash
$ heroku run bundle exec ruby bin/amazon_start_stop
# to check the script output
$ heroku logs
```

In development.

```bash
$ foreman start
```

