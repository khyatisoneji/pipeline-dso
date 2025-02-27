The goal of this repository is to integrate security into DevOps pipeline.

A conventional CI/CD pipeline mostly has tasks like fetch, build, test, deploy. We need to integrate security into the pipeline. This could be further extended to the pipeline being failed if the security check is not fulfilled and right set of people could be notified.



## DevSecOps Pipeline
The DevSecOps Pipeline consists of the following task which are given below:


* [clone-git-repo](https://github.com/tektoncd/catalog/blob/main/task/git-clone/0.3/git-clone.yaml)
Git clone repo task clones given repository url into the workspace.


* [secret-detection](https://github.com/ayushi-24git/pipeline-dso/blob/main/Tasks/GITLEAKS/gitleaks.yaml)
Secret detection task is responsible for detecting secrets like api keys, passwords present in the repository.


### CRDA
* [setup-env](https://github.com/ayushi-24git/pipeline-dso/blob/main/Tasks/CRDA/setup-env.yaml)
Setup-env task downloads all the dependencies used by the project depending on the ecosystem so Redhat CodeReady Dependency Analytics can perform the analysis and check for vulnerabilities.

* [crda-task](https://github.com/ayushi-24git/pipeline-dso/blob/main/Tasks/CRDA/crda-task.yaml)
CRDA task uses RedHat CodeReady Dependency Tool to scan and check if any vulnerabilities lie in the dependencies used by the project and generates a whole report.

* [check-vulnerabilities](https://github.com/ayushi-24git/pipeline-dso/blob/main/Tasks/CRDA/setup-env.yaml)
Check vulnerabilities task reads the report generated in CRDA task and checks if all the vulnerabilities are within limits or not. It breaks the build if the vulnerabilities are not within the limits.

You can read more about the need, prerequistes and working of these three tasks please go through [CRDA](https://github.com/tektoncd/catalog/blob/edb15d9f4334822f319393ad271bf11106eca926/task/redhat-codeready-dependency-analysis/0.3/README.md)


* [build-push](https://github.com/urvashigupta7/Go-Server/blob/master/ci/task/build-push.yaml)
Build push task is responsible for building the image from docker file and pushing it to the docker registry.


* scan-repo


* [check-dep-version](https://github.com/ayushi-24git/pipeline-dso/blob/main/Tasks/KUBERNETES_MANIFEST_CHECKS/check-dep-version.yaml)
Check dep version task is responsible for finding out the deprecated or removed apiVersions in kubernetes manifest files. Read more about the task [here](https://github.com/urvashigupta7/kubernetes_manifest_checks/tree/master/tasks/pluto)


* [lint-yaml](https://github.com/ayushi-24git/pipeline-dso/blob/main/Tasks/KUBERNETES_MANIFEST_CHECKS/lint-yaml.yaml)
Lint yaml task checks for misconfigurations in kubernetes objects by scanning the kubernetes manifest files. Read more about the task [here](https://github.com/urvashigupta7/kubernetes_manifest_checks/tree/master/tasks/kube_linter)



## Secrets
For build and push task docker credentials are required (to push the image). build-push task uses the `docker-creds` secret to get `USERNAME` and `PASSWORD`. Use the below command to create `docker-creds` secret; replace `yourusername` and `yourpassword` to your own.

```
 kubectl create secret generic docker-creds \
      --from-literal=USERNAME=yourusername\
      --from-literal=PASSWORD=yourpassword
```



## Workspaces
Workspace is used as a common filesystem between tasks and used for inputs and outputs of task, in workspace dependencies are installed for further usage while running the task.
* **source** : A Workspace containing your source directory.
