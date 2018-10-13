
# Lab 900 - Oracle Streams Analytics - Retail Demo

## Start Ravello OSA App and determine instance url
![](images/900/image900_1.png)

Log into to OSA URL is http://yourhostname:9080/osa

![](images/900/image900_2.png)

![](images/900/image900_3.png)

Select Catalog

![](images/900/image900_4.png)

## 1.	Create pipeline
Tag: retail
Stream: Customer Locations

![](images/900/image900_5.png)

![](images/900/image900_6.png)

![](images/900/image900_7.png)


## 2.	Add pattern stage for left outerjoin on customers (enrich customer location data)
Stream: Customers
Correlation: CUSTID = CUSTID

![](images/900/image900_8.png)

![](images/900/image900_9.png)

![](images/900/image900_10.png)

![](images/900/image900_11.png)

## 3. start retail stream into Kafka
Log into instance (ssh)
$ cd ~/demo
$ retail

![](images/900/image900_12.png)

Leave stream running in ssh and view streaming data in OSA console

![](images/900/image900_13.png)


## 4.	Add pattern for Spatial -> Geo Fence (visualization)
Malls San Jose
Parameters
Latitude: lat
Longitude: long
Cust id

![](images/900/image900_14.png)

![](images/900/image900_15.png)

![](images/900/image900_16.png)

![](images/900/image900_17.png)

![](images/900/image900_18.png)

![](images/900/image900_19.png)

* View visualization tab

![](images/900/image900_20.png)

## 5.	Add scoring stage (ML algorithm) 
Model Name: Predict Customer Purchase
Model Version: 2.0
Mapping: CUSTID   FCUSTID

![](images/900/image900_21.png)

![](images/900/image900_22.png)

![](images/900/image900_23.png)

## 6.	Add query filter on probability (identify likely buyers – top 30%)
Probability1 greater than or equal 0.7

![](images/900/image900_24.png)

![](images/900/image900_25.png)

![](images/900/image900_26.png)

![](images/900/image900_27.png)

![](images/900/image900_28.png)


## 7.	Add target (send likely buyers offers)
send offer 
(* may call rest endpoint or API)

![](images/900/image900_29.png)

![](images/900/image900_30.png)

![](images/900/image900_31.png)