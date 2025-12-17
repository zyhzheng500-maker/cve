# Vulnerability Introduction

The 1.0 version of Yougou all's ResourceController. java interface has an arbitrary file upload vulnerability, as its interface does not detect file suffixes. Attackers can upload any type of file, which may result in getshell and more serious consequences.

# Vulnerability analysis

Vulnerability class file：src/main/java/per/ccm/ygmall/extra/controller/ResourceController.java

![1](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/1.6wr6uanzv5.webp)

![f9134b7781bdba26bd072b5bb48a6800](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/f9134b7781bdba26bd072b5bb48a6800.58htx3xpoy.webp)

In the upload method, after receiving the file suffix, the file suffix is directly concatenated into the new file name without any processing or restriction on the file suffix, which allows attackers to upload any type of file and creates an arbitrary file upload vulnerability, and there is no such thing as a vulnerability Performing detection may result in directory traversal

# Repair recommendations

Repair recommendations

Check the file suffix of uploaded files, only allowing files with suffixes in the whitelist to be uploaded, and also check ./, prevent directory traversal

