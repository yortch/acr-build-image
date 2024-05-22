# Base Images

Use this directory for utility images, e.g. for custom images, e.g. that do not exist in public repositories or combine multiple utilities needed for a given process

To contribute images:
1. Create Dockerfile 
1. Use this directory structure: `utility/<image_name>/<image_tag>`
1. Create a PR with a comment including the following command:
    ```
    #build: <relative Dockerfile path> <imagename> <tag>
    ```
    Example:
    ```
    #build: images/utility/azcopy/10.24.0 azcopy 10.24.0
    ```