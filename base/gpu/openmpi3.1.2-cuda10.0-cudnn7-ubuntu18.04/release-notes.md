-  **Image Name**:  docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.0-cudnn7-ubuntu18.04 : 

: 20210531.v1
-------------------

 -   **Addressed vulnerabilities**
  
     -   [CVE-2021-3449](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-March/005947.html)

 -   **Dependencies** : 
  
     -   added openssl package in dockerfile to mitigate vulnerabilities.

: 20210513.v1
-------------------

 -   **Dependency** : 
  
     -   updated new python_assets tag to 20210428.36856618.
   
: 20210507.v1
-------------------

-   **Addressed vulnerabilities**

-   [CVE-2021-20305](https://ubuntu.com/security/notices/USN-4906-1)

 -   **Dependencies** : 
  
     -   added libnettle6 package in dockerfile to mitigate vulnerabilities.
 
: 20210428.v1
-------------------

-   **Addressed vulnerabilities**
  
-   [CVE-2021-24031](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-March/005923.html)
-   [CVE-2021-3449](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-March/005947.html)

 -   **Dependencies** : 
  
     -   added libzstd1 and libssl1.1 packages in dockerfile to mitigate vulnerabilities.

: 20210331.v1
-------------------

-   **Addressed vulnerabilities**
-   [CVE-2020-25704](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005859.html)

: 20210301.v1
-------------------

-   **Addressed vulnerabilities**
-   [CVE-2019-19770](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005822.html)
-   [CVE-2018-20482](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005839.html)
-   [CVE-2021-1052](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-January/005851.html)
-   [CVE-2018-10322](https://lists.ubuntu.com/archives/ubuntu-security-announce/2020-October/005689.html)
-   [CVE-2020-14351](https://lists.ubuntu.com/archives/ubuntu-security-announce/2020-December/005793.html)
-   [CVE-2020-27350](https://lists.ubuntu.com/archives/ubuntu-security-announce/2020-December/005802.html)

 -   **Dependencies** : 
  
     -   added all latest packages in dockerfile to mitigate vulnerabilities.
     -   improvement -- Upgraded miniconda version to 4.9.2. Default python version is 3.7.9.
