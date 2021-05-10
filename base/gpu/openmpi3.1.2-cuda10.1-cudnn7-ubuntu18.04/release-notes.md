-  **Image Name**: docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04 : 


: 20210507.v1
-------------------

-   **Addressed vulnerabilities**
  
-   [CVE-2021-24031](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-March/005923.html)
-   [CVE-2021-20305](https://ubuntu.com/security/notices/USN-4906-1)

 -   **Dependencies** : 
  
     -   added libzstd1 and libnettle6 packages in dockerfile to mitigate vulnerabilities.
 
: 20210405.v1
-------------------

 -   **Dependency** : 
  
     -   updated redis package.

: 20210301.v1
-------------------

-   **Addressed vulnerabilities**

-   [CVE-2021-1052](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005851.html)
-   [CVE-2020-27350](https://lists.ubuntu.com/archives/ubuntu-security-announce/2020-December/005802.html)
-   [CVE-2020-29361](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005819.html)
-   [CVE-2018-20482](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005839.html)

 -   **Dependencies** : 
  
     -   added all latest packages in dockerfile to mitigate vulnerabilities.
     -   improvement -- Upgraded miniconda version to 4.9.2. Default python version is 3.7.9.
