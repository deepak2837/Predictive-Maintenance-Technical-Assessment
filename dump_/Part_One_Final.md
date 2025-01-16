# Remote Quantitative Developer Assessment Test

**Candidate's Name:** Gayatri Masu

---

## PART ONE

**1. Are you currently employed?**

Yes, I'm currently working as a Business Analyst at PNC Bank here in the US. I've been in this role since August 2024, primarily supporting enterprise reporting, data validation, and analytics across multiple business units.

---

**2. How many hours can you devote weekly to the Company?**

I can commit to a full-time 40-hour week. I'm also happy to put in extra hours when project deadlines or month-end/quarter-end reporting cycles call for it.

---

**3. Why do you think you fit the role well? Please provide examples of your skills and experiences that align with the requirements of this role.**

I think I'm a good fit because my day-to-day work already overlaps with a lot of what this role asks for:

- At PNC Bank, I work with millions of banking records using Python (pandas, NumPy) and SQL daily — preprocessing data, running validations, and building out KPI logic.
- I've applied machine learning clustering to surface trends in that data, which helped boost our SLA (Service Level Agreement) adherence by 18%.
- On the quantitative side, I've used statistical regression and linear algebra models to do root cause analysis on 50+ data variances across source systems.
- I've led 20+ cross-functional requirement workshops where I translated business needs into actual functional specs and code logic.
- My Master's in Information Systems with a Business Analytics specialization from Saint Louis University gave me a solid foundation in statistics, modeling, and data engineering.

So the combination of hands-on Python/SQL work, quantitative analysis, and being able to communicate across technical and non-technical teams is what I'd bring to this role.

---

**4. Have you worked from home before?**

Yes. I've worked in hybrid mode at both CVS Health and PNC Bank, collaborating with cross-functional teams across the US and rest of the countries  — including data engineering, compliance, operations, and finance. 
I stay organized using JIRA, Confluence, Git, AWS, Azure, Microsoft Teams, Zoom, Slack, and Power BI. Remote work has always been productive for me.

---

**5. What interests you about this role?**

I genuinely love working with data — the bigger the dataset, the more exciting it gets for me. I believe data is a game changer in pretty much in  everything, whether it's improving operations, making smarter business decisions, or uncovering patterns nobody expected. I like to dig deep into it.

This role excites me because it combines that passion with quantitative problem-solving and real software development. The chance to build analytical models, work on optimization problems, and develop data pipelines that actually drive business impact — that's exactly where I want to be in my career.

---

**6. What motivates you?**

I'm naturally curious — I'm the kind of person who won't stop until I find the root cause. At CVS Health, I made Python-automated reporting pipelines that cut manual effort by 30%, and seeing that direct impact on the team's daily life made me to make more such pipelines .

What really keeps me going is that feeling when you crack a tough data problem after hours of digging — when the puzzle finally clicks. I'm also motivated by staying ahead of the curve. Right now, I'm really fascinated by how agentic AI is changing data analysis — the idea of building intelligent systems that can autonomously explore data, surface insights, and adapt their approach is something I actively follow and experiment with.


---

**7. Are you seeking employment in a company of a specific size, such as a small startup, medium-sized Company, or large corporation? Please explain your preference.**


What matters more to me is the work itself , unless or untill i am happy to do what align to my role i would be happy to work in any company size.
 At the end of the day, I'm driven by building things that make a real difference, and constantly learning while doing it. But I also appreciate smaller and mid-sized companies where you get to own things end-to-end — from writing the code to seeing it deployed.

---

**8. Describe your experience working with a culturally diverse group of people.**

 it's one of the best parts of my career so far. Working with people from different backgrounds is fun — you pick up completely different ways of thinking about the same problem, and that always leads to better solutions.

I started my career at CVS Health in India and then moved to the US to work at PNC Bank, so I've experienced both sides of that cultural bridge. At CVS, I coordinated across business and technical teams spread across multiple locations.

Beyond work, some of my closest friendships at PNC came from this diversity. We'd go trekking on weekends, grab food from each other's cultures, and just enjoy time together outside the office. That kind of bonding naturally made us a stronger, more connected team at work too.

---

**9a. Write a Python function that takes an array of real numbers and returns the maximum drawdown. Include a test case. Explain the time and space complexity of your solution above.**


**Python Solution:**

```python
def calculate_maximum_drawdown(historical_prices):
    """
    Computes the maximum drawdown from a list of historical prices.
    Uses a single-pass algorithm — O(n) time, O(1) space.
    """
    if not historical_prices or len(historical_prices) < 2:
        return 0.0

    running_peak_price = historical_prices[0]
    maximum_drawdown_observed = 0.0

    for current_price in historical_prices:
        # Update the running peak if current price exceeds it
        if current_price > running_peak_price:
            running_peak_price = current_price

        # Calculate percentage decline from the peak
        current_drawdown_percentage = (running_peak_price - current_price) / running_peak_price

        # Track the worst drawdown seen so far
        if current_drawdown_percentage > maximum_drawdown_observed:
            maximum_drawdown_observed = current_drawdown_percentage

    return maximum_drawdown_observed


# Test Case
historical_test_prices = [100, 120, 115, 130, 90, 95, 140]
computed_max_drawdown = calculate_maximum_drawdown(historical_test_prices)
print(f"Maximum Drawdown: {computed_max_drawdown:.4f}")
# Output: Maximum Drawdown: 0.3077
# Explanation: Peak is 130, subsequent trough is 90
# Drawdown = (130 - 90) / 130 = 30.77%
```

**Complexity:**
- **Time Complexity:** O(n) — single pass through the array. No nested loops, no sorting, no extra traversals. Each element is visited exactly once.
- **Space Complexity:** O(1) — only three scalar variables (`running_peak_price`, `maximum_drawdown_observed`, `current_drawdown_percentage`) are used regardless of input size. No auxiliary arrays or data structures.

---

**9b. Given a large dataset of price ticks (timestamp, price), how would you efficiently compute the moving average for every 5-minute window? Provide a brief algorithm or code snippet.**

For large streaming tick data, the most efficient approach is a **sliding window** with a running sum. Instead of recalculating the entire sum from scratch every time a new tick arrives, we simply add the new price and remove any prices that have fallen outside the 5-minute window. This keeps the computation constant per tick.

**How it works:**
- A `deque` (double-ended queue) holds the active window of ticks — it allows O(1) removal from both ends.
- A running sum tracks the total price in the window — so the average is always just one division away.
- As each new tick comes in, we expire old ticks from the front that are older than 5 minutes.

```python
from collections import deque

# 5-minute window in seconds
FIVE_MINUTE_WINDOW_IN_SECONDS = 300

def compute_five_minute_moving_average(price_tick_data):
    """
    Computes the 5-minute moving average for streaming price ticks.
    Each tick is a tuple: (timestamp_in_seconds, tick_price)
    
    Uses a sliding window with a running sum to avoid
    recalculating the full sum on every tick.
    
    Time:  O(n) total, O(1) amortized per tick
    Space: O(w) where w = number of ticks in a 5-min window
    """
    
    # Holds the current window of active ticks
    active_window_deque = deque()
    
    # Running total of prices in the current window
    running_price_sum = 0.0
    
    # Store the computed averages
    moving_average_results = []

    for current_timestamp, current_tick_price in price_tick_data:
        
        # Add the new tick to the right side of the window
        active_window_deque.append((current_timestamp, current_tick_price))
        running_price_sum += current_tick_price

        # Remove all ticks from the left that are older than 5 minutes
        # This ensures only ticks within the window remain
        while current_timestamp - active_window_deque[0][0] > FIVE_MINUTE_WINDOW_IN_SECONDS:
            expired_timestamp, expired_price = active_window_deque.popleft()
            running_price_sum -= expired_price

        # The average is simply the running sum divided by active tick count
        current_window_size = len(active_window_deque)
        current_moving_average = running_price_sum / current_window_size
        
        moving_average_results.append((current_timestamp, current_moving_average))

    return moving_average_results
```

