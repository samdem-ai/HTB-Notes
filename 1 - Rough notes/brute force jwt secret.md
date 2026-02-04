
there are multiple ways:
```
echo "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiYWRtaW4iLCJlbWFpbCI6ImFkbWluQGdtYWlsLmNvbSIsImlzcyI6ImxvY2FsaG9zdCIsImF1ZCI6ImxvY2FsaG9zdCIsImlhdCI6MTc1NjcwNjQwNywiZXhwIjoxNzU2NzQ5Nzc3fQ.vdt28jJW4e8kmiQheb9yYpYPS7B1UXpEoitF0GyAAxo" > hash```
```

best way for me:
```
john --wordlist=Documents/rockyou.txt --format=HMAC-SHA256 hash
```

``` hashcat -a 0 -m 16500 hash Documents/rockyou.txt```

or using jwtcat

```
python jwtcat.py brute-force "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiZGVtb191c2VyIiwiZW1haWwiOiJkZW1vX2VtYWlsIiwiaXNzIjoibG9jYWxob3N0IiwiYXVkIjoibG9jYWxob3N0IiwiaWF0IjoxNzU2NzA2NDA3LCJleHAiOjE3NTY3MTAwMDd9.wI6hVkcK_c_YMUfGRmGPAXANns2bko66UfnbBxEXjno"
```
