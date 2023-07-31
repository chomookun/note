# Jenkins


# Script Sample

## Docker Image Build (with ECR)
```bash
# defines
image=xxxxxxx.dkr.ecr.ap-northeast-2.amazonaws.com/oopscraft/arch4j-web

# docker builds and push
cd arch4j-web
aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin xxxxxxxx.dkr.ecr.ap-northeast-2.amazonaws.com
sudo docker rmi $(sudo docker images ${image} -q) || true
sudo docker build -t ${image}:${STAGE} .
sudo docker push ${image}:${STAGE}
```

## Web Application Deploy (with EC2 instance)
```bash
# define
keypair=~/.aws/keypair_dev.pem
port=8080
ips=("10.149.235.119" "10.149.235.20")

# deploy
for ip in ${ips[@]}
do
  # copy and restart
  scp -i ${keypair} ./arch4j-web/build/libs/arch4j-web-*-BOOT.jar ec2-user@${ip}:~/
  scp -i ${keypair} ./arch4j-web/arch4j-web.sh ec2-user@${ip}:~/
  ssh -i ${keypair} ec2-user@${ip} "./arch4j-web.sh restart --spring.profiles.active=${STAGE}"
  
  # check response
  for i in $(seq 10)
  do 
   sleep 10
      if [ ${i} -ge 10 ]
      then 
       exit 1
      fi
   res_code=$(curl -L -s -o /dev/null -w "%{http_code}" "http://${ip}:${port}")
   if [ ${res_code} -ne "000" ]
   then
       break;
      fi
  done || true
done
```

## Web Application Deploy (with AWS ECR)
```bash
# defines
image=xxxxxxxx.dkr.ecr.ap-northeast-2.amazonaws.com/oopscraft/arch4j-web
keypair=~/.aws/keypair.pem
port=8080
ips=("192.168.0.100" "192.168.0.101")

# docker builds and push
cd arch4j-web
aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin xxxxxx.dkr.ecr.ap-northeast-2.amazonaws.com
sudo docker rmi $(sudo docker images ${image} -q) || true
sudo docker build -t ${image}:${STAGE} .
sudo docker push ${image}:${STAGE}

# deploy
for ip in ${ips[@]}
do
  # remote restart
  ssh -i ${keypair} ec2-user@${ip} "aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin xxxxxxxx.dkr.ecr.ap-northeast-2.amazonaws.com"
  ssh -i ${keypair} ec2-user@${ip} "sudo docker pull ${image}:${STAGE}"
  ssh -i ${keypair} ec2-user@${ip} "sudo docker stop \$(sudo docker container ps -q) || true"
  ssh -i ${keypair} ec2-user@${ip} "sudo docker run -d -e SPRING_PROFILES_ACTIVE=${STAGE} -p ${port}:${port} -v /home/ec2-user/logs:/home/ec2-user/logs ${image}:${STAGE}"
  
  # check response
  for i in $(seq 10)
  do 
   sleep 30
      if [ ${i} -ge 10 ]
      then 
       exit 1
      fi
   res_code=$(curl -L -s -o /dev/null -w "%{http_code}" "http://${ip}:${port}")
   if [ ${res_code} -ne "000" ]
   then
       break;
      fi
  done || true
done
```