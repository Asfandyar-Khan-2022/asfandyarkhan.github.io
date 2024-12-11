---
title: DAG Scheduling
date: 2024-12-11 08:00:00 -500
categories: [cloud, airflow]
tags: [dag] # TAG names should always be lowercase
---
# <center>Airflow: DAG Scheduling</center>

### What is a DAG run?
As soon as the scheduler starts scheduling the data pipeline, you end up with a DAG run. That DAG run has two things. A <span class="rainbow-text">data interval start</span> and a <span class="rainbow-text">data interval end</span>. By default a DAG has the state queued. As soon as the first task runs. The state of that task becomes running. Once all the tasks are finished running. The state of the DAG becomes success.
> Note: The state of the DAG success or failure is determined by the last task in the DAG.

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/dag_run.png?raw=true){: .shadow .rborder}

### Properites of DAG
state = Final DAG Run's state
dag id = DAG ID of the DAG
logical date = Data interval start
start date = Start timestamp
end date = End timestamp
duration = Time to complete the DAG Run

> Note: The start date is the actual timestamp when the DAG run begins execution, while the logical date (or execution date) represents the scheduled time and data interval for the DAG run.

### How DAGs are scheduled
The two parameters to remember are the <span class="rainbow-text">start_date</span> and the <span class="rainbow-text">schedule_interval</span>. The start_date is the date at which the DAG starts being scheduled. The schedule_interval determined how frequently the DAG is triggered. 

The way the scheduling happens is shown in the example below. The start date is at 10:00. Which means that the DAG starts at that time. Though the task is not triggered until the first scheduled interval. So the task is actually triggered at 10:10. This trend continues for all consecutive DAGs as well.

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/dag_schedule_run.png?raw=true){: .shadow .rborder}

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/dag_trigger.png?raw=true){: .shadow .rborder}

### The start_date

The start_date is the date when your DAG starts being scheduled. The real definition for it is that the start_date is the timestamp from which the scheduler will attempt to <span class="rainbow-text">backfill</span>.

 Backfill : Manually triggered to process historical data for specific past intervals. 

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/dag_backfill.png?raw=true){: .shadow .rborder}

### The schedule_interval

The schedule interval determines how often a DAG is triggered. To trigger the DAG daily then the below code would suffice.

```python
with DAG('my_dag', start_date=datetime(2022, 1, 1),
        schedule_interval='@daily') as dag:
```
> Note: The crone expression will trigger only within the specific date. So if you have a schedule_interval of 3 days. Since all months don't have exactly 30 days. Then it will create an issue where it will fill in by running for less days. To resolve this and have the days rollover into the next month as well, timedelta object is used.

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/crone_expression.png?raw=true){: .shadow .rborder}

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/dag_timedelta.png?raw=true){: .shadow .rborder}

```python
with DAG('my_dag', start_date=datetime(2022, 1, 1),
        schedule_interval=timedelta(days=3)) as dag:
```
### The catchup parameter.
Catchup: Automatically runs all missed DAG runs from the start date up to the current date when the DAG is resumed or the scheduler is back online. It can be enabled as shown below.

```python
with DAG('my_dag', start_date=datetime(2022, 1, 1),
        schedule_interval='@daily' catchup=True)as dag:
```

![image](https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/dag_catchup.png?raw=true){: .shadow .rborder}

<div class="logo-container">
        <img src="https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/airflow.png?raw=tru" alt="Airflow Logo" class="spinning-logo">
    </div>

<style>
  .logo-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 18vh;
  }

  .spinning-logo {
      width: 100px;
      height: 100px;
      animation: spin 4s linear infinite;
  }

  @keyframes spin {
      from {
          transform: rotate(0deg);
      }
      to {
          transform: rotate(360deg);
      }
  }

  .rborder {
    border-radius:15px;
  }

  @keyframes rainbow {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  .rainbow-text {
    background: linear-gradient(45deg, red, orange, yellow, green, blue, indigo, violet);
    background-size: 400% 400%;
    -webkit-background-clip: text;
    color: transparent;
    animation: rainbow 6s ease infinite;
  }
</style>