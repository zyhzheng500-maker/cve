# Vulnerability Introduction

The 1.0 version of Yougou all's ResourceController. java interface has an arbitrary file deletion vulnerability, as its interface does not fully detect file names and directories, allowing attackers to exploit it The./symbol is encoded to bypass detection, causing arbitrary file deletion.

# Vulnerability analysis

Vulnerability class file：src/main/java/per/ccm/ygmall/extra/controller/ResourceController.java

![1](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/1.6bhjdeu04m.webp)

Follow up on specific delete methods:

![2](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/2.8s3rsc0w13.webp)

You can see that the file path and file name have been manipulated in. object, following the. object method:

![3](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/3.1ovwcpvgh3.webp)

As can be seen, the validateObject Name method validates file names and paths, following up with the validateObject Name method:

![8155ebcd5301efca262f1aac77116e7a](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/4.9o097sbr7z.webp)

This code only segments the target string using '/' and only verifies if the segmented segment is' Or To prevent path traversal risks, this protection mechanism has significant flaws. Attackers can bypass detection in various ways, triggering directory traversal vulnerabilities and ultimately leading to high-risk security consequences such as arbitrary file deletion

1. Encoding bypass: Construct traversal characters using URL encoding (such as'% 2e% 2e% 2f ','..% 2f ','.% 2e/') or Unicode transformation encoding (such as'.. \ u200b/'), and the segmented segments will not be recognized as' ' '/ '. . ', but if the subsequent business logic decodes the string, it will be restored to path traversal characters;
2. Absolute path bypass: Directly upload absolute paths (such as `/etc/passwd `, ` C:/Windows/System32/`), as the code did not verify the legality of the absolute path, there is no 'after splitting '/ '. Segmented, directly accessible/operating system sensitive paths;
3. Special character/delimiter bypass: Using super long dot combinations (such as `.../`, some systems will parse them as `../`), cross platform path separators (such as ` a.. \ b ` in Windows), etc. to construct payloads, the code cannot recognize such traversal scenarios by only dividing them by '/', and ultimately triggers path traversal;

In summary, the protection logic does not cover the core path traversal attack scenario. Attackers can use the above methods to bypass detection, achieve directory traversal, and cause serious security issues such as arbitrary file deletion.

# Repair recommendations

1. It is recommended to first decode the URL of the input path and unify the cross platform path delimiter, then parse the path into a normalized absolute path, and force the verification that the target path must fall within the basic directory range allowed by the business to eliminate directory traversal risks.
2. It is necessary to perform URL decoding and path normalization on the incoming object names, and verify whether the target path is strictly included in the business restricted basic directory, in order to completely defend against various directory traversal and bypass methods.
