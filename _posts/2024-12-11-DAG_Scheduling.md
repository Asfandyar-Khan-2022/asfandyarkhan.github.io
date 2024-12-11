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

<div class="logo-container">
        <img src="https://github.com/Asfandyar-Khan-2022/asfandyarkhan.github.io/blob/main/images/airflow.png?raw=tru" alt="Airflow Logo" class="spinning-logo">
    </div>

<style>
  .logo-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 12vh;
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