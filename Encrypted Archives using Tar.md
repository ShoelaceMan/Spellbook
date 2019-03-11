You have to apply the unix-philosophy to this: one tool for each task.
Tarring and compression is a job for tar and either `gzip` or `bzip2`, crypto is a job for either `gpg` or `openssl`

Encrypt using SSL (With passwords)
===
`tar -cvpz /path/to/folder/ | openssl enc -aes-256-cbc -e -out "/path/to/filename.tar.gz.enc" -pass pass:passwordgoeshere`

Encrypt using GPG
===
`gpg --encrypt filename.tar.gz`

Decrypt using SSL
===
`openssl enc -aes-256-cbc -d -in filename.tar.gz.enc | tar xzv`