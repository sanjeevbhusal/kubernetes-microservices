### This repository deploys 5 different applications to kubernetes cluster. The 5 applications are

- Voting App
- Redis Database
- Worker App
- Postresql Database
- Voting Result App

### Flow of the application

- User will submit their votes on voting app.
- Voting app will then write to a inmemory redis database.
- Worker app will then fetch the data from redis app and write it to postregsql.
- Result app will fetch the data from postgres and display it on browser.

### Docker Images Used

- Voting App (kodekloud/examplevotingapp_vote:v1)
- Redis Database (redis)
- Worker App (kodekloud/examplevotingapp_worker:v1)
- Postgresql Database (postresql:9.4)
- Voting Result App (kodekloud/examplevotingapp_result:v1)

### Steps

1. Create yaml manifests to deploy pods for all 5 images. Configure all the labels and ports correctly. Donot use any replica sets or deployments.
2. Create yaml manifests to deploy services for all necessary pods. Configure all the labels ports and selectors correctly.

- Voting App will be used by users to cast a vote. So, use a Nodeport to expose port 80 on the pod to pod 30000 on host.
- Redis App will be used by Voting App to update the entry and Worker App to fetch the entry. As it is not used by any external users, Cluster IP Service should be used. Make sure to name the service as `redis` because the voting application and worker application use redis as DNS name.
- Worker App is not used by any other application. So, no service is required.
- Postgresql Database is used by Worker App to update the entry and Voting Result App to fetch the entry. As it is not used by external users, Cluster IP Service should be used. Make sure to name the service as `db` because the voting application and worker application use db as DNS name.
- Voting Result App is used by users to see the result of the voting. So, use a Nodeport to expose port 80 on pod to pod 30001 on host.

### Note:

Run worker pod only after starting postgres database and configuring db service.
