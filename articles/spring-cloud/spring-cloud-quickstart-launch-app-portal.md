---
title: Quickstart - Azure portalını kullanarak mevcut bir Azure Bulutu uygulamasını başlatın
description: Bu hızlı başlangıçta, Azure portalını kullanarak Bir Bahar Bulutu uygulamasını Azure Bulutu'na dağıtın.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 02/15/2020
ms.author: brendm
ms.openlocfilehash: 9cd59fdf81e9b5d56872d20c76e8ea177b3c8577
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79470903"
---
# <a name="quickstart-launch-an-existing-azure-spring-cloud-application-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak mevcut bir Azure Bulutu uygulamasını başlatma

Bu hızlı başlangıç, varolan bir Bahar Bulutu uygulamasını Azure'a nasıl dağıtabileceğinizi gösterir. Azure Bulutu, Azure'da Bahar Bulutu tabanlı mikro hizmet uygulamalarını kolayca çalıştırmanızı sağlar. 

Bu eğitimde kullanılan örnek uygulama kodunu [GitHub örnek deposumuzda](https://github.com/Azure-Samples/PiggyMetrics)bulabilirsiniz. İşi bittiğinde, sağlanan örnek uygulamaya çevrimiçi olarak erişilen ve Azure portalı üzerinden yönetilmeye hazır olacaktır.

Bu hızlı başlangıcın ardından, nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneğini sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikrohizmet uygulaması oluşturma
> * Her microservice dağıtma
> * Başvurunuz için genel bitiş noktası atama

## <a name="prerequisites"></a>Ön koşullar

>[!Note]
> Azure İlkbahar Bulutu şu anda genel önizleme olarak sunulmaktadır. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.  Önizlemeler sırasında destek hakkında daha fazla bilgi için lütfen [SSS'mizi](https://azure.microsoft.com/support/faq/) gözden geçirin veya daha fazla bilgi edinmek için bir [Destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) dosyalayın.

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLI'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, Azure [Bulut Shell'inizi](https://shell.azure.com) shell.azure.com başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Bulut BulutU Hakkında Daha Fazla Bilgi Edinebilirsiniz

Bu hızlı başlangıcı tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [JDK 8'i yükleyin](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3.0 veya üzeri yükleme](https://maven.apache.org/download.cgi)
4. [Azure CLI sürümünü 2.0.67 veya daha yüksek yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliği için kaydolun](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Azure CLI uzantısını yükleme

Aşağıdaki komutu kullanarak Azure CLI için Azure İlkbahar Bulutu uzantısını yükleme

```azurecli
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portalında bir hizmet örneği sağlama

1. Yeni bir sekmede [Azure portalını](https://ms.portal.azure.com/)açın. 

2. Üstteki arama kutusundan **Azure Bahar Bulutu'nu**arayın.

3. Sonuçlardan **Azure Bahar Bulutu'nu** seçin.

 ![ASC simgesi](media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. Azure İlkbahar Bulutu sayfasında **+ Ekle'yi**tıklatın.

 ![ASC simgesi](media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. Formu Azure İlkbahar Bulut **Uyr'u Oluşturma** sayfasında doldurun.  Aşağıdaki yönergeleri göz önünde bulundurun:
    - **Abonelik**: Bu kaynak için faturalandırılmak istediğiniz aboneliği seçin.  Bu aboneliğin Azure Bahar Bulutu için izin listemize eklendiğini sağlayın.
    - **Kaynak grubu**: Yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi yöntemdir.
    - **Hizmet Ayrıntıları/Adı**: Hizmet örneğinizin adını belirtin.  Ad 4 ile 32 karakter uzunluğunda olmalı ve yalnızca küçük harfler, sayılar ve tireler içerebilir.  Hizmet adının ilk karakteri bir harf, son karakter ise bir harf veya sayı olmalıdır.
    - **Konum**: Servis örneğinizin konumunu seçin. Şu anda desteklenen yerler arasında Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya yer alıyor.

    ![ASC portal başlangıcı](media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. Aşağıdaki iletişim kutusunu açmak için **Tanılama Ayarı** sekmesini tıklatın.

7. **Enable günlüklerini** gereksinimlerinize göre *evet* veya *hayır* olarak ayarlayabilirsiniz.

    ![Günlükleri etkinleştirme](media/spring-cloud-quickstart-launch-app-portal/diagnostic-setting.png)

8. **İzleme** sekmesini tıklatın.

9. **Enable izlemeyi** gereksinimlerinize göre *evet* veya *hayır* olarak ayarlayabilirsiniz.  **İzlemeyi** evet olarak ayarlarsanız, varolan bir uygulama öngörüsünü de seçin veya yeni bir uygulama oluşturun. Application **Insights** belirtimi olmadan bir doğrulama hatası olacaktır.


    ![İzleme](media/spring-cloud-quickstart-launch-app-portal/tracing.png)

10. **Gözden geçir ve oluştur**’a tıklayın.

11. Teknik özelliklerinizi doğrulayın ve **Oluştur'u**tıklatın.

Hizmetin dağıtılması yaklaşık 5 dakika sürer.  Dağıtıldıktan sonra, hizmet örneği için **Genel Bakış** sayfası görüntülenir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=provision)


## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **Genel Bakış** sayfasına gidin ve **Config Server'ı**seçin.

2. Varsayılan **depo** bölümünde URI' **yi** "https://github.com/Azure-Samples/piggymetrics-config" olarak ayarlayın.

3. Yaptığınız değişiklikleri kaydetmek için **Apply**'ı (Uygula) seçin.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/portal-config.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=config-server)

## <a name="build-and-deploy-microservice-applications"></a>Mikro hizmet uygulamaları oluşturma ve dağıtma

1. Bir [Azure Bulut Kabuğu](https://shell.azure.com) açın ve örnek uygulama deposunu yerel makinenize kopyala.  Burada, uygulamayı klonlamadan önce `source-code` geçici bir dizin oluştururuz.

    ```console
    mkdir source-code
    cd source-code
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Klonlanmış paketi oluşturun.

    ```console
    cd piggymetrics
    mvn clean package -DskipTests
    ```

3. Kaynak grubunuza ve hizmetinize ad lar atayın. Aşağıdaki yer tutucuları, bu eğitimde daha önce vermiş olduğunuz kaynak grubu adı ve hizmet adı ile değiştirdiğinizden emin olun.

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

4. `gateway` Uygulamayı oluşturun ve JAR dosyasını dağıtın.

    ```azurecli
    az spring-cloud app create -n gateway
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    ```

5. Aynı deseni izleyerek, uygulamaları `account-service` ve uygulamaları `auth-service` oluşturun ve JAR dosyalarını dağıtın.

    ```azurecli
    az spring-cloud app create -n account-service
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app create -n auth-service
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

6. Uygulamaları dağıtmayı bitirmek birkaç dakika sürer. Dağıttıklarını doğrulamak için Azure portalındaki **Uygulamalar** bıçağına gidin. Üç uygulamanın her biri bir satır görmelisiniz.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=deploy)

## <a name="assign-a-public-endpoint-to-gateway"></a>Ağ geçidine ortak bir bitiş noktası atama

1. Soldaki menüdeki **Uygulamalar** sekmesini açın.

2. Genel `gateway` **Bakış** sayfasını göstermek için uygulamayı seçin.

3. Ağ geçidine genel bir bitiş noktası atamak için **Bitiş Noktası Atay'ı** seçin. Bu işlem birkaç dakika sürebilir.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/portal-endpoint.png)

4. Çalışan uygulamanızı görüntülemek için tarayıcınıza atanan genel bitiş noktasını (etiketli **URL)** girin.

    ![ASC portalının ekran görüntüsü](media/spring-cloud-quickstart-launch-app-portal/sample-app.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Hizmet örneğini sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikrohizmet uygulaması oluşturma
> * Her microservice dağıtma
> * Uygulama ağ geçidiniz için genel bitiş noktası atama

> [!div class="nextstepaction"]
> [Azure Bahar Bulutu uygulamanızı dağıtıma hazırlayın](spring-cloud-tutorial-prepare-app-deployment.md)

GitHub: Azure Bahar [Bulut Örnekleri'nde](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql)daha fazla örnek mevcuttur.
