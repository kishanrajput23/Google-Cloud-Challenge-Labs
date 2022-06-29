## TODO: Create a globally unique Google Cloud Storage bucket for artifact storage.
```
GCS_BUCKET = f"gs://<PROJECT_ID>-vertex-challenge-lab"
```
 
## Task 4: Build and train model

Build and compile a TensorFlow BERT sentiment classifier

TODO: Add a hub.KerasLayer for BERT text preprocessing using the hparams dict.

Name the layer 'preprocessing' and store in the variable preprocessor.

```
preprocessor = hub.KerasLayer(hparams['tfhub-bert-preprocessor'],name='preprocessing')
```
 
TODO: Add a trainable hub.KerasLayer for BERT text encoding using the hparams dict.

Name the layer 'BERT_encoder' and store in the variable encoder.

```
encoder = hub.KerasLayer(hparams['tfhub-bert-encoder'], trainable=True, name='BERT_encoder')
```
 
Train and evaluate your BERT sentiment classifier

TODO: Save your BERT sentiment classifier locally.

Hint: Save it to './bert-sentiment-classifier-local'. Note the key name in model.save().

```
"model-dir": "./bert-sentiment-classifier-local"
```
 
## Task 4: Create artifact registry for custom container images

Create Artifact Registry for custom container images

TODO: create a Docker Artifact Registry using the gcloud CLI. Note the required respository-format and location flags.

Documentation link: https://cloud.google.com/sdk/gcloud/reference/artifacts/repositories/create

```
!gcloud artifacts repositories create {ARTIFACT_REGISTRY} \
--repository-format=docker \
--location={REGION} \
--description="Artifact registry for ML custom training images for sentiment classification"
```

Build and submit your container image to Artifact Registry using Cloud Build
 
TODO: use Cloud Build to build and submit your custom model container to your Artifact Registry.

Documentation link: https://cloud.google.com/sdk/gcloud/reference/builds/submit

Hint: make sure the config flag is pointed at {MODEL_DIR}/cloudbuild.yaml defined above and you include your model directory.

```
!gcloud builds submit {MODEL_DIR} --timeout=20m --config {MODEL_DIR}/cloudbuild.yaml
```
 
## Task 5: Define a pipeline using the KFP V2 SDK
 
```
USER = "qwiklabsdemo" 
```
 
TODO: fill in the remaining arguments from the pipeline constructor.

```
display_name=display_name,
     container_uri=container_uri,
     model_serving_container_image_uri=model_serving_container_image_uri,
     base_output_dir=GCS_BASE_OUTPUT_DIR,
```
 
TODO: Generate online predictions using your Vertex Endpoint.

```
endpoint = vertexai.Endpoint(
 endpoint_name=ENDPOINT_NAME,
 project=PROJECT_ID,
 location=REGION
 
)
```
 
TODO: write a movie review to test your model e.g. "The Dark Knight is the best Batman movie!"

```
test_review = "The Dark Knight is the best Batman movie!"
```
 
TODO: use your Endpoint to return prediction for your test_review.

```
prediction = endpoint.predict([test_review])
```
