PATH_TO_DAGS = /Users/josephcorrado/getting_started_with_airflow_using_docker/dags
AIRFLOW_DAGS = /usr/local/airflow/dags
DOCKER_IMAGE = puckel/docker-airflow
AIRFLOW_COMMAND = webserver
DOCKER_CONTAINER_NAME = AirflowTutorial
PORT = 8080
DOCKER_EXECUTE_COMMAND = docker exec -ti

# you define DAGs by creating the script, and adding it to a `dags` folder inside `$AIRFLOW_HOME`

# this is the command for launching the airflow docker container and mounting our directory containing DAGs inside Airflow
airflow:
	docker run --name $(DOCKER_CONTAINER_NAME) -d -p $(PORT):$(PORT) -v $(PATH_TO_DAGS):$(AIRFLOW_DAGS) $(DOCKER_IMAGE) $(AIRFLOW_COMMAND)

# this is the command for jumping in to the command line inside our Airflow instance
shell:
	$(DOCKER_EXECUTE_COMMAND) $(DOCKER_CONTAINER_NAME) bash

list_dags:
	$(DOCKER_EXECUTE_COMMAND) $(DOCKER_CONTAINER_NAME) airflow list_dags

# you can test individual parts of your DAG and output the logs to command line
# you specify a date and time, and it simulates the run at that time
# this command doesn't communicate to the database, so you won't see the results of test in the Airflow GUI
# this will simulate running task_1 at 2015-06-01
test_dag:
	$(DOCKER_EXECUTE_COMMAND) $(DOCKER_CONTAINER_NAME) airflow test Helloworld task_1 2015-06-01

# you can also test how backfilling would work, by passing the DAG and a start date and end date
# the output will be what would have happened at those points in time
test_backfill:
	$(DOCKER_EXECUTE_COMMAND) $(DOCKER_CONTAINER_NAME) airflow backfill Helloworld -s 2015-06-01 -e 2015-06-07

teardown:
	docker stop $(DOCKER_CONTAINER_NAME) && \
	docker ps -a && \
	docker rm $(DOCKER_CONTAINER_NAME) && \
	docker ps -a

.PHONY: teardown

