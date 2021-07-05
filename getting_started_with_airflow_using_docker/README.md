This is the tutorial from the Getting Started with Airflow medium article, located [here](https://towardsdatascience.com/getting-started-with-airflow-using-docker-cd8b44dbff98).

Essentially, what you'll find in this repo are a Makefile that contains the docker commands to launch
the Airflow container, with the correct volumes mounted, as well as the Python files that need to be executed by Airflow.

To pull the docker container used in this tutorial, you should run:

```shell
docker pull puckel/docker-airflow
```

You can list what images you have in your docker installation with:

```shell
docker images
```

Essentially, in my Makefile, I have the target `airflow`, which means that you just run:

```shell
make airflow
```

And this will spin up your docker container with airflow, give it the name `AirflowTutorial` and it will mount the folder
containing the DAG that was used in this tutorial as a binded directory onto the docker container in the correct path.

When that runs successfully, all you need to do is visit `localhost:8080` in your browser. 

You should then be able to run:
```shell
docker ps
```

You view the airflow process that's running in docker.
In particular, you should be able to see one container (process) with the name `AirflowTutorial`.

Note that when you want Airflow to detect your DAGs, your DAGs need to be located in a folder called `dags` inside your
`$AIRFLOW_HOME`, so that's the target location our dags directory is mounted onto in the Makefile.

In the tutorial, a few docker commands are explored to investigate the example DAG provided.

Those commands are also listed in the Makefile found in this repo.


If you want to drop into the shell inside your docker container, the command to do that from the Makefile is:
```shell
make shell
```

To see the DAGs that are detected by Airflow, you can run `airflow list_dags`.
To run this from the Makefile, you'd run:
```shell
make list_dags
```

One of the useful commands that is explored is the `airflow test` command, which allows you to specify a task, and a date, to simulate
what the output would have been if that task were run on that date. The output of test is not recorded in the underlying
Airflow database, so you won't see your results in the GUI.

To run that command with the makefile, you can run:
```shell
make test_dag
```


The final command that's explored in this tutorial is how to simulate a backfill using Airflow.
The example in the tutorial basically runs the DAG 7 times - one for each day from 2015-06-01 to 2015-06-07.
You can run this command with the Makefile with:
```shell
make test_backfill
```

This command might take some time to run, since there are four tasks, and they need to run for each of 7 days.

However, the tasks for this hello world DAG are simple, and just print out hello world.

To clean up, run:
```shell
make teardown
```

This will stop your docker container, then remove the container from your docker.
Note that the mounted volumes will be untouched, and remain where they were.
The output of this command should be an empty list of containers.