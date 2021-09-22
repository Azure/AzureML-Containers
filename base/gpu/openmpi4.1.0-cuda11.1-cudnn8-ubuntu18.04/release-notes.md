-  **Image Name**: docker pull mcr.microsoft.com/azureml/openmpi4.1.0-cuda11.1-cudnn8-ubuntu18.04 : 

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

: 20210719.v1
-------------------

 -   **Dependencies** : 
 
     -   Installed nccl-rdma-sharp-plugins
