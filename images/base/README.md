# Base Images

Use this directory for base images, e.g. OS images or SDK images that contain customizations

To contribute images:
1. Create Dockerfile 
1. Use this directory structure: `base/<image_name>/<image_tag>`
1. Create a PR with a comment including the following command:
    ```
    #build: <relative Dockerfile path> <imagename> <tag>
    ```
    Example:
    ```
    #build: images/base/jdk-17/latest jdk-17 latest
    ```