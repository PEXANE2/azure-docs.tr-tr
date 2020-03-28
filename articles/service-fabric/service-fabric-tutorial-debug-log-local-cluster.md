---
title: Yerel Hizmet Kumaşı kümesinde bir Java uygulamasını hata ayıklama
description: Bu öğreticide, yerel kümede çalıştırılan bir Service Fabric Java uygulamasından nasıl hata ayıklama yapılacağını ve günlüklerin alınacağını öğreneceksiniz.
author: suhuruli
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: c664b586260957138249028e4d521c29b411d56d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75465393"
---
# <a name="tutorial-debug-a-java-application-deployed-on-a-local-service-fabric-cluster"></a>Öğretici: Yerel Service Fabric kümesinde dağıtılan bir Java uygulamasının hatasını ayıklama

Bu öğretici, bir dizinin ikinci bölümüdür. Eclipse for the Service Fabric uygulaması kullanılarak uzak bir hata ayıklayıcının nasıl ekleneceğini öğreneceksiniz. Ayrıca çalıştırılan uygulamalardaki günlüklerin geliştirici için uygun bir konuma nasıl yeniden yönlendirileceğini de öğreneceksiniz.

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Java Service Fabric Güvenilir Hizmetler uygulaması derleme](service-fabric-tutorial-create-java-app.md)
> * Yerel kümede uygulamayı dağıtma ve uygulamanın hatasını ayıklama
> * [Azure kümesine uygulama dağıtma](service-fabric-tutorial-java-deploy-azure.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-java-elk.md)
> * [CI/CD ayarlama](service-fabric-tutorial-java-jenkins.md)


Serinin ikinci bölümünde şunları öğrenirsiniz:
> [!div class="checklist"]
> * Eclipse kullanarak Java uygulamasının hatasını ayıklama
> * Günlükleri yapılandırılabilir bir konuma yeniden yönlendirme


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* [Mac](service-fabric-get-started-mac.md) veya [Linux](service-fabric-get-started-linux.md) için geliştirme ortamınızı ayarlayın. Eclipse eklentisini, Gradle’ı, Service Fabric SDK’yı ve Service Fabric CLI’yı (sfctl) yükleme yönergelerini izleyin.

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serinin birinci bölümünde](service-fabric-tutorial-create-java-app.md)Oylama örnek uygulama inşa etmediyseniz, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

Uygulamayı yerel geliştirme kümesine [oluşturun ve dağıtın.](service-fabric-tutorial-create-java-app.md#deploy-application-to-local-cluster)

## <a name="debug-java-application-using-eclipse"></a>Eclipse kullanarak Java uygulamasının hatasını ayıklama

1. Makinenizde Eclipse IDE’yi açın ve **Dosya -> İçeri Aktar....** seçeneklerine tıklayın.

2. Açılan pencerede **Genel -> Mevcut Projeleri Çalışma Alanına** seçeneğini belirleyin ve İleri düğmesine basın.

3. Projeleri İçeri Aktar penceresinde **Kök dizini seçin** seçeneğini belirleyin ve **Oylama** dizinini seçin. Birinci öğretici serisini izlediyseniz **Oylama** dizini, **Eclipse çalışma alanı** dizinindedir.

4. Hatasını ayıklamak istediğiniz hizmetin entryPoint.sh öğesini güncelleştirin; böylece hizmet, uzaktan hata ayıklama parametreleriyle Java işlemini başlatır. Bu öğretici için vatansız ön uç kullanılır: *Oylama / OylamaApplication / VotingWebPkg / Kod / entryPoint.sh*. Bağlantı noktası 8001 bu örnekte hata ayıklama için ayarlanır.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingWeb.jar
    ```

5. Hatası ayıklanmakta olan hizmet için çoğaltma sayısını veya örnek sayısını ayarlayarak Uygulama Bildirimini güncelleştirin. Bu ayar, hata ayıklama için kullanılan bağlantı noktası için çakışmaları önler. Örneğin, şu işlemleri izleyerek durum bilgisi olmayan hizmetler için ``InstanceCount="1"`` ayarını yapın ve durum bilgisi olan hizmetler için hedefi ve en az çoğaltma kümesi boyutlarını 1 olarak ayarlayın: ``TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

6. Eclipse IDE’de **Çalıştır -> Hata Ayıklama Yapılandırmaları -> Uzak Java Uygulaması** seçeneklerini belirleyin, **Yeni** düğmesine basın, aşağıdaki şekilde özellikleri ayarlayın ve **Uygula**’ya tıklayın.

    ```
    Name: Voting
    Project: Voting
    Connection Type: Standard
    Host: localhost
    Port: 8001
    ```

7. *Voting/VotingWeb/src/statelessservice/HttpCommunicationListener.java* dosyasının 109. satırına bir kesme noktası ekleyin.

8. Paket Gezgini’nde **Oylama** projesine sağ tıklayın ve **Service Fabric -> Uygulama Yayımla ...** seçeneklerine tıklayın.

9. **Uygulama Yayımla** penceresinde açılan listeden **Local.json** seçeneğini belirleyin ve **Yayımla**’ya tıklayın.

10. Eclipse IDE’de **Çalıştır -> Hata Ayıklama Yapılandırmaları -> Uzak Java Uygulaması** seçeneklerini belirleyin, oluşturduğunuz **Oylama** yapılandırmasına ve sonra **Hata Ayıklama**’ya tıklayın.

11. Web tarayıcınıza gidin ve **localhost erişin:8080**. Bu otomatik olarak kesme noktasına çarpar ve Eclipse **Hata Ayıklama perspektifine**girer.

Şimdi Eclipse herhangi bir Hizmet Kumaş uygulaması hata ayıklamak için bu aynı adımları uygulayabilirsiniz.

## <a name="redirect-application-logs-to-custom-location"></a>Uygulama günlüklerini özel konuma yeniden yönlendirme

Aşağıdaki adımlarda, varsayılan */var/log/syslog* konumundaki uygulama günlüklerinin özel bir konuma nasıl yeniden yönlendirileceği gösterilmektedir.

1. Şu anda Service Fabric Linux kümelerinde çalışan uygulamalar yalnızca tek bir günlük dosyasını almayı desteklemez. Günlüklerin her zaman */tmp/mysfapp0.0.log*adresine gidecek şekilde bir uygulama ayarlamak için, aşağıdaki konum *oylama/OylamaApplication/VotingWebPkg/Code/logging.properties* adresinde niçin logging adlı bir dosya oluşturun ve aşağıdaki içeriği ekleyin.

    ```
    handlers = java.util.logging.FileHandler

    java.util.logging.FileHandler.level = ALL
    java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter

    # This value specifies your custom location.
    # You will have to ensure this path has read and write access by the process running the SF Application
    java.util.logging.FileHandler.pattern = /tmp/mysfapp0.0.log
    ```

2. Java yürütme komutu için *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh* konumuna aşağıdaki parametreyi ekleyin:

    ```bash
    -Djava.util.logging.config.file=logging.properties
    ```

    Aşağıdaki örnek, önceki bölümdeki yürütmeye benzer şekilde hata ayıklama ekli bir örnek yürütme gösterir.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=logging.properties -jar VotingWeb.jar
    ```

Bu aşamada, Service Fabric Java uygulamalarınızı geliştirirken nasıl hata ayıklaması yapacağınızı ve uygulama günlüklerinize erişeceğinizi öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Eclipse kullanarak Java uygulamasının hatasını ayıklama
> * Günlükleri yapılandırılabilir bir konuma yeniden yönlendirme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Uygulamayı Azure'a dağıtma](service-fabric-tutorial-java-deploy-azure.md)