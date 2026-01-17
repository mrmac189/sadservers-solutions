```bash
nohup /bin/bash -c 'while true; do echo "this is a test message being sent to the pipe" > /home/admin/namedpipe;  sleep 2; done' &
```
Without `sleep 2;` the command spams the STDIN making logging unpredictable 
