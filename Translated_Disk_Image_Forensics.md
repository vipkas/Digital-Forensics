
# Introduction to Disk Image Forensics

Digital storage devices like hard drives, solid-state drives, and USB drives hold vast amounts of data that can be crucial in digital forensics investigations. Disk image forensics involves the process of analyzing these devices and their contents to find valuable information for investigations.

In this lab, we will cover the basics of disk image forensics, including essential terminology and the steps involved in acquiring and analyzing a disk image.

## Basic Terminology

### Disk Image
A disk image is a bit-by-bit copy of an entire disk (like a hard drive, USB, etc.) or partition that preserves the exact content and structure of the original data. It includes not only files and folders but also empty space, metadata, and other hidden data that is not normally visible.

### Disk Imaging
Disk imaging is the process of creating a forensic copy of a storage device, such as a hard drive or USB drive. This step is crucial in digital forensics as it ensures the original data remains intact and unmodified. Cryptographic hashes are used to verify that the copy matches the original exactly, ensuring no modifications have been made to the original data. This approach allows forensic investigators to work with the copy without the risk of accidentally altering the original data.

### Disk Image Forensics
Disk image forensics involves analyzing a disk image to search for evidence of interest. This includes using tools like Autopsy and FTK Imager to uncover useful information and examine system artifacts like the Windows registry, web browsers, LNK Files, event logs, command prompt history, etc.

There are several tools available for disk image forensics, such as Autopsy and FTK Imager. While Autopsy provides more features during analysis, we will use FTK Imager for its lightweight nature.

You can download the tool from https://www.exterro.com/ftk-imager, so make sure you have FTK Imager installed on your computer before proceeding with the lab.

## Acquiring a Disk Image

Here is a step-by-step guide to creating a disk image using FTK Imager:

1. Open FTK Imager and go to **File → Create Disk Image**.
2. Select **Physical Drive**, click *Next*, and choose the drive you wish to create an image of. In this example, I’ll choose my USB drive.
3. Next, click the **Add** button under **Image Destination(s)**, then select **Raw (dd)** as the destination image type.
4. Enter relevant details about the evidence, then select a folder and name for saving the disk image.
5. Finally, click **Start** to begin the imaging process. It may take some time to complete depending on the drive size.

## Analyzing a Disk Image

Once a disk image is acquired, the next step is to analyze it for any evidence related to the investigation.

...

## Conclusion

In this lab, we covered basic terminology, followed by how to acquire a disk image. We then explored common files found within a disk image and performed basic analysis using FTK Imager. However, it’s essential to remember that the disk image analyzed here is a small USB drive, and there is much more to analyze in larger hard drive disk images, which may involve identifying common artifacts such as the Windows Registry, Browser History, Event Logs, Console History, and other valuable insights for investigations.
