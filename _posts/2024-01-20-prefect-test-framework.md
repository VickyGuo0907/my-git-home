---
layout: post
title:  "Test and Validate data pipeline/workflows with the Prefect"
date:   2024-01-20
desc: ""
keywords: " Open Source, solutions, Prefect, Innovation"
categories: [Solutions]
tags: [Innovation, solutions, Open Source, Prefect]
icon: icon-html
---


AI is presently a day-to-day hot topic in the world. Naturally, data is a crucial component of AI models and products. For this reason, many companies create their own ETL or ELT data pipeline/workflow to manage their data for the essential component to build AI models or future data analysis. 


## Describing the problem I faced
Data pipelines can be complex and overwhelming, requiring significant time and effort to ensure the quality of all related services for the development team. I start asking myself some questions: 

* Beyond relying on mainstream automation testing platforms or tools, How can we enhance the efficiency and extensibility of validating data workflows/pipelines by implementing API level of testing and end-to-end integration testing?


## The desired solution
While the Modern Data Stack continuously blossoms and provides a lot of flexibility with the various components that play well with each other, open-source orchestration tools like Apache Airflow, Prefect, Argo, and KubeFlow give you more options to fit different purpose. 
After some reserach and comparision, I decide to use Prefect to test and validation our existing complicated data pipeline.   

![Prefect Dashboard](../../../../static/assets/img/blog/prefect_dashboard.png)

* Prefect is a modern workflow orchestrator tool,  which makes it easier to create, deploy, and monitor tasks. With the help of an orchestrator, teams can quickly build repeatable processes that are easy to run remotely, observe, and troubleshoot if they fail.

* Prefect would be suitable if you need to get something lightweight up and running as soon as possible.

* Prefect offers a comprehensive UI dashboard that provides all the necessary information related to flows and tasks, work pool details, and workflow logs. 

* Additionally, Prefect offers both cloud-hosted and self-hosted deployment strategies, allowing users to maintain it using their own CI/CD pipeline, providing greater flexibility.

* Prefect integrations are structured as collections of pre-built tasks, flows, blocks, and more, which can be easily installed as PyPI packages. They offer extensive capabilities. 


## Solution Demo: How to use Prefect for testing and data pipeline validation


Prefect have two key concepts **Flows** and **Tasks** you should know. 

* **Flows**: A Prefect workflow, defined as a Python function.

* **Tasks**: Discrete units of work in a Prefect workflow

By defining task units as API entry points and integrating data pipeline processes into the workflow, you can ensure efficient and seamless task revocation. Take advantage of this approach to streamline your workflow and enhance code readability.


### Sample Code

* You can almost effortlessly include all REST API CRUD operations as tasks, such as GET, CREATE, UPDATE, DELETE API entry points.The sample **user_api_tasks.py** is provided below:

```
@task(name="get_user_task", description="get user information", log_prints=True)
def get_user_task(api_token: str, user_id: str) -> dict:
    """
    :param api_token:
    :param user_id:
    :return:
    """
    logger = get_run_logger()
    dict_result = {}
    request_headers = {"Authorization": f"Bearer {api_token}",
                       "content-type": "application/json"}
    try:
        response = requests.get(USER_API_URL, headers=request_headers, params={"user_id": user_id})
        dict_result["status_code"] = response.status_code
        dict_result["result"] = json.loads(response.content)
        if response.status_code!=200:
            logger.warning("failed to get user information, error: %s", response.content)
    except Exception as err:
        dict_result["status_code"] = 502
        dict_result["result"] = str(err)
        logger.error("get_user_task request failed, error: %s", err)

    return dict_result
```

* With defined prefect workflow, you gain the capability to seamlessly invoke specific combinations of tasks. This empowers you to comprehensively test or validate your data pipeline, aligning with the dynamic and evolving needs of your business requirements. Leveraging the versatility of Prefect Workflow, you can efficiently design and execute workflows tailored to the intricacies of your data processes, ensuring a thorough examination of your pipeline's functionality and adherence to business standards. 

![Prefect workflow sample](../../../../static/assets/img/blog/prefect_user_workflow.png)


BTW, You could organize your workflow code into smaller flow and task units lets you take advantage of Prefect features like retries, more granular visibility into the runtime state, the ability to determine the final state regardless of individual task state, and more. 


### Test Case and Test Data Management

* For testing and data validation purposes, it's essential to have a flexible and straightforward method for maintaining test cases and test data. I declide to create yaml file to present test cases. 

below is sample of ** user_workflow.yaml** test case. 

```
test-scenario-1:
  user_id: "123"
  new_user_json_path: "../test_data/new_user_1.json"
  update_user_json_path: "../test_data/update_user_1.json"
test-scenario-2:
  user_id: "234"
  new_user_json_path: "../test_data/new_user_2.json"
  update_user_json_path:  "../test_data/update_user_2.json"
```
below is **new_user_1.json**

```
{
  "user_id": "1",
  "name": "user_1",
  "phone_number": "123-456-7890"
}
```

* Think about this, by creating a common set of functions dedicated to reading test cases and extracting test data from JSON files. The beauty of this approach lies in its simplicity – any new test case or test data can effortlessly be placed into a designated folder. This enables the seamless addition of a multitude of data-driven test scenarios without the need to delve into the workflow or tasks code.


### Extra Bonus: Task Runners in parallel

Many real-world data workflows benefit from true parallel, distributed task execution. For these use cases, [Prefect Integrations](https://docs.prefect.io/latest/integrations/) offers Prefect-developed task runners for parallel task execution as below: 

* **DaskTaskRunner** runs tasks requiring parallel execution using dask.distributed.
* **RayTaskRunner** runs tasks requiring parallel execution using Ray.

This provides us with the opportunity not only to conduct data workflow/pipeline testing but also to explore performance testing capabilities by executing certain tasks in parallel. Below is sample code for parallels workflow: 

```
@task(name="square_calculator", description="square calculator", log_prints=True)
def square_calculator(input_data_1: int):
    square = input_data_1 ** 2
    print(square)

@flow(name="workflow_with_parallel_tasks", task_runner=DaskTaskRunner())
def workflow_with_parallel_tasks(data_list: list):
    for data_item in data_list:
        square_calculator.submit(data_item)
```

* Prefect UI dashboard would give you detail parallel tasks process.

![Prefect parallel tasks](../../../../static/assets/img/blog/prefect_parallel_tasks.png)


### Monitoring, Logs and Artifacts

* Prefect enables you to log a variety of useful information about your flow and task runs,capturing information about your workflows for purposes such as monitoring, troubleshooting, and auditing. This give us good information for testing step process. 

![Prefect workflow](../../../../static/assets/img/blog/prefect_workflow.png)


* Prefect have Artifacts which are persisted outputs such as tables, Markdown, or links. It make it easy to track and monitor the objects that your flows produce and update over time, especially you could reused to build your own cutomized test result summary.  

![Prefect artifacts result](../../../../static/assets/img/blog/prefect_artifacts.png)

### Deployment, CI/CD and Schedule


