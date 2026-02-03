<h1>
  <span class="headline">Data Pipelines and Workflow Orchestration</span>
  <span class="subhead">Recap & Preparing for Apache Airflow</span>
</h1>

## **Lesson Recap**
Over the past lessons, you have learned how to:
- **Design and implement** end-to-end data pipeline using real-world data.
- **Optimize and troubleshoot** pipeline performance and failures.
- **Automate ETL workflows** with best practices for data orchestration.

### **Key Takeaways**
✅ **Data Pipelines** enable reliable data movement and transformation.  
✅ **Optimization Techniques** improve performance and scalability.  
✅ **Troubleshooting Skills** help identify and fix failures effectively.  
✅ **Workflow Orchestration** streamlines execution and scheduling.  

<div class="mermaid">
  graph TD;
    A[Raw Data] -->|Extract| B[Transform Data];
    B -->|Load| C[Database];
    C -->|Orchestrate| D[Apache Airflow];
</div>

## **Engaging Introduction to Apache Airflow**
Before jumping into the lab, let’s get hands-on with **some examples** that introduce core Apache Airflow concepts. 

> **⚠️ Important Educational Note:** The code examples below demonstrate Airflow concepts and syntax for educational purposes. **Running these examples in Jupyter notebook cells will not create actual DAGs in the Airflow web UI** because:
> - Jupyter executes code in memory only
> - Airflow requires DAG definitions to be saved as `.py` files in `~/airflow/dags/`
> - Airflow must be properly installed and running (`airflow standalone`) to discover and execute DAGs
> - These examples are conceptual demonstrations to help you understand Airflow's core concepts before the hands-on lab
> 
> **To actually work with these DAGs:**
> 1. Save each example as a separate `.py` file in `~/airflow/dags/`
> 2. Ensure Airflow is running with `airflow standalone`
> 3. Check `http://127.0.0.1:8080` to see the DAGs appear
> 4. Unpause and trigger DAGs from the web UI

### **Instructor-Guided Mini-Challenges**

#### **1️. DAGs & Task Dependencies**
❓ ***Challenge**: "How would you define a workflow where Step A must finish before Step B starts?"*  

```python
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetime

default_args = { 'start_date': datetime(2024, 3, 1) }

with DAG('simple_dag', default_args=default_args, schedule_interval='@daily') as dag:
    task_a = DummyOperator(task_id='task_A')
    task_b = DummyOperator(task_id='task_B')

    task_a >> task_b  # Defines dependency
```
**Try running this DAG and modifying dependencies!**

#### **2️. Task Scheduling**
***Challenge**: "If a data pipeline should run at 3 AM daily, how would you configure it?"*  

**Example: Set up a DAG to run daily at 3 AM**
```python
with DAG('scheduled_dag', default_args=default_args, schedule_interval='0 3 * * *') as dag:
    start = DummyOperator(task_id='start')
```

#### **3️. Task Failures & Retries**
***Challenge**: "What happens if a task fails? How can you retry it automatically?"*  

**Example: Configuring retries**
```python
from airflow.operators.python_operator import PythonOperator
import random

def unstable_task():
    if random.random() < 0.7:
        raise Exception("Random failure!")
    print("Success!")

with DAG('retry_dag', default_args={'retries': 3}, schedule_interval='@daily') as dag:
    task = PythonOperator(task_id='unstable_task', python_callable=unstable_task)
```
**Try modifying retry settings to observe behavior!**

## **Preparing for the Apache Airflow Lab**
In the next session, you will **build a complete data pipeline from scratch** using Apache Airflow.

## **Next Steps**
**Review Airflow Documentation:** [Apache Airflow Docs](https://airflow.apache.org/docs/)  

