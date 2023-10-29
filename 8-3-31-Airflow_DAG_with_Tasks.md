## USER
Write an AIRFLOW DAG with 2 tasks, one task is a Python function that prints "Hooray! I got a Scholarship to the Airflow Summit on September 19 - September 2021". The second task is a Python function that prints "See you at Toronto. Excitement in the air :)" 

## ASSISTANT
Certainly! Here's an example of an Airflow DAG with two tasks, each running a Python function to print the specified messages:

```python
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from datetime import datetime

# Define the default arguments for the DAG
default_args = {
    'owner': 'airflow',
    'start_date': datetime(2021, 9, 1),
    'retries': 1,
}

# Instantiate the DAG
dag = DAG('scholarship_dag', default_args=default_args, schedule_interval=None)

# Define the Python functions for the tasks
def print_scholarship_message():
    print("Hooray! I got a Scholarship to the Airflow Summit on September 19 - September 2021")

def print_excitement_message():
    print("See you at Toronto. Excitement in the air :)")

# Define the tasks
task1 = PythonOperator(
    task_id='print_scholarship_message',
    python_callable=print_scholarship_message,
    dag=dag,
)

task2 = PythonOperator(
    task_id='print_excitement_message',
    python_callable=print_excitement_message,
    dag=dag,
)

# Set task dependencies
task1 >> task2
```

This DAG defines two tasks, each represented by a `PythonOperator` that runs the specified Python functions. Task 1 prints the scholarship message, and task 2 prints the excitement message. Task 2 will only run after task 1 has completed successfully due to the task dependencies (`task1 >> task2`).