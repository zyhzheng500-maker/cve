# Vulnerability Introduction

The PmsProductController.java interface of moga mall version 1.0 has an arbitrary file upload vulnerability, which allows attackers to exploit /,. The encoding method of./bypasses detection, causing directory traversal, and there is no restriction on file suffix types, resulting in arbitrary file uploads that may lead to getshell and more serious consequences.

# Vulnerability analysis

Vulnerability class file：src/main/java/com/ms/product/controller/PmsProductController.java

![1](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/1.5q7vu4vv8u.webp)

As can be seen, the handling of file names is done using the uploadObject function, following up with the uploadObject function:

![d3359e8eb2c9781142283a49aa5cfc3d](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/2.4n86j901db.webp)

Focus on the first parameter, which is the processing of file names. It can be seen that this is done through the. object function, following up with the. object function:

![a1ea5f9274a7a0292e2d6a477fd571c3](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/3.4g4yntdvxr.webp)

As can be seen, the file name is validated through the validateObjectively function, following up with the validateObjectively function:

![4](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/4.2yytm29r74.webp)

This code only segments the target string using '/' and only verifies if the segmented segment is' Or To prevent the risk of path traversal, this protection mechanism has significant flaws. Attackers can bypass detection in various ways, triggering directory traversal vulnerabilities and ultimately leading to high-risk security consequences such as directory traversal and arbitrary file uploads

1. Encoding bypass: Construct traversal characters using URL encoding (such as'% 2e% 2e% 2f ','..% 2f ','.% 2e/') or Unicode transformation encoding (such as'.. \ u200b/'), and the segmented segments will not be recognized as' ' '/ '. . ', but if the subsequent business logic decodes the string, it will be restored to path traversal characters;
2. Absolute path bypass: Directly upload absolute paths (such as `/etc/passwd `, ` C:/Windows/System32/`), as the code did not verify the legality of the absolute path, there is no 'after splitting '/ '. Segmented, directly accessible/operating system sensitive paths;
3. Special character/delimiter bypass: Using super long dot combinations (such as `.../`, some systems will parse them as `../`), cross platform path separators (such as ` a.. \ b ` in Windows), etc. to construct payloads, the code cannot recognize such traversal scenarios by only dividing them by '/', and ultimately triggers path traversal;

In summary, this protection logic does not cover the core directory traversal attack scenario. Attackers can use the above methods to bypass detection, achieve directory traversal, and cause serious security issues such as arbitrary file uploads.

# Repair recommendations

1. It is recommended to first decode the URL of the input path and unify the cross platform path delimiter, then parse the path into a normalized absolute path, and force the verification that the target path must fall within the basic directory range allowed by the business to eliminate directory traversal risks.
2. It is necessary to perform URL decoding and path normalization on the incoming object names, and verify whether the target path is strictly included in the business restricted basic directory, in order to completely defend against various directory traversal and bypass methods.
