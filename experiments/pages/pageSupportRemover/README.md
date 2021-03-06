# Page Support File Remover

As of Skuid release 12.2.0, page support files are no longer needed by Skuid to include inline Javascript and CSS. This is not a breaking change; orgs with page support files will continue to work as expected. However, if users want to keep their static resources clean, this tool will attempt to find any page support files and then allow users to remove them.

## Usage

1. Create a Skuid page in your Salesforce org with the page XML stored in this folder. 

   **Note**: The Page Support File Remover must be created as a v2 page. Creating it as a v1 page will result in errors.
   
1. Once created, click **Preview**.
1. Click **Load all Page Support Files**. 
1. Select the files you wish to be deleted.
1. Click **Delete Page Support Files**.

Using this tool does not delete your inline Javascript or CSS, it only removes the page support file generated by Skuid when the page was created.
