# WORKS

curl --http1.1 --insecure -v  --dump-header - https://localhost/A/A.html
curl --http2 --insecure -v  --dump-header - https://localhost/A/A.html

curl --http1.1 --insecure -v  --dump-header - https://localhost/B/B.html

# DOESNT WORK

curl --http2 --insecure -v  --dump-header - https://localhost/B/B.html