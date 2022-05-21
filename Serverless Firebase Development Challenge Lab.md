Firstly run all the commands which given in lab itself.

----------------------------TASK 1---------------------------------------

--> Create a Firestore database

- Cloud Firestore Database
- Use Firestore Native Mode
- Add location Nam5 (United States)

```
export DATASET_SERVICE_NAME=
```

```
export FRONTEND_STAGING_SERVICE=
```

```
export FRONTEND_PRODUCTION_SERVICE=
```

--------------------------------TASK 2----------------------------------------------
```
cd pet-theory/lab06/firebase-import-csv/solution
npm install
node index.js netflix_titles_original.csv
```

--------------------------------TASK 3-----------------------------------------------
```
cd ~/pet-theory/lab06/firebase-rest-api/solution-01
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1
gcloud beta run deploy $DATASET_SERVICE_NAME --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.1 --allow-unauthenticated
```

-------------------------------TASK 4-----------------------------------------------

```
cd ~/pet-theory/lab06/firebase-rest-api/solution-02
npm install
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2
gcloud beta run deploy $DATASET_SERVICE_NAME --image gcr.io/$GOOGLE_CLOUD_PROJECT/rest-api:0.2 --allow-unauthenticated
```

Now go to Cloud Run & click on service then copy link from there 

```
SERVICE_URL=<copy url from your netflix-dataset-service>
```
```
curl -X GET $SERVICE_URL/2019
```

-----------------------------TASK 5----------------------------------------------

```
cd ~/pet-theory/lab06/firebase-frontend/public
nano app.js
```

- When file opens firstly comment out first line which starts with const by using "//"

- After that uncomment second line which starts with const and change the link.

- In link you have to paste same link which you copied on cloud run and remember replace link before /2020 only  

- Then Type Ctrl+X then Y then Enter

```
npm install
cd ~/pet-theory/lab06/firebase-frontend
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
gcloud beta run deploy $FRONTEND_STAGING_SERVICE --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-staging:0.1
```

---------------------------TASK 6-------------------------------------------------

```
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
gcloud beta run deploy $FRONTEND_PRODUCTION_SERVICE --image gcr.io/$GOOGLE_CLOUD_PROJECT/frontend-production:0.1
```
