## Sanal Makinede Çalışmak - Cloud Foundry

Bluemix CLI ve CloudFoundry CLI kullanarak servisi sanal bir makinede ayağa kaldıracağız. 

**Not :** CloudFoundry Command Line Bluemix altında çalışmaktadır. Bu nedenle cf cli komutlarını kullanırken "**bx cf** --komut" şeklinde kullanmalısınız.
             
Şimdi kendi bilgisayarınızda, $.../proje_dizini/multiple-deployment-options/**service** dizininde olduğunuzdan emin olun.

Daha sonra bu dizindeyken, 

  ```
  $ bx cf push
  ```
komutu ile dizindeki Fibonacci mikroservisini bir CloudFoundry sanal makinesinde ayağa kaldırın.


Oluşan loglarda servisinizin URL'sini göreceksiniz. Bu URL adresini kopyalayın.
  

  ```
  ...
  requested state: started
  instances: 1/1
  usage: 256M x 1 instancess
  urls: fibonacci-service-aggregate-gramps.mybluemix.net  #Servis URL'si
  last uploaded: Wed Oct 4 11:23:20 UTC 2017
  stack: cflinuxfs2
  ...
  ```

Servis çalışmaya başladıktan sonra, servis isteklerimizi bu URL aracılığıyla yapacağız. 
Curl komutundaki URL'ye kendi URL'nizi yazın. 
Benim durumumda bu fibonacci-service-aggregate-gramps.mybluemix.net

  **1. 1000.ci Fibonacci sayısını bulmak için**
  ```
  $ curl -v http://fibonacci-service-<random-string>.mybluemix.net/fibonacci?iteration=1000
  
  #Cevap

  {"number":"43466557686937456435688527675040625802564660517371780402481729089536555417949051890403879840079255169295922593080322634775209689623239873322471161642996440906533187938298969649928516003704476137795166849228875","length":209,"iterations":"1000","ms":166}

  #Farklı Fibonacci sayılarını bulmak için iteration'a, ..iteration=6 gibi istediğiniz sayıyı verebilirsiniz.
  ```
  
  **2. 5000 milisaniye çalışıp durduğunda son hesapladığı Fibonacci sayısını bulmak için**
  ```
  $ curl -v http://fibonacci-service-<random-string>.mybluemix.net/fibonacci?duration=5000

  #Cevap

  {"number":"12200568776393358311795938145608518514285062513840604956816948486021034777530794919417191678949426161320208111203758649248108173709993091530675981281103537762594384441927823154990041443649933119458129656450328236630353746408122663540888104508401459234914638689357876787876965039173160644881193973314404680822428220058192230027591823855096918891025541592329233213010717133321003789631715866434682138911662206125637174772090302020390965608296465024304921897409499306077470544762976177612740421023296509071106453511901655692518733714306020618575085111023241622396221823028220601329980654661774909002020006106953600265907303833911112853907422593197745618480313257271010060130981552323189246162177071083388698555814617011739090883865211987433072111394748091396693748668666070480796648665584260774008816024983941130397402329553926034795730359577026008402597017908330383273710563382978177306696544697981766897994933160734239794695135917341546817713918349949072976527692562725408052886992467452873163209286774709601813434290571329502914994835183486883698739510685519271372158227391029596342268884201468039318935543716825662718208143002584990456510785390846476054591965352362968947972365881888140691557109096367476492239113919237851325860528637804756743504458933921495805788280221","length":1271,"iterations":"6079","ms":5000}

  #Farklı süreleri denemek için duration'ı, ..duration=100 gibi istediğiniz sayıyı verebilirsiniz.
  ```

  **3. Hata oluşmasını sağlamak için**
  ```
  $ curl -v -X POST http://fibonacci-service-<random-string>.mybluemix.net/fibonacci?crash=true

  #Hata olmaması için crash=false yazmanız yetreli. Kod normal çalışacaktır.

  #Servis çalışmayı sonlandıracaktır.
  > HTTP/1.1 500 Internal Server Error
  >...
  ```


Daha sonra bulutta çalışan CloudFoundry servislerini temizlemek isterseniz, alanda çalışan CloudFoundry App'lerinizi şu şekilde görüntüleyebiliriz.

  ```
  $ bx cf apps

  #Sanal makinayı durdurmak için
  $ bx cf stop fibonacci-service

  #Sanal makinayı silmek için
  $ bx cf delete fibonacci-service
  ```