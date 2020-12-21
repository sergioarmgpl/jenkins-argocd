push_az:
	az login -u ${AZ_EMAIL} -P ${AZ_PASSWORD}
	az acr login --name ${AZ_REGISTRY}
	docker build . -t ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
	docker push ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
push_base:
	cat ${GCR_KEY} | docker login -u _json_key --password-stdin ${DOCKER_REGISTRY}
	docker build . -t ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
	docker push ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}
clean:
	docker stop api || echo "done" 
	docker rm api || docker rmi ${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER} || echo "everything clean"
deploy:
	curl -sSL -k -o argocd https://${ARGOCD_SERVER}/download/argocd-linux-amd64
	chmod 755 argocd
	./argocd app set ${ARGO_PROJECT} -p dockerImage="${DOCKER_REPO}/${IMG_NAME}:${APP_VERSION}-${BUILD_NUMBER}" \
	-p namespace="${NAMESPACE}" -p message="hello world USAC :) :)" --auth-token ${ARGO_TOKEN}
	./argocd app sync ${ARGO_PROJECT} --auth-token ${ARGO_TOKEN}
	rm argocd
	echo "deployed done"
deploy_helm:
	echo "deploy with helm"
