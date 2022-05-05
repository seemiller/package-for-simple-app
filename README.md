# package-for-simple-app

Commands to create the package and repository

```shell
export CR_PAT=ghp_ABC123...
echo $CR_PAT | docker login ghcr.io -u seemiller --password-stdin

export VERSION=1.0.0

imgpkg push -f ${VERSION}/bundle -b ghcr.io/seemiller/simple-app:${VERSION}

ytt -f ${VERSION}/bundle/config/values.yaml --data-values-schema-inspect -o openapi-v3 > ${VERSION}/bundle/config/schema-openapi.yaml

ytt -f package-template.yaml  --data-value-file openapi=${VERSION}/bundle/config/schema-openapi.yaml -v version=${VERSION} > ${VERSION}/pacakge.yaml

cp metadata.yaml repo/packages/simple-app.corp.com

cp ${VERSION}/package.yaml repo/packages/simple-app.corp.com/${VERSION}.yaml

kbld -f repo/packages/ --imgpkg-lock-output repo/.imgpkg/images.yml

imgpkg push -b ghcr.io/seemiller/simple-app-repo:${VERSION} -f repo
```