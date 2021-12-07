# Jenkins


# Script Sample

## Docker Image Build (with ECR)
```bash
# defines
image=837398788587.dkr.ecr.ap-northeast-2.amazonaws.com/sk/core-boot-batch

# docker builds and push
cd core-boot-batch
aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin 837398788587.dkr.ecr.ap-northeast-2.amazonaws.com
sudo docker rmi $(sudo docker images ${image} -q) || true
sudo docker build -t ${image}:${STAGE} .
sudo docker push ${image}:${STAGE}
```

## Web Application Deploy (with EC2 instance)
```bash
# define
keypair=~/.aws/sk_keypair_dev.pem
port=8080
ips=("10.149.235.119" "10.149.235.20")

# deploy
for ip in ${ips[@]}
do
  # copy and restart
  scp -i ${keypair} ./core-boot-api/build/libs/core-boot-api-*-BOOT.jar ec2-user@${ip}:~/
  scp -i ${keypair} ./core-boot-api/core-boot-api.sh ec2-user@${ip}:~/
  ssh -i ${keypair} ec2-user@${ip} "./core-boot-api.sh restart --spring.profiles.active=${STAGE}"
  
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
image=837398788587.dkr.ecr.ap-northeast-2.amazonaws.com/sk/core-boot-api
keypair=~/.aws/sk_keypair_dev.pem
port=8080
ips=("10.149.235.119" "10.149.235.20")

# docker builds and push
cd core-boot-api
aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin 837398788587.dkr.ecr.ap-northeast-2.amazonaws.com
sudo docker rmi $(sudo docker images ${image} -q) || true
sudo docker build -t ${image}:${STAGE} .
sudo docker push ${image}:${STAGE}

# deploy
for ip in ${ips[@]}
do
  # remote restart
  ssh -i ${keypair} ec2-user@${ip} "aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin 837398788587.dkr.ecr.ap-northeast-2.amazonaws.com"
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