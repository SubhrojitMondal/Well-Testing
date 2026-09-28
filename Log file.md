# **Week** 1



## Task 1 — Set up your notebook



Your job



Create a new Jupyter notebook and import the three fundamental libraries:



NumPy

Pandas

Matplotlib



Don't copy code from me yet.



Questions you should be able to answer



Q1. Why do we need NumPy if Pandas can also hold numbers?



Q2. What is the usual alias for NumPy? np



Q3. What is the usual alias for Pandas? pd



Q4. Which part of Matplotlib is normally imported for basic plotting? pyplot



Hint 1



You have probably seen:



import \_numpy\_ as np

import \_pandas\_\_\_\_ as pd

import matplotlib.\_pyplot\_\_\_\_ as plt

Deliverable



~~Your first notebook cell should successfully import all three libraries without an error.~~ *Done*



## Task 2 — Generate the 100 time points



Now we start creating your synthetic well-test experiment.



Suppose your pressure-transient test runs from:



$$ 0.001\\text{ hr} \\rightarrow 100\\text{ hr} $$



You need exactly 100 measurements.



But there is an engineering question.



Should we generate them using something like:



0.001, 1.011, 2.021, 3.031, ...



or something more like:



0.001

0.00112

0.00126

...

0.01

...

0.1

...

1

...

10

...

100

Your questions



Q1. Should you use np.linspace() or np.logspace()? np.logspace() as this one creates a logarithmic range of data while the other just creates an equidistant data.



Q2. Why might logarithmically spaced time points be better for pressure-transient analysis? Because we need to do lo-log plot and if its logarithmic spaced it will be a straight line in the loglog plot.



Q3. What does this mean?



$$ 10^{-3}=0.001 $$



Q4. What does this mean?



$$ 10^2=100 $$

Hint 1



Look at:



np.logspace(start, stop, number\_of\_points)



But there's a catch: start and stop represent powers of 10, not the actual starting and ending time.



Your deliverable



~~Create an array called something like:~~



~~time\_hr~~



~~containing exactly 100 values.~~



~~Then prove that it is correct by finding:~~



~~Number of points:~~

~~Minimum time:~~

~~Maximum time:~~ Done

Don't continue until you can explain



Why does:



np.logspace(-3, 2, 100)



start at 0.001 rather than -3? for logspace its not -3 rather 10^-3.



That conceptual understanding is more important than typing the command.

So:



linspace → constant difference



$$ t\_{i+1}-t\_i= constant $$



logspace → constant ratio



t\_i+1/t\_i = constant

&#x09;​



## Task 3 — Define the physical problem



Now we make it an actual reservoir-engineering problem.



Imagine a well undergoing a constant-rate drawdown test.



For example, we eventually need parameters representing:



$$ k,\\ h,\\ \\phi,\\ \\mu,\\ B,\\ c\_t,\\ q,\\ r\_w,\\ p\_i $$



Your job is to investigate what each symbol represents.



Questions



For each parameter, tell me:



Q1. What physical property does it represent?



Q2. What units would you normally use?



Q3. Is it a reservoir property, fluid property, well property, or operating condition?



Then answer the conceptual question:



If permeability \\(k\\) increases while everything else remains constant, would you expect the pressure drop required to sustain the same production rate to increase or decrease?



Don't worry about the equation yet.



Deliverable



Create a Markdown table in the notebook:



Symbol	Meaning	Value	Unit	Type

\\(k\\)	?	?	?	Reservoir

\\(h\\)	?	?	?	?

\\(\\phi\\)	?	?	?	?

...				



This becomes the documentation for your synthetic experiment.



Task 4 — Generate the pressure response



This is the main engineering portion.



Instead of arbitrarily saying:



$$ p=3000-50\\log(t) $$



we'll generate pressure using the Line Source Solution.



Eventually you'll encounter the exponential integral:



$$ Ei(x) $$



and SciPy provides the function:



scipy.special.expi()



But I'm deliberately not giving you the complete pressure equation yet.



Your research questions



Before implementing anything, figure out:



Q1. What physical problem does the Line Source Solution represent?



Q2. Why is it useful in well testing?



Q3. What is the dimensionless time \\(t\_D\\)?



Q4. Why does permeability affect the pressure response?



Q5. What assumptions are made in the simplest line-source model?



For example, does it assume constant rate? Infinite reservoir? Single-phase flow?



Deliverable



Write a short Markdown section in your notebook:



Line Source Solution



followed by about 4–6 sentences in your own words explaining what you're about to calculate.



Then we'll implement the equation together.



Task 5 — Convert the results into a Pandas DataFrame



Once you have:



time\_hr

pressure\_psi



you'll turn them into a DataFrame.



Questions



Q1. What is a Pandas DataFrame?



Q2. Why might it be preferable to keeping two independent NumPy arrays?



Q3. What does df.head() show?



Q4. What does df.info() tell you?



Q5. What does df.describe() tell you?



Deliverable



Your table should resemble:



Time\_hr	Pressure\_psi

0.0010	...

0.0011	...

0.0013	...

...	...



Then use Python to answer:



How many rows are present, and what are the minimum and maximum pressure values?



Task 6 — Intentionally damage your dataset



This part is important.



Perfect synthetic data doesn't teach you much about data cleaning.



So I will have you intentionally introduce problems such as:



negative time

zero time

missing pressure

missing time



For example, after corruption you might have:



Time\_hr	Pressure\_psi

0.001	2998

\-0.01	2997

0	2995

0.1	NaN

NaN	2980

Your problem



Without manually deleting rows, write Python that identifies these bad measurements.



Questions



Q1. How can Pandas detect NaN?



Q2. How can you count the number of missing values?



Q3. How can you select only:



$$ t>0 $$



?



Q4. Why specifically must \\(t>0\\) for a logarithmic time axis?



Deliverable



Show:



Rows before cleaning: \_\_\_

Missing values found: \_\_\_

Invalid time values found: \_\_\_

Rows after cleaning: \_\_\_



That is much more convincing than simply writing dropna().



Task 7 — Plot pressure vs. time



Now use Matplotlib.



Your figure must have:



x-axis label

y-axis label

title

grid

sensible line/marker representation

Questions



Q1. Which variable belongs on the x-axis?



Q2. Which belongs on the y-axis?



Q3. For a drawdown test, should pressure generally rise or fall?



Q4. Why should every engineering plot include units?



Deliverable



Figure 1 — Pressure vs. Time



Then write 2–3 sentences underneath interpreting your figure, not merely describing it.



For example, don't just say:



"Pressure decreases."



Think about why pressure changes with production time.



Task 8 — Build the log-log plot



This is the main Week 1 plotting deliverable.



But I want you to encounter an important issue yourself.



Try thinking about plotting:



$$ p(t) $$



versus time on logarithmic axes.



Then consider whether absolute pressure is really the most useful quantity for PTA.



Questions



Q1. What does plt.loglog() do?



Q2. Why can't \\(t=0\\) appear on a logarithmic axis?



Q3. What is pressure change?



$$ \\Delta p = ? $$



Q4. If



$$ p\_i=3000\\text{ psi} $$



and



$$ p(t)=2875\\text{ psi}, $$



what is \\(\\Delta p\\)?



Q5. Which is more meaningful for PTA:



$$ p $$



or



$$ \\Delta p $$



on the diagnostic plot?



Deliverable



Produce:



Figure 2 — Pressure Change vs. Time on Log-Log Axes



This starts bringing your notebook closer to a real PTA workflow.



Task 9 — Explain what your plot means



No Python for this part.



Answer these in Markdown:



What happens at early time?

What happens at later time?

Why is a log scale useful?

What information is difficult to see on a linear plot but easier to see logarithmically?

What additional quantity would we need for a standard pressure-derivative diagnostic plot?



The answer to #5 leads directly into Week 2: Bourdet derivative.



Task 10 — Final Week 1 deliverable



Your notebook should ultimately have this structure:



WEEK 1 — SYNTHETIC WELL-TEST DATA



1\. Objective



2\. Import Libraries



3\. Generate Time Array

&#x20;  - 100 logarithmically spaced points



4\. Define Reservoir/Fluid Parameters

&#x20;  - parameter table



5\. Generate Synthetic Pressure

&#x20;  - Line Source Solution



6\. Create Pandas DataFrame

&#x20;  - head()

&#x20;  - info()

&#x20;  - describe()



7\. Data Quality Exercise

&#x20;  - introduce bad data

&#x20;  - detect problems

&#x20;  - clean data



8\. Visualization

&#x20;  - pressure vs time

&#x20;  - pressure change vs time

&#x20;  - log-log plot



9\. Interpretation

&#x20;  - explain physical behavior



10\. Export Clean Dataset

&#x20;  - week1\_synthetic\_welltest.csv



11\. Conclusions

&#x20;  - What I learned

&#x20;  - Problems encountered

&#x20;  - What comes next

Your final project checklist



By the end, you should be able to say yes to:



I understand np.logspace.

I can create and manipulate NumPy arrays.

I can create a Pandas DataFrame.

I can inspect a dataset.

I can identify missing values.

I can remove physically invalid time values.

I can make linear plots.

I can make log-log plots.

I understand the difference between \\(p\\) and \\(\\Delta p\\).

I understand at a basic level where my synthetic pressure data came from.

I can export my cleaned dataset.



That is a much stronger Week 1 deliverable than simply showing someone a log-log plot.

