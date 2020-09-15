---
title: 'Hızlı başlangıç: Azure App Service Java uygulaması oluşturma'
description: İlk Java Merhaba Dünya Azure App Service dakikalar içinde dağıtın. Maven için Azure Web uygulaması eklentisi, Java uygulamalarını dağıtmayı kullanışlı hale getirir.
keywords: Azure, App Service, Web uygulaması, Windows, Linux, Java, Maven, hızlı başlangıç
author: jasonfreeberg
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 08/01/2020
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 7f8e87b22e3d8f6e265789f910863b2790024cbf
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532418"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service"></a>Hızlı başlangıç: Azure App Service Java uygulaması oluşturma

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıçta, bir. jar dosyası veya. war dosyası dağıtmak üzere [Maven için Azure Web App eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) Ile [Azure CLI](/cli/azure/get-started-with-azure-cli) 'nın nasıl kullanılacağı gösterilmektedir. Java & Tomcat yönergeleri arasında geçiş yapmak için sekmeleri kullanın.


> [!NOTE]
> Aynı zamanda IntelliJ ve tutulma gibi popüler Ides 'ler kullanılarak da yapılabilir. [Azure Toolkit for IntelliJ hızlı](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app) başlangıç veya [Azure Toolkit for Eclipse hızlı başlangıç](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app)aşamasında benzer belgelerimize göz atın.


![Azure App Service 'de çalışan örnek uygulama](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

# <a name="java-se"></a>[Java SE](#tab/javase)

[Spring Boot Başlarken](https://github.com/spring-guides/gs-spring-boot) örnek projesini klonlayın.

```bash
git clone https://github.com/spring-guides/gs-spring-boot
```

Tamamlanmış projenin dizinine geçin.

```bash
cd gs-spring-boot/complete
```

# <a name="tomcat"></a>[Tomcat](#tab/tomcat)

Aşağıdaki Maven komutunu Cloud Shell istemine yürütün, adlı yeni bir uygulama oluşturun `helloworld` :

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp" "-Dversion=1.0-SNAPSHOT"
```

Sonra çalışma dizininizi proje klasörü olarak değiştirin:

```bash
cd helloworld
```

---

## <a name="configure-the-maven-plugin"></a>Maven eklentisini yapılandırma

Azure App Service dağıtım işlemi, Azure CLı 'daki Azure kimlik bilgilerinizi otomatik olarak kullanacaktır. Azure CLı yerel olarak yüklü değilse, Maven eklentisi OAuth veya cihaz oturum açma bilgileriyle kimlik doğrulaması yapılır. Daha fazla bilgi için bkz. [Maven eklentileri ile kimlik doğrulama](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication).

Dağıtımı yapılandırmak için aşağıdaki Maven komutunu çalıştırın. Bu komut, App Service işletim sistemini, Java sürümünü ve Tomcat sürümünü ayarlamanıza yardımcı olur.

```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.10.0:config
```

::: zone pivot="platform-windows"

# <a name="java-se"></a>[Java SE](#tab/javase)

1. İstendiğinde, girerek **Windows** ' u seçin `2` .
2. Varsayılan Java sürümü olan 1,8 ' i kullanarak ENTER tuşuna basın.
3. Son olarak, Seçimlerinizi onaylamak için son sorulduğunda ENTER tuşuna basın.

    Özet çıktınızı aşağıda gösterilen kod parçacığına benzer şekilde görünecektir.

    ```
    Please confirm webapp properties
    AppName : spring-boot-1599007390755
    ResourceGroup : spring-boot-1599007390755-rg
    Region : westeurope
    PricingTier : PremiumV2_P1v2
    OS : Windows
    Java : 1.8
    WebContainer : java 8
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 41.118 s
    [INFO] Finished at: 2020-09-01T17:43:45-07:00
    [INFO] ------------------------------------------------------------------------
    ```

# <a name="tomcat"></a>[Tomcat](#tab/tomcat)

1. İstendiğinde, girerek **Windows** ' u seçin `2` .
1. Varsayılan Java sürümü olan 1,8 ' i kullanarak ENTER tuşuna basın.
1. ENTER tuşuna basarak, Tomcat 8,5 varsayılan Web kapsayıcısını kullanın.
1. Son olarak, Seçimlerinizi onaylamak için son sorulduğunda ENTER tuşuna basın.

    Özet çıktınızı aşağıda gösterilen kod parçacığına benzer şekilde görünecektir.

    ```
    Please confirm webapp properties
    AppName : helloworld-1599003152123
    ResourceGroup : helloworld-1599003152123-rg
    Region : westeurope
    PricingTier : PremiumV2_P1v2
    OS : Windows
    Java : 1.8
    WebContainer : tomcat 8.5
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 03:03 min
    [INFO] Finished at: 2020-09-01T16:35:30-07:00
    [INFO] ------------------------------------------------------------------------
    ```

---

::: zone-end
::: zone pivot="platform-linux"

### <a name="java-se"></a>[Java SE](#tab/javase)

1. İstendiğinde, ENTER tuşuna basarak **Linux** ' u seçin.
2. Varsayılan Java sürümü olan 1,8 ' i kullanarak ENTER tuşuna basın.
3. Son olarak, Seçimlerinizi onaylamak için son sorulduğunda ENTER tuşuna basın.

    ```
    Please confirm webapp properties
    AppName : spring-boot-1599007116351
    ResourceGroup : spring-boot-1599007116351-rg
    Region : westeurope
    PricingTier : PremiumV2_P1v2
    OS : Linux
    RuntimeStack : JAVA 8-jre8
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 20.925 s
    [INFO] Finished at: 2020-09-01T17:38:51-07:00
    [INFO] ------------------------------------------------------------------------
    ```

### <a name="tomcat"></a>[Tomcat](#tab/tomcat)

1. İstendiğinde, ENTER tuşuna basarak **Linux** ' u seçin.
1. Varsayılan Java sürümü olan 1,8 ' i kullanarak ENTER tuşuna basın.
1. ENTER tuşuna basarak, Tomcat 8,5 varsayılan Web kapsayıcısını kullanın.
1. Son olarak, Seçimlerinizi onaylamak için son sorulduğunda ENTER tuşuna basın.

    ```
    Please confirm webapp properties
    AppName : helloworld-1599003744223
    ResourceGroup : helloworld-1599003744223-rg
    Region : westeurope
    PricingTier : PremiumV2_P1v2
    OS : Linux
    RuntimeStack : TOMCAT 8.5-jre8
    Deploy to slot : false
    Confirm (Y/N)? : Y
    [INFO] Saving configuration to pom.
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 50.785 s
    [INFO] Finished at: 2020-09-01T16:43:09-07:00
    [INFO] ------------------------------------------------------------------------
    ```

---

::: zone-end

Gerekirse, App Service yapılandırmasını doğrudan ' de değiştirebilirsiniz `pom.xml` . Bazı yaygın olanlar aşağıda listelenmiştir:

Özellik | Gerekli | Açıklama | Sürüm
---|---|---|---
`<schemaVersion>` | yanlış | Yapılandırma şemasının sürümünü belirtin. Desteklenen değerler şunlardır: `v1` , `v2` . | 1.5.2
`<resourceGroup>` | true | Web uygulamanız için Azure Kaynak grubu. | 0.1.0 +
`<appName>` | true | Web uygulamanızın adı. | 0.1.0 +
`<region>` | true | Web uygulamanızın barındırılacak bölgeyi belirtir; Varsayılan değer **westeurope**' dir. [Desteklenen bölgelerde](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) tüm geçerli bölgeler bölümü. | 0.1.0 +
`<pricingTier>` | yanlış | Web uygulamanız için fiyatlandırma katmanı. Varsayılan değer **P1V2**' dir.| 0.1.0 +
`<runtime>` | true | Çalışma zamanı ortamı yapılandırması, [burada](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)ayrıntıları görebilirsiniz. | 0.1.0 +
`<deployment>` | true | Dağıtım yapılandırması, ayrıntıları [burada](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)görebilirsiniz. | 0.1.0 +

`<appName>`Ve `<resourceGroup>` ( `helloworld-1590394316693` `helloworld-1590394316693-rg` demo olarak da buna göre) değerleri hakkında dikkatli olun, daha sonra kullanılır.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=quickstart-java&step=config)

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Maven eklentisi, uygulama hizmetlerine dağıtmak için Azure CLı 'den hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI Ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) .

```azurecli
az login
```

Ardından, aşağıdaki komutu kullanarak Java uygulamanızı Azure 'a dağıtabilirsiniz.

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra uygulamanız ' `http://<appName>.azurewebsites.net/` `http://helloworld-1590394316693.azurewebsites.net` de (tanıtımda) hazırlanacaktır. Yerel Web tarayıcınızla URL 'yi açın, şunu görmeniz gerekir:

![Azure App Service 'de çalışan örnek uygulama](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Tebrikler!** App Service için ilk Java uygulamanızı dağıttınız.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu portaldan silin veya Cloud Shell aşağıdaki komutu çalıştırın:

```azurecli-interactive
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Bu komutun çalıştırılması bir dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Java ile Azure SQL veritabanı 'na bağlanma](../azure-sql/database/connect-query-java.md?toc=%2fazure%2fjava%2ftoc.json)

> [!div class="nextstepaction"]
> [Java ile MySQL için Azure DB 'ye bağlanma](../mysql/connect-java.md)

> [!div class="nextstepaction"]
> [Java ile PostgreSQL için Azure DB 'ye bağlanma](../postgresql/connect-java.md)

> [!div class="nextstepaction"]
> [Java geliştiricileri için Azure kaynakları](/java/azure/)

> [!div class="nextstepaction"]
> [Java uygulamasını yapılandırma](configure-language-java.md)

> [!div class="nextstepaction"]
> [Jenkins ile CI/CD](/azure/developer/jenkins/deploy-to-azure-app-service-using-plugin)

> [!div class="nextstepaction"]
> [Özel etki alanı eşleme](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Azure için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugins)
