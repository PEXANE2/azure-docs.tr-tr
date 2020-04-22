---
title: 'Quickstart: Linux Java uygulaması oluşturun'
description: İlk Java uygulamanızı App Service'teki bir Linux konteynerine dağıtarak Azure Uygulama Hizmeti'ndeki Linux uygulamalarıyla başlayın.
keywords: azure, uygulama hizmeti, web uygulaması, linux, java, maven, quickstart
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/27/2019
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 8f2e99ffc9f9ee5c5553e8d933d82f83999c8ab2
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732894"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-linux"></a>Quickstart: Linux'ta Azure Uygulama Hizmeti'nde bir Java uygulaması oluşturun

[Linux'taki App Service,](app-service-linux-intro.md) Linux işletim sistemini kullanarak yüksek ölçeklenebilir, kendi kendine yama, web barındırma hizmeti sağlar. Bu hızlı başlangıç, [Maven için Azure Web App Eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) ile [Azure CLI'nin](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) Linux işletim sistemine bir Java web arşivi (WAR) dosyası dağıtmak için nasıl kullanılacağını gösterir.

> [!NOTE]
>
> Aynı şey de IntelliJ, Eclipse ve VS Kodu gibi popüler IES kullanılarak yapılabilir. [IntelliJ Quickstart için Azure Toolkit,](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app) [Eclipse Quickstart için Azure Toolkit](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app) veya [VS Code Quickstart](https://code.visualstudio.com/docs/java/java-webapp)adresindeki benzer belgelerimize göz atın.
>
![Azure Uygulama Hizmeti'nde çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

Adlı `helloworld`yeni bir uygulama oluşturmak için Bulut Kabuğu isteminde aşağıdaki Maven komutunu uygulayın:

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp"
```
Ardından çalışma dizininizi proje klasörüne değiştirin:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Maven eklentisini yapılandırma

Azure Uygulama Hizmeti'ne dağıtma işlemi, Azure CLI'nin hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI ile oturum açın.](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

```azurecli
az login
```

Daha sonra dağıtımı yapılandırabilir, Komut İstemi'nde maven komutunu çalıştırabilir ve **Onay (Y/N)** komut istemini alana kadar **ENTER** tuşuna basarak varsayılan yapılandırmaları kullanabilir, ardından **'y'** tuşuna basıp yapılandırma tamamlanır. 
```cmd
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```
Örnek bir işlem gibi görünür:

```cmd
~@Azure:~/helloworld$ mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< example.demo:helloworld >-----------------------
[INFO] Building helloworld Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.9.1:config (default-cli) @ helloworld ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. Java 11
2. Java 8 [*]
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. TOMCAT 8.5 [*]
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1558400876966
ResourceGroup : helloworld-1558400876966-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : TOMCAT 8.5-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

> [!NOTE]
> Bu makalede yalnızca WAR dosyalarıyla paketlenmiş Java uygulamalarıyla çalışacağız. Eklenti ayrıca JAR web uygulamalarını da destekler. Denemek için [Linux'ta App Service'e Java SE JAR dosyası dağıtma](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

Eklenti `pom.xml` yapılandırmasının güncelleştirilip güncelleştirileni görmek için tekrar gidin, Gerekirse doğrudan pom dosyanızda App Service için diğer yapılandırmaları değiştirebilirsiniz, bazı yaygın olanlar aşağıda listelenmiştir:

 Özellik | Gerekli | Açıklama | Sürüm
---|---|---|---
`<schemaVersion>` | yanlış | Yapılandırma şemasının sürümünü belirtin. Desteklenen değerler şunlardır: `v1`, `v2`. | 1.5.2
`<resourceGroup>` | true | Web Uygulamanız için Azure Kaynak Grubu. | 0.1.0+
`<appName>` | true | Web Uygulamanızın adı. | 0.1.0+
`<region>` | true | Web Uygulamanızın barındırılan bölgeyi belirtir; varsayılan değer **westeurope'dur.** [Desteklenen Bölgeler](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) bölümündeki tüm geçerli bölgeler. | 0.1.0+
`<pricingTier>` | yanlış | Web Uygulamanızın fiyatlandırma katmanı. Varsayılan değer **P1V2'dir.**| 0.1.0+
`<runtime>` | true | Çalışma zamanı ortamı yapılandırması, [burada](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)ayrıntıları görebilirsiniz. | 0.1.0+
`<deployment>` | true | Dağıtım yapılandırması, [burada](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)ayrıntıları görebilirsiniz. | 0.1.0+

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=config)

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Aşağıdaki komutu kullanarak Java uygulamanızı Azure'a dağıtın:

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna gidin; örneğin `http://<webapp>.azurewebsites.net`. 

![Azure Uygulama Hizmeti'nde çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Tebrikler!** Linux üzerinde App Service’e ilk Java uygulamanızı dağıttınız.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayı beklemiyorsanız, kaynak grubunu portaldan silin veya Bulut Kabuğu'nda aşağıdaki komutu çalıştırarak:

```azurecli-interactive
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Bu komutun çalıştırılması bir dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Java ile Azure SQL veritabanına bağlanma](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Java ile MySQL için Azure DB'ye bağlanın](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Java ile PostgreSQL için Azure DB'ye bağlanın](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Java uygulamasını yapılandırma](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Jenkins ile CI/CD](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Java Geliştiricileri Kaynakları için Diğer Azure](/java/azure/)

> [!div class="nextstepaction"]
> [Azure için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugins)
