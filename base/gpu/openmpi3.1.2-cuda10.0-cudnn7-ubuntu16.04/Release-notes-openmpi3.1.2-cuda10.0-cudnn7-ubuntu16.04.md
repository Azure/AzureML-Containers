-  **Image Name**: docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.0-cudnn7-ubuntu16.04 : 

: 20210331.v1
-------------------

-   **Addressed vulnerabilities**
-   [CVE-2019-0148](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005823.html)
-   [CVE-2018-13093](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005857.html)

-   **Description** : 
  
     -   This vulnerabilities are mitigated in last release since we are not pinned version in our dockerfiles.
     

: 20210301.v1
-------------------

-   **Addressed vulnerabilities**
-    [CVE-2019-0148](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005823.html)
-    [CVE-2018-20482](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005839.html)
-    [CVE-2020-27350](https://lists.ubuntu.com/archives/ubuntu-security-announce/2020-December/005802.html)

 -   **Dependency** : 
  
     -   added all latest packages in dockerfile to mitigate vulnerabilities.
     -   improvement -- Upgraded miniconda version to 4.9.2. Default python version is 3.7.9.
