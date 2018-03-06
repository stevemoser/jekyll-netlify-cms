---
layout: post
title: "Falsehoods Programmers Believe About Systems of Measurement"
date: 2013-08-07 10:15
comments: true
tags: [Open Source, UnitsKit] 
---

When starting [UnitsKit](https://github.com/stevemoser/UnitsKit) I had many false assumptions about how irregular systems of measurement could be.  Even after studying the subject and other existing libraries I was surprised by how many more irregularities I found. So inspired by Patrick McKenzie's post about ['Falsehoods Programmers Believe About Names'](http://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) I have set out to form a noncompelete list of irregulars when working with systems of measurement and converting between them.

1. There is more than one variant of the metric system. (MKS vs CGS)
2. All base SI units are prefix-less. (The kilogram is the base unit of mass)
3. All derived metric units are derived of base SI units. (The liter is based off of decimeters)
4. Heterogenous units are of no practical use. (Radar beam height formula uses a constant expressed in nautical miles per foot)
5. Fractional dimensions are of no practical use. (Radar beam height formula uses a constant expressed in [nautical miles per foot^(1/2)](http://www.boost.org/doc/libs/1_37_0/doc/html/boost_units/Units.html))
6. Non-metric systems have base units as well.
7. Just as liters has decimeters so that it doesn't need a numerical proportionality constant, pints has BLANK. (I made up my own unit for this, a little more than 11 of them fit in a yard).
8. A gallon is treated much like a liter in their respective systems of measure. (Liter is not an official SI unit but a gallon is an official Imperial unit).
9. All base units are made of base dimensions. (The gallon 'base' unit made of the derived volume dimension)
10. A gallon is a gallon is a gallon. (Did you mean a liquid or dry gallon, US, Imperial, or a ten-gallon hat?)
11. Written out unit names are not case-sensitive (Calorie vs. calorie)
12. Base units always reduce to the same derived units. (Did you mean a Joule or a Newton meter? You might know but then you have to keep track of plane and solid angle units.)
13. Written out base units should always be presented in the same order (Are capital symbols, units with positive exponents, or length dimension units first?)
14. There is no need to differentiate between absolute and relative measurements. (Kelvin and Fahrenheit)
15. Derived unit symbols are represented the same way across systems of measure (mph vs km/h)
16. All units accepted for use with the SI are base ten (the hour and minute are base 60, Sexagesimal, blame the moon)
17. You will never have any namespace issues. (Is that h an hour or the Planck constant?
