# DEPLOYMENT AUTOMATION WITH: 'IBM APP CONNECT 12 & OPENSHIFT PIPELINE (TEKTON) [INTEGRACIÓN]':

## 1. CONTEXT:
This DEMO allows the INTEGRATION between IBM APP CONNECT 12 & OPENSHIFT PIPELINE (TEKTON), with the objetive of providing AUTOMATION for the deployments of: WEBSERVICES, MICROSERVICES, etc. created in ACE12, on: INTEGRATION SERVERs & INTEGRATION RUNTIMEs.


## 2. ARCHITECTURE:
The following ARCHITECTURE shows how the different RESOURCES used within the OPENSHIFT cluster and the interaction with the AUTOMATION initiated MANUALLY or AUTOMATICALLY by the Developers are related.

![alt text](https://github.com/maktup/dummy-tekton-ace/blob/main/images/Arquitectura.jpg?raw=true)


## 3. DIRECTORIES:
The following DIRECTORIES are those existing within the managed GIT repository, in this case (GITHUB):

| NAME |  DETAIL  |
| ------------ | ------------ |
| dummy_csm_microservice | consists of the DIRECTORY of the ACE12 project, with the MICROSERVICE SOURCES. |
| scripts | contains the YAML scripts to be installed ORDERLY, for the construction of the DEVOPs flow with TEKTON. |
| apis | contains the Swagger & OpenAPI APIs that can be used to build the MICROSERVICE in ACE12. |
| doc | contains the step-by-step DOCUMENT (PDF) to use. |


## 4. SCRIPTs (YAML):
The following YAML files are all those managed within the DIRECTORY: "scripts" that contain, in an orderly and documented manner, the FUNCTIONALITY to install:

| NAME | DETAIL |
| ------------ | ------------ |
| 0_dummy-appconnect-tekton-dashboard.yaml   |  YAML script used for the installation of the TEKTON DASHBOARD. |
| 1_dummy-appconnect-requerimientos.yaml  | YAML script used to install ALL the REQUIREMENTS prior to handling OPENSHIFT PIPELINE (access, security, etc.). |
| 2_dummy-appconnect-pipeline.yaml | YAML script used to install the RESOURCE with the PIPELINE structure. |
| 3_dummy-appconnect-task.yaml | YAML script used to install TASK-type RESOURCES with flow LOGIC (more IMPORTANT). |
| 4_dummy-appconnect-trigger.yaml  | YAML script used for the installation of the RESOURCES for the AUTOMATION of the deployment. |
| 5_dummy-appconect-ejecucion.yaml | YAML script used for the TEST through the PIPELINE RUN. |


## 5. INSTALLATION:
The BASE NAMESPACE that will be used and will support everything for the DEMO will be: **dummy-tekton-appconnect**. Likewise, the INSTALLATION will be as follows:

**//NAMESPACE:**

    $ oc create ns dummy-tekton-appconnect


**//REQUIREMENTS:**

    $ oc create -f https://raw.githubusercontent.com/maktup/dummy-tekton-ace/main/scripts/1_dummy-appconnect-requerimientos.yaml

**//PERMISSIONS:**

    $ oc adm policy add-scc-to-user privileged -z pipeline -n dummy-tekton-appconnect
    $ oc adm policy add-role-to-user edit -z pipeline -n dummy-tekton-appconnect

**//PIPELINE:**

    $ oc create -f https://raw.githubusercontent.com/maktup/dummy-tekton-ace/main/scripts/2_dummy-appconnect-pipeline.yaml

**//TASKs:**

    $ oc create -f https://raw.githubusercontent.com/maktup/dummy-tekton-ace/main/scripts/3_dummy-appconnect-task.yaml

**//TRIGGERs:**

        $ oc create -f https://raw.githubusercontent.com/maktup/dummy-tekton-ace/main/scripts/4_dummy-appconnect-trigger.yaml


## 6. TESTING:
The AUTOMATION test can be MANUAL and AUTOMATIC:

**A. MANUAL:** 
Running the Script.

    $ oc create -f https://raw.githubusercontent.com/maktup/dummy-tekton-ace/main/scripts/5_dummy-appconect-ejecucion.yaml

**B. AUTOMATIC:** 
Running the command:

    $ tkn pipeline start pipeline-build-and-deploy -w name=workspace-pipeline,claimName=source-pvc-pipe -p nombre-integration-server=csm-microservicio -p nombre-namespace-appconnect=dummy-tekton-appconnect -p url-repositorio-git=https://github.com/maktup/dummy-tekton-ace.git -p nombre-subdirectorio-git=dummy_csm_microservice -p nombre-repositorio-git=dummy-tekton-ace -p branch-repositorio-git=main -p nombre-proyecto-appconnect=dummy_csm_microservice -p version-image-appconnect="12.0.7.0-r3" -p licencia-image-appconnect="L-APEH-CJUCNR" -p ruta-image-appconnect="docker.io/maktup/ibm-appconnect-12:latest" -p tipo-licencia-appconnect="CloudPakForIntegrationNonProductionFREE" -p tipo-despliegue-servidor="3" -n dummy-tekton-appconnect

**IMPORTANT:** "depending on the value in PARAMETER: "**server-deployment-type**" sent (**1,2,3**)".

**1 => INSTALLATION/UPDATE in INTEGRATION SERVER.**

**2 =>INSTALL/UPGRADE in INTEGRATION RUNTIME.**

**3 => INSTALL/UPDATE on BOTH.**


## 7. RESULTADOS:
Finally, either for an INSTALLATION or UPDATE of an ACE12 SOURCE (MICROSERVICE), the result will be the deployment in:

**INTEGRATION SERVER:**

http://csm-microservicio-is-http-dummy-tekton-appconnect.apps.rey.coc-ibm.com/csm-microservice/get/empleados

**INTEGRATIO RUNTIME:**

http://csm-microservicio-ir-http-dummy-tekton-appconnect.apps.rey.coc-ibm.com/csm-microservice/get/empleados

![alt text](https://github.com/maktup/dummy-tekton-ace/blob/main/images/Testing.jpg?raw=true)


## 8. STEP & STEP:
The document with the complete PROCEDURE is here (**only in Spanish**):

https://github.com/maktup/dummy-tekton-ace/blob/main/doc/IBM%20App%20Connect%20%2B%20OPENSHIFT%20Pipeline%20(Tekton).pdf

