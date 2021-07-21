# helmChart-Job-Example

## [single cronjob 실행 방법] 
1. kubectl 로 테스트 
    ```shell
    $ cd helm-cronjob-example/single-cronjob
    $ kubectl create -f templates/cronjob.yaml
    ```
    - cronjob 작업 상태 확인
    ```shell
    $ kubectl get cronjob  
   
    NAME              SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
    hello-world-job   */1 * * * *   False     0        34s             5m48s
    
   ```
    - cronjob 삭제
    ```shell
    $ kubectl delete cronjob.batch/hello-world-job
    ```

2. helm chart 로 설치
    ```shell
   $ cd helm-cronjob-example/single-cronjob

    # helm install 명령어
    $ helm install [helm name] [heml-chart folder]

    # 여기서는 아래와같이 실행하면 된다.
    $ helm install helm-cronjob[helm name] .[heml-chart folder]

    ```
   - helm 작업 상태 확인 
    ```shell
    $ helm list

    NAME        	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART            	APP VERSION
    helm-cronjob	default  	1       	2021-07-21 16:05:46.42831 +0900 KST 	deployed	hello-world-0.1.0	1.16.0     
    ```
3. 수행 결과

- cronjob 실행 중 
```shell
Every 1.0s: kubectl get all                                                             iMac.local: Wed Jul 21 16:07:12 2021

NAME                                   READY   STATUS              RESTARTS   AGE
pod/hello-world-job-1626851220-f2q5l   0/1     ContainerCreating   0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   128d

NAME                                   COMPLETIONS   DURATION   AGE
job.batch/hello-world-job-1626851220   0/1           4s         4s

NAME                            SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
cronjob.batch/hello-world-job   */1 * * * *   False     1        12s             86s

```

- cronjob 종료 

```shell

Every 1.0s: kubectl get all                                                             iMac.local: Wed Jul 21 16:06:24 2021

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   128d

NAME                            SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
cronjob.batch/hello-world-job   */1 * * * *   False     0        24s             38s

```

[comment]: <> (## [multi cronjob 실행 방법])

[comment]: <> (작업중...)



