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
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 64e4c05e9439c164329dede5d714bec160bc5ae2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102050384"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service"></a>Hızlı başlangıç: Azure App Service Java uygulaması oluşturma

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıçta, bir. jar dosyası veya. war dosyası dağıtmak üzere [Maven için Azure Web App eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) Ile [Azure CLI](/cli/azure/get-started-with-azure-cli) 'nın nasıl kullanılacağı gösterilmektedir. 

Bu makalenin IDE sürümleri de vardır. [Azure Toolkit for IntelliJ hızlı başlangıç](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app) veya [Azure Toolkit for Eclipse hızlı başlangıç](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app)yapın.

## <a name="1-prepare-your-environment"></a>1. ortamınızı hazırlayın

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Adet <abbr title="Azure kullanımı için fatura bilgilerini tutan profil.">Azure hesabı</abbr> etkin bir <abbr title="Azure 'da, genellikle bir kuruluş içindeki bir birey veya departmanla ilişkili kaynakları yönettiğiniz temel kuruluş yapısıdır.">aboneliği</abbr>. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure CLI](/cli/azure/install-azure-cli).

+ [Java geliştirici seti](/azure/developer/java/fundamentals/java-jdk-long-term-support), sürüm 8 veya 11.

+ [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri.

Maven eklentisi, uygulama hizmetlerine dağıtmak için Azure CLı 'den hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI Ile oturum açın](/cli/azure/authenticate-azure-cli) . Daha fazla bilgi için bkz. [Maven eklentileri ile kimlik doğrulama](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication).

```azurecli
az login
```

<br>
<hr/>

## <a name="2-create-a-java-app"></a>2. Java uygulaması oluşturma

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

<br>
<hr/>

## <a name="3-configure-the-maven-plugin"></a>3. Maven eklentisini yapılandırma

Dağıtımı yapılandırmak için aşağıdaki Maven komutunu çalıştırın. Bu komut, App Service işletim sistemini, Java sürümünü ve Tomcat sürümünü ayarlamanıza yardımcı olur.

```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.12.0:config
```

::: zone pivot="platform-windows"

# <a name="java-se"></a>[Java SE](#tab/javase)

1. **Abonelik** seçeneğiyle istendiğinde, satır başındaki numarayı girerek uygun `Subscription` öğesini seçin.
1. **Web uygulaması** seçeneği istendiğinde, `<create>` ENTER tuşuna basarak veya mevcut bir uygulamayı seçerek varsayılan seçeneği kabul edin.
1. **Işletim sistemi** seçeneğinde sorulduğunda, girerek **Windows** ' u seçin `3` .
1. **Fiyatlandırma katmanı** seçeneği ile sorulduğunda, girerek **B2** ' ı seçin `2` .
1. ENTER tuşuna basarak Java **8** varsayılan Java sürümünü kullanın.
1. Son olarak, seçimlerinizi onaylamak için son istemde Enter tuşuna basın.

    Özet çıktınızı aşağıda gösterilen kod parçacığına benzer şekilde görünecektir.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : spring-boot-1599007390755
    ResourceGroup : spring-boot-1599007390755-rg
    Region : westeurope
    PricingTier : Basic_B2
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
    </pre>

# <a name="tomcat"></a>[Tomcat](#tab/tomcat)

1. **Abonelik** seçeneğiyle istendiğinde, satır başındaki numarayı girerek uygun `Subscription` öğesini seçin.
1. **Web uygulaması** seçeneği istendiğinde, `<create>` ENTER tuşuna basarak veya mevcut bir uygulamayı seçerek varsayılan seçeneği kabul edin.
1. **Işletim sistemi** seçeneğinde sorulduğunda, girerek **Windows** ' u seçin `3` .
1. **Fiyatlandırma katmanı** seçeneği ile sorulduğunda, girerek **B2** ' ı seçin `2` .
1. ENTER tuşuna basarak Java **8** varsayılan Java sürümünü kullanın.
1. ENTER tuşuna basarak, **Tomcat 8,5** varsayılan Web kapsayıcısını kullanın.
1. Son olarak, seçimlerinizi onaylamak için son istemde Enter tuşuna basın.

    Özet çıktınızı aşağıda gösterilen kod parçacığına benzer şekilde görünecektir.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : helloworld-1599003152123
    ResourceGroup : helloworld-1599003152123-rg
    Region : westeurope
    PricingTier : Basic_B2
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
    </pre>

---

::: zone-end
::: zone pivot="platform-linux"

### <a name="java-se"></a>[Java SE](#tab/javase)

1. **Abonelik** seçeneğiyle istendiğinde, satır başındaki numarayı girerek uygun `Subscription` öğesini seçin.
1. **Web uygulaması** seçeneği istendiğinde, `<create>` ENTER tuşuna basarak veya mevcut bir uygulamayı seçerek varsayılan seçeneği kabul edin.
1. **Işletim sistemi** seçeneğinde sorulduğunda, ENTER tuşuna basarak **Linux** ' u seçin.
1. **Fiyatlandırma katmanı** seçeneği ile sorulduğunda, girerek **B2** ' ı seçin `2` .
1. ENTER tuşuna basarak Java **8** varsayılan Java sürümünü kullanın.
1. Son olarak, seçimlerinizi onaylamak için son istemde Enter tuşuna basın.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : spring-boot-1599007116351
    ResourceGroup : spring-boot-1599007116351-rg
    Region : westeurope
    PricingTier : Basic_B2
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
    </pre>

### <a name="tomcat"></a>[Tomcat](#tab/tomcat)

1. **Abonelik** seçeneğiyle istendiğinde, satır başındaki numarayı girerek uygun `Subscription` öğesini seçin.
1. **Web uygulaması** seçeneği istendiğinde, `<create>` ENTER tuşuna basarak veya mevcut bir uygulamayı seçerek varsayılan seçeneği kabul edin.
1. **Işletim sistemi** seçeneğinde sorulduğunda, ENTER tuşuna basarak **Linux** ' u seçin.
1. **Fiyatlandırma katmanı** seçeneği ile sorulduğunda, girerek **B2** ' ı seçin `2` .
1. ENTER tuşuna basarak Java **8** varsayılan Java sürümünü kullanın.
1. ENTER tuşuna basarak, **Tomcat 8,5** varsayılan Web kapsayıcısını kullanın.
1. Son olarak, seçimlerinizi onaylamak için son istemde Enter tuşuna basın.

    <pre class="is-monospace is-size-small has-padding-medium has-background-tertiary has-text-tertiary-invert">
    Please confirm webapp properties
    Subscription Id : ********-****-****-****-************
    AppName : helloworld-1599003744223
    ResourceGroup : helloworld-1599003744223-rg
    Region : westeurope
    PricingTier : Basic_B2
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
    </pre>

---

::: zone-end

Gerekirse, App Service yapılandırmasını doğrudan ' de değiştirebilirsiniz `pom.xml` . Bazı yaygın olanlar aşağıda listelenmiştir:

Özellik | Gerekli | Açıklama | Sürüm
---|---|---|---
`<schemaVersion>` | yanlış | Yapılandırma şemasının sürümünü belirtin. Desteklenen değerler şunlardır: `v1` , `v2` . | 1.5.2
`<subscriptionId>` | yanlış | Abonelik KIMLIĞINI belirtin. | 0.1.0 +
`<resourceGroup>` | true | Azure <abbr title="Birim olarak yönetebileceğiniz ilgili Azure kaynakları için mantıksal bir kapsayıcı.">kaynak grubu</abbr> Web uygulamanız için. | 0.1.0 +
`<appName>` | true | Web uygulamanızın adı. | 0.1.0 +
`<region>` | true | Web uygulamanızın barındırılacak bölgeyi belirtir; Varsayılan değer **westeurope**' dir. [Desteklenen bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=app-service) tüm geçerli bölgeler bölümü. | 0.1.0 +
`<pricingTier>` | yanlış | Web uygulamanız için fiyatlandırma katmanı. Varsayılan değer, üretim iş yükü için **P1V2** , **B2** ise Java geliştirme ve test için önerilen en düşük gereksinimdir. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/linux/)| 0.1.0 +
`<runtime>` | true | Çalışma zamanı ortamı yapılandırması, [burada](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Web-App:-Configuration-Details)ayrıntıları görebilirsiniz. | 0.1.0 +
`<deployment>` | true | Dağıtım yapılandırması, ayrıntıları [burada](https://github.com/microsoft/azure-maven-plugins/wiki/Azure-Web-App:-Configuration-Details)görebilirsiniz. | 0.1.0 +

`<appName>`Ve `<resourceGroup>` ( `helloworld-1590394316693` örnek çıkışa göre) değerleri hakkında dikkatli olun `helloworld-1590394316693-rg` , daha sonra kullanılır.

[Sorun bildirin](https://www.research.net/r/javae2e?tutorial=quickstart-java&step=config)

<br>
<hr/>

## <a name="4-deploy-the-app"></a>4. uygulamayı dağıtma

Aşağıdaki komutu kullanarak Java uygulamanızı Azure 'a dağıtın.

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra uygulamanız tarihinde hazırlanacaktır `http://<appName>.azurewebsites.net/` . Yerel Web tarayıcınızla URL 'yi açın. Aşağıdaki ekranı görmeniz gerekir:

![Azure App Service 'de çalışan örnek uygulama](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Tebrikler!** App Service için ilk Java uygulamanızı dağıttınız.

[Sorun bildirin](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

<br>
<hr/>

## <a name="5-clean-up-resources"></a>5. Kaynakları Temizleme

Daha fazla maliyet ödemeden kaçınmak için Java uygulamasını ve ilgili kaynaklarını silin.

```azurecli
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Bu komutun çalıştırılması bir dakika sürebilir.

<br>
<hr/>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Java ile PostgreSQL için Azure DB 'ye bağlanma](../postgresql/connect-java.md)

> [!div class="nextstepaction"]
> [CI/CD ayarlama](deploy-continuous-deployment.md)

> [!div class="nextstepaction"]
> [Fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/app-service/linux/)

> [!div class="nextstepaction"]
> [Toplu Günlükler ve ölçümler](troubleshoot-diagnostic-logs.md)

> [!div class="nextstepaction"]
> [Ölçeği artırma](manage-scale-up.md)

> [!div class="nextstepaction"]
> [Java geliştiricileri için Azure kaynakları](/java/azure/)

> [!div class="nextstepaction"]
> [Java uygulamanızı yapılandırma](configure-language-java.md)
