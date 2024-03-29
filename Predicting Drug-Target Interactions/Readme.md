Aim: To predict effects of Chemical Compounds('Drugs') on cell lines and Biological assays('Targets').

Data Description:

train.csv: Each line of this file represents row,column and value where the rows are indices of compounds and the columns indices of targets.
```
122839,0,4.72
49129,2,4.2
53115,2,4.36
2402,3,5.7
23327,3,4.07
45087,3,5.38
```
missing.csv: Each line of this file represents row,column values, that is the location of matrix where values are missing.
```
89115,2
7402,3
53327,3
95087,3
```
targ_sim.txt: Each line represents target1, target2, similarity value.
```
765,2,4.06
123,3,5.78
345,5,4.07
675,1,5.38
```
correct_cmpd_sim.txt[300GB]: Each line represents compound1, compound2, similarity value.
```
11765,2,4.06
13423,3,5.78
3445,4,4.07
56675,2,5.38
```

Highlights:
- Google Cloud Dataproc provides Spark and Hadoop clusters used for big data and distributed computing.
- Implemented Alternating Least Squares(ALS) algorithm, which Completes the missing values in the Matrix, in a distributed environment on a Spark cluster with 32 quad-core worker nodes which gave 71.2% accuracy within 30mins.
- 300GB of data was processed in 30mins in a most optimal way.

Technology Stack: Google Cloud SDK, Google Cloud Dataproc, Spark, PySpark, Python, MLlib.

Important steps:

- Download Google Cloud SDK and login using your google account credentials to continue with the next steps on its console.
- Select a project associated with your billing account.
- Run the execution steps shown below with your own cluster name, resources and location preferences.
- It is better if you have your storage bucket and cluster in the same region.
- Dont't forget to delete the cluster once you finish the execution otherwise, money will be deducted from your billing account for using the resources.


Execution:
```linux
gcloud dataproc clusters create cluster_name --zone us-east1-b --region us-east1 --num-workers 32 --properties spark:spark.executor.heartbeatInterval=120,spark:spark.default.parallelism=128,spark:spark.executor.extraJavaOptions=-Xss128M,spark:spark.driver.extraJavaOptions=-Xss128M  --initialization-actions gs://kmeans_data/dataproc.init
gcloud dataproc jobs submit pyspark --region us-east1 --cluster cluster_name assign_sim_v8.py -- gs://mscbio2065-data/randomtrain.csv gs://mscbio2065-data/randommissing.csv  output.csv
gcloud dataproc clusters delete cluster_name --region us-east1 --quiet

```
