docker build --rm -t ck/centos .

docker run --privileged=true -d -p 50022:22 -p 50080:80 -v /Users/ckarthik/tmp:/home/build/tmp --name test ck/centos

ssh build@192.168.99.100 -p 50022

pwd is build
