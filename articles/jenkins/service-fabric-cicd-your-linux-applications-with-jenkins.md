---
title: Jenkins kullanarak Linux uygulamaları için sürekli yapı
description: Jenkins kullanarak Service Fabric Linux uygulamanız için sürekli yapı ve entegrasyon
keywords: jenkins, masmavi, devops, cicd, linux
ms.topic: tutorial
ms.date: 07/31/2018
ms.openlocfilehash: 96e8c3721b5849b874878ea6a846271fb6760ffd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77675242"
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Linux uygulamalarınızı oluşturmak ve dağıtmak için Jenkins'i kullanın

Bu öğretici, Jenkins ortamınızı ayarlamanın birkaç olası yolunun yanı sıra uygulamanızı bir Service Fabric kümesine, bir Hizmet Kumaşı kümesine dağıtmanın farklı yollarını da kapsar. Jenkins'i başarıyla yapılandırmak, Değişiklikleri GitHub'dan çekmek, uygulamanızı oluşturmak ve kümenize dağıtmak için aşağıdaki genel adımları izleyin:

1. [Önkoşulları](#prerequisites)yüklediğinizden emin olun.
1. Sonra Jenkins kurmak için bu bölümlerden birinde adımları izleyin:
   * [Bir Service Fabric küme içinde Jenkins kurmak](#set-up-jenkins-inside-a-service-fabric-cluster), 
   * [Jenkins'i Service Fabric kümesinin dışında ayarlama](#set-up-jenkins-outside-a-service-fabric-cluster)veya
   * [Service Fabric eklentisini mevcut bir Jenkins ortamına yükleyin.](#install-service-fabric-plugin-in-an-existing-jenkins-environment)
1. Jenkins'i ayarladıktan sonra, [Uygulamanızda](#create-and-configure-a-jenkins-job) değişiklikler yapıldığında Jenkins'i tetiklemek ve Değişiklikleri GitHub'dan çekmek ve uygulamanızı oluşturmak için jenkins iş ardışık lığınızı yapılandırır. 
1. Son olarak, uygulamanızı Hizmet Kumaşı kümenize dağıtmak için Jenkins iş sonrası adımını yapılandırın. Uygulamanızı bir kümeye dağıtmak için Jenkins'i yapılandırmanın iki yolu vardır:    
   * Geliştirme ve test ortamları [için, küme yönetimi bitiş noktasını kullanarak dağıtımı yapılandır'ı](#configure-deployment-using-cluster-management-endpoint)kullanın. Bu, ayarlanması en basit dağıtım yöntemidir.
   * Üretim ortamları için, [Azure kimlik bilgilerini kullanarak dağıtımı Yapılandır'ı](#configure-deployment-using-azure-credentials)kullanın. Microsoft bu yöntemi üretim ortamları için önerir, çünkü Azure kimlik bilgileriyle Jenkins işinin sahip olduğu erişimi Azure kaynaklarınızla sınırlandırabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Git'in yerel olarak yüklendiğinden emin olun. İşletim sisteminizi temel alan [Git indirme sayfasından](https://git-scm.com/downloads) uygun Git sürümünü yükleyebilirsiniz. Git'de yeniyseniz, [Git belgelerinden](https://git-scm.com/docs)bu konuda daha fazla bilgi edinin.
- Bu makalede, GitHub'da Başlangıç Örneği [https://github.com/Azure-Samples/service-fabric-java-getting-started](https://github.com/Azure-Samples/service-fabric-java-getting-started) Olan *Hizmet Kumaşı* kullanılır: uygulamanın oluşturulması ve dağıtılması için. Bu depoyu takip etmek için çatallayabilir veya talimatlarda yapılan bazı değişikliklerle kendi GitHub projenizi kullanabilirsiniz.


## <a name="install-service-fabric-plugin-in-an-existing-jenkins-environment"></a>Mevcut Jenkins ortamına Servis Kumaşı eklentisi yükleyin

Mevcut bir Jenkins ortamına Service Fabric eklentisini ekliyorsanız, aşağıdaki adımları yapmanız gerekir:

- [Servis Kumaş CLI (sfctl)](../service-fabric/service-fabric-cli.md). Jenkins'in CLI komutlarını çalıştırabilmesi için CLI'yi kullanıcı düzeyinde değil, sistem düzeyinde yükleyin. 
- Java uygulamalarını dağıtmak için hem Gradle hem de [Open JDK 8.0'ı](../service-fabric/service-fabric-get-started-linux.md#set-up-java-development)yükleyin. 
- .NET Core 2.0 uygulamalarını dağıtmak için [.NET Core 2.0 SDK'yı](../service-fabric/service-fabric-get-started-linux.md#set-up-net-core-20-development)yükleyin. 

Ortamınız için gereken ön koşulları yükledikten sonra, Jenkins marketplace'te Azure Hizmet Kumaş Eklentisi'ni arayabilir ve yükleyebilirsiniz.

Eklentiyi yükledikten sonra, Bir Jenkins [işi oluşturmak ve yapılandırmak](#create-and-configure-a-jenkins-job)için ileri ye atlayın.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins’i bir Service Fabric kümesi içinde ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümler, kapsayıcı örneğinin durumunu kaydetmek için bir Azure depolama hesabı kullanırken kümenin içinde nasıl ayarlanış yapılacağını gösterir.

1. Docker yüklü bir Service Fabric Linux kümesine sahip olduğundan emin olun. Azure'da çalışan Service Fabric kümeleri zaten Docker'a yüklendi. Kümeyi yerel olarak çalıştırıyorsanız (OneBox geliştirme ortamı), Docker'ın makinenize `docker info` komutla yüklü olup olmadığını kontrol edin. Yüklenmezse, aşağıdaki komutları kullanarak yükleyin:

   ```sh
   sudo apt-get install wget
   wget -qO- https://get.docker.io/ | sh
   ``` 

   > [!NOTE]
   > 8081 bağlantı noktasının kümeüzerinde özel bir bitiş noktası olarak belirtildiğinden emin olun. Yerel bir küme kullanıyorsanız, ana bilgisayar makinesinde bağlantı noktası 8081'in açık olduğundan ve halka açık bir IP adresi olduğundan emin olun.
   >

1. Aşağıdaki komutları kullanarak uygulamayı klonla:
   ```sh
   git clone https://github.com/suhuruli/jenkins-container-application.git
   cd jenkins-container-application
   ```

1. Jenkins kapsayıcısının durumunu dosya paylaşımında devam edin:
   1. Kümenizle **aynı bölgede** bir Azure depolama hesabı `sfjenkinsstorage1`oluşturun.
   1. Depolama Hesabı altında bir **Dosya Paylaşımı** oluşturma `sfjenkins`gibi bir adla .
   1. Dosya paylaşımı için **Connect'e** tıklayın ve **Linux'tan Bağlanma**altında görüntülenedeki değerleri not edin , değer aşağıdakine benzer olmalıdır:

      ```sh
      sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
      ```

   > [!NOTE]
   > Cifs hisselerini monte etmek için küme düğümlerine cifs-utils paketinin yüklenmesi gerekir.      
   >

1. Komut dosyasındaki `setupentrypoint.sh` yer tutucu değerlerini adım 2'deki azure depolama ayrıntılarıyla güncelleştirin.
   ```sh
   vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
   ```
   * Yukarıdaki `[REMOTE_FILE_SHARE_LOCATION]` adım `//sfjenkinsstorage1.file.core.windows.net/sfjenkins` 2'de bağlantının çıktısından gelen değerle değiştirin.
   * Yukarıdaki `[FILE_SHARE_CONNECT_OPTIONS_STRING]` adım `vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777` 2'deki değerle değiştirin.

1. **Yalnızca Güvenli Küme:** 
   
   Jenkins'ten güvenli bir kümeüzerinde uygulama dağıtımını yapılandırmak için küme sertifikasına Jenkins kapsayıcısı içinde erişilebilmelidir. *ApplicationManifest.xml* dosyasında, **ContainerHostPolicies** etiketinin altında bu sertifika başvurusunu ekleyin ve küçük parmak izi değerini küme sertifikasınınkiyle güncelleştirin.

   ```xml
   <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
   ```

   Ayrıca, *ApplicationManifest.xml* dosyasındaki **ApplicationManifest** (root) etiketinin altına aşağıdaki satırları ekleyin ve parmak izi değerini küme sertifikasıyla güncelleştirin.

   ```xml
   <Certificates>
     <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[Thumbprint]" />
   </Certificates> 
   ```

1. Kümeye bağlanın ve kapsayıcı uygulamasını yükleyin.

   **Güvenli Küme**
   ```sh
   sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
   bash Scripts/install.sh
   ```
   Önceki komut, sertifikayı PEM biçiminde alır. Sertifikanız PFX biçimindeyse, dönüştürmek için aşağıdaki komutu kullanabilirsiniz. PFX dosyanız parola korumalı değilse, **passin** parametresini ' olarak `-passin pass:`belirtin.
   ```sh
   openssl pkcs12 -in cert.pfx -out cert.pem -nodes -passin pass:MyPassword1234!
   ``` 
   
   **Güvenli Olmayan Küme**
   ```sh
   sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
   bash Scripts/install.sh
   ```

   Bu işlem, kümeye bir Jenkins kapsayıcısı yükler ve küme, Service Fabric Explorer kullanılarak izlenebilir.

   > [!NOTE]
   > Jenkins görüntüsünün kümeye indirilmesi birkaç dakika sürebilir.
   >

1. Tarayıcınızdan `http://PublicIPorFQDN:8081` sayfasına gidin. Bu işlem, oturum açmak için gereken ilk yönetici parolasının yolunu sağlar. 
1. Jenkins konteynerinin hangi düğümde çalıştığını belirlemek için Service Fabric Explorer'a bakın. Bu düğümde Güvenli Kabuk (SSH) oturum açın.
   ```sh
   ssh user@PublicIPorFQDN -p [port]
   ``` 
1. `docker ps -a` kullanarak kapsayıcı örneğinin kimliğini alın.
1. Güvenli Shell (SSH) konteynırı oturuma imzalayın ve Jenkins portalında gösterilen yolu yapıştırın. Örneğin, portalda yolu `PATH_TO_INITIAL_ADMIN_PASSWORD`gösteriyorsa, aşağıdaki komutları çalıştırın:

   ```sh
   docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
   ```
   ```sh
   cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the password value
   ```
1. Jenkins Başlarken sayfasında, yüklemek için eklentileri seç seçeneğini seçin, **Yok** onay kutusunu seçin ve yükle'yi tıklatın.
1. Bir kullanıcı oluşturun veya yönetici olarak devam etmek için seçin.

Jenkins'i ayarladıktan sonra, [bir Jenkins işi oluşturmak ve yapılandırmak](#create-and-configure-a-jenkins-job)için ileri atlayın.  

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins’i Service Fabric kümesi dışında ayarlama

Jenkins’i bir Service Fabric kümesinin içinde veya dışında ayarlayabilirsiniz. Aşağıdaki bölümlerde, kümenin dışında nasıl ayarlandığı gösterilmektedir.

1. Terminalde çalıştırarak `docker info` Docker'ın makinenize monte edildiğine emin olun. Çıktı, Docker hizmetinin çalışıyor sayılıp çalışmayacağı konusunda işaret ediyor.

1. Docker yüklü değilse, aşağıdaki komutları çalıştırın:

    ```sh
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
    ```

1. Service Fabric Jenkins kapsayıcı görüntüsünü çekin: `docker pull rapatchi/jenkins:latest`. Bu görüntü, Service Fabric Jenkins eklentisi önceden yüklenmiş şekilde gelir.
1. Kapsayıcı görüntüsünü çalıştırın:`docker run -itd -p 8080:8080 rapatchi/jenkins:latest`
1. Kapsayıcı görüntüsü örneğinin kimliğini alın. `docker ps –a` komutuyla tüm Docker kapsayıcılarını listeleyebilirsiniz
1. Aşağıdaki adımları ile Jenkins portalında oturum açın:

   1. Ev sahibinden Jenkins mermisi için oturum açın. Kapsayıcı kimliğinin ilk dört basamağı kullanın. Örneğin, kapsayıcı kimliği `2d24a73b5964`, kullanın `2d24`.

      ```sh
      docker exec -it [first-four-digits-of-container-ID] /bin/bash
      ```
   1. Jenkins kabuğundan, konteyner örneğiniz için yönetici parolasını alın:

      ```sh
      cat /var/jenkins_home/secrets/initialAdminPassword
      ```      
   1. Jenkins panosunda oturum açmak için aşağıdaki URL'yi `http://<HOST-IP>:8080`bir web tarayıcısında açın: . Jenkins'in kilidini açmak için önceki adımdaki parolayı kullan.
   1. (İsteğe bağlı.) Oturum ilk kez oturum açındıktan sonra, kendi kullanıcı hesabınızı oluşturabilir ve bunu aşağıdaki adımlar için kullanabilir veya yönetici hesabını kullanmaya devam edebilirsiniz. Bir kullanıcı oluşturursanız, bu kullanıcıyla devam etmeniz gerekir.
1. GitHub'ı jenkins ile yeni bir [SSH anahtarı oluşturma ve SSH aracısına ekleyerek](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)çalışacak şekilde ayarlayın.
   * GitHub’dan sağlanan yönergeleri kullanarak SSH anahtarını oluşturun ve depoyu barındıran GitHub hesabına SSH anahtarını ekleyin.
   * Önceki bağlantıda belirtilen komutları Jenkins Docker kabuğunda (ana bilgisayarınızda değil) çalıştırın.
   * Ana bilgisayarınızdan Jenkins kabuğunda oturum açmak için aşağıdaki komutu kullanın:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Jenkins kapsayıcı görüntüsünün barındırıldığı küme veya makinenin halka açık bir IP adresi olduğundan emin olun. Bunun olması, Jenkins örneğinin GitHub'dan bildirimler almasını sağlar.

Jenkins'i kurduktan sonra, bir sonraki bölüme devam edin, [bir Jenkins işi oluşturun ve yapılandırın.](#create-and-configure-a-jenkins-job)

## <a name="create-and-configure-a-jenkins-job"></a>Bir Jenkins işi oluşturma ve yapılandırma

Bu bölümdeki adımlar, Bir GitHub repo'sundaki değişikliklere yanıt verecek, değişiklikleri getirecek ve bunları oluşturacak bir Jenkins işini nasıl yapılandırabileceğinizi gösterir. Bu bölümün sonunda, bir geliştirme/test ortamına mı yoksa üretim ortamına mı dağıtdığınıza bağlı olarak uygulamanızı dağıtmak için işi yapılandırmak için son adımlara yönlendirilirsiniz. 

1. Jenkins panosunda **Yeni Öğe'yi**tıklatın.
1. Bir öğe adını girin (örneğin, **MyJob**). **Serbest stil proje**’yi seçip **Tamam**’a tıklayın.
1. İş yapılandırma sayfası açılır. (Jenkins panosundan yapılandırmaya ulaşmak için işi tıklatın ve sonra **Yapıl'ı**tıklatın).

1. **Genel** sekmesinde, **GitHub projesi**nin kutusunu işaretleyin ve GitHub proje URL'nizi belirtin. Bu URL, Jenkins sürekli tümleştirme, sürekli dağıtım (CI/CD) akışı ile tümleştirmek istediğiniz Service Fabric Java uygulamasını barındırır (örneğin, `https://github.com/{your-github-account}/service-fabric-java-getting-started`).

1. Kaynak **Kod Yönetimi** sekmesinde **Git'i**seçin. Jenkins CI/CD akışıyla tümleştirmek istediğiniz Service Fabric Java uygulamasını barındıran deponun URL'sini belirtin (örneğin, `https://github.com/{your-github-account}/service-fabric-java-getting-started`). Ayrıca hangi dalı oluşturabileceğinizi de `/master`belirtebilirsiniz (örneğin, ).
1. *GitHub* deponuzu Jenkins ile konuşacak şekilde yapılandırın:

   a. GitHub depo sayfanızda **Ayarlar** > **Tümleştirmeleri ve Hizmetleri'ne**gidin.

   b. **Hizmet Ekle**’yi seçin, **Jenkins** yazın ve **Jenkins-GitHub eklentisi**’ni seçin.

   c. Jenkins web kancası URL'nizi girin (varsayılan olarak, `http://<PublicIPorFQDN>:8081/github-webhook/` olmalıdır). **Ekle/Güncelle hizmetini**tıklatın.

   d. Jenkins örneğinize bir test olayı gönderilir. GitHub’da web kancasının yanında yeşil renkli bir onay işareti görürsünüz ve projeniz derlenir.

1. Jenkins'teki **Tetikleyiciler Oluştur** sekmesinde, istediğiniz yapı seçeneğini seçin. Bu örnekiçin, depoya bir itme olduğunda bir yapıyı tetiklemek istiyorsunuz, bu nedenle **GITScm yoklaması için GitHub bağlantı tetikleyicisini**seçin. (Daha önce bu seçenek **GitHub’a bir değişiklik uygulandığında derle** olarak adlandırılıyordu.)
1. **Yapı** sekmesinde, java uygulaması mı yoksa .NET Core uygulaması mı oluşturduğunuza bağlı olarak aşağıdakilerden birini yapın:

   * **Java Uygulamaları için:** Yapı **adım** ını bırak-aşağı **ekle'den, Yükseltme Komut Dosyasını Çağır'ı**seçin. **Gelişmiş**'e tıklayın. Gelişmiş menüde, uygulamanız için **Root yapı komut dosyasına** giden yolu belirtin. Belirtilen yoldan build.gradle’ı alır ve buna göre çalışır. [ActorCounter uygulaması için,](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/reliable-services-actor-sample/Actors/ActorCounter)bu: `${WORKSPACE}/reliable-services-actor-sample/Actors/ActorCounter`.

     ![Service Fabric Jenkins Derleme eylemi](./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png)

   * **.NET Çekirdek Uygulamaları için:** Yapı **adım** ını bırak-aşağı ekle'den Kabuk Yürüt'ün'u seçin. **Execute Shell** Görünen komut kutusunda, dizinin önce build.sh dosyasının bulunduğu şekilde değiştirilmesi gerekir. Dizin değiştirildikten sonra, build.sh komut dosyası çalıştırılabilir ve uygulamayı oluşturur.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

     Aşağıdaki ekran görüntüsü, Karşı Hizmet örneğini Jenkins iş adı olan [CounterServiceApplication'ı](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) oluşturmak için kullanılan komutların bir örneğini gösterir.

      ![Service Fabric Jenkins Derleme eylemi](./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png)

1. Jenkins'i, uygulamanızı yapı sonrası eylemlerde bir Service Fabric kümesine dağıtacak şekilde yapılandırmak için, Jenkins kapsayıcınızdaki kümenin sertifikasının konumuna ihtiyacınız vardır. Jenkins kapsayıcınızın kümenizin içinde veya dışında çalışıp çalışmadığına bağlı olarak aşağıdakilerden birini seçin ve küme sertifikasının konumuna dikkat edin:

   * **Jenkins'in kümenin içinde koşması için:** Sertifikaya giden yol, *kapsayıcının* içinden Certificates_JenkinsOnSF_Code_MyCert_PEM ortam değişkeninin değerini yansıtarak bulunabilir.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
   * **Jenkins kümenizin dışında koşmak için:** Küme sertifikasını kapsayıcınıza kopyalamak için aşağıdaki adımları izleyin:
     1. Sertifikanız PEM formatında olmalıdır. PEM dosyanız yoksa, PFX sertifikası dosyasından bir dosya oluşturabilirsiniz. PFX dosyanız parola korumalı değilse, ana bilgisayarınızdan aşağıdaki komutu çalıştırın:

        ```sh
        openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:
        ``` 

        PFX dosyası parola korumalıysa, parolayı `-passin` parametreye ekleyin. Örnek:

        ```sh
        openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:MyPassword1234!
        ``` 

     1. Jenkins konteynerinizin konteyner kimliğini almak `docker ps` için ev sahibinizden çalıştırın.
     1. PEM dosyasını aşağıdaki Docker komutuyla kabınıza kopyalayın:
    
        ```sh
        docker cp clustercert.pem [first-four-digits-of-container-ID]:/var/jenkins_home
        ``` 

Neredeyse bitirdin! Jenkins işini açık tut. Geriye kalan tek görev, uygulamanızı Hizmet Kumaşı kümenize dağıtmak için yapı sonrası adımları yapılandırmaktır:

* Bir geliştirme veya test ortamına dağıtmak için, [küme yönetimi bitiş noktasını kullanarak dağıtımı yapılandır'daki](#configure-deployment-using-cluster-management-endpoint)adımları izleyin.
* Bir üretim ortamına dağıtmak için, [Azure kimlik bilgilerini kullanarak dağıtımı yapılandır'daki](#configure-deployment-using-azure-credentials)adımları izleyin.

## <a name="configure-deployment-using-cluster-management-endpoint"></a>Küme yönetimi bitiş noktasını kullanarak dağıtımı yapılandırma

Geliştirme ve test ortamları için, uygulamanızı dağıtmak için küme yönetimi bitiş noktasını kullanabilirsiniz. Uygulamanızı dağıtmak için küme yönetimi bitiş noktası ile yapılandırma en az kurulum gerektirir. Bir üretim ortamına dağıtım yapıyorsunuz, dağıtım sırasında kullanılacak bir Azure Active Directory hizmet yöneticisini yapılandırmak için [Azure kimlik bilgilerini kullanarak dağıtımı Yapılandırma'ya](#configure-deployment-using-azure-credentials) geçin.    

1. Jenkins işinde, Yapı **Sonrası Eylemler** sekmesini tıklatın. 
1. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. 
1. **Service Fabric Cluster Configuration**altında, Servis Kumaş Yönetimi Bitiş Noktası radyo **düğmesini doldurun'u** seçin.
1. **Yönetim Ana Bilgisayarı**için, kümenizin bağlantı bitiş noktasını girin; örneğin `{your-cluster}.eastus.cloudapp.azure.com`.
1. **İstemci Anahtarı** ve **İstemci Cert**için, Jenkins konteyner pem dosyasının konumunu girin; örneğin `/var/jenkins_home/clustercert.pem`. (Sertifikanın konumunu kopyaladınız, Bir [Jenkins işi oluştur ve yapılandırmanın](#create-and-configure-a-jenkins-job)son adımı.)
1. **Uygulama Yapılandırması**altında, **Uygulama Adı,** **Uygulama Türü**ve (göreli) Uygulama **Bildirimi alanlarına giden yolu** yapılandırın.

   ![Hizmet Kumaş Jenkins Post-Build Eylem yönetim bitiş noktası yapılandırmak](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-endpoint.png)

1. **Yapılandırmayı Doğrula'yı**tıklatın. Başarılı doğrulamada **Kaydet'i**tıklatın. Jenkins iş boru hattınız artık tamamen yapılandırılmış. Dağıtımınızı test etmek için [Sonraki adımlara](#next-steps) ilerleyin.

## <a name="configure-deployment-using-azure-credentials"></a>Azure kimlik bilgilerini kullanarak dağıtımı yapılandırma

Üretim ortamları için, uygulamanızı dağıtmak için bir Azure kimlik bilgisi yapılandırmanız önerilir. Bu bölümde, uygulamanızı yapı sonrası eylemde dağıtmak için kullanılacak bir Azure Etkin Dizin hizmet ilkesini nasıl yapılandırabileceğiniz gösterilmektedir. Jenkins işinin izinlerini sınırlamak için dizininizdeki rollere hizmet ilkeleri atayabilirsiniz. 

Geliştirme ve test ortamları için, uygulamanızı dağıtmak için Azure kimlik bilgilerini veya küme yönetimi bitiş noktasını yapılandırabilirsiniz. Küme yönetimi bitiş noktasının nasıl yapılandırılabildiğini hakkında ayrıntılı bilgi için, [küme yönetimi bitiş noktasını kullanarak dağıtımı yapılandır'a](#configure-deployment-using-cluster-management-endpoint)bakın.   

1. Bir Azure Active Directory hizmet yöneticisi oluşturmak ve Azure aboneliğinizde izinler atamak [için, bir Azure Active Directory uygulaması ve hizmet ilkesi oluşturmak için portalı kullan'daki](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)adımları izleyin. Aşağıdakilere dikkat edin:

   * Konuyla ilgili adımları takip ederken, aşağıdaki değerleri kopyalayıp kaydettiğinden emin olun: *Uygulama Kimliği, Uygulama* *anahtarı,* *Dizin Kimliği (Kiracı Kimliği)* ve *Abonelik Kimliği*. Jenkins'teki Azure kimlik bilgilerini yapılandırmak için bunlara ihtiyacınız var.
   * Dizininizde [gerekli izinlere](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) sahip değilseniz, bir yöneticiden size izin vermesini veya sizin için hizmet ilkesini oluşturmasını istemeniz veya Jenkins'teki işiniz için Yapı **Sonrası Eylemler'de** kümenizin yönetim bitiş noktasını yapılandırmanız gerekir.
   * Azure [Etkin Dizin Oluştur uygulaması](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application) bölümünde, Oturum Açma URL'si için iyi biçimlendirilmiş herhangi bir **URL**girebilirsiniz.
   * Rolü [Bir Bölüme Uygulama Atarken,](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) uygulamanızı kümeniziçin kaynak grubundaki *Okuyucu* rolünü atayabilirsiniz.

1. Jenkins işinde, **Yapı Sonrası Eylemler** sekmesini tıklatın.
1. **Derleme Sonrası Eylemler** açılır listesinden **Service Fabric Projesini Dağıt**’ı seçin. 
1. **Hizmet Kumaş Küme yapılandırması**altında , Servis Kumaş Kümesi ni **seçin'i**tıklatın. Azure Kimlik **Bilgileri'nin**yanına **Ekle'yi** tıklatın. Jenkins Kimlik Bilgileri Sağlayıcısı'nı seçmek için **Jenkins'i** tıklatın.
1. Jenkins Kimlik Bilgileri Sağlayıcısı'nda, **Tür** açılır tarafından **Microsoft Azure Hizmet Sorumlusu'nu** seçin.
1. Aşağıdaki alanları ayarlamak için hizmet üstaişinizi Adım 1'de ayarlarken kaydettiğiniz değerleri kullanın:

   * **İstemci Kimliği**: *Başvuru Kimliği*
   * **İstemci Secret**: *Uygulama anahtarı*
   * **Kiracı Kimliği**: *Dizin Kimliği*
   * **Abonelik Kimliği**: *Abonelik Kimliği*
1. Jenkins'teki kimlik bilgisini ve kısa bir **Açıklamayı**seçmek için kullandığınız açıklayıcı **bir kimlik** girin. Ardından **Servis Müdürü'ne doğrula'yı**tıklatın. Doğrulama başarılı olursa, **Ekle'yi**tıklatın.

   ![Hizmet Kumaş Jenkins Azure kimlik bilgilerini girin](./media/service-fabric-cicd-your-linux-application-with-jenkins/enter-azure-credentials.png)
1. Service **Fabric Cluster Configuration**altında, **Azure Kimlik Bilgileri**için yeni kimlik bilgilerinizin seçildiğinden emin olun. 
1. Kaynak **Grubu** açılır tarafından, uygulamayı dağıtmak istediğiniz kümenin kaynak grubunu seçin.
1. Hizmet **Kumaşı** açılır tarihinden, uygulamayı dağıtmak istediğiniz kümeyi seçin.
1. **İstemci Anahtarı** ve **İstemci Cert**için, Jenkins konteynerinize PEM dosyasının yerini girin. Örneğin, `/var/jenkins_home/clustercert.pem`. 
1. **Uygulama Yapılandırması**altında, **Uygulama Adı,** **Uygulama Türü**ve (göreli) Uygulama **Bildirimi alanlarına giden yolu** yapılandırın.
    ![Hizmet Kumaş Jenkins Post-Build Eylem - Azure kimlik bilgilerini yapılandırma](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-credentials.png)
1. **Yapılandırmayı Doğrula'yı**tıklatın. Başarılı doğrulamada **Kaydet'i**tıklatın. Jenkins iş boru hattınız artık tamamen yapılandırılmış. Dağıtımınızı sınamak için [Sonraki adımlara](#next-steps) devam edin.

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunları giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="ideas-to-try"></a>Denemek için fikirler

GitHub ve Jenkins yapılandırılmıştır. Deponun çatalında `reliable-services-actor-sample/Actors/ActorCounter` projede bazı örnek değişiklikler yapmayı düşünün. https://github.com/Azure-Samples/service-fabric-java-getting-started Değişikliklerinizi uzak `master` dala (veya çalışmak üzere yapılandırdığınız herhangi bir dala) itin. Bunun yapılması, yapılandırmış olduğunuz `MyJob` Jenkins işini tetikler. Değişiklikleri GitHub'dan alır, oluşturur ve uygulamayı oluşturma sonrası eylemlerde belirttiğiniz kümeye dağıtur.  

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Jenkins](/azure/Jenkins/)