# cl-cron

A simple tool that provides cron like facilities directly inside of common lisp.

a fork of https://bitbucket.org/mackram/cl-cron (unavailable)

Install with Quicklisp:

    (ql:quickload "cl-cron")

Note: Quicklisp points to this repository since the release of October, 2021.

Example:

Print a message every minute.

~~~lisp
(defun say-hi ()
  (print "Hi!"))

(cl-cron:make-cron-job #'say-hi)
(cl-cron:start-cron)
~~~

Wait a minute to see output.

Stop all jobs with `stop-cron`.

To get the current jobs hash table: `(jobs)`.

To pretty print current jobs: `(print-jobs)`.

## API

```lisp
(make-cron-job function-symbol &key (minute :every) (step-min 1)
                                    (hour :every) (step-hour 1)
                                    (day-of-week :every) (step-dow 1)
                                    (day-of-month :every) (step-dom 1)
                                    (month :every) (step-month 1)
                                    (boot-only nil)
                                    (hash-key nil))

Create a new instance of a cron-job object and append it to the cron-jobs-list after
processing its time.

Key parameters:
- hash-key (optional): give a name to the job.
- minute, step-min
- hour, step-hour
- day-of-week, step-dom
- month, step-month
- day-of-month, step-dom
- boot-only: run the job only when START-CRON is called.

Please note that as by ANSI Common Lisp:
- for the month variable the possible values are between 1 and 12 inclusive with January=1 and
- for the day of week the possible values are between 0 and 6 with Monday=0.

If you wish to use multiple values for each parameter you need to provide a list of numbers or use the gen-list function. You can not have a list of symbols when it comes to month or day-of-week.

Return: the job hash-key (name).


(delete-cron-job cron-key)

(time-to-run-job job)

start-cron

restart-cron

stop-cron

jobs

print-jobs
```

Example:

~~~lisp
(cl-cron:make-cron-job
            (lambda ()
              (format t "Wake Up!~%"))

            ;; Days of week are numbered from 0,
            ;; where 0 is Monday.
            ;; Run every Sunday:
            :day-of-week 6
            :hour 10
            :minute 0
            ;; hash-key is the name
            :hash-key :sunday-alarm)

(cl-cron:delete-cron-job
            :sunday-alarm)
~~~

https://40ants.com/lisp-project-of-the-day/2020/06/0087-cl-cron.html

<!-- http://quickdocs.org/cl-cron/api -->


## Changelog

- 2026-06-13: added `(jobs)` and `(print-jobs)`.
- 2026-06-12: the jobs hash-table is now `equal` (names can be a string or anything else).
- 2020-10-13: we added a name to the cl-cron thread.

Licence: GPL
