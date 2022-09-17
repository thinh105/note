---
id: tnzheh0omab759v3s5s5emf
title: 1   Fundamental Ideas around Microservices
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: 1 - Fundamental Ideas Around Microservices
updated_imported: '2022-01-17T12:42:56.000Z'
created_imported: '2022-01-17T12:11:20.000Z'
---


# The big problem of Microservices is Data Management

We should use Database-per-Service

![9d1cb2971ad60796c148e0d478a0e167.png](/assets/9d1cb2971ad60796c148e0d478a0e167-2rboh3kcpx0i.png)

So we need Sync communication between services

![bf2bc8c0734d920d91c9136820d05a8e.png](/assets/bf2bc8c0734d920d91c9136820d05a8e-mz6o84293k26.png)

# We have two strategies :
## 1. Sync
![0d6af355c12b6e5461a741552efd77ff.png](/assets/0d6af355c12b6e5461a741552efd77ff-4u33fuleqcp6.png)
![d2c81395b43fa107a0cdac290858db94.png](/assets/d2c81395b43fa107a0cdac290858db94-wjdr41czfett.png)

Web of requests
![8b28954fdbced87be51bd576d013c76f.png](/assets/8b28954fdbced87be51bd576d013c76f-74ri5csfq6fi.png)

## 2. Async
![5d4307ef744c2bcd09acd6e82785c0b2.png](/assets/5d4307ef744c2bcd09acd6e82785c0b2-x6ppd3gp1cd4.png)

We can create the own database for Services D
![299e1d2f2ddfdc744ec095ee1a9525d5.png](/assets/299e1d2f2ddfdc744ec095ee1a9525d5-k00j80n125w7.png)

Everytime some request visit other services and we got database updated with new data, it will emit some events to Event Bus

![8658ba9d104528ad4a5cdec93125208a.png](/assets/8658ba9d104528ad4a5cdec93125208a-9w4vfflfbdc4.png)

And then these event will be sent to Service D and we can also update the necessary info into the Databases D
![09d04b8baa4dfda9f2eeeea0c691fe91.png](/assets/09d04b8baa4dfda9f2eeeea0c691fe91-94p5l4t0hhmx.png)

### Pros And cons on Async Communication
![88182fa6527912702ee1c6835e3a10ac.png](/assets/88182fa6527912702ee1c6835e3a10ac-txs1igq2eagt.png)


