
-----
- **CWE map**: [[CWE-345(insufficient-verification-of-data-authenticity)]]
- Sources:
	https://portswigger.net/web-security/file-upload
	https://www.yeswehack.com/learn-bug-bounty/file-upload-attacks-part-1
	https://openreview.net/pdf?id=thJGSQcS5y
---
### Introduction
Web application can have the functionality to upload an image selected by the user, the application read code such as static things `html`, `png` but also if we could upload a file from the language utilized for the backend, the application may run scripts because its configured to do it so. 

### Sending a post request
When we submit a file these are usually a POST request that it's utilization a multipart form data. `multipart/form-data` it's a value of content type, the header required in the post method, a deep oversight about the multipart and its usage could be found in [RFC-7578](https://www.rfc-editor.org/rfc/rfc7578) 
or `js` for node js. But in summary there's a ID and this id is used for the different fields, which are well separated and used in a header called content disposition.
Here are some examples where there's no any restrictions to load the files, which allows webshells, RCE to an attacker
A vanilla PHP web that does not restrict any uploading file will be vulnerable to command execution:
- [[lab-file-upload-remote-code-execution-via-web-shell-upload]]


However in the wild it's more common to find libraries, framework, more elaborated script that does block the execution of code.
To see further explotations techniques, let's explore the different parameters that are used for the **content disposition.** 
### File name attacks
Starting by the file name parameter, there are several attacks based only in the filename value. In request for comment 2183 and 7578 says the following about this attribute,
> *The Content-Disposition header field that does represents a content of a file, SHOULD use the parameter filename, `(before on rfc2183)` the suggested filename should be used as a basis for the actual filename, where possible.*

So basically the filename parameter used to be used as the location of the file content in the web application. If you note the ambiguity in the use of filename the explotation cases may vary, as **Martin Doyhenard** says, when rfc say "should", "may", "should not", there's a vulnerability.
##### Path traversal
The filename parameter may be vulnerable to path taversal;
- [[lab-file-upload-web-shell-upload-via-path-traversal]]
##### Filename injection
We can circumvent the application by appending an `.php` at the end of `image.png` and result in `image.png.php` being accepted as php file
##### Null byte injection
The null byte is the utf-8 character that represents "cero bytes" and could be used to upload a php file
- [[lab-file-upload-web-shell-upload-via-obfuscated-file-extension]]
Black listing files like php, but an attacker could circumvet this restriction by using less known alternatives, like for php there's some common alternatives: `.php5`, `.shtml`,

### File type attacks
Another content-disposition used is to check the file type is the content-type parameter, which can be spoofed
##### MIME Type Spoofing:
The application blacklist content type values, but this headercan be modified by capturing/replaying the request with proxies like burp or caido:
- [[lab-file-upload-web-shell-upload-via-content-type-restriction-bypass]]
##### Magic Byte Spoofing:
For some file types, there are magic bytes at the start of the file that indicates that the file is of a certain format. The applicatgion may validate the header by checking the magic header type

### File content Attacks
A more robust check for the server, instead of relying in contet-types or filenames, it checks whether the file uploaded match the required file type, e.g based on specific signatures of the file. Hence, files that can be interpreted as multiple file types are an attacker vector. Different files extensions, such as gifar which is gif and rar

In this example is added php code into an image bynary and gets executed
	[[lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload]]
There are difdferent ways to create specific poligots
- **Phar-JPEG Polygot**
	https://medium.com/swlh/polyglot-files-a-hackers-best-friend-850bf812dd8a
- **JS+JPEG Polyglot**
	https://portswigger.net/research/bypassing-csp-using-polyglot-jpegs
- **HTML+PDF Polyglot**
	https://arxiv.org/pdf/1907.12735
formidable, multer, express-fileupload, formidable, graphql-upload, connect-multiparty, multer 