# esp-idf-cron
Example of CRON with esp-idf.   
I found [this](https://github.com/staticlibs/ccronexpr) library.   
Defining dates and times in crontab is complicated, but this library handles it well.   
It's Great job.   

# Installation
```
git clone https://github.com/nopnop2002/esp-idf-cron
cd esp-idf-cron
idf.py menuconfig
idf.py flash
```

# Configuration

![config-main](https://user-images.githubusercontent.com/6020549/189264672-92703f21-5a57-41e6-ab12-cafee710632e.jpg)
![config-app](https://user-images.githubusercontent.com/6020549/189264696-81ad38a4-df28-4e6f-bd36-16fe42deecfc.jpg)

## WiFi Setting
![config-wifi](https://user-images.githubusercontent.com/6020549/189264706-08d5ec0e-d11a-4c17-b375-a73d551ebdd1.jpg)


## NTP Setting
![config-ntp](https://user-images.githubusercontent.com/6020549/189264712-ed02b5f1-46b7-4023-89e3-79d7605ea9b5.jpg)


# crontab
crontab is in the crontab directory.
Supports cron expressions with seconds field.
```
      +---------------------------  second (0 - 59)
      |  +------------------------  minute (0 - 59)
      |  |  +---------------------  hour (0 - 23)
      |  |  |  +------------------  day of month (1 - 31)
      |  |  |  |  +---------------  month (1 - 12)
      |  |  |  |  |  +------------  day of week (0 - 6) (Sunday to Saturday;
      |  |  |  |  |  |                                   7 is also Sunday on some systems)
      |  |  |  |  |  |  +---------  task name
      |  |  |  |  |  |  |
      *  *  *  *  *  *  task
```

In Linux you specify the command, but in this project you specify the task name.   
Note:   
Task names in FreeRTOS are function names.   
Tasks are created using the ```xTaskCreate``` function.
However, each task works independently.   

```
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# s m h  dom mon dow   task
*/15 * 0-11 * * * task1        # every 15 seconds in the morning
*/15 * 12-23 * * * task2       # every 15 seconds in the afternoon
0 */1 0-11 * * * task3         # every 1 minutes in the morning
0 */1 12-23 * * * task4        # every 1 minutes in the afternoon
0 0 9-17 * * MON-FRI task5     # on the hour 9 to 17 weekdays
0 23 0 25 1,3,5,7,9,11 * task6 # Midnight on odd months
```

# Limitations
This example does not judge the end of the task.   
Example of starting every 10 seconds:   
```
10:00:00 Wake up a task.
10:00:10 Wake up a task again. But the task is still running.
10:00:15 Task ends. The task wakes up immediately.
```
You must consider the time required to perform the task.   

# Screen Shot
Tasks are different in the morning and afternoon.   
In the morning, execute task1 and task2.   
In the afternoon, execute task3 and task4.   

![ScreenShot-2](https://user-images.githubusercontent.com/6020549/189264777-07366c62-39d5-4061-854c-f161b5739b7c.jpg)


