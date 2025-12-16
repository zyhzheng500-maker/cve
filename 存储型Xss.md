# Vulnerability Introduction

The 1.0 version of ecms/updateProductServlet interface has an XSS storage vulnerability, where attackers can pass in the product name (i.e. productName parameter) to cause the server to execute JS code, resulting in an XSS storage vulnerability

# Vulnerability analysis

Vulnerability class file: src/servlet/product/updateProductServlet.java

![1](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/1.5fl1qwdq67.webp)

![2](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/2.39ln54m2f4.webp)

Receiving the productName parameter in the updateProductServlet class and directly updating it to the database without verifying the incoming content, there is an XSS storage vulnerability

# Vulnerability reproduction

![3](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/3.1ovw5nouyp.webp)

After entering the following code in the product name and clicking the submit button, it was found that the JS code was successfully executed.

<script>alert(1)</script>

![4](https://github.com/zyhzheng500-maker/picx-images-hosting/raw/master/ecms/4.1ovw5nouyq.webp)

