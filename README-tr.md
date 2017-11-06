# Üç Farklı Platformda Fibonacci Mikroservisi'nin Çalıştırılması

## Bluemix Hesabı Edinme

Bu örnekte bir Bluemix hesabına ihtiyacımız vardır. *30 günlük ücretsiz deneme hesabına [buradan](https://console.bluemix.net/registration/) kayıt yaptırabilirsiniz.*

## Üç Farklı Platform

Bu örnek gösterimde bir Fibonacci Servisini üç farklı mimaride sırasıyla ayağa kaldırıp, 3 farklı API isteği için test edeceğiz.

1. Servis aldığı sıra sayısı (n)'de bulunan fibonacci sayısını döner. **.../fibonacci?iteration=1000**
2. Verilen süre içerisinde son ulaştığı Fibonacci sayısını döner. **.../fibonacci?duration=1000**
3. Koda hatalı girdi yaparak servisin çökmesini test etmek. **.../fibonacci?crash=true**


**Kullanacağımız mimari modelleri ve servisler aşağıdaki tabloda sıralanmıştır.**

  Sayı  | Tür | Ürün | Link
--- | --- | --- | ---
1 | Sanal Makine | [Cloud Foundry](https://www.cloudfoundry.org/) | [Buradan](https://github.com/ceroloy/multiple-deployment-options/blob/README-cf.md)
2 | Konteyner | [Kubernetes cluster](https://kubernetes.io/) | [Buradan](https://github.com/ceroloy/multiple-deployment-options/blob/README-kuber.md)
3 | Sunucusuz Mimari | [OpenWhisk](http://openwhisk.org/) | [Buradan](https://github.com/ceroloy/multiple-deployment-options/blob/README-ow.md)


**Bu gösterim için ayrıca aşağıdaki ek paketlere de ihtiyacımız olacaktır.**

* [Bluemix CLI](https://clis.ng.bluemix.net/ui/home.html)
* [OpenWhisk CLI](https://console.ng.bluemix.net/openwhisk/learn/cli)
* [Bluemix Container Registry plugin](https://console.ng.bluemix.net/docs/cli/plugins/registry/index.html)
* [Bluemix Container Service plugin](https://console.ng.bluemix.net/docs/containers/cs_cli_devtools.html)
* [Node.js](https://nodejs.org), version 6.9.1 (veya sonrası)
* [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/tasks/kubectl/install/) version 1.5.3 (veya sonrası)
* [Docker CLI](https://docs.docker.com/engine/installation/) version 1.9 (veya sonrası)


Servisi iki ayrı şekilde ayağa kaldırıp test edebiliriz. Burada terminalde yazılı komutlarla çalışacağız.


## Bluemix'e Giriş ve Çalışma Alanı Seçmek

Bluemix hesabını açtıktan sonra, bir bölge seçmemiz gerekli.

  Bölge | Link
  --- | --- 
 US South and US East | api.ng.bluemix.net
 Sydney | api.au-syd.bluemix.net
 Germany | api.eu-de.bluemix.net
 United Kingdom | api.eu-gb.bluemix.net

Bu seçtiğimiz bölgenin linkini komuta ekleyerek giriş yapacağız.

# Bluemix'e giriş yapmak

  ```
  $ bx login -a <BOLGE_URL>
    Email> mail adresiniz
    Password> sifreniz

  #Eğer tek kullanımlık kod kullanmak isterseniz

  $ bx login -a <BOLGE_URL> -u <EMAIL> --sso
  One Time Code (Get one at https://iam.ng.bluemix.net/oidc/passcode)> TekKullanımKod

  #Daha sonra organizasyon ve alan seçeceğiz
  $ bx target -o OrganizasyonAdı -s AlanAdı

  ```

# Kaynak Kodu Edinmek

Öncelikle kendi bilgisayarınızda bir proje dizini yaratıp dizin değiştirin.

  ```
  $ cd proje_dizini
  ```

Daha sonra Fibonacci servisinin kaynak kodunu github'dan kendi proje dosyanıza klonlayın.
  

  ```
  #Doğru dizinde olduğunuzdan emin olun
  $ pwd
  $ .../proje_dizini

  $ git init
  $ git clone https://github.com/IBM-Bluemix/multiple-deployment-options.git
  $ cd multiple-deployement-options
  ```

[Cloud Foundry sanal makinası olarak çalıştırmak için buraya tıklayın.](https://github.com/ceroloy/multiple-deployment-options/blob/README-cf.md)

[Docker Konteyner ile Kubernetes üzerinde çalıştırmak için buraya tıklayın.](https://github.com/ceroloy/multiple-deployment-options/blob/README-kuber.md)

[OpenWhisk ile sunucusuz çalıştırmak için buraya tıklayın.](https://github.com/ceroloy/multiple-deployment-options/blob/README-ow.md)