-  **Image Name**: docker pull mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.2-cudnn8-ubuntu18.04 :

:20220401.v1 
------------------- 

-   **Dependencies** :

-  [CVE-2021-20193](https://ubuntu.com/security/notices/USN-5329-1)
-  [CVE-2022-0778](https://ubuntu.com/security/notices/USN-5328-1)
 - [CVE-2022-23218](https://ubuntu.com/security/notices/USN-5310-1)
-  [CVE-2022-23219](https://ubuntu.com/security/notices/USN-5332-1)
-  [CVE-2022-24407](https://ubuntu.com/security/notices/USN-5332-1) 

     - released images to mitigate for **tar** Vulnerability (USN-5329-1) 
     - released images to mitigate for **OpenSSL** Vulnerability (USN-5328-1) 
     - released images to mitigate **GNU C** library Vulnerability (USN-5310-1) 
     - released images to mitigate **Bind** Vulnerability (USN-5332-1)  
     - released images to mitigate **Cyru SASL** Vulnerability (USN-5301-1)

:20220314.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2020-6096](https://ubuntu.com/security/notices/USN-5310-1)
 -   [CVE-2018-7169](https://ubuntu.com/security/notices/USN-5254-1)

     - released images to mitigate GNU C Library and shadow vulnerabilities.

:20220303.v1
-------------------

-   **Dependencies** : 

 -   [CVE-2018-7169](https://ubuntu.com/security/notices/USN-5254-1)
 -   [CVE-2022-24407](https://ubuntu.com/security/notices/USN-5301-1)

     - released images to mitigate shadow  - cyrus sasl  vulnerabilities.

:20220218.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2018-7169](https://ubuntu.com/security/notices/USN-5254-1)

     - released images to mitigate shadow - system login tools vulnerabilities.

:20220208.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-3997](https://ubuntu.com/security/CVE-2021-3997)

     - released images to mitigate systemd vulnerabilities.

:20220127.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-3997](https://ubuntu.com/security/CVE-2021-3997)

     - released images to mitigate systemd vulnerabilities.

:20220113.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-3800](https://ubuntu.com/security/notices/USN-5189-1)

     - released images to mitigate GLib vulnerabilities.

:20211221.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-3800](https://ubuntu.com/security/notices/USN-5189-1)

     - released images to mitigate GLib vulnerabilities.

:20211210.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2020-21913](https://ubuntu.com/security/notices/USN-5133-1)
 -   
     - released images to mitigate ICU vulnerabilities.

:20211124.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2020-16592](https://ubuntu.com/security/CVE-2020-16592)
 -   [CVE-2021-3487](https://ubuntu.com/security/CVE-2021-3487)
  
     - released images to mitigate binutils vulnerabilities.

:20211111.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2020-16592](https://ubuntu.com/security/CVE-2020-16592)
 -   [CVE-2021-3487](https://ubuntu.com/security/CVE-2021-3487)
  
     - released images to mitigate binutils-multiarch vulnerabilities.

:20211029.v1
-------------------

 -   **Dependencies** : 

 -   [CVE-2021-22945](https://ubuntu.com/security/notices/USN-5079-3)
 -   [CVE-2021-22946](https://ubuntu.com/security/notices/USN-5079-3)
 -   [CVE-2021-22947](https://ubuntu.com/security/notices/USN-5079-3)
  
     - released images to mitigate curl vulnerabilities.

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

: 20210922.v1
-------------------

-   **Addressed vulnerabilities**
  
-   [CVE-2021-3711](https://ubuntu.com/security/notices/USN-5051-1)
-   [CVE-2021-3712](https://ubuntu.com/security/notices/USN-5051-1)

 -   **Dependencies** : 
  
     -   added libssl1.1 package in dockerfile to mitigate vulnerabilities.

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

-   **Addressed vulnerabilities**
  
-   [CVE-2021-33910](https://ubuntu.com/security/notices/USN-5013-2)

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
-   [CVE-2021-28153](https://lists.ubuntu.com/archives/ubuntu-security-announce/2021-March/005931.html)

 -   **Dependencies** : 
  
     -   added glib and libzstd1 packages in dockerfile to mitigate vulnerabilities.

: 20210405.v1
-------------------

 -   **Dependency** : 
  
     -   updated redis package.

: 20210301.v1
-------------------

-   **Addressed vulnerabilities**

-   None

-   improvement -- Upgraded miniconda version to 4.9.2. Default python version is 3.7.9.
