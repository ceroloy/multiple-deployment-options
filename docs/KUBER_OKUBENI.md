## Konteyner ile Kubernetes Cluster'da Çalıştırmak

Bluemix CLI ve Kubernetes Cluster kullanarak servisi bir konteynerde ayağa kaldıracağız.

Docker hakkında Türkçe [Kaynak](http://www.gokhansengun.com/docker-nedir-nasil-calisir-nerede-kullanilir/)

Öncelikle ana sayfadaki adımları izleyerek Bluemix'e giriş yapın ve projenin kaynak kodunu klonlayın. Daha sonra Bluemix CLI ve Kubernetes CLI'yi edinin.


Bluemix üzerinden Docker servisine giriş yapın. 

```
 bx cr login
```

Alanadlarını listelemek için aşağıdaki komutu kullanın.

```
 bx cr namespace-list
```

Eğer bir alan adınız yoksa, konteyneri buluta göndermeden önce bir tane oluşturmalısınız.

 bx cr namespace-add fibonacci_myName
``

Bilgisayarınızda $.../proje_dizini/multiple-deployment-options/service dizininde olduğunuzdan emin olun.

Servisi bir Docker Image olarak oluşturacağız. Komuttaki namespace'e kendi alan adınızı yazın ve Docker Image'ı oluşturun. Komutun sonunda bir nokta olduğuna dikkat edin.

```
 docker build -t registry.ng.bluemix.net/<namespace>/fibonacci:latest .
```

Daha sonra Docker Image'ı buluta yükleyin.

```
 docker push registry.ng.bluemix.net/<namespace>/fibonacci:latest
```

Kubernetes Cluster Oluşturma

Hazır bir clusterı da kullanabilirsiniz.

```
 bx cs cluster-create --name fibonacci-cluster
```

Clusterın hazırlanması biraz sürebilir. Clusterın durumunu aşağıdaki komutla kontrol edebilirsiniz. Hazır clusterın durumu "normal" olmalı. Ayrıca cluster "worker"larının durumu "ready" yani hazır hale gelmeli. 

```
 bx cs clusters
 bx cs workers fibonacci-cluster
```

## Servisi Ayağa Kaldırmak


Konfigurasyon detaylarını "fibonacci-cluster"ı için aşağıdaki komutla indirin.

```
 bx cs cluster-config <cluster-name>
```

Komutun çıktısı aşağıdaki gibi olmalı

```
 OK
The configuration for fibonacci-cluster was downloaded successfully. Export environment variables to start using Kubernetes.

export KUBECONFIG=/home/<myUser>/.bluemix/plugins/container-service/clusters/<cluster-name>/kube-config-hou02-fibonacci-cluster.yml

```

``` export KUBECONFIG=...``` komutunu kopyala-yapıştır yaparak çalıştırın.


``` kubectl get nodes``` komutu ile konfigurasyonun çalıştığını teyit edin.

fibonacci-deployement.yml dosyasındaki <namespace> yerine kendi alan adınızı yazın ve dosyayı kaydedin.

``` 
...
  containers:
    ...

    image: "registry.ng.bluemix.net/<namespace>/fibonacci:latest"
...

``` 


Son olarak Fibonacci Servisini cluster'da ayağa kaldırın.

``` 
kubectl create -f fibonacci-deployment.yml
```


## Fibonacci Servisi Test Etmek

Public IP adresinizi bulmak için aşağıdaki komutu kullanın.

``` 
 bx cs workers fibonacci-cluster
```

Curl komutunda bu IP'yi kullanarak fibonacci servisi çağıracağız.

**1. 1000.ci Fibonacci sayısını bulmak için**
  ```
   curl -v http://<cluster-public-ip>:30080/fibonacci?iteration=1000
  
  #Farklı Fibonacci sayılarını bulmak için iteration'a, ..iteration=6 gibi
  #istediğiniz sayıyı verebilirsiniz.


  {"number":"43466557686937456435688527675040625802564660517371780402481729089536555417949051890403879840079255169295922593080322634775209689623239873322471161642996440906533187938298969649928516003704476137795166849228875","length":209,"iterations":"1000","ms":380}
    
  ```
  
  **2. 5000 milisaniye çalışıp durduğunda son hesapladığı Fibonacci sayısını bulmak için**
  ```
   curl -v http://<cluster-public-ip>:30080/fibonacci?duration=5000

  #Farklı süreleri denemek için duration'ı, ..duration=100 gibi
  #istediğiniz sayıyı verebilirsiniz.

  #100.ci Fibonacci sayısı, ve kaç milisaniyede çalıştığı
  
  {"number":"3239872837186251637496544858080328307436116917364101336101223337069418481637550689364396186268869537553128518864841504149582839430242293284699821045096632707026769499151076153917679497222333479744092150821686390451917888507467367555288646127476003877079206468601188844946416454123836281985702590759523677323137101681532285890150502300571143022913102461005597340737884419197495893307192915877095078779131472651456461428430222019329765892737807168617099290768948085787213861091131452033418244072059853936962373618530526739589264939878047013181","length":541,"iterations":"2588","ms":5000}

  ```

  **3. Hata oluşmasını sağlamak için**
  ```
  curl -v -X POST http://<cluster-public-ip>:30080/fibonacci?crash=true

  #Hata olmaması için crash=false yazmanız yetreli. Kod normal çalışacaktır.

  #Servis çalışmayı sonlandıracaktır.
  HTTP/1.1 500 Internal Server Error
  X-Powered-By: Express
  ...
  ```