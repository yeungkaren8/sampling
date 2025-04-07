# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Karen Yeung

```
Sampling in the model:

Sampling occurs twice in the model: 1) during primary contact tracing, and 2) during secondary contact tracing. 

Primary contact tracing was conducted with the following code:
    ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS
This was a simple random sampling conducted on the sample frame of all infected people, as people within this frame were randomly chosen to be traced. The sample size was 20% of the total number of infected people, which in the case of this simulation was 10% of the population (0.2*0.1*1000), i.e. 20 people. Within this sample frame, infections from weddings and infections from brunches would follow a normal distribution, with wedding infections centering around 20% and brunch infections centering around 80%.

Secondary contact tracing was conducted with the following code:
    event_trace_counts = ppl[ppl['traced'] == True]['event'].value_counts()
    events_traced = event_trace_counts[event_trace_counts >= SECONDARY_TRACE_THRESHOLD].index
    ppl.loc[ppl['event'].isin(events_traced) & ppl['infected'], 'traced'] = True
This was a judgement sampling conducted on events in which the secondary trace threshold (2 events) was reached, wherein 100% of infected individuals were sampled in events where 2 infections were indepently traced to the same source event. The sampling frame was everyone who attended an event in which the secondary trace threshold was reached, and the sample size is the # of infected people who went to the traced event. This sampling method tends to overestimate the proportion of cases coming from weddings, as all infected individuals coming from a secondary trace-linked event will be sampled.

The provided Python script file does not appear to reproduce the graphs from the original blog posts as the observed infections traced to weddings appears to be centered near the true proportion of infections coming from weddings. This might be due to the high number of simulations ran (1000) or the way events were designated in this script (800 people all attending 1 brunch, instead of 800 people attending 80 separate brunches).

After modifying the number of repetitions in the simulation to 100, the output graphs became less reproducible over multiple runs as the smaller number of repetitions means that there will be a greater variation in the histogram distribution from the true distribution. 

To make the code more reproducible, I added the line of code:
 > np.random.seed(123)
before running the simulation to set a fixed seed such that the random unmber generation generates the same set of random numbers with every iteration. This makes the results of the simulation reproducible across different runs, making the output graphs similar across runs.

```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 09/04/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
