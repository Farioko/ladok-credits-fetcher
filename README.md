# Ladok credits fetcher

This Python script fetches the total number of credits that you have earned from Ladok and stores it in `~/.cache/ladok/credits.txt`. This script runs in headless mode and is designed to run as a periodic task through a systemd timer or as a cron task and will only run if the credits file hasn't been updated in more than 12 hours.

# Requirements and limitations

To run this script, you'll need to have Python 3 installed, Firefox with geckodriver and Selenium. You'll also have to supply your username (which could be an email address) and password which are defined as constants within the script. Note that this script will only work when your university is Mid Sweden University (Mittuniversitetet), but adding support for other universities would be trivial if anyone ever finds this repository.

# Running the script as a systemd timer

The unit file could look something like this:

```
[Unit]
Description=Runs the ladok script once to get the current credits

[Service]
User=fabian
Group=fabian
Type=oneshot
ExecStart=/home/fabian/.config/i3/scripts/ladok

[Install]
WantedBy=graphical.target
```

And the timer file could look something like this:

```
[Unit]
Description=Run ladok script hourly

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

# Integration with i3bar

To display the credits in i3bar, you could simply add something like this to your i3status config:

```
order += "read_file ladok"

read_file ladok {
    format = "🎓 %content"
    path = "/home/fabian/.cache/ladok/credits.txt"
    color_good = "#03a5fc"
}
```
