---
title: Hızlı başlangıç-Azure portal kullanarak mevcut bir Azure Spring Cloud uygulamasını başlatın
description: Bu hızlı başlangıçta, Azure portal kullanarak Azure Spring buluta bir yay bulutu uygulaması dağıtın.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 02/15/2020
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 163940095c4a3a25123706ca47a7d059897cad20
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89046876"
---
# <a name="quickstart-launch-an-existing-azure-spring-cloud-application-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak mevcut bir Azure Spring Cloud uygulamasını başlatma

Bu hızlı başlangıçta, mevcut bir yay bulutu uygulamasının Azure 'a nasıl dağıtılacağı gösterilir. Azure Spring Cloud, Azure 'da Spring Cloud tabanlı mikro hizmet uygulamalarını kolayca çalıştırmanıza olanak sağlar. 

Bu örneği çalıştırmadan önce [temel hızlı](spring-cloud-quickstart.md)başlangıcı deneyebilirsiniz.

Bu öğreticide kullanılan örnek uygulama kodunu [GitHub örnekleri depomız](https://github.com/Azure-Samples/PiggyMetrics)bölümünde bulabilirsiniz. İşiniz bittiğinde, belirtilen örnek uygulama çevrimiçi olarak erişilebilir ve Azure portal aracılığıyla yönetilmeye hazırlıyordu.

Bu hızlı başlangıcı izleyerek şunları nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneği sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikro Hizmetler uygulaması oluşturma
> * Her mikro hizmeti dağıtma
> * Uygulamanız için genel uç nokta atama

## <a name="prerequisites"></a>Ön koşullar

>[!Note]
> Azure yay bulutu Şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, müşterilerin resmi sürümünden önceki yeni özelliklerle deneme yapmasına olanak tanır.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.  Önizleme sırasında destek hakkında daha fazla bilgi için lütfen [SSS](https://azure.microsoft.com/support/faq/) veya dosya dosyası [destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) inceleyerek daha fazla bilgi edinebilirsiniz.

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

Bu hızlı başlangıcı tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3,0 veya üstünü yükler](https://maven.apache.org/download.cgi)
4. [Azure CLı sürüm 2.0.67 veya üstünü yükler](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portal bir hizmet örneği sağlayın

1. Yeni bir sekmede [Azure Portal](https://ms.portal.azure.com/)açın. 

2. Üst arama kutusundan **Azure yay bulutu**' nı arayın.

3. Sonuçlardan **Azure yay bulutu** ' nı seçin.

 ![ASC başlangıcı](media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. Azure yay bulutu sayfasında **+ Ekle**' ye tıklayın.

 ![ASC ekleme](media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. Azure yay bulutu **Oluştur** sayfasında formu doldurun.  Aşağıdaki yönergeleri göz önünde bulundurun:
    - **Abonelik**: Bu kaynak için faturalandırılması istediğiniz aboneliği seçin.  Bu aboneliğin Azure yay bulutu için izin verilenler listenize eklendiğinden emin olun.
    - **Kaynak grubu**: yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi uygulamadır.
    - **Hizmet Ayrıntıları/adı**: hizmet örneğinizin adını belirtin.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.
    - **Konum**: hizmet örneğinizin konumunu seçin. Şu anda desteklenen konumlar Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya 'yı içerir.

    ![ASC portalı başlangıcı](media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. Aşağıdaki iletişim kutusunu açmak için **Tanılama ayarı** sekmesine tıklayın.

7. Gereksinimlerinize göre **günlüğü** *Evet* veya *Hayır* olarak ayarlayabilirsiniz.

    ![Günlükleri etkinleştirme](media/spring-cloud-quickstart-launch-app-portal/diagnostic-setting.png)

8. **İzleme** sekmesine tıklayın.

9. **İzlemeyi** , gereksinimlerinize göre *Evet* veya *Hayır* olarak ayarlayabilirsiniz.  **Izlemeyi etkinleştir** ' i Evet olarak ayarlarsanız, var olan bir uygulama öngörüyü seçin veya yeni bir tane oluşturun. **Application Insights** belirtimi olmadan bir doğrulama hatası olur.


    ![İzleme görünümü](media/spring-cloud-quickstart-launch-app-portal/tracing.png)

10. **Gözden geçir ve oluştur**’a tıklayın.

11. Belirtimlerinizi doğrulayıp **Oluştur**' a tıklayın.

Hizmetin dağıtılması yaklaşık 5 dakika sürer.  Hizmet örneği için **genel bakış** sayfası, dağıtıldıktan sonra görüntülenir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=provision)


## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **genel bakış** sayfasına gidin ve **yapılandırma sunucusu**' nu seçin.

2. **Varsayılan depo** bölümünde, **URI** 'yi "" olarak ayarlayın https://github.com/Azure-Samples/piggymetrics-config .

3. Yaptığınız değişiklikleri kaydetmek için **Apply**'ı (Uygula) seçin.

    ![ASC portalı](media/spring-cloud-quickstart-launch-app-portal/portal-config.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=config-server)

## <a name="build-and-deploy-microservice-applications"></a>Mikro hizmet uygulamaları oluşturma ve dağıtma

1. Azure CLı yüklü bir [Azure Cloud Shell](https://shell.azure.com) veya yerel kabuğunuz açın. Burada, önce `source-code` Örnek uygulamayı kopyalamadan önce adlı geçici bir dizin oluşturacağız.

    ```console
    mkdir source-code
    cd source-code
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Kopyalanmış paketi oluşturun.

    ```console
    cd piggymetrics
    mvn clean package -DskipTests
    ```

3. Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler

    ```azurecli
    az extension add --name spring-cloud
    ```

4. Kaynak grubunuza ve hizmetinize ad atayın. Aşağıdaki yer tutucuları, bu öğreticide daha önce sağladığınız kaynak grubu adı ve hizmet adı ile değiştirdiğinizden emin olun.

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

5. Uygulamayı oluşturun `gateway` ve jar dosyasını dağıtın.

    Yay bulutu uzantısını kullanarak uygulamayı oluşturun:

    ```azurecli
    az spring-cloud app create -n gateway
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    ```

6. Aynı kalıbı izleyerek, `account-service` ve `auth-service` UYGULAMALARıNı oluşturup jar dosyalarını dağıtın.

    ```azurecli
    az spring-cloud app create -n account-service
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app create -n auth-service
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

7. Uygulamaların dağıtılmasının tamamlanmasının birkaç dakika sürer. Dağıtıldığını doğrulamak için Azure portal **uygulamalar** dikey penceresine gidin. Her üç uygulamanın bir satırını görmeniz gerekir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=deploy)

## <a name="assign-a-public-endpoint-to-gateway"></a>Ağ geçidine genel uç nokta atama

1. Soldaki menüden **uygulamalar** sekmesini açın.

2. `gateway` **Genel bakış** sayfasını göstermek için uygulamayı seçin.

3. Ağ geçidine genel bir uç nokta atamak için **uç nokta ata** ' yı seçin. Bu işlem birkaç dakika sürebilir.

    ![ASC portalı uç noktası](media/spring-cloud-quickstart-launch-app-portal/portal-endpoint.png)

4. Çalışan uygulamanızı görüntülemek için tarayıcınıza atanan ortak uç noktayı ( **URL etiketli URL**) girin.

    ![ASC portalı örnek uygulaması](media/spring-cloud-quickstart-launch-app-portal/sample-app.png)

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-portal-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Hizmet örneği sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikro Hizmetler uygulaması oluşturma
> * Her mikro hizmeti dağıtma
> * Uygulama ağ geçidiniz için genel uç nokta atama

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md)

GitHub 'da daha fazla örnek vardır: [Azure Spring Cloud Samples](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
