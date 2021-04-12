COMMIT_ID := $(shell git log|head -n1|awk '{print $$2}'|head -c 8)
all: build login tag push
build:
	docker build -t lab1-udemy-make:$(COMMIT_ID) .
login:
	aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 972777588758.dkr.ecr.eu-west-1.amazonaws.com
tag:
	docker tag lab1-udemy-make:$(COMMIT_ID) 972777588758.dkr.ecr.eu-west-1.amazonaws.com/test-lab-make-kubernetes:$(COMMIT_ID)
	docker tag lab1-udemy-make:$(COMMIT_ID) 972777588758.dkr.ecr.eu-west-1.amazonaws.com/test-lab-make-kubernetes:latest
push:
	docker push 972777588758.dkr.ecr.eu-west-1.amazonaws.com/test-lab-make-kubernetes:$(COMMIT_ID)
	docker push 972777588758.dkr.ecr.eu-west-1.amazonaws.com/test-lab-make-kubernetes:latest