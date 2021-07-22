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


## [multi cronjob 실행 방법]
1. kubectl 로 테스트
    ```shell
    $ cd helm-cronjob-example/multi-cronjob
    $ kubectl create -f templates/cronjob.yaml
    ```
   - cronjob 작업 상태 확인
    ```shell
    $ kubectl get cronjob  

      Every 1.0s: kubectl get all                                                             iMac.local: Thu Jul 22 15:52:25 2021
      
      NAME                              SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
      cronjob.batch/hello-world-2-job   */2 * * * *   False     0        25s             6m14s
      cronjob.batch/hello-world-job     */1 * * * *   False     0        25s             6m14s
      

   ```
   - cronjob 삭제
    ```shell
    $ kubectl delete cronjob.batch/hello-world-job
    ```

2. helm chart 로 설치
    ```shell
   $ cd helm-cronjob-example/multi-cronjob
   
   # helm install 명령어
   $ helm install [helm name] [heml-chart folder]
   
   # 여기서는 아래와같이 실행하면 된다.
   $ helm install helm-cronjob[helm name] .[heml-chart folder]

    ```
   - helm 작업 상태 확인
    ```shell
    $ helm list

    NAME        	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART            	APP VERSION
    helm-cronjob2	default  	1       	2021-07-22 15:46:10.901233 +0900 KST	deployed	hello-world-2-0.1.0	1.16.0     
    ```
3. 수행 결과

   - cronjob 시작

   ```shell
   Every 1.0s: kubectl get all                                                             iMac.local: Thu Jul 22 15:55:40 2021
   
   NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
   service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   129d
   
   NAME                              SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
   cronjob.batch/hello-world-2-job   */2 * * * *   False     0        <none>          6s
   cronjob.batch/hello-world-job     */1 * * * *   False     0        <none>          6s
   ```

   - cronjob 실행 중
   ```shell
   Every 1.0s: kubectl get all                                                             iMac.local: Thu Jul 22 15:54:59 2021
   
   NAME                                     READY   STATUS      RESTARTS   AGE
   pod/hello-world-2-job-1626936600-lhsp6   0/1     Completed   0          4m51s
   pod/hello-world-2-job-1626936720-pszsv   0/1     Completed   0          2m51s
   pod/hello-world-2-job-1626936840-xc696   0/1     Completed   0          50s
   pod/hello-world-job-1626936720-tgf7f     0/1     Completed   0          2m51s
   pod/hello-world-job-1626936780-7l85s     0/1     Completed   0          110s
   pod/hello-world-job-1626936840-qzcsj     0/1     Completed   0          50s
   
   NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
   service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   129d
   
   NAME                                     COMPLETIONS   DURATION   AGE
   job.batch/hello-world-2-job-1626936600   1/1           6s         4m51s
   job.batch/hello-world-2-job-1626936720   1/1           8s         2m51s
   job.batch/hello-world-2-job-1626936840   1/1           4s         50s
   job.batch/hello-world-job-1626936720     1/1           5s         2m51s
   job.batch/hello-world-job-1626936780     1/1           4s         110s
   job.batch/hello-world-job-1626936840     1/1           6s         50s
   
   NAME                              SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
   cronjob.batch/hello-world-2-job   */2 * * * *   False     0        59s             8m48s
   cronjob.batch/hello-world-job     */1 * * * *   False     0        59s             8m48s
   

   ```


