-  **Image Name**: docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.2-cudnn7-ubuntu18.04 :

:20210519.v1
-------------------

-   **Addressed vulnerabilities**
  
-   [CVE-2021-20305](https://ubuntu.com/security/notices/USN-4906-1)

 -   **Dependencies** : 
  
     -   added libnettle6 package in dockerfile to mitigate vulnerabilities.

: 20210513.v1
-------------------

 -   **Dependency** : 
  
     -   updated new python_assets tag to 20210428.36856618.

: 20210405.v1
-------------------

 -   **Dependency** : 
  
     -   updated redis package.

: 20210301.v1
-------------------

-   **Addressed vulnerabilities**
-   [CVE-2020-27350](https://lists.ubuntu.com/archives/ubuntu-security-announce/2020-December/005802.html)
-   [CVE-2020-29361](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005819.html)
-   [CVE-2019-19770](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005822.html)

 -   **Dependencies** : 
  
      -   added all latest packages in dockerfile to mitigate vulnerabilities.
      -   improvement -- Upgraded miniconda version to 4.9.2. Default python version is 3.7.9.