**Why this is optimal:**
- **Time: O(1) amortized per tick** — each tick enters the deque once and leaves at most once. No tick is processed more than twice, so across all n ticks the total work is O(n).
- **Space: O(w)** — where w is the number of ticks that fit within a 5-minute window. We only store what's active, nothing historical.
- **No redundant computation** — we never re-sum the entire window. The running sum updates incrementally with each add/remove.

---

**10a. Suppose you have two uncorrelated assets, A and B, with expected returns of 5% and 7%, and standard deviations of 10% and 15%, respectively. What is the expected return and variance of a portfolio with 60% in A and 40% in B?**

**Given:**
- Asset A: Return = 5%, Risk (SD) = 10%, Weight = 60%
- Asset B: Return = 7%, Risk (SD) = 15%, Weight = 40%
- The two assets are uncorrelated (they don't move together)

**Step 1 — Expected Return:**

<!-- Simply a weighted average of both returns -->

E(R_p) = (0.60 × 0.05) + (0.40 × 0.07)

<!-- 60% of A's return + 40% of B's return -->

E(R_p) = 0.03 + 0.028 = **0.058 or 5.8%**

<!-- So if you put 60% in A and 40% in B, you'd expect a 5.8% return overall -->

**Step 2 — Portfolio Variance:**

<!-- Since the assets are uncorrelated, we don't need to worry about covariance — it's zero -->

Var(R_p) = (weight_A² × SD_A²) + (weight_B² × SD_B²)

<!-- Plug in the numbers -->

Var(R_p) = (0.60² × 0.10²) + (0.40² × 0.15²)

Var(R_p) = (0.36 × 0.01) + (0.16 × 0.0225)

Var(R_p) = 0.0036 + 0.0036 = **0.0072**

<!-- The variance tells us how spread out the portfolio's returns could be -->

**Step 3 — Portfolio Standard Deviation (Risk):**

SD = √0.0072 = **8.49%**

<!-- Even though Asset B alone has 15% risk, combining them brings the overall portfolio risk down to 8.49% — this is the benefit of diversification -->

---

**10b. Describe how you would test whether a financial time series is stationary. What steps would you take if it is not?**

I'd approach this in two stages — first a visual check, then formal statistical testing.

**Visual Inspection:**
I'd start by plotting the series and looking at rolling mean and rolling standard deviation over time. If either shows a clear trend or changing variance, that's a quick indicator of non-stationarity.

**Statistical Tests:**
- **Augmented Dickey-Fuller (ADF) Test:** This is the go-to. The null hypothesis assumes a unit root (non-stationary). If the p-value comes in below 0.05, I'd reject the null and conclude the series is stationary.
- **KPSS (Kwiatkowski-Phillips-Schmidt-Shin) Test:** I'd also run this as a confirmation, since it tests the opposite hypothesis (null = stationary). Using both together gives a more reliable picture.

**If the series is not stationary, I would apply:**
1. **Differencing** — subtract the previous observation from the current one to stabilize the mean.
2. **Log transformation** — helps stabilize the variance, especially for price data that grows exponentially.
3. **Seasonal differencing** — if there's a seasonal pattern in the data.
4. **Fractional differencing** — this is useful when you want stationarity but also want to retain as much memory of the original series as possible.

After each transformation, I'd re-run the ADF/KPSS tests to confirm stationarity before proceeding. This step is critical because models like ARIMA require stationary input to produce reliable forecasts.

---
---

## PART TWO

**1. Write the Black-Scholes formula for pricing a European call option. What are the key assumptions behind this model?**

The Black-Scholes formula for a European call option (C) is:

**C = S₀ × N(d₁) − K × e⁻ʳᵀ × N(d₂)**

Where:

- **d₁ = [ln(S₀ / K) + (r + σ²/2) × T] / (σ × √T)**
- **d₂ = d₁ − σ × √T**

<!-- Breaking down what each variable means -->

- **S₀** = Current stock price
- **K** = Strike price of the option
- **r** = Risk-free interest rate
- **T** = Time to expiration (in years)
- **σ** = Volatility of the underlying asset
- **N()** = Cumulative standard normal distribution function

**Key Assumptions:**

- Stock prices follow a lognormal distribution
- The risk-free rate (r) and volatility (σ) remain constant over the option's life
- No dividends are paid during the option's life
- The option can only be exercised at expiration (European style — not early)
- Markets are frictionless — no transaction costs, no taxes
- Continuous trading is possible, including fractional shares
- No arbitrage opportunities exist



---

**2. Share a challenging technical issue you've encountered and how you resolved it, focusing on your analytical and critical thinking skills.**

One of the trickiest problems I dealt with was at PNC Bank during our Q3 month-end close cycle on a project we internally called **"Enterprise KPI Consolidation"** — basically an initiative to unify reporting across 6 business units into a single Power BI dashboard suite called InsightBridge

The problem? Different teams — operations, compliance, and finance — were pulling the same KPIs (things like *Average Turnaround Time*, *SLA Breach Rate*, and *Defect Density*) but getting different numbers. Leadership was getting two conflicting versions of the same monthly performance snapshot, and trust in the data was eroding fast.

**What I did:**

- I started by mapping the full data lineage — tracing every metric from the source tables in our SQL Server warehouse, through the ETL (Extract, Transform, Load) jobs running on Azure Data Factory, all the way to the final Power BI reports.
- I wrote Python validation scripts using pandas to compare the transformation logic at each stage — essentially building a "reconciliation layer" that flagged where numbers started diverging.
- I ran SQL queries to cross-check aggregation logic between the operations and finance reporting tables — and that's where things got interesting.
- I used statistical regression to quantify the magnitude of each variance across 50+ discrepancies.

**What I found:**

The root cause turned out to be embarrassingly simple but deeply buried — the operations team was calculating *SLA Breach Rate* using calendar days, while the finance team was using business days. On top of that, three ETL jobs had slightly different NULL-handling rules, so certain records were being dropped in one pipeline but included in another. None of this was documented anywhere.

**The result:**

- I created a shared Python module  that codified every metric formula — stored in Git so any change was version-controlled and reviewable
- Built an automated validation framework that ran nightly and flagged any cross-pipeline discrepancies before they hit the reports
- Recurring defects dropped by 28%
- The InsightBridge dashboard became the single source of truth, and leadership stopped asking "which number is right?"

This was one of those problems where you really had to be patient and systematic — no shortcuts, just tracing the data step by step until the root cause surfaced. But once we fixed it, the payoff was huge.

---

**3. How do you handle feedback and criticism from supervisors or colleagues?**

I'd rather someone tell me what's not working than let me keep going in the wrong direction. I've worked in Agile setups where feedback is constant — retrospectives, code reviews, demos — so I'm used to it being part of the rhythm.

When I get feedback, I just try to listen properly first, ask questions if something's unclear, and then act on it. I don't take it personally. At the end of the day, it's about making the work better, not about being right.

what i think is feedback is what makes me learn and improve on my mistakes.

---

**4. What is your greatest strength? How will it help you in this role?**

I'd say I have two big strengths that work together — my curiosity to keep learning, and my ability to communicate across technical and non-technical teams.

On the technical side, I'm always exploring new things. Right now, I'm really deep into agentic AI and how it can transform data analytics — the idea of building autonomous agents that can clean, explore, and analyze data without constant hand-holding genuinely excites me. That curiosity is what keeps my skills sharp — whether it's picking up a new Python library, learning a different modeling approach, or figuring out a better way to architect a pipeline.

On the people side, I've led 20+ cross-functional requirement workshops at PNC Bank — sitting with operations, finance, and compliance teams, understanding their pain points, and translating those into functional specs and Python code. That process cut clarification cycles by 30%. I'm comfortable managing stakeholders, presenting insights to leadership, and coordinating across teams.

For this role, this combination means:

- I can talk to portfolio managers or traders and understand what they actually need
- I can translate that into accurate quantitative models and clean code
- I don't need someone to sit in between and explain things back and forth
- And I'll keep pushing to learn and apply newer, better approaches — not just stick with what I already know

That mix of curiosity, communication, and technical depth is what I'd bring here.

---

**5. We want to understand your long-term career goals and how this position aligns with them. Please share your aspirations and how you see this role contributing to your career path.**

I want to eventually grow into a lead quantitative developer — someone who's not just building models but actually shaping how an organization uses data to make decisions.

I've spent the last ~5 years building a strong base in Python, machine learning, and enterprise data systems, but I know there's a lot more I want to learn — quantitative finance, advanced statistical modeling, and building production-grade systems that go beyond dashboards and reports by implementing agentic ai in building these systems.

This role feels like the right next step for me. It's where I get to go deeper into the kind of work I genuinely enjoy, while growing the skills I'll need long-term.

---

**6. What do you know about our Company?**

I first came across Russ Bassett through LinkedIn, and out of curiosity I dug deeper. They design and manufacture mission-critical workstations, consoles, and technical furniture for industries like public safety, military, government, air traffic control, and emergency response — environments where reliability is non-negotiable.

What impressed me is their 60+ years in the business and their focus on ergonomic design and technology integration. 

---

**7. How do you see yourself contributing to our Company's continued success and growth in this role?**

My approach has always been simple — find what's slow or broken, automate it, and make data-driven decisions the default. I'd bring that same energy here.

I want to build scalable data pipelines and analytical models that leadership can actually rely on — not just pretty reports, but systems that improve data quality, catch issues before they reach production, and turn operational data into clear, actionable insights.

I'm not the kind of person who waits to be told what to do. I look for gaps, propose solutions, and execute. That's how I'd contribute to Russ Bassett's growth.

---

**8. In your opinion, what are the biggest challenges facing our Company in the market?**

From what I've read about the mission-critical console space, two challenges stand out the most:

- **The NG911 transition and growing tech complexity** — dispatch centers and command rooms are upgrading to Next Generation 911, which means consoles now need to handle significantly more data feeds, multi-media inputs, GIS mapping, and AI-driven tools all at once. Keeping the infrastructure reliable while integrating all of this is a serious engineering challenge — especially since any downtime in these environments has life-safety consequences.

- **Staffing shortages and operator retention** — there's a well-documented nationwide shortage of 911 telecommunicators, with vacancy rates often above 30%. That puts pressure on companies like Russ Bassett to design workstations that actively reduce fatigue and burnout — better ergonomics, sit-to-stand adjustability, improved acoustics — because the physical workspace directly impacts whether operators stay or leave.

On top of that, cybersecurity is becoming critical as command centers get more connected, and supply chain disruptions for specialized components remain unpredictable. Companies that can combine reliable hardware with smart, data-driven design will be the ones that stay ahead.

---

**9. Discuss your familiarity with recent technological trends and how they can be leveraged within the industry.**

The trend I'm most excited about right now is **Agentic AI** — the idea of building autonomous AI systems that don't just respond to prompts but can independently explore datasets, surface anomalies, and adapt their analysis based on what they find. For an industry like mission-critical operations, this could mean agents that continuously monitor system health, flag risks before operators even notice them, and suggest corrective actions in real time. I actively experiment with this in my own projects.

Beyond that, a few other trends are reshaping how this space operates:

- **Cloud-native infrastructure (AWS, Azure)** is making it possible to process massive datasets faster and scale storage without heavy upfront investment.
- **Digital Twins** — maintaining virtual replicas of physical systems allows organizations to simulate, test, and optimize without disrupting live operations.
- **CI/CD for analytical models** — treating quantitative models like software (version-controlled, tested, deployable) ensures they stay reliable and auditable as they evolve.

The companies that treat these as working systems rather than slide deck buzzwords are the ones that will pull ahead.

---

**10. You are given a new dataset containing minute-level trading data for a new asset class. Describe your approach for cleaning, exploring, and building a predictive model based on this data.**

**Cleaning & Validation:**

First, I'd enforce strict data types and align all timestamps to UTC — timezone mismatches in minute-level data will silently corrupt everything downstream. Then I'd run a Z-score filter or Median Absolute Deviation (MAD) to catch bad ticks — MAD handles fat-tailed financial distributions better than simple percentile cutoffs. For missing data, I'd forward-fill micro-gaps under 5 minutes and flag longer gaps for review. Finally, I'd deduplicate any repeated timestamp-price pairs and verify the series is in monotonic order.

**Exploration (EDA):**

Before touching any model, I'd run ADF and KPSS stationarity tests to decide whether I need to difference or log-transform the series. I'd plot ACF/PACF to check for autocorrelation — this tells me if an ARIMA-style approach makes sense. I'd also test for GARCH effects to see if there's volatility clustering, and compute realized volatility, the Garman-Klass estimator, and rolling Sharpe to understand the asset's risk profile. I'd visualize intraday patterns too — most assets behave differently at market open versus close, and catching that early shapes the feature engineering.

**Model Development:**

For features, I'd engineer lagged returns, RSI, Bollinger Band width, VWAP deviation, momentum signals, and rolling skewness/kurtosis. I'd split the data strictly by time — walk-forward validation with expanding windows, never random splits for time-series. I'd start with an XGBoost classifier as the baseline for direction prediction since it handles non-linearity well and gives feature importance out of the box. If the baseline shows signal, I'd escalate to LightGBM for speed or LSTM for sequence-dependent patterns. For evaluation, I wouldn't rely on accuracy alone — I'd use precision/recall, log-loss, and backtest with the Sharpe Ratio and maximum drawdown to check real-world viability.

**Production & Monitoring:**

Once the model is performing, I'd deploy it with versioned artifacts and set up automated retraining on a schedule. I'd monitor for concept drift using Population Stability Index (PSI) — financial regimes shift constantly, and a stale model is worse than no model at all.

---
---

## PART THREE

**1. What tools or strategies are most effective for communication and collaboration in a remote work environment?**

For me it comes down to three things — clear communication, good documentation, and the right tools.

I used JIRA for sprint planning and defect tracking, Confluence for maintaining data dictionaries and project documentation, and Git for version control so the whole team stays aligned on the codebase. For day-to-day communication, Microsoft Teams, Zoom, and Slack keep things moving.

 the tools only work if our habits are right. I make sure to over-communicate progress — quick async updates, clear standup notes, and documenting decisions so nobody has to guess what was agreed on. That consistency is what makes remote work actually work.

---

**2. If you were hiring for this role, which key personality traits and qualities would you look for?**

I'd look for someone who's genuinely curious — not just technically skilled, but someone who asks "why" and digs deeper into problems instead of stopping at the surface.

Beyond that:
one should have good analytical thinking 
one should adaptable 
one should take ownership of his/her work  
one should have  relevant experience alignment 
---

**3. How do you approach problem-solving as a team member versus an individual contributor?**

As an individual contributor, I go deep. I take personal ownership of the problem — whether it's debugging a data pipeline or running root cause analysis on a set of variances. That's where focused, uninterrupted analytical work matters most.

As a team member, my approach shifts to alignment and transparency. I make sure everyone's on the same page about what we're building and why. At PNC, this looked like leading requirement workshops, standardizing reporting processes with the team, and making sure our shared codebases stayed clean and well-documented.

Both modes require ownership — the difference is just whether I'm owning the solution alone or owning the collaboration.

---

**4. How do you prioritize and manage your workload in a fast-paced environment?**

I prioritize based on business impact and deadlines — not everything that's urgent is actually important, and knowing the difference matters.

I work in Agile sprints, so my backlog is always organized in JIRA with clear priorities. When things get hectic, I break large tasks into smaller milestones so I can show progress and catch issues early. I also make a habit of flagging blockers immediately rather than waiting — it's better to raise a concern early than to miss a deadline silently.

The key for me is staying organized without being rigid. Plans change, and I'm comfortable reprioritizing on the fly when business needs shift.

---

**5. What specific skills or experiences do you hope to gain from this role to help you achieve your career goals?**

I've built a solid foundation in data analytics and pipeline development, but I want to go deeper. This role gives me the chance to work on real quantitative financial models — pricing, risk, optimization — and build systems that are production-grade, not just prototypes. That's the direction I want my career to go, and this is the right place to get there, i want to play with more quantitive and high frequency data sets 

---

**6. Imagine that you told a client you would be there at 10 am. It is now 10:30 am, and you won't finish your job until 11:30. You have a lunch meeting with another client at noon, followed by another job at 1:15 pm. How would you handle this situation?**

First thing — I'd call the 10 am client immediately. No waiting, no hoping they won't notice. I'd apologize, explain honestly that I'm running behind and won't finish until 11:30, and give them the option to reschedule if that doesn't work for them.

Then I'd look at the rest of my day realistically. If finishing at 11:30 makes the noon lunch tight, I'd proactively call the noon client and ask to push it back 15 minutes — most people appreciate the heads-up rather than you showing up flustered and late.

I'd keep the lunch brief and focused to make sure I'm at the 1:15 pm appointment on time. The goal is to be upfront with everyone — people can handle delays, but they can't handle surprises.

---

**7. In your experience, how frequently does the following problem occur: employees being afraid to express disagreement with their managers?**

It happens a lot — more than people admit. I've seen it in both India and the US. People hold back because they don't want to seem difficult or risk being on the wrong side of their manager.
Personally, I generally  speak up — respectfully, but clearly. The trick is framing it around data, not ego,The best teams I've worked on are the ones where pushing back is normal, not risky.

---

**8. Your experience working on projects involving a consortium of companies is valuable. Please briefly share your experience in this area.**

At both CVS Health and PNC Bank, I worked in heavily matrixed environments where coordinating across multiple groups was everyday life — not a one-off thing.

At PNC, the best example was the Enterprise KPI Consolidation project I mentioned earlier. I had to bring together operations, finance, compliance, and engineering — and honestly, the hardest part wasn't the technical work. It was getting four teams to agree on what "SLA Breach Rate" even meant. Everyone had their own version. I led 20+ requirement workshops where we sat in a room (or a Zoom call), hashed out definitions, and aligned on a single source of truth. It was messy at times, but that's how cross-team work actually goes.

At CVS in India, it was a similar dynamic — I coordinated between business users, compliance teams, and engineers while navigating HIPAA regulations. What I learned is that when multiple groups with competing priorities need to deliver together, the person who documents clearly, communicates proactively, and doesn't take sides becomes the glue that holds it together. That was my role, and I genuinely enjoyed it.

---

**9. How much would you request per hour?**

I'm looking at $45–$60 per hour, depending on the full scope of responsibilities, project complexity, and any benefits included. I'm open to discussing what works best for both sides.

---

**10. When would you be available to start if hired?**

I'd need to give a standard two-week notice at PNC Bank to wrap up my current projects and transition smoothly. After that, I'd be fully available to start.

---
---

## TECHNICAL EXERCISE

**Notebook:** Technical_Exercise_Gayatri_Masu.ipynb (attached)

**Dataset:** AI4I 2020 Predictive Maintenance Dataset

**Source:** Kaggle (UCI Machine Learning Repository)
https://www.kaggle.com/datasets/stephanmatzka/predictive-maintenance-dataset-ai4i-2020

**Downloaded Data File:** ai4i2020.csv (10,000 rows × 14 columns)

**Problem:** Built a classification model to predict machine failures from real-time sensor data — enabling proactive maintenance scheduling and reducing unplanned downtime in mission-critical environments.

**Approach:**

- Performed comprehensive EDA with 8 visualizations covering failure distributions, sensor correlations, and product quality analysis
- Engineered 4 domain-informed features: Temperature Difference, Mechanical Power, Torque-to-Speed Ratio, and Wear-Torque Interaction
- Trained and compared 4 models: Logistic Regression, Random Forest, Gradient Boosting, and XGBoost using Stratified 5-Fold Cross-Validation

**Results:**

- XGBoost achieved the best performance with ROC-AUC of 0.986
- Top failure drivers identified: Torque-Speed Ratio, Mechanical Power, and Tool Wear
- Engineered features outperformed raw sensor readings — confirming the value of domain-informed feature engineering

**Key Recommendations:**

- Monitor torque and rotational speed in real-time with automated threshold-based alerts
- Implement condition-based tool replacement policies instead of fixed schedules
- Adjust inspection frequency by product quality grade — low-quality products show significantly higher failure rates

All code, outputs, visualizations, and a detailed summary report are included in the attached Jupyter notebook. The notebook can be viewed directly without execution — all outputs are pre-rendered.
