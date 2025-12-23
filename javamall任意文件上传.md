# Vulnerability Introduction

The MinioController.java interface of JavaMall 1.0 version has an arbitrary file upload vulnerability. Its interface does not detect file suffixes and does not have a method to prevent directory traversal. Attackers can upload any type of file, which may result in getshell and more serious consequences

# Vulnerability analysis

Vulnerability class file:src/main/java/com/macro/mall/controller/MinioController.java

![485d218779fe07d8ee709c701f402ce3](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/485d218779fe07d8ee709c701f402ce3.2a5k23u4kg.webp)

In the upload method, after receiving the file name and file suffix, the file name and file suffix are directly concatenated into the new file name without any processing or type restrictions on the file suffix, which allows attackers to upload any type of file, causing any file upload loophole, and also without any interference Detecting and filtering, resulting in directory traversal vulnerabilities.

# Repair recommendations

Check the file suffix of uploaded files, only allowing files with suffixes in the whitelist to be uploaded, and also check ./, prevent directory traversal
