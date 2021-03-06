[![CPAN version](https://badge.fury.io/pl/Date-Pregnancy.svg)](http://badge.fury.io/pl/Date-Pregnancy)
[![Build Status](https://travis-ci.org/jonasbn/perl-date-pregnancy.svg?branch=master)](https://travis-ci.org/jonasbn/perl-date-pregnancy)
[![Coverage Status](https://coveralls.io/repos/github/jonasbn/Date-Pregnancy/badge.svg?branch=master)](https://coveralls.io/github/jonasbn/Date-Pregnancy?branch=master)

# NAME

Date::Pregnancy - calculate birthdate and week numbers for a pregnancy

# VERSION

This documentation describes version 0.05

# SYNOPSIS

        use Date::Pregnancy qw(calculate_birthday);

        my $dt = DateTime->new(
                year  => 2004,
                month => 3,
                day   => 19,
        );
        $birthday = calculate_birthday(first_day_of_last_period => $dt);

        $birthday = calculate_birthday(
                first_day_of_last_period => $dt,
                period_cycle_length      => 28,
        );


        use Date::Pregnancy qw(calculate_week);

        $week = calculate_week(first_day_of_last_period => $dt);

        $dt2 = DateTime->new(
                year  => 2004,
                month => 12,
                day   => 24,
        );

        $week = calculate_week(
                first_day_of_last_period => $dt,
                date                     => $dt2,
        );

        $week = calculate_week(
                first_day_of_last_period => $dt,
                method                   => '40weeks',
        );


        $week = calculate_week(birthday => $birthday);


        use Date::Pregnancy qw(calculate_month);

        $month = calculate_month(first_day_of_last_period => $dt);

        $month = calculate_month(
                first_day_of_last_period => $dt,
                date                     => $dt2,
        );

        $month = calculate_month(
                first_day_of_last_period => $dt,
                method                   => 'countback',
        );

        $week = calculate_month(birthday => $birthday);

# DESCRIPTION

This module can be used to calculate the due date for a pregnancy, it implements
3 different methods, which will give different results.

The different methods are described below in detail (SEE: METHODS).

This module relies heavily on DateTime objects in order to calculate
dates of birth, week numbers and month numbers. It does however not
require Michael Schwern's module [Sex](https://metacpan.org/pod/Sex).

## calculate\_birthday

Calculates date of birth.

Takes one named parameter:

\* first\_day\_of\_last\_period

Which should be a DateTime object indicating the first day of the last
period.

If the period cycle length varies from the average of 28 days,
you can optionally provide the named parameter:

\* period\_cycle\_length

This defaults to 28.

The default method used for calculating date of birth is the 266 days
method (SEE: Methods). If you want to use one of the other methods you
can use the named parameter **method** and specify either:

\* 40weeks

\* countback

The function returns a DateTime object indicating the calculated date of birth
or undef upon failure.

## calculate\_week

Calculates in what week the pregnant person currently is. A pregnancy
is on average 40 weeks; these week numbers are normally used when
talking pregnancy and most guides, books and websites refer to the
different stages of pregnancy using these week numbers.

Takes one named parameter:

\* first\_day\_of\_last\_period

Which should be a DateTime object indicating the first day of the last
period.

Optionally you can provide it with a named **date** parameter if you want
to calculate the week number for a given date. The date specified
should be a DateTime object, this defaults to now, when not provided.

Also a named parameter called **birthday** can be provided, if you
already have a birthday DateTime object. If this is not provided
**calculate\_week** will call **calculate\_birthday** internally.

As for **calculate\_birthday** the function also takes the named parameter:

\* first\_day\_of\_last\_period

and

\* method

Please refer to **calculate\_birthday**.

The function returns an integer indicating the week of the pregnancy or
undef upon failure.

## calculate\_month

Calculates in what month the pregnant person currently is (see also
**calculate\_week**). A pregnancy is on average 9 months.

Takes one named parameter:

\* first\_day\_of\_last\_period

Which should be a DateTime object indicating the first day of the last
period.

Optionally you can provide it with a named **date** parameter if you want
to calculate the month number for a given date. The date specified
should be a DateTime object, this defaults to now, when not provided.

Also a named parameter called **birthday** can be provided, as for
**calculate\_week**, if you already have a birthday DateTime object. If
this is not provided **calculate\_month** will call **calculate\_birthday**
internally.

As for **calculate\_birthday** the function also takes the named parameter:

\* first\_day\_of\_last\_period

and

\* method

Please refer to **calculate\_birthday**.

The function returns an integer indicating the month of the pregnancy
or undef upon failure.

# METHODS

This module implements 3 different methods for calculating the date of
birth based on data such as first day of last period (LMP) and average period
cycle length (APCL).

The 3 methods are:

- 266 Days (the one used by default)
- 40 Weeks
- Count Back

## 266 Days

This method uses the APCL together with the LMP.

It adds the APCL divided by 2 to the LMP in the case where APCL is
equal to or lower than the average of 28 days.

In the case where the APCL is higher than the average it adds the APCL
multiplied by 0.85 multiplied by 2 divided by 3 to the LMP.

## 40 Weeks

This method does not use the APCL, but simply counts 40 weeks, the
number of weeks in an average pregnancy from the LMP date.

## Count Back

The count back method adds 7 days to the LMP, then deducts 3 months, and
finally adds 1 year to give the estimated date of birth.

# TEST DATA

I am very interested in improving this module, so if you have the
opportunity of submitting me test data it is more than welcome.

The format should be either a test file (\*.t), where you choose the
file name yourself (see t/Villads.t) or you simply submit me the date
of the first day of the last period for the pregnant person, the length of
time between periods (if this varies from the avarage of 28), and possibly
the result of your own/your doctor's week number calculation. I can
then use these data to validate the calculation methods used in this
module.

If possible please include the information on what method your doctor
is using if this is available (SEE: METHODS).

# BUGS

Please report issues via CPAN RT:

    http://rt.cpan.org/NoAuth/Bugs.html?Dist=Date-Pregnancy

or by sending mail to

    bug-Date-Pregnancy@rt.cpan.org

See the BUGS file for known bugs.

# SEE ALSO

- [DateTime](https://metacpan.org/pod/DateTime)
- [Sex](https://metacpan.org/pod/Sex)
- [Time::Clock::Biological](https://metacpan.org/pod/Time::Clock::Biological)
- [http://www.paternityangel.com/Articles\_zone/How\_it\_happens/How-6.htm](http://www.paternityangel.com/Articles_zone/How_it_happens/How-6.htm)
- [http://javascript.internet.com/calculators/pregnancy.html](http://javascript.internet.com/calculators/pregnancy.html)
- [http://www.plus-size-pregnancy.org/figuring.htm](http://www.plus-size-pregnancy.org/figuring.htm)

# DISCLAIMER

The method of calculating day of birth and week numbers implemented in
this module is based on simple formulas.

The ultrasound scan is a much more accurate method and finally babies
seem to have a will of their own, so please do only use the results of
this module as a guideline, the author of this module cannot be held
responsible for the results of calculations based on use of this module.

Feedback is welcome though as well as test data (please see TEST DATA
above).

# ACKNOWLEDGEMENTS

- Nick Morrott, corrections to multiple spelling errors
- Thomas Eibner, who ALWAYS asks me whether I am a father by now
and acuses me of pregnant-talk (just because he cannot calculate the
weeks), now he can find out by looking at the tests included in this
module or by using this module.
- Lars Balker Rasmussen, who could not find Date-Pregnancy in his include
path "lbr can't locate Date/Pregnancy.pm in @INC" - Now he has no
excuse [Date::Pregnancy](https://metacpan.org/pod/Date::Pregnancy) is a reality.
- Lars Thegler for making me revisit the alpha version.

# AUTHOR

Jonas B. Nielsen, (jonasbn) - `<jonasbn@cpan.org>`

# COPYRIGHT

Date-Pregnancy is (C) by Jonas B. Nielsen, (jonasbn) 2004-2016

Date-Pregnancy is released under the artistic license 2.0
