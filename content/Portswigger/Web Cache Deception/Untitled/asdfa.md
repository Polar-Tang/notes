
#### Identify delimiters
```
ffuf -u "https://vendors.paddle.com/dashboard/icons/empty-table.svgFUZZnone.ico" -w wordlist/delimiters.txt -d "email=asd%40trsd.com&csrf=QXAvevw2iutJhBQ8dSNcdE5KaaNJ27m9" -X POST -H "Content-Type: application/x-www-form-urlencoded" -mc "all" -fc 404 -H "Cookie: paddle_session_vendor=eyJpdiI6ImxVdS8xc1RDTTYwZk9pVUxwNGtMZ2c9PSIsInZhbHVlIjoiV0lUdHNzU0wrV0JSSkxoNWRpTmpPY1kzZmxqTFk1SGlJeXJUQU9uN05DejFWbnhlUjBxazhxOHFzajB3VnphZDFacmFNbFlOVzNMTVZWY3ZBOFp1Q0UxOEhFbVBwVHNhWUo2dkVTNDA4OEJVMzJiSmZGd0hXVHM2MGdDbmtkVEsiLCJtYWMiOiI0NTA0OGE1NGRjMWQ5ZGExM2FkNTUyNGQ5Mzk3M2JjNzZlYWRiZjdhNWRmOGQzODhmYzI3NjYwOGRhZTA2NWQ2IiwidGFnIjoiIn0%3D;" -replay-proxy http://127.0.0.1:8080
```
Three delimiters not utilized by the CDN
![[Pasted image 20250405122606.png]]
#### Identify normalization


```
wcvs -u https://vendors.paddle.com/transactions-v -sc "Cookie: paddle_session_vendor=eyJpdiI6ImxVdS8xc1RDTTYwZk9pVUxwNGtMZ2c9PSIsInZhbHVlIjoiV0lUdHNzU0wrV0JSSkxoNWRpTmpPY1kzZmxqTFk1SGlJeXJUQU9uN05DejFWbnhlUjBxazhxOHFzajB3VnphZDFacmFNbFlOVzNMTVZWY3ZBOFp1Q0UxOEhFbVBwVHNhWUo2dkVTNDA4OEJVMzJiSmZGd0hXVHM2MGdDbmtkVEsiLCJtYWMiOiI0NTA0OGE1NGRjMWQ5ZGExM2FkNTUyNGQ5Mzk3M2JjNzZlYWRiZjdhNWRmOGQzODhmYzI3NjYwOGRhZTA2NWQ2IiwidGFnIjoiIn0%3D;" -purl http://127.0.0.1:8080
```
