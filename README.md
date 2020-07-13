# Data Set Viewer

Data Set Viewer enables you to view data sets, including VSAM data sets, in CSV format, using layouts and selection criteria.

## Use Cases
- Within an organization, users and administrators can retrieve VSAM data set content for manual post-processing in a spreadsheet of their choice.
- As a developer, you can verify COBOL program output written into a physical sequential data set.
- As a tester, you can create test data from production VSAM data sets (see [**Limitations**](#limitations) for more information).

## **Contents**

- [**Prerequisites**](#prerequisites)
- [**Configure CAWAVSMN REXX member**](#configure-cawavsmn-rexx-member)
- [**Configure Extension Settings**](#configure-extension-settings)
- [**Using**](#using)
- [**Limitations**](#limitations)
- [**Report a Problem**](#report-a-problem)


## **Prerequisites**

Before you install Data Set Viewer, ensure that you have:

- Access to Mainframe 
- [CA File Master Plus](http://www.broadcom.com/fmp) version 10.0 or higher
- [Zowe Explorer](https://marketplace.visualstudio.com/items?itemName=Zowe.vscode-extension-for-zowe) configured with a z/OSMF profile
- CA File Master Plus VS Code REXX library (see [**Configure CAWAVSMN REXX member**](#configure-cawavsmn-rexx-member))

## **Configure CAWAVSMN REXX member**

Before you use Data Set Viewer, ensure that you have the CAWAVSMN member set up on the mainframe. Data Set Viewer uses the member to perform CA File Master Plus actions. You can create your own library or use a shared library for multiple users. We recommend that you use a shared library at your site. The instructions below describe how to create a new library.

**Follow these steps:**
1. Create a new data set library on the mainframe.
2. Upload the [CAWAVSMN](https://raw.githubusercontent.com/BroadcomMFD/data-set-viewer/master/rexxlib/CAWAVSMN) REXX member to your new data set.
3. Edit the CAWAVSMN member and update the FMPLOADLIB default value (CAI.CDBILOAD) to specify your CA File Master Plus load library.

    You successfully configured the CAWAVSMN REXX member.
 
## **Configure Extension Settings**

Before you start using the extension, check your configuration settings.

**Follow these steps**
1. In **File** (Windows) / **Code** (OS X) - **Preferences** - **Settings**, select **Extensions** and then **Data Set Viewer Configuration**.
    
    Your configuration displays.
2. Populate the following fields:

    - **Number of records per page**

        Specifies the number of records that you want to download at one time. Specify '0' if you want to download all records.
        
        **Default:** 100 
    - **Rexx Lib**

        Specifies the name of the data set that contains the CAWAVSMN member, as described in [**Configure CAWAVSMN REXX member**](#configure-cawavsmn-rexx-member).    
  
    - **SSH Port Number**

        Specifies the SSH port number that is used to connect to z/OS UNIX.
        
        **Default:** 22

    You can now use the extension to export data sets using layouts.

## **Using**

### **Functionality**
- Apply your existing CA File Master Plus layouts and selection criteria to display records.
- Download records in batches to decrease load times and optimize the use of hardware resources.
- Export data sets to a comma separated value format (CSV) using a custom record layout.

### **Display Records Using Layouts and Selection Criteria**

![Display records using a layout](https://github.com/BroadcomMFD/data-set-viewer/blob/master/resources/Display%20Records%20Using%20Layouts.gif?raw=true)

You can view records using layouts and selection criteria. Layouts generate a composite view of records that contain multiple pieces. They also allow you to hide unwanted fields and display data from a particular offset in a record. Selection criteria are rules that define which records are displayed based on your preferences, and enable you to further filter the data that you want to view.

**Follow these steps:**
1. In Zowe Explorer, locate a data set, right-click and select **View as CSV...**

    An input box appears that asks you to specify the name of the layout member that you want to use.

2. Specify the name of a layout and press **Enter**.

    **TIP:** You can locate a member in Zowe Explorer, right-click, select **Copy data set name**, and paste the name of the member into the field.
    
    An input box appears that asks you to specify the name of a selection criteria member, if you want to use one.

3. (Optional) Specify the name of a selection criteria member, or leave the field blank if you do not want to use one, and press **Enter**.

    Your records are displayed in a CSV format with the layout and selection criteria that you applied.

If you have a CSV VS Code extension installed, you can use it to view, sort and filter records in a table format. 

### **Download Records in Batches**

![Download records in batches](https://github.com/BroadcomMFD/data-set-viewer/blob/master/resources/Download%20Records%20in%20Batches.gif?raw=true)

If you have a data set that contains a large number of records, you can sequentially download the records to load the data faster and use hardware resources more efficiently.

**Follow these steps:**
1. In **File** - **Preferences** - **Settings**, select **Extensions** and then **Data Set Viewer Configuration**.
    
    Your configuration displays.

2. Populate the **Number of records per page** field with a value to specify the number of records that you want to download in a batch.

    You can now download the specified number of records at a time.
2. In Zowe Explorer, locate a data set, right-click and select **View as CSV...**
    
    A prompt appears that asks you to specify the name of a layout member that you want to use.

3. Specify the name of the corresponding layout member and, optionally, a selection criteria member.
Press **Enter** to confirm. 

    Your first batch of records is displayed in a CSV format.
5. Press **F1** and select the **data-set-viewer: View next records as CSV** command from the dropdown.
    
    Your next batch of records is displayed. 

### **Display Records Using a Custom Record Layout**

![Display records using a custom record layout](https://raw.githubusercontent.com/BroadcomMFD/data-set-viewer/master/resources/Display%20Records%20Using%20a%20Custom%20Record%20Layout.gif)

If you export a data set using custom record layout, your records are displayed in a separate tab for every ordinary layout applied.  

**Follow these steps:**

1. In Zowe Explorer, locate a data set that you want to export using a custom record layout. Right-click and select **View as CSV...**.

    An input box appears that asks you to specify the name of a layout member that you want to use.
2. Specify the name of a custom record layout and press **Enter**.

    An input box appears that asks you to specify the name of a selection criteria member, if you want to use one.
3. (Optional) Specify the name of a selection criteria member, or leave the field blank if you do not want to use one, and press **Enter**.

    Your records are displayed in a CSV format with the layout and selection criteria that you applied. 
    
    **NOTE:** For every ordinary layout applied, the extension displays records in a separate tab. The record numbers indicate where the application of one ordinary layout stops and the next ordinary layout starts. If you use **F1** and select the **data-set-viewer: View next records as CSV** command, the tabs with records are populated based on the ordinary layout that is applied to the next batch. For example, if you have only two tabs open with ordinary layouts of type A and B and the next batch of records have ordinary layouts of type A, B and C applied, the tabs with the records that correspond to layouts of type A and B are populated and a new tab opens and is populated with records that correspond to the layout of type C.

### **Create a new Layout or Selection Criteria**

For information about how to create layouts and selection criteria for your records, see the following pages of the CA File Master Plus documentation.

- [Record Layouts](https://techdocs.broadcom.com/content/broadcom/techdocs/us/en/ca-mainframe-software/devops/ca-filemaster-plus/11-0/using-ispf/ispf-user-interface-for-ca-file-master-plus/record-layouts.html)
- [Filters](https://techdocs.broadcom.com/content/broadcom/techdocs/us/en/ca-mainframe-software/devops/ca-filemaster-plus/11-0/using-ispf/ispf-user-interface-for-ca-file-master-plus/filters.html)

## **Limitations** 
- In order to create test data, you need to have the CA File Master Plus Plug-in for Zowe CLI to populate your data back to the mainframe. For information about the plug-in, see [CA File Master Plus Plug-in for Zowe CLI](http://techdocs.broadcom.com/content/broadcom/techdocs/us/en/ca-mainframe-software/devops/ca-brightside/3-0/ca-brightside-command-line-interface-cli/available-cli-plug-ins/ca-brightside-plug-in-for-ca-file-master-plus.html).
- Currently you can only export data sets. At this time, importing changes you have made locally is not supported. 

## **Report a Problem**

Report problems by filing an [issue](https://github.com/BroadcomMFD/data-set-viewer/issues) on our [GitHub project](https://github.com/BroadcomMFD/data-set-viewer.git).
