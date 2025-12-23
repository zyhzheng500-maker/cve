# Vulnerability Introduction

The MinioController.java interface of JavaMall 1.0 version has an arbitrary file deletion vulnerability. Its interface does not detect file names and file suffixes, nor does it have a method to prevent directory traversal. Attackers can delete arbitrary files by modifying the passed file names and file suffixes, causing serious consequences

# Vulnerability analysis

Vulnerability class file：src/main/java/com/macro/mall/controller/MinioController.java

![1](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/1.9o09aw56up.webp)

As can be seen, the file deletion operation is performed on the file name through the removeObject function, following up with the removeObject function:

![a5f31a9f3c28e8aaac36bcd5e3e1f302](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/2.2a5k24kj5e.webp)

Then, operate on the file name using the 'ExecuteDelete' function, following up with the 'ExecuteDelete' function:

![798b186c2adf4f98abffe4c2015fb054](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/3.73uey9588j.webp)

It can be seen that it executes the corresponding operation by passing in the corresponding operation type (such as delete), following up with the execute function:

![bf73a7a105d0588414e90e808a8520c4](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/4.3d59d0gd10.webp)

After following up with the execute function, it was found that its operation on file names involves directly creating objects that need to be deleted. In this call chain, there are no restrictions on file names and file suffixes, nor are there any restrictions By filtering, attackers can cause directory traversal and arbitrary file deletion by controlling the incoming file names.

# Repair recommendations

Limit file names and file suffixes, restrict file types that can be deleted, and Filter to prevent directory traversal.
