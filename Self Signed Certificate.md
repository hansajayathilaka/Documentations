# Create Privet and Public Key
	openssl req -new -newkey rsa:2048 -nodes -keyout localhost.key -out localhost.csr

# Create Actual Certificate
	openssl x509 -req -days 365 -in localhost.csr -signkey localhost.key -out localhost.crt

# Output Files
	-rw-r--r-- 1 root root     1383 ඔක්    7 15:01 localhost.crt
	-rw-r--r-- 1 root root     1115 ඔක්    7 14:59 localhost.csr
	-rw------- 1 root root     1704 ඔක්    7 14:59 localhost.key

# Youtube Video Link
	https://youtu.be/LHUbQtUeQ0o

