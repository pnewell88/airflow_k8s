# Synchronizing dev and prod Airflow instances
Ensuring the team is building dags in a uniform environment reduces development and deployment 
complexity. Assuming your team's local machines support docker-desktop or microk8s setups, just
clone this repo and make adjustments to fit your team's needs.

### Requirements.txt
For this example and the `git-user` example I've added the requirements list in through `airflow.extraPipPackages`.
This is one way of doing things, probably the better way is to build your team's own production airflow image
with all dependencies and have this helm chart use that image as the base.

### Running this locally
Is the same as in `git-user/README.md`, the difference being this values file is going to be listening
to a specific dev branch. So the workflow goes like this:

1. Dev writes a dag locally, pushes it to the project dev branch
2. Dev updates the git ref in the `custom-values.yaml` to sync with the dev branch
3. Dev updates their helm chart: `helm3 update {release name} --values custom-values.yaml`
4. Dev ensures the dag runs correctly in their local environment
5. Pull request is made/approved; other automated tests are run
6. Dev branch is merged into the dags repo main branch
7. Someone turns on the new dag in the production UI
8. rinse and repeat