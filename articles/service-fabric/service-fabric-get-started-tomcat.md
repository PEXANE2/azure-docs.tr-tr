---
title: Linux'ta Apache Tomcat için bir konteyner oluşturma
description: Azure Service Fabric'te Apache Tomcat sunucusunda çalışan bir uygulamayı ortaya çıkarmak için Linux kapsayıcısı oluşturun. Uygulamanız ve Apache Tomcat sunucunuzla bir Docker görüntüsü oluşturun, görüntüyü bir konteyner kayıt defterine itin, Service Fabric konteyner uygulaması oluşturun ve dağıtın.
ms.topic: conceptual
ms.date: 6/08/2018
ms.author: pepogors
ms.openlocfilehash: 1a699f3b35970270a9800162a6d8717682a168ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614426"
---
# <a name="create-service-fabric-container-running-apache-tomcat-server-on-linux"></a>Linux üzerinde Apache Tomcat sunucu çalıştıran Service Fabric konteyner oluşturun
Apache Tomcat, Java Servlet ve Java Server teknolojilerinin popüler, açık kaynak uygulamasıdır. Bu makalede, Apache Tomcat ve basit bir Web uygulaması ile bir kapsayıcı oluşturmak, Linux çalıştıran bir Service Fabric küme için konteyner dağıtmak ve Web uygulamasına bağlanmak nasıl gösterilmektedir.  

Apache Tomcat hakkında daha fazla bilgi edinmek için [Apache Tomcat ana sayfasına](https://tomcat.apache.org/)bakın. 

## <a name="prerequisites"></a>Ön koşullar
* Şunları çalıştıran bir geliştirme bilgisayarı:
  * [Service Fabric SDK’sı ve araçları](service-fabric-get-started-linux.md).
  * [Linux için Docker CE](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric CLI](service-fabric-cli.md)

* Azure Kapsayıcı Kayıt Defteri'nde bir kapsayıcı kayıt defteri. [Azure portalı](../container-registry/container-registry-get-started-portal.md) veya [Azure CLI'yi](./service-fabric-tutorial-create-container-images.md#deploy-azure-container-registry)kullanarak Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturabilirsiniz. 

## <a name="build-a-tomcat-image-and-run-it-locally"></a>Bir Tomcat görüntüsü oluşturun ve yerel olarak çalıştırın
Apache Tomcat görüntüsünü ve basit bir Web uygulamasını temel alan bir Docker görüntüsü oluşturmak için bu bölümdeki adımları izleyin ve ardından yerel sisteminizdeki bir kapsayıcıda çalıştırın. 
 
1. Geliştirme bilgisayarınızdaJava örnekleri deposu [ile başlayan Hizmet Kumaşını](https://github.com/Azure-Samples/service-fabric-java-getting-started) klonla.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   ```

1. Dizinleri Apache Tomcat sunucu örnek dizinine değiştirin (*service-fabric-java-getting-started/container-apache-tomcat-web-server-sample):*

   ```bash
   cd service-fabric-java-getting-started/container-apache-tomcat-web-server-sample
   ```

1. Docker Hub'da bulunan resmi [Tomcat resmi görüntüsünü](https://hub.docker.com/_/tomcat/) ve Tomcat sunucu örneğini temel alan bir Docker dosyası oluşturun. *Service-fabric-java-getting-started/container-apache-tomcat-web-server-sample* directory'sinde *Dockerfile* (dosya uzantısı olmadan) adında bir dosya oluşturun. Aşağıdakini *Dockerfile* dosyasına ekleyin ve değişikliklerinizi kaydedin:

   ```
   FROM library/tomcat

   EXPOSE 8080

   COPY ./ApacheTomcat /usr/local/tomcat
   ```

   Daha fazla bilgi için [Dockerfile başvurusuna](https://docs.docker.com/engine/reference/builder/) bakın.


4. Web `docker build` uygulamanızı çalıştıran görüntüyü oluşturmak için komutu çalıştırın:

   ```bash
   docker build . -t tomcattest
   ```

   Bu komut, Dockerfile'daki yönergeleri kullanarak yeni görüntüyü oluşturur ve görüntüyü `tomcattest`adlandırma (-t etiketleme) oluşturur. Bir kapsayıcı görüntüsü oluşturmak için, temel görüntü önce Docker Hub'dan indirilir ve uygulama eklenir. 

   Oluşturma komutu tamamlandıktan sonra, yeni görüntü üzerindeki bilgileri görmek için `docker images` komutunu çalıştırın:

   ```bash
   $ docker images
    
   REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
   tomcattest                    latest              86838648aab6        2 minutes ago       194 MB
   ```

5. Kapsayıcılı uygulamanızın kapsayıcı kayıt defterini itmeden önce yerel olarak çalıştığını doğrulayın:
 
   ```bash
   docker run -itd --name tomcat-site -p 8080:8080 tomcattest.
   ```
   
   * `--name`kapsayıcıyı adlandırır, böylece kimliği yerine dostu bir ad kullanarak ona başvurabilirsiniz.
   * `-p`kapsayıcı ve ana bilgisayar işletim sistemi arasındaki bağlantı noktası eşleğini belirtir. 

   > [!Note]
   > `-p` Parametre ile açtığınız bağlantı noktası, Tomcat uygulamanızın isteklerini dinlediği bağlantı noktası olmalıdır. Geçerli örnekte, *ApacheTomcat/conf/server.xml* dosyasında HTTP istekleri için bağlantı noktası 8080'i dinlemek üzere yapılandırılan bir Bağlayıcı vardır. Bu bağlantı noktası, ana bilgisayardaki 8080 portuna eşlenir. 

   Diğer parametreler hakkında bilgi edinmek için [Docker çalıştırılabelgesini](https://docs.docker.com/engine/reference/commandline/run/)görün.

1. Kapsayıcınızı test etmek için bir tarayıcı açın ve aşağıdaki URL'lerden birini girin. "Merhaba Dünya!" her URL için hoş geldiniz ekranı.

   - `http://localhost:8080/hello` 
   - `http://localhost:8080/hello/sayhello` 
   - `http://localhost:8080/hello/sayhi` 

   ![Merhaba dünya /sayhi](./media/service-fabric-get-started-tomcat/hello.png)

2. Kapsayıcıyı durdurun ve geliştirme bilgisayarınızdan silin:

   ```bash
   docker stop tomcat-site
   docker rm tomcat-site
   ```

## <a name="push-the-tomcat-image-to-your-container-registry"></a>Tomcat görüntüsünü konteyner kayıt defterinize itin
Tomcat görüntüsünün geliştirme bilgisayarınızdaki bir kapsayıcıda çalıştığını doğruladığınıza göre, bunu konteyner kayıt defterindeki bir depoya itin. Bu makalede, görüntüyü depolamak için Azure Kapsayıcı Kayıt Defteri kullanılır, ancak bazı adımlar değişikliğiyle seçtiğiniz herhangi bir kapsayıcı kayıt defterini kullanabilirsiniz. Bu makalede, kayıt defteri adı *myregistry* olarak kabul edilir ve tam kayıt defteri adı myregistry.azurecr.io. Bunları senaryonuz için uygun şekilde değiştirin. 

1. Kayıt `docker login` defteri [kimlik bilgilerinizle](../container-registry/container-registry-authentication.md)konteyner kayıt defterinizde oturum açmanız için çalıştırın.

   Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/develop/app-objects-and-service-principals.md) kimliği ve parolası geçirilmiştir. Örneğin, bir otomasyon senaryosu için kayıt defterinize bir hizmet sorumlusu atamış olabilirsiniz. Veya, kayıt defteri kullanıcı adınızı ve parolanızı kullanarak oturum açabilirsiniz.

   ```bash
   docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
   ```

2. Aşağıdaki komut, görüntünün kayıt defterinize ait tam yolu içeren bir etiketini veya diğer adını oluşturur. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanına görüntüyü yerleştirir.

   ```bash
   docker tag tomcattest myregistry.azurecr.io/samples/tomcattest
   ```

3. Görüntüyü kapsayıcı kayıt defterinize gönderin:

   ```bash
   docker push myregistry.azurecr.io/samples/tomcattest
   ```

## <a name="build-and-deploy-the-service-fabric-container-application"></a>Service Fabric konteyner uygulamasını oluşturun ve dağıtın
Artık Tomcat görüntüsünü bir konteyner kayıt defterine ittiğinize göre, Tomcat görüntüsünü kayıt defterinizden çeken ve kümenizde kapsayıcı bir hizmet olarak çalıştıran bir Service Fabric kapsayıcı uygulaması oluşturabilir ve dağıtabilirsiniz. 

1. Yerel klonunuzun dışında yeni bir dizin oluşturun *(hizmet-kumaş-java-getting-started* directory ağacının dışında). Ona geçin ve bir konteyner uygulaması için bir iskele oluşturmak için Yeoman kullanın: 

   ```bash
   yo azuresfcontainer 
   ```
   İstendiğinde aşağıdaki değerleri girin:

   * Başvurunuzu adlandırın: ServiceFabricTomcat
   * Uygulama hizmetinin adı: TomcatService
   * Resim Adı Girişi: Konteyner kayıt defterinizdeki kapsayıcı resminin URL'sini sağlayın; örneğin, myregistry.azurecr.io/samples/tomcattest.
   * Komutlar: Bunu boş bırakın. Bu görüntüde iş yükü giriş noktası tanımlanmış olduğundan, giriş komutlarının açıkça belirtilmesi gerekmez (komutlar kapsayıcının içinde çalıştırılır ve bu da başlatma sonrasında kapsayıcıyı çalışır durumda tutar).
   * Konuk konteyner uygulama örnekleri sayısı: 1

   ![Kapsayıcılar için Service Fabric Yeoman oluşturucusu](./media/service-fabric-get-started-tomcat/yo-generator.png)

10. Hizmet manifestosunda *(ServiceFabricTomcat/ServiceFabricTomcat/TomcatServicePkg/ServiceManifest.xml),* uygulamanızın isteklerini dinlediği portu açmak için root **ServiceManfest** etiketinin altına aşağıdaki XML'yi ekleyin. **Endpoint** etiketi bitiş noktası için protokolü ve bağlantı noktasını bildirir. Bu makale için, konteynerservis port 8080 dinler: 

   ```xml
   <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
       listen. Please note that if your service is partitioned, this port is shared with 
       replicas of different partitions that are placed in your code. -->
      <Endpoint Name="endpointTest" Port="8080" Protocol="tcp"/>
    </Endpoints>
   </Resources>
   ```

11. Uygulama bildiriminde (*ServiceFabricTomcat/ServiceFabricTomcat/ApplicationManifest.xml),* **ServiceManifestImport** etiketi altında aşağıdaki XML'yi ekleyin. **Depo Kimlik Bilgileri** etiketindeki Hesap **Adı** ve **Parola'yı** konteyner kayıt defterinizin adı ve oturum açmanız için gereken parolayla değiştirin.

   ```xml
   <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="8080" EndpointRef="endpointTest"/>
      <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
    </ContainerHostPolicies>
   </Policies>
   ```

   **ContainerHostPolicies** etiketi, kapsayıcı ana bilgisayarlarını etkinleştirme ilkeleri belirtir.
    
   * **PortBinding** etiketi, kapsayıcı bağlantı noktası bağlantı noktası eşleme ilkesini yapılandırır. Konteyner Dockerfile belirtildiği gibi port 8080 ortaya çıkarır, çünkü **ContainerPort** özniteliği 8080 olarak ayarlanır. **EndpointRef** özniteliği, önceki adımda hizmet bildiriminde tanımlanan bitiş noktası olan "endpointTest" olarak ayarlanır. Böylece, 8080 portundaki servise gelen talepler konteyner üzerindeki 8080 portuna eşlenir. 
   * **Depo Kimlik Bilgileri** etiketi, kapsayıcının görüntüyü çektiği (özel) depoyla kimlik doğrulaması gereken kimlik bilgilerini belirtir. Görüntü halka açık bir depodan çekilecekse bu politikaya ihtiyacınız yoktur.
    

12. *ServiceFabricTomcat* klasöründe servis kumaş kümenize bağlanın. 

   * Yerel Service Fabric kümesine bağlanmak için çalıştırın:

     ```bash
     sfctl cluster select --endpoint http://localhost:19080
     ```
    
   * Güvenli bir Azure kümesine bağlanmak için, istemci sertifikasının *ServiceFabricTomcat* dizininde .pem dosyası olarak bulunduğundan emin olun ve çalıştırın: 

     ```bash
     sfctl cluster select --endpoint https://PublicIPorFQDN:19080 -pem your-certificate.pem -no-verify
     ```
     Önceki komutta, istemci `your-certificate.pem` sertifika dosyanızın adıyla değiştirin. Geliştirme ve test ortamlarında küme sertifikası genellikle istemci sertifikası olarak kullanılır. Sertifikanız kendi imzalı değilse, parametreyi `-no-verify` atla. 
       
     Küme sertifikaları genellikle .pfx dosyaları olarak yerel olarak indirilir. Pem formatında sertifikanız zaten yoksa, .pfx dosyasından bir .pem dosyası oluşturmak için aşağıdaki komutu çalıştırabilirsiniz:

     ```bash
     openssl pkcs12 -in your-certificate.pfx -out your-certificate.pem -nodes -passin pass:your-pfx-password
     ```

     .pfx dosyanız parola korumalı `-passin pass:` değilse, son parametreyi kullanın.


13. Uygulamayı kümenize dağıtmak için şablonda sağlanan yükleme komut dosyasını çalıştırın. Komut dosyası, uygulama paketini kümenin görüntü deposuna kopyalar, uygulama türünü kaydeder ve uygulamanın bir örneğini oluşturur.

     ```bash
     ./install.sh
     ```

   Yükleme komut dosyasını çalıştırdıktan sonra bir tarayıcı açın ve Service Fabric Explorer'a gidin:
    
   * Yerel bir kümede, (Mac `http://localhost:19080/Explorer` OS X'te kullanıyorsanız yerel ev *sahibini* VM'nin özel IP'si ile değiştirin).
   * Güvenli bir Azure `https://PublicIPorFQDN:19080/Explorer`kümesinde. 
    
   **Uygulamalar** düğümünüzü genişletin ve artık uygulama türünüz, **ServiceFabricTomcatType**ve bu tür ilk örneği için başka bir giriş olduğunu unutmayın. Uygulamanın tam olarak dağıtılması birkaç dakika sürebilir, bu nedenle sabırlı olun.

   ![Service Fabric Explorer](./media/service-fabric-get-started-tomcat/service-fabric-explorer.png)


1. Tomcat sunucusundaki uygulamaya erişmek için bir tarayıcı penceresi açın ve aşağıdaki URL'lerden birini girin. Yerel kümeye dağıttıysanız, *PublicIPorFQDN*için *localhost'u* kullanın. "Merhaba Dünya!" her URL için hoş geldiniz ekranı.

   * http://PublicIPorFQDN:8080/hello  
   * http://PublicIPorFQDN:8080/hello/sayhello
   * http://PublicIPorFQDN:8080/hello/sayhi

## <a name="clean-up"></a>Temizleme
Uygulama örneğini kümenizden silmek ve uygulama türünü kaldırmak için şablonda sağlanan kaldırma komut dosyasını kullanın.

```bash
./uninstall.sh
```

Görüntüyü kapsayıcı kayıt defterine gönderdikten sonra yerel görüntüyü geliştirme bilgisayarınızdan silebilirsiniz:

```
docker rmi tomcattest
docker rmi myregistry.azurecr.io/samples/tomcattest
```

## <a name="next-steps"></a>Sonraki adımlar
* Ek Linux kapsayıcı özellikleri nde hızlı adımlar için, [Linux'ta ilk Service Fabric konteyner uygulamanızı oluşturun'i](service-fabric-get-started-containers-linux.md)okuyun.
* Linux kapsayıcıları hakkında daha ayrıntılı adımlar için, [Bir Linux konteyner uygulaması öğretici oluştur'u](service-fabric-tutorial-create-container-images.md) okuyun.
* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.


