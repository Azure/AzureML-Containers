-  **Image Name**:  docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.0-cudnn7-ubuntu18.04 : 

:20211012.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-22945](https://ubuntu.com/security/notices/USN-5079-3)
 -   [CVE-2021-22946](https://ubuntu.com/security/notices/USN-5079-3)
 -   [CVE-2021-22947](https://ubuntu.com/security/notices/USN-5079-3)
  
     - released images to mitigate curl vulnerabilities.

:20211005.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-40145](https://ubuntu.com/security/notices/USN-5068-1)
 -   [CVE-2021-22945](https://ubuntu.com/security/notices/USN-5079-3)
 -   [CVE-2021-22946](https://ubuntu.com/security/notices/USN-5079-3)
 -   [CVE-2021-22947](https://ubuntu.com/security/notices/USN-5079-3)
  
     - Added libcurl3-nss and libgd3 packages in dockerfile to mitigate vulnerabilities.

:20210906.v1
-------------------

 -   **Dependencies** : 
  
     - Removed unneeded packages and released the images.

:20210806.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-3580](https://ubuntu.com/security/notices/USN-4990-1)
  
     - Added libnettle6 package in dockerfile to mitigate vulnerabilities.

:20210727.v1
-------------------

 -   **Dependencies** : 
  
     - Removed unneeded dependencies.

: 20210714.v1
-------------------

-   **Addressed vulnerabilities**
  
-   [CVE-2021-3516](https://usn.ubuntu.com/4991-1)

 -   **Dependencies** : 
  
     -   added libxml2-dev package in dockerfile to mitigate vulnerabilities.

: 20210701.v1
-------------------

 -   **Dependencies** : 
 
     -   updated new python_assets tag to 20210623.40134510.

: 20210615.v1
-------------------

-   **Addressed vulnerabilities**
  
-   [CVE-2021-31535](https://usn.ubuntu.com/4966-1)
-   [CVE-2021-23017](https://usn.ubuntu.com/4967-1)
-   [CVE-2021-3520](https://usn.ubuntu.com/4968-1)

 -   **Dependencies** : 
  
     -   added libx11-dev,  nginx and liblz4-1 packages in dockerfile to mitigate vulnerabilities.
     -   updated new python_assets tag to 20210603.39066660.

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
