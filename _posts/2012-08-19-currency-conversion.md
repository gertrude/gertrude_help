---
layout: post
category: help
tags: [ manual, gertrude, currency ]
---
{% include JB/setup %}

Gertrude's currency module provides currency information and converts monetary amounts between currencies
using the latest exchange rates.

## Finding the currency code for a country

To find the currency code for a country, use the `currency for` command with a string that's a partial
match for the country you're interested in:

    currency for pound

Gertrude will list the full name and the corresponding currency symbol for all candidates matching your
string. If there are two many, she'll just show the currency symbols.

## Converting between currencies

To convert an amount from one currency to another, use the `convert` command:

    convert 120 USD to GBP
    convert 120 GBP USD
    change 120 GBP to USD
    change 120 GBP USD

## Using the converted value in maths

Once you've made a currency conversion, the final amount is available as the `@ans` variable in the
[maths module](/2012-08-09-maths.md) for further calculation.

## Source of data

Gertrude's exchange rate data comes from [OpenExchangeRates] - her rates are updated every hour.

[OpenExchangeRates]: http://www.openexchangerates.org/

