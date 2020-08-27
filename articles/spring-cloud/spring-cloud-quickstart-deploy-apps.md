---
title: Hızlı başlangıç-Azure yay bulutuna uygulama oluşturma ve dağıtma
description: Azure yay bulutu 'na uygulama dağıtımını açıklar.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java
ms.openlocfilehash: 770dd021a09e3ba0b1c2c6742ded3a73424b042f
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951753"
---
# <a name="quickstart-build-and-deploy-apps-to-azure-spring-cloud"></a>Hızlı başlangıç: Azure yay bulutuna uygulama oluşturma ve dağıtma

Bu belgede, aşağıdaki kullanılarak mikro hizmet uygulamalarının Azure Spring Cloud 'a nasıl oluşturulacağı ve dağıtılacağı açıklanmaktadır:
* Azure CLI
* Maven eklentisi
* IntelliJ

Azure CLı veya Maven kullanarak dağıtımdan önce [Azure Spring Cloud 'ın bir örneğini](spring-cloud-quickstart-provision-service-instance.md) sağlayan örnekleri tamamlayın ve [yapılandırma sunucusunu ayarlayın](spring-cloud-quickstart-setup-config-server.md).

## <a name="prerequisites"></a>Önkoşullar

* [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
* Seçim [Azure CLI sürüm 2.0.67 veya üstünü yükleyip](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure Spring Cloud uzantısını şu komutla birlikte yüklersiniz: `az extension add --name spring-cloud`
* Seçim [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) ve [oturum açmayı](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in) yükleyip

## <a name="deployment-procedures"></a>Dağıtım yordamları

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

### <a name="build-the-microservices-applications-locally"></a>Mikro hizmet uygulamalarını yerel olarak oluşturma

1. Örnek uygulama deposunu Azure bulut hesabınıza kopyalayın.  

    ```azurecli
    git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Dizini değiştirin ve projeyi derleyin.

    ```azurecli
    cd piggymetrics
    mvn clean package -DskipTests
    ```

Projenin derlenmesi yaklaşık 5 dakika sürer. Tamamlandıktan sonra, ilgili klasörlerinde her bir hizmet için bireysel JAR dosyaları olmalıdır.

### <a name="create-and-deploy-the-apps"></a>Uygulamaları oluşturma ve dağıtma

1. Aşağıdaki komutları kullanarak varsayılan kaynak grubu adınızı ve küme adınızı ayarlayın:

    ```azurecli
    az configure --defaults group=<resource group name>
    az configure --defaults spring-cloud=<service instance name>
    ```

1. Önceki adımda oluşturulan JAR dosyalarını kullanarak Azure Spring Cloud mikro hizmetleri oluşturun. Üç uygulama oluşturacaksınız: **Gateway**, **AUTH-Service**ve **Account-Service**.

    ```azurecli
    az spring-cloud app create --name gateway
    az spring-cloud app create --name auth-service
    az spring-cloud app create --name account-service
    ```

1. Uygulamalarımızı gerçekten Azure 'a dağıtmamız gerekiyor. Üç uygulamayı da dağıtmak için aşağıdaki komutları kullanın:

    ```azurecli
    az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
    az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
    az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
    ```

### <a name="assign-public-endpoint-to-gateway"></a>Ağ geçidine genel uç nokta ata

Bir Web tarayıcısı aracılığıyla uygulamaya erişmek için bir yönteme ihtiyacımız var. Ağ Geçidi uygulamamız, herkese açık bir uç nokta gerektirir.

1. Aşağıdaki komutu kullanarak bitiş noktasını atayın:

    ```azurecli
    az spring-cloud app update -n gateway --is-public true
    ```

2. Uygulamanın çalıştığını doğrulayabilmeniz için **ağ geçidi** uygulamasını ortak IP 'si için sorgulayın:

    ```azurecli
    az spring-cloud app show --name gateway --query properties.url
    ```

#### <a name="maven"></a>[Maven](#tab/Maven)

### <a name="clone-and-build-the-sample-application-repository"></a>Örnek uygulama deposunu kopyalama ve derleme

1. Aşağıdaki komutu çalıştırarak Git deposunu kopyalayın:

    ```
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Dizini değiştirin ve aşağıdaki komutu çalıştırarak projeyi derleyin:

    ```
    cd piggymetrics
    mvn clean package -DskipTests
    ```

### <a name="generate-configurations-and-deploy-to-the-azure-spring-cloud"></a>Azure yay bulutuna yapılandırma ve dağıtım oluşturma

1. Üst Pod 'yi içeren Pıbu ölçümlerin kök klasöründe aşağıdaki komutu çalıştırarak yapılandırma oluşturun. Zaten Azure CLı ile oturum açtıysanız, komut kimlik bilgilerini otomatik olarak seçer. Aksi takdirde, komut istemi yönergeleriyle oturumunuzu açacaksınız. Daha fazla ayrıntı için [wiki](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication) sayfamıza bakın.

    ```
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:1.1.0:config
    ```
    
    Şunları seçmeniz istenir:
    * **Modüller:** Select `gateway` , `auth-service` ve ' Account-Service ' ')
    * **Abonelik:** Azure Spring Cloud Instance ile aboneliğiniz oluşturuldu
    * **Hizmet örneği:** oluşturduğunuz Azure Spring Cloud örneğinin adı
    * **Ortak uç nokta:** Belirtilen projeler listesinde, `gateway` ortak erişim sağlamak için öğesine karşılık gelen sayıyı girin.

1. Pod artık eklenti bağımlılıklarını ve yapılandırmasını içerir. Aşağıdaki komutu kullanarak uygulamaları dağıtın. 

    ```
    mvn azure-spring-cloud:deploy
    ```

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

### <a name="import-sample-project-in-intellij"></a>IntelliJ 'de örnek projeyi içeri aktar

1. Bu öğretici için kaynak depoyu indirip sıkıştırmasını açın veya Git kullanarak kopyalayın: `git clone https://github.com/Azure-Samples/piggymetrics` 

1. IntelliJ **karşılama** iletişim kutusunu açın, içeri aktar sihirbazını açmak Için **projeyi içeri aktar** ' ı seçin.

1. `piggymetric`Klasör seçin.

    ![Projeyi içeri aktar](media/spring-cloud-intellij-howto/revision-import-project-1.png)

### <a name="deploy-gateway-app-to-azure-spring-cloud"></a>Ağ Geçidi uygulamasını Azure Spring Cloud 'a dağıtma
Azure 'a dağıtmak için Azure Toolkit for IntelliJ Azure hesabınızla oturum açmanız ve aboneliğinizi seçmeniz gerekir. Oturum açma ayrıntıları için bkz. [yükleme ve oturum açma](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in).

1. IntelliJ Proje Gezgini ' nde projenize sağ tıklayın ve **Azure**  ->  **yay bulutu 'na Azure dağıtımı**' nı seçin.

    ![Azure 'a dağıtma 1](media/spring-cloud-intellij-howto/revision-deploy-to-azure-1.png)

1. **Ad** alanına append: var olan **ada** ait *ağ geçidi* , yapılandırmaya başvurur.
1. **Yapıt** metin kutusunda, *com. pınmetriölçümleri: Gateway: 1.0-Snapshot*' ı seçin.
1. **Abonelik** metin kutusunda aboneliğinizi doğrulayın.
1. **Yay bulutu** metin kutusunda, [Azure Spring Cloud Instance sağlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-provision-service-instance)bölümünde oluşturduğunuz Azure Spring Cloud örneğini seçin.
1. **Genel uç noktayı** *Etkinleştir*olarak ayarlayın.
1. **Uygulama:** metin kutusunda **uygulama oluştur...** seçeneğini belirleyin.
1. *Ağ geçidini*girin, ardından **Tamam**' a tıklayın.

    ![Azure 'a dağıtma Tamam](media/spring-cloud-intellij-howto/revision-deploy-to-azure-2.png)

1. İletişim kutusunun **başlatma öncesi** bölümünde * Maven hedefini Çalıştır * * öğesine çift tıklayın.
1. **Çalışma dizini** metin kutusunda, *Pize ölçümleri/ağ geçidi* klasörüne gidin.
1. **Komut satırı** metin kutusuna *Package-dskiptests*yazın. **Tamam** düğmesine tıklayın.
1. **Azure yay bulut uygulaması dağıtma** iletişim kutusunun alt kısmındaki **Çalıştır** düğmesine tıklayarak dağıtımı başlatın. Eklenti uygulamada komutunu çalıştırır `mvn package` `gateway` ve komut tarafından oluşturulan jar 'yi dağıtır `package` .

### <a name="deploy-auth-service-and-account-service-apps-to-azure-spring-cloud"></a>Azure yay bulutuna auth-Service ve Account-Service uygulamaları dağıtma
' İ dağıtmak için yukarıdaki adımları yineleyebilirsiniz `auth-service` ve `account-service` uygulamaları Azure Spring Cloud 'a uygulayabilirsiniz. Emin ol:

1. Uygulamayı tanımlamak için **adı** ve **yapıtı** değiştirin `auth-service` .
1. **Uygulama:** metin kutusunda, uygulamalar oluşturmak Için **uygulama oluştur...** seçeneğini belirleyin `auth-service` .
1. **Ortak uç nokta** seçeneğinin *devre dışı*olarak ayarlandığını doğrulayın.
1. İletişim kutusunun **başlatma öncesi** bölümünde, **çalışma dizinini** *pıbu ölçümler/auth-Service* klasörüne geçirin.
1. **Azure yay bulut uygulaması dağıtma** iletişim kutusunun alt kısmındaki **Çalıştır** düğmesine tıklayarak dağıtımı başlatın. 
1. Yapılandırmak ve dağıtmak için bu yordamları tekrarlayın `account-service` .
---

Plımetrik uygulamasına erişmek için önceki adımlarda çıktıda belirtilen URL 'ye gidin. DomainName. `https://<service instance name>-gateway.azuremicroservices.io`

![Erişim ölçümleri](media/spring-cloud-quickstart-launch-app-cli/launch-app.png)

URL 'YI bulmak için Azure portal de gidebilirsiniz. 
1. Hizmete git
2. **Uygulama** seçin
3. **Ağ geçidini** seçin

    ![Uygulamada gezin](media/spring-cloud-quickstart-launch-app-cli/navigate-app1.png)
    
4. Ağ geçidinde URL 'YI bul **| Genel Bakış** sayfası

    ![Uygulamaya git ikinci](media/spring-cloud-quickstart-launch-app-cli/navigate-app2-url.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu portaldan silin veya Cloud Shell aşağıdaki komutu çalıştırın:
```azurecli
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```
Yukarıdaki adımlarda, varsayılan kaynak grubu adını da ayarlarsınız. Bu varsayılan seçimini kaldırmak için Cloud Shell aşağıdaki komutu çalıştırın:
```azurecli
az configure --defaults group=
```
## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Günlükler, ölçümler ve Izleme](spring-cloud-quickstart-logs-metrics-tracing.md)
