---
title: "Module 3: ML Model Deployment and Monitoring"
date: 2026-02-13
series: "International Programme on AI Evaluation: Capabilities and Safety"
---

## Overview

This lecture covered the practical realities of putting machine learning models into production and keeping them working over time. The focus was on deployment strategies, why models degrade after deployment, and comprehensive monitoring approaches for both model performance and system health.

This lecture was taught by [Cèsar Ferri](https://www.upv.es/ficha-personal/ceferri) from the Technical University of Valencia.

---

## Key Takeaways & Concepts
1. Unlike traditional software, ML models degrade from day one in production and need constant monitoring
2. Choose deployment strategies (server-side, client-side, federated) based on your connectivity, privacy, and resource constraints
3. Monitor both what your model does (functional) and how your systems perform (operational)
4. Data drift is inevitable, so set up automated detection and response before it hurts your business
5. Test new models safely using shadow testing or A/B testing before full deployment

- **Server-side deployment**: Run models on central servers or cloud
- **Client-side deployment**: Run models locally on user devices  
- **Federated learning**: Hybrid approach splitting model across server and clients
- **Data drift**: When input data changes from training distribution
- **Model drift**: When the relationship between inputs and outputs changes
- **Shadow testing**: Run old and new models in parallel to compare
- **Functional monitoring**: Track model inputs, outputs, and performance
- **Operational monitoring**: Track system resources and infrastructure health

---

## Detailed Notes

### ML Model Lifecycle

Machine learning models behave very differently from traditional software once deployed. A banking system might run the same code for 30+ years without issues, but ML models start losing performance immediately after deployment.

<div class="example-box" markdown="1">
**Supermarket Sales Prediction** <br>
Imagine you build a model to predict milk sales for a chain of supermarkets. It works great in testing, but once deployed:
- Customer preferences change seasonally
- New competitors open nearby stores  
- Economic conditions shift buying patterns
- The model's predictions become less accurate each month
</div>

This degradation happens because ML models operate in dynamic environments where the patterns they learned during training evolve over time. From the very first day you deploy a model, you'll typically see performance start to decline.


### Deployment Strategies

**Server-Side Deployment**
This is the easiest approach for ML engineers. Your model runs on a central server or cloud instance, and applications make API calls to get predictions. You maintain full control over the model, can easily update it, and have good monitoring capabilities.

The downside is network dependency. Every prediction requires an internet connection, and you're limited by network latency.

**Client-Side Deployment**  
Sometimes you need to run models directly on user devices or edge locations. This works well when internet connectivity is unreliable or when privacy regulations prevent sending data to external servers.

The tradeoffs are significant: limited computational resources, harder to update models, and reduced monitoring capabilities.

**Federated Learning**
This hybrid approach splits model execution between server and client. You can keep sensitive data local while still benefiting from centralized model updates. It's more complex to implement but helps with privacy compliance and resource optimization.

<div class="example-box" markdown="1">
**Supermarket Deployment Comparison**: <br>
Your supermarket chain has locations with varying internet connectivity and different privacy concerns: <br>
**Server-side**: All stores call your central server for predictions. Works great for well-connected urban stores, but rural stores can't make predictions when internet fails.<br>
**Client-side**: Each store runs its own local model. Rural stores can operate offline, but models become outdated since they can't learn from other stores' data.<br>
**Federated**: Each store runs a local model for offline predictions, but when connected, they share learning updates (not raw data) with the central server. The server combines insights from all stores and sends improved models back. Rural stores get offline capability plus collective learning benefits.
</div>

### Why Monitoring Matters

The goal of monitoring is to catch problems before they hurt your business. Without proper monitoring, a degrading model might cause you to overstock products (wasting money) or understock them (losing customers).

Effective monitoring should detect performance drops automatically and provide actionable alerts before business impact. You can make a distinction between **funtional** and **operational** monitoring. 

### Functional Monitoring

Functional monitoring covers everything related to your model's inputs, outputs, and performance. It's the most critical type of monitoring because it directly affects your business outcomes.

**Data Quality**
Your model was trained on clean, well-formatted data that was carefully preprocessed. But production data comes from real users and systems that may challenge your preprocessing rules. Therefore, we need to automatically check for problems like missing values, data type mismatches, and values outside expected ranges.

<div class="example-box" markdown="1">
**Production Data Problems** <br>
Your supermarket sales model was trained on historical data where product prices were always positive numbers. In production, a cashier accidentally scans an item twice and then voids it, creating a negative price entry. This wasn't in your training data, but should be handle immediately (reject the data, send an alert) and then update the preprocessing steps to handle voids properly in the future.
</div>

**Outlier Detection**
Beyond basic data quality issues, you need to watch for outliers. These are data points that fall outside your expected distributions. These can break your model's predictions even if they're technically "valid" data.

<div class="example-box" markdown="1">
**Outlier Impact Example** <br>
Your supermarket model expects milk prices between $2-6 per gallon. A data entry error creates a $200 milk price. While this passes basic validation (it's a positive number), it's an outlier that could skew your sales predictions dramatically.
</div>

You can detect outliers using unsupervised learning methods like clustering to categorize normal vs. anomalous inputs. It's especially important to identify which features in your model are most sensitive to outliers, so you can focus monitoring efforts there.


**Data Drift**
Data drift occurs when your input data distribution changes from what the model saw during training. This is one of the most common causes of model performance degradation. Data drift can be detected using statistical tests that compare current data distributions to your reference training data.

<div class="example-box" markdown="1">
**Data Drift in Customer Behavior** <br>
The sales prediction model was trained on pre-COVID shopping patterns. After the pandemic, people shop differently - more bulk buying, different product preferences, changed shopping frequencies. The input data distribution has shifted significantly from the training data.
</div>


When you detect significant drift, you have several options:
* Send alerts to the model owner
* Automatically trigger model retraining with recent data
* Weight recent data more heavily in retraining
* Temporarily flag the model as unreliable

**Model Performance Evaluation**
Ideally, you'd monitor performance over time, but comparing your model's predictions to actual outcomes isn't always possible or immediate. Instead, you can use proxy metrics that correlate with success, compare prediction distributions over time or wait for delayed feedback (if the delay is reasonable).

<div class="example-box" markdown="1">
**Supermarket Feedback Challenge** <br>
Your sales prediction model forecasts you'll sell 100 gallons of milk tomorrow, so you order that amount. But you can't immediately tell if the prediction was accurate - if you sell out by noon, was the prediction too low, or did you just have unexpectedly high demand? If you have leftover milk, was the prediction too high, or was it a slow day for other reasons?
</div>


**Model Drift Types**
Model drift happens when the relationship between inputs and outputs changes, not just the input data itself. Even if your data looks the same, the underlying patterns your model learned may no longer hold.

*Instantaneous drift* occurs when the relationship between features and outcomes suddenly breaks. For example, your model learned that rainy weather increases umbrella sales, but a new competitor opens next door and suddenly rainy days don't predict your umbrella sales anymore - customers go there instead.

*Gradual drift* happens when there are slow changes in how inputs relate to outputs. Your model learned that Friday evenings have high ice cream sales, but over time, health trends shift and people gradually buy less ice cream on weekends while weekday sales stay stable.

*Temporary drift* are short-term disruptions in input-output relationships. A viral social media trend temporarily makes a product popular among a completely different demographic than usual, breaking your normal customer behavior patterns for a few weeks.

**Online Learning for Frequent Changes**
When model drift happens very frequently - faster than you can retrain and redeploy models - you might need online learning. This approach continuously updates your model as new data arrives, rather than waiting for scheduled retraining cycles.

<div class="example-box" markdown="1">
**High-Frequency Updates** <br>
Your supermarket operates in a rapidly changing market where competitor prices shift daily, seasonal trends change weekly, and customer preferences evolve constantly. Traditional monthly retraining can't keep up, so you implement online learning to adjust predictions in real-time as new sales data comes in.
</div>

**Shadow Testing and A/B Testing**
Part of functional monitoring involves safely evaluating new models before fully deploying them, as you need to monitor how well your updated model performs compared to your current one. Before replacing a production model, you need to verify that your new model actually performs better. *Shadow testing* runs both models in parallel on the same data, but only uses the old model's predictions for business decisions. After collecting enough data, you can statistically compare their performance. *A/B testing* splits your users into control (old model) and treatment (new model) groups. This gives you real-world performance comparisons but requires careful experimental design.

**Adversarial Attacks**
Some applications face deliberate attempts to fool your model. This is especially common in spam filters, credit systems, and fraud detection, where attackers actively try to game your predictions. Defend against these attacks by flagging inputs that look like outliers (attackers often exploit edge cases) and routing suspicious predictions to human review before acting on them. Adversarial robustness toolboxes can help detect these patterns automatically.

<div class="example-box" markdown="1">
**Spam Filter Attack** <br>
Spammers learn that your email filter flags messages with certain keywords. They start using creative misspellings, invisible characters, or images instead of text to bypass your model's detection patterns.
</div>




### Operational Monitoring

While functional monitoring focuses on model behavior, operational monitoring ensures your infrastructure can support that model reliably.

<div class="example-box" markdown="1">
**Cost Control Example**: <br>
Your automated retraining system starts triggering too frequently due to a misconfigured drift threshold. Each training job spins up expensive GPU instances. Without cost monitoring, you might not notice until you get a massive cloud bill at month-end.
</div>

| Monitoring Area | What to Track | Why |
|:---|:---|:---|
| **System Performance** | CPU and GPU usage, memory consumption, API response times, request throughput, system uptime | Ensure your system meets performance requirements and can handle expected load without slowdowns or crashes |
| **Pipeline Health** | External API availability and response times, data pipeline completion rates, software dependency versions, database connection health | Modern ML systems depend on complex data pipelines pulling from multiple sources and any failure breaks your predictions |
| **Cost Management** | Data storage costs, compute resource usage (especially GPU time), API call volumes, training job costs | Cloud-based ML systems can generate surprising bills, especially from automated retraining jobs and GPU usage |


### Best Practices

**Build a Monitoring Culture**
Don't make monitoring one person's responsibility. The entire team should understand that models and data are products that need ongoing care. Centralize your monitoring tools but distribute the responsibility for acting on alerts.

**Start Early**
Begin thinking about monitoring during model development, not after deployment. Consider factors like model complexity, retraining requirements, and interpretability when choosing between candidate models.

**Log Strategically**
Comprehensive logging is essential for debugging and compliance, but be strategic about what you log. Uncontrolled logging can create storage and processing problems that become bigger issues than the original model problems.

**Accept Performance Degradation**
Model performance will decline over time. This is normal, not a failure. What's abnormal is sudden, dramatic performance drops that suggest system problems rather than natural drift.

**Document Everything**
Teams change, and good documentation becomes invaluable when new people need to understand and maintain your systems. Document your monitoring setup, alert thresholds, and response procedures.

## Conclusion

Deployment and monitoring might not be as exciting as model development, but they're what separate successful ML projects from research experiments. Models that work well in Jupyter notebooks often fail in production without proper deployment planning and ongoing monitoring.

The field is still maturing. Many organizations focus heavily on model accuracy during development but neglect the infrastructure needed to maintain that accuracy over time. As the ML industry evolves, deployment and monitoring practices will become as important as the algorithms themselves.

<div class="note" markdown="1">

## Additional Notes

### Deployment Infrastructure Components
- **Model serving**: APIs and endpoints for getting predictions
- **Data pipelines**: Systems for preprocessing input data
- **Evaluation frameworks**: Tools for measuring production performance  
- **Orchestration**: Automation for updates, rollbacks, and scaling
- **Version control**: Tracking models, data, and environment configurations

### Common Monitoring Failures
- **Alert fatigue**: Too many false positives make teams ignore real problems
- **Delayed detection**: Finding problems after significant business impact
- **Incomplete coverage**: Monitoring model performance but ignoring infrastructure
- **Poor documentation**: Teams can't respond effectively to alerts they don't understand

### Drift Detection Tools
- **Statistical tests**: KL divergence, Kolmogorov-Smirnov, chi-square
- **Commercial platforms**: Fiddler AI, Arize, WhyLabs
- **Open source**: Evidently AI, Alibi Detect
- **Custom solutions**: Often needed for domain-specific requirements

### Model Update Strategies
- **Blue-green deployment**: Maintain two identical environments for safe switching
- **Canary releases**: Gradually roll out new models to small user segments
- **Feature flags**: Toggle between model versions without code changes
- **Rollback procedures**: Quick reversion when new models underperform

</div>
