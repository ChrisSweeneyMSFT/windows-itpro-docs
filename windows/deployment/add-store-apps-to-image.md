---
title: Add Microsoft Store for Business applications to a Windows 10 image
description: This article describes the correct way to add Microsoft Store for Business applications to a Windows 10 image.
keywords: upgrade, update, windows, windows 10, deploy, store, image, wim
ms.prod: w10
ms.mktglfcycl: deploy
ms.localizationpriority: medium
ms.sitesec: library
ms.pagetype: deploy
audience: itpro
author: aczechowski
ms.author: aaroncz
ms.reviewer: 
manager: dougeby
ms.topic: article
ms.custom: seo-marvel-apr2020
---

# Add Microsoft Store for Business applications to a Windows 10 image

**Applies to**

-   Windows 10

This topic describes the correct way to add Microsoft Store for Business applications to a Windows 10 image. This will enable you to deploy Windows 10 with pre-installed Microsoft Store for Business apps.

>[!IMPORTANT]
>In order for Microsoft Store for Business applications to persist after image deployment, these applications need to be pinned to Start prior to image deployment.

## Prerequisites

* [Windows Assessment and Deployment Kit (Windows ADK)](windows-adk-scenarios-for-it-pros.md) for the tools required to mount and edit Windows images.

* Download an offline signed app package and license of the application you would like to add through [Microsoft Store for Business](/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app). 
* A Windows Image. For instructions on image creation, see [Create a Windows 10 reference image](deploy-windows-mdt/create-a-windows-10-reference-image.md).

>[!NOTE]
> If you'd like to add an internal LOB Microsoft Store application, please follow the instructions on **[Sideload LOB apps in Windows 10](/windows/application-management/sideload-apps-in-windows-10)**.

## Adding a Store application to your image

On a machine where your image file is accessible:
1. Open Windows PowerShell with administrator privileges.
2. Mount the image. At the Windows PowerShell prompt, type:
`Mount-WindowsImage -ImagePath c:\images\myimage.wim -Index 1 -Path C:\test`
3. Use the Add-AppxProvisionedPackage cmdlet in Windows PowerShell to preinstall the app. Use the /PackagePath option to specify the location of the Store package and /LicensePath to specify the location of the license .xml file. In Windows PowerShell, type:
`Add-AppxProvisionedPackage -Path C:\test -PackagePath C:\downloads\appxpackage -LicensePath C:\downloads\appxpackage\license.xml`

>[!NOTE]
>Paths and file names are examples. Use your paths and file names where appropriate.
>
>Do not dismount the image, as you will return to it later.

## Editing the Start Layout

In order for Microsoft Store for Business applications to persist after image deployment, these applications need to be pinned to Start prior to image deployment.

On a test machine:
1. **Install the Microsoft Store for Business application you previously added** to your image.
2. **Pin these apps to the Start screen**, by typing the name of the app, right-clicking and selecting **Pin to Start**.
3. Open Windows PowerShell with administrator privileges.
4. Use `Export-StartLayout -path <path><file name>.xml` where *\<path>\<file name>* is the path and name of the xml file your will later import into your Windows Image.
5. Copy the XML file you created to a location accessible by the machine you previously used to add Store applications to your image.

Now, on the machine where your image file is accessible:
1. Import the Start layout. At the Windows PowerShell prompt, type: 
`Import-StartLayout -LayoutPath "<path><file name>.xml" -MountPath "C:\test\"`
2. Save changes and dismount the image. At the Windows PowerShell prompt, type:
`Dismount-WindowsImage -Path c:\test -Save`

>[!NOTE]
>Paths and file names are examples. Use your paths and file names where appropriate.
>
>For more information on Start customization see [Windows 10 Start Layout Customization](/archive/blogs/deploymentguys/windows-10-start-layout-customization)


## Related topics
* [Customize and export Start layout](/windows/configuration/customize-and-export-start-layout)
* [Export-StartLayout](/powershell/module/startlayout/export-startlayout)
* [Import-StartLayout](/powershell/module/startlayout/import-startlayout)
* [Sideload LOB apps in Windows 10](/windows/application-management/siddeploy-windows-cmws-10)
* [Prepare for Zero Touch Installation of Windows 10 with Configuration Manager](deploy-windows-cm/prepare-for-zero-touch-installation-of-windows-10-with-configuration-manager.md)
* [Deploy Windows 10 with the Microsoft Deployment Toolkit](./deploy-windows-mdt/prepare-for-windows-deployment-with-mdt.md)
* [Windows Assessment and Deployment Kit (Windows ADK)](windows-adk-scenarios-for-it-pros.md)
