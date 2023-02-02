# Template 문서
해당 문서는 bookinfo cicd를 위한 template 설명이 작성 되어 있습니다.<br>
IntegrationConfig에 관한 내용은 [<span style="color:yellow">"문서"</span>](https://github.com/tmax-cloud/cicd-operator/blob/master/docs/integration_config.md)에서 보다 상세하게 확인이 가능합니다.

## 기본 필드 값
```
apiVersion: tmax.io/v1
kind: ClusterTemplate
metadata:
  labels:
    cicd-template-test: reviews
  name: tmaxcloud-platofrmps-template
categories:
  - reviews
imageUrl: >-
    https://static.wixstatic.com/media/adcb2b_6acdb885bf744458bf152ab5854621cd~mv2.jpg/v1/fill/w_562,h_528,al_c,lg_1,q_80,enc_auto/adcb2b_6acdb885bf744458bf152ab5854621cd~mv2.jpg
longDescription: TmaxCloud CI/CD Test Sample
markdownDescription: reviews-test-template
objectKinds:
  - PersistentVolumeClaim
  - ConfigMap
  - Secret
  - IntegrationConfig
```
- categories : 좌측에 표시는 카테고리 종류 입력 (배열이기 때문에 여러 종류 입력 가능)
- imageUrl : 미리보기 이미지 주소(주로 해당 모듈의 로고 등 사용)
- longDescription : 서비스 인스턴스 생성 화면에서 이름 아래 보여지는 설명
- markdownDescription : 잘 모르겠슴다... 어디에 사용되는 값인지 모르겠어요... OTL
- objectKinds : 해당 템플릿을 통해 생성되는 object들의 종류


##  parameters
```
parameters:
  - description: Application name
    displayName: AppName
    name: APP_NAME
    required: true
    value: testapp
    valueType: string
  - description: Git Type (gitlab or github)
    displayName: GitType
    name: GIT_TYPE
    required: true
    value: gitlab
    valueType: string
  - description: 'Git API URL (e.g., http://)'
    displayName: GitApiUrl
    name: GIT_API_URL
    required: false
    value: gitlab.jhcloud.kr
    valueType: string
  - description: 'Git Repo. (e.g., tmax-cloud/cicd-operator)'
    displayName: GitRepository
    name: GIT_REPOSITORY
    required: true
    value: root/bookinfo
    valueType: string
・・・・・ 이하 생략
```
- 서비스 카탈로그를 통해 만들어지는 오브젝트들의 생성에 필요한 변수의 값을 입력 받아 적용.
- description : 값에 대한 설명을 작성하는 부분
- displayName : 템플릿 작성시 구분을 위한 이름 (사실 이름만 봐서는 해당 부분의 값이 콘솔을 통해 보지는게 맞는거 같다 - 버그로 추정)
- name : 콘솔에 보여지는 이름
- required : 필수 입력 여부 (true : 필수 입력 / false : 생략 가능)
- value : default 입력 값 (해당 구문을 지우거나 공란으로 둘 경우 default 입력 값 없음)
- valueType : value의 타입 (string, number. array 등이 있음)
- 입력 받은 parameters의 값을 사용하기 위해서는 <span style="color:yellow">"${변수이름}"</span> 형식으로 사용

## objects
```
objects:
  - apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: 'cicd-pvc-${APP_NAME}'
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 1Gi
      storageClassName: '${STORAGE_CLASS}'
  - apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: 'build-pvc-${APP_NAME}'
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 1Gi
      storageClassName: '${STORAGE_CLASS}'
・・・・ 이하 생략
```
- 필요 오브젝트를 생성하는 부분
- 이 부분에 pvc, secret, configmap, integrationconfig 등 정의