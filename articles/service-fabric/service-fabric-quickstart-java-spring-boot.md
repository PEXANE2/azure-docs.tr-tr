---
title: "Quickstart: Azure Hizmet Kumaşı'nda Bahar Önyükleme uygulaması oluşturun"
description: Bu hızlı başlangıçta, bir Spring Boot örnek uygulaması kullanarak Azure Service Fabric için Spring Boot uygulaması dağıtırsınız.
author: suhuruli
ms.topic: quickstart
ms.date: 01/29/2019
ms.author: suhuruli
ms.custom: mvc, devcenter, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: eb96989b4a2731e78471b848d690b48352408d1c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77121488"
---
# <a name="quickstart-deploy-a-java-spring-boot-app-on-azure-service-fabric"></a>Hızlı başlangıç: Azure Hizmet Kumaşına Java İlkyükleme uygulaması dağıtma

Bu hızlı başlangıçta, Linux veya MacOS'ta tanıdık komut satırı araçlarını kullanarak Azure Hizmet Kumaşı'na bir Java Bahar Önyükleme uygulaması dağıtabilirsiniz. Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur. 

## <a name="prerequisites"></a>Ön koşullar

#### <a name="linux"></a>[Linux](#tab/linux)

- [Java çevre](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development) ve [Yeoman](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
- [Servis Kumaş SDK & Servis Kumaş Komut Hattı Arayüzü (CLI)](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
- [Git](https://git-scm.com/downloads)

#### <a name="macos"></a>[Macos](#tab/macos)

- [Java çevre ve Yeoman](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)
- [Servis Kumaş SDK & Servis Kumaş Komut Hattı Arayüzü (CLI)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli#cli-mac)
- [Git](https://git-scm.com/downloads)

--- 

## <a name="download-the-sample"></a>Örneği indirme

Terminal penceresinde, Bahar Önyükleme [Deneme](https://github.com/spring-guides/gs-spring-boot) örneği uygulamasını yerel makinenize klonlamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="build-the-spring-boot-application"></a>Spring Boot uygulamasını derleme 
*gs-spring-boot/complete* dizininin içinde, uygulamayı oluşturmak için aşağıdaki komutu çalıştırın 

```bash
./gradlew build
``` 

## <a name="package-the-spring-boot-application"></a>Spring Boot uygulamasını paketleme 
1. Klonunuzdaki *gs-spring-boot* dizininin içinde, `yo azuresfguest` komutu çalıştırın. 

1. Her istem için aşağıdaki ayrıntıları girin.

    ![Bahar Boot Yeoman girişleri](./media/service-fabric-quickstart-java-spring-boot/yeoman-entries-spring-boot.png)

1. *SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code* klasöründe *entryPoint.sh*adında bir dosya oluşturun. *entryPoint.sh* dosyasına aşağıdaki kodu ekleyin. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

1. *GS-spring-boot/SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/ServiceManifest.xml* dosyasına **Uç Nokta** kaynağı ekleyin

    ```xml 
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
    ```

    *ServiceManifest.xml* şimdi şu şekilde görünür: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceManifest Name="SpringGettingStartedPkg" Version="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" >

       <ServiceTypes>
          <StatelessServiceType ServiceTypeName="SpringGettingStartedType" UseImplicitHost="true">
       </StatelessServiceType>
       </ServiceTypes>

       <CodePackage Name="code" Version="1.0.0">
          <EntryPoint>
             <ExeHost>
                <Program>entryPoint.sh</Program>
                <Arguments></Arguments>
                <WorkingFolder>CodePackage</WorkingFolder>
             </ExeHost>
          </EntryPoint>
       </CodePackage>
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
     </ServiceManifest>
    ```

Bu aşamada, Spring Boot Kullanmaya Başlama örneği için Service Fabric’e dağıtabileceğiniz bir Service Fabric uygulaması oluşturdunuz.

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

1. Aşağıdaki komutu çalıştırarak Ubuntu makinelerinde yerel kümenizi başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    Mac kullanıyorsanız Docker görüntüsünden yerel kümeyi başlatın (burada Mac için yerel kümenizi ayarlama [önkoşullarına](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac#create-a-local-container-and-set-up-service-fabric) uyduğunuz varsayılır). 

    ```bash
    docker run --name sftestcluster -d -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 -p 8080:8080 mysfcluster
    ```

    Yerel kümenin başlatılması biraz zaman alabilir. Kümenin sorunsuz çalıştığından emin olmak için, `http://localhost:19080` adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir. 
    
    ![Hizmet Kumaş Explorer sağlıklı düğümleri gösterir](./media/service-fabric-quickstart-java-spring-boot/service-fabric-explorer-healthy-nodes.png)

1. *gs-spring-boot/SpringServiceFabric* klasörünü açın.
1. Yerel kümenize bağlanmak için aşağıdaki komutu çalıştırın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
1. *install.sh* komut dosyasını çalıştırın.

    ```bash
    ./install.sh
    ```

1. Sık kullandığınız web tarayıcısını açın ve`http://localhost:8080` adresine giderek uygulamaya erişin.

    ![Bahar Önyükleme Servisi Kumaş örneği](./media/service-fabric-quickstart-java-spring-boot/spring-boot-service-fabric-sample.png)

Bir Service Fabric kümesine dağıtılmış Spring Boot uygulamasına artık erişebilirsiniz.

Daha fazla bilgi için, Bahar Boot [Başlangıç](https://spring.io/guides/gs/spring-boot/) örneği Bahar web sitesinde bakın.

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme

Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklendirmenin birçok yolu vardır; örneğin, Service Fabric CLI’den (sfctl) betikler veya komutlar kullanabilirsiniz. Aşağıdaki adımlarda Service Fabric Explorer kullanılmaktadır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalıştırılır ve tarayıcıdan kümelerin HTTP yönetim bağlantı noktasına (19080) göz atılarak (örneğin, `http://localhost:19080`) erişilebilir.

Web ön uç hizmetini ölçeklendirmek için aşağıdakileri yapın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
1. Kumaşın yanındaki elipsleri (**...**) **seçin:/SpringServiceFabric/SpringGettingStarted** düğüm ağacı görünümünde ve **Ölçek Hizmeti'ni**seçin.

    ![Servis Kumaş Explorer ölçek servis örneği](./media/service-fabric-quickstart-java-spring-boot/service-fabric-explorer-scale-sample.png)

    Şimdi hizmetin örnek sayısını ölçeklendirebilirsiniz.

1. Sayıyı **3** olarak değiştirin ve **Ölçek Hizmeti'ni**seçin.

    Komut satırını kullanarak hizmeti ölçeklendirmenin alternatif bir yolu aşağıda verilmiştir.

    ```bash
    # Connect to your local cluster
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted' --instance-count 3 --stateless 
    ``` 

1. **Kumaşı seçin:/SpringServiceFabric/SpringGettingStarted** düğüm ağacı görünümünde ve bölüm düğümünü genişletin (GUID ile temsil edilir).

    ![Servis Kumaş Explorer ölçek hizmeti tamamlandı](./media/service-fabric-quickstart-java-spring-boot/service-fabric-explorer-partition-node.png)

    Hizmette üç örnek bulunur ve ağaç görünümünde örneklerin çalıştığı düğümler gösterilir.

Bu basit yönetim görevi sayesinde ön uç hizmetinin kullanıcı yükünü işlemek için kullanabileceği kaynakları iki katına çıkarmış oldunuz. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığınızı anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric, kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="fail-over-services-in-a-cluster"></a>Bir kümedeki hizmetlerin yükünü devretme

Hizmet yük devretmesini göstermek için Service Fabric Explorer'ı kullanarak bir düğümü yeniden başlatma işleminin benzetimi yapılmıştır. Hizmetinizin yalnızca bir örneğinin çalıştığından emin olun.

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://localhost:19080`.
1. Hizmetinizin örneğini çalıştıran düğümün yanındaki elipsleri (**...**) seçin ve düğümü yeniden başlatın.

    ![Hizmet Kumaş Explorer yeniden düğüm](./media/service-fabric-quickstart-java-spring-boot/service=fabric-explorer-restart=node.png)
1. Hizmetinizin örneği bu durumda farklı bir düğüme taşınmıştır ve uygulamanızda kesinti yaşanmaz.

    ![Hizmet Kumaş Explorer yeniden başlatma düğümü başarılı](./media/service-fabric-quickstart-java-spring-boot/service-fabric-explorer-service-moved.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Service Fabric’e Spring Boot uygulaması dağıtma
* Uygulamayı yerel kümenize dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme
* Kullanılabilirliği etkilemeden hizmetinizin yük devretmesini gerçekleştirme

Service Fabric’te Java uygulamalarıyla çalışma hakkında daha fazla bilgi için Java uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Java uygulaması dağıtma](./service-fabric-tutorial-create-java-app.md)
