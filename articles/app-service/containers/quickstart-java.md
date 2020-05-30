---
title: 'Hızlı başlangıç: Linux Java uygulaması oluşturma'
description: İlk Java uygulamanızı App Service bir Linux kapsayıcısına dağıtarak Azure App Service Linux uygulamalarını kullanmaya başlayın.
keywords: Azure, App Service, Web uygulaması, Linux, Java, Maven, hızlı başlangıç
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/27/2019
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 4c65160cb648954ef97bd2a3d3e9ac0d38440ce7
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193841"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-linux"></a>Hızlı başlangıç: Linux üzerinde Azure App Service Java uygulaması oluşturma

[Linux üzerinde App Service](app-service-linux-intro.md) , Linux işletim sistemini kullanarak yüksek düzeyde ölçeklenebilir, kendini yayama bir Web barındırma hizmeti sağlar. Bu hızlı başlangıçta, Linux işletim sisteminde bir Java Web arşivi (WAR) dosyasını dağıtmak üzere [Maven için Azure Web App eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) Ile [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) 'nın nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
>
> Ayrıca, IntelliJ, tutulma ve VS Code gibi popüler Ides 'ler kullanılarak da aynı şey yapılabilir. [Azure Toolkit for IntelliJ hızlı](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app)başlangıç [Azure Toolkit for Eclipse hızlı](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app) başlangıç veya [vs Code hızlı](https://code.visualstudio.com/docs/java/java-webapp)başlangıç aşamasında benzer belgelerimize göz atın.
>
![Azure App Service 'de çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

Aşağıdaki Maven komutunu Cloud Shell istemine yürütün, adlı yeni bir uygulama oluşturun `helloworld` :

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp"
```
Sonra çalışma dizininizi proje klasörü olarak değiştirin:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Maven eklentisini yapılandırma

Azure App Service dağıtım işlemi, Azure CLı 'den hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI Ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) .

```azurecli
az login
```

Ardından, dağıtımı yapılandırabilir, komut Isteminde Maven komutunu çalıştırabilir ve **Onayla (Y/N)** istemi görüntüleninceye ve ardından **' Y '** tuşuna basarak ve yapılandırma yapıldıktan sonra **ENTER** tuşuna basarak varsayılan yapılandırmaları kullanabilirsiniz. 
```cmd
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```
Örnek bir işlem şöyle görünür:

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

`pom.xml`Eklenti yapılandırmasının güncelleştirildiğini görmek için bir kez daha App Service diğer yapılandırmaları, gerekirse doğrudan Pod dosyanızda değiştirebilirsiniz, bazı yaygın olanlar aşağıda listelenmiştir:

 Özellik | Gerekli | Açıklama | Sürüm
---|---|---|---
`<schemaVersion>` | yanlış | Yapılandırma şemasının sürümünü belirtin. Desteklenen değerler şunlardır: `v1` , `v2` . | 1.5.2
`<resourceGroup>` | true | Web uygulamanız için Azure Kaynak grubu. | 0.1.0 +
`<appName>` | true | Web uygulamanızın adı. | 0.1.0 +
`<region>` | true | Web uygulamanızın barındırılacak bölgeyi belirtir; Varsayılan değer **westeurope**' dir. [Desteklenen bölgelerde](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) tüm geçerli bölgeler bölümü. | 0.1.0 +
`<pricingTier>` | yanlış | Web uygulamanız için fiyatlandırma katmanı. Varsayılan değer **P1V2**' dir.| 0.1.0 +
`<runtime>` | true | Çalışma zamanı ortamı yapılandırması, [burada](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)ayrıntıları görebilirsiniz. | 0.1.0 +
`<deployment>` | true | Dağıtım yapılandırması, ayrıntıları [burada](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)görebilirsiniz. | 0.1.0 +

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=config)

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Aşağıdaki komutu kullanarak Java uygulamanızı Azure'a dağıtın:

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna gidin; örneğin `http://<webapp>.azurewebsites.net`. 

![Azure App Service 'de çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**Tebrikler!** Linux üzerinde App Service’e ilk Java uygulamanızı dağıttınız.

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
> [Java ile Azure SQL veritabanı 'na bağlanma](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Java ile MySQL için Azure DB 'ye bağlanma](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Java ile PostgreSQL için Azure DB 'ye bağlanma](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Java uygulamasını yapılandırma](configure-language-java.md)

> [!div class="nextstepaction"]
> [Jenkins ile CI/CD](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Diğer Java geliştiricileri için Azure kaynakları](/java/azure/)

> [!div class="nextstepaction"]
> [Azure için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugins)
