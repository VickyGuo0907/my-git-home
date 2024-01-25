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
After some reserach and comparision, I decide to use Prefect to test and validation our existing enterprise data pipeline.   

* Prefect is a modern workflow orchestrator tool,  which makes it easier to create, deploy, and monitor tasks. With the help of an orchestrator, teams can quickly build repeatable processes that are easy to run remotely, observe, and troubleshoot if they fail.
* Prefect would be suitable if you need to get something lightweight up and running as soon as possible.

* Prefect offers a comprehensive UI dashboard that provides all the necessary information related to flows and tasks, work pool details, and workflow logs. 

* Additionally, Prefect offers both cloud-hosted and self-hosted deployment strategies, allowing users to maintain it using their own CI/CD pipeline, providing greater flexibility.

* Prefect integrations are structured as collections of pre-built tasks, flows, blocks, and more, which can be easily installed as PyPI packages. They offer extensive capabilities.


![Prefect Dashboard](../../../../static/assets/img/blog/prefect_dashboard.png)


## Sample: How to use Prefect for testing and validation

* Below is a sample code to create one of API entry point as task. 

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

* You can almost effortlessly include all REST API CRUD operations as tasks, such as GET, CREATE, UPDATE, DELETE API entry points. The sample **user_api_tasks.py** is provided below:

![Prefect tasks sample](../../../../static/assets/img/blog/prefect_user_api_tasks.png)



* Prefect have Artifacts concept which are persisted outputs such as tables, Markdown, or links. It make it easy to track and monitor the objects that your flows produce and update over time, especially you could reused as test result summary.  

![Prefect artifacts result](../../../../static/assets/img/blog/prefect_artifacts.png)


* Now you could be able to see how whole workflow excuted 

![Prefect workflow](../../../../static/assets/img/blog/prefect_workflow.png)


