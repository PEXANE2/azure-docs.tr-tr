---
title: 'Hızlı başlangıç: Windows üzerinde Java uygulaması oluşturma'
description: Birkaç dakika içinde Windows üzerinde Azure App Service için ilk Java Merhaba Dünya dağıtın. Maven için Azure Web uygulaması eklentisi, Java uygulamalarını dağıtmayı kullanışlı hale getirir.
keywords: Azure, App Service, Web uygulaması, Windows, Java, Maven, hızlı başlangıç
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 248e4e6966004ee70cd2c84a1f526bdad06cfa26
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005268"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-windows"></a>Hızlı Başlangıç: Windows üzerinde Azure App Service’ta Java uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux_üzerinde App Service dağıtmak için bkz. [Linux üzerinde Java Web uygulaması oluşturma](./containers/quickstart-java.md).
>

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıçta, bir Java Web arşivi (WAR) dosyası dağıtmak üzere [Maven için Azure Web App eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) Ile [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) 'nın nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> Aynı şey, IntelliJ ve tutulma gibi popüler Ides 'ler kullanılarak da yapılabilir. [Azure Toolkit for IntelliJ hızlı](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app) başlangıç veya [Azure Toolkit for Eclipse hızlı başlangıç](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app)aşamasında benzer belgelerimize göz atın.
>
![Azure App Service 'de çalışan örnek uygulama](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

Aşağıdaki Maven komutunu Cloud Shell istemine yürütün, adlı yeni bir uygulama oluşturun `helloworld` :

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp" "-Dversion=1.0-SNAPSHOT"
```

Sonra çalışma dizininizi proje klasörü olarak değiştirin:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Maven eklentisini yapılandırma

Azure App Service dağıtım işlemi, Azure CLı 'dan Azure CLı 'dan otomatik olarak tanıtım edebilir. Azure CLı yüklü değilse, Maven eklentisi OAuth veya cihaz oturum açma bilgilerini kullanarak oturumunuzu açacaktır. Gerekirse [Maven eklentilerle kimlik doğrulama](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication) ayrıntılarını denetleyin.

Dağıtımı yapılandırmak için komut Isteminde aşağıdaki Maven komutunu çalıştırabilirsiniz, ilk adımda **Windows** işletim sistemi için **' 2 '** seçeneğini belirleyin, ardından **onay (Y/N)** **istemi görünene kadar** varsayılan yapılandırmaları kabul edin ve ardından **' Y '** tuşuna basın ve yapılandırma yapılır. 

```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```

Örnek bir işlem şöyle görünür:

```console
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
Enter index to use: 2
Define value for javaVersion(Default: 1.8): 
1. 1.7
2. 1.7.0_191_ZULU
3. 1.7.0_51
4. 1.7.0_71
5. 1.7.0_80
6. 1.8 [*]
7. 1.8.0_102
8. 1.8.0_111
9. 1.8.0_144
10. 1.8.0_172
11. 1.8.0_172_ZULU
12. 1.8.0_181
13. 1.8.0_181_ZULU
14. 1.8.0_202
15. 1.8.0_202_ZULU
16. 1.8.0_25
17. 1.8.0_60
18. 1.8.0_73
19. 1.8.0_92
20. 11
21. 11.0.2_ZULU
Enter index to use:
Define value for webContainer(Default: tomcat 8.5): 
1. jetty 9.1
2. jetty 9.1.0.20131115
3. jetty 9.3
4. jetty 9.3.13.20161014
5. tomcat 7.0
6. tomcat 7.0.50
7. tomcat 7.0.62
8. tomcat 8.0
9. tomcat 8.0.23
10. tomcat 8.5 [*]
11. tomcat 8.5.20
12. tomcat 8.5.31
13. tomcat 8.5.34
14. tomcat 8.5.37
15. tomcat 8.5.6
16. tomcat 9.0
17. tomcat 9.0.0
18. tomcat 9.0.12
19. tomcat 9.0.14
20. tomcat 9.0.8
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1590394316693
ResourceGroup : helloworld-1590394316693-rg
Region : westeurope
PricingTier : PremiumV2_P1v2
OS : Windows
Java : 1.8
WebContainer : tomcat 8.5
Deploy to slot : false
Confirm (Y/N)? :
[INFO] Saving configuration to pom.
```

> [!NOTE]
> Bu makalede yalnızca WAR dosyalarıyla paketlenmiş Java uygulamalarıyla çalışacağız. Eklenti ayrıca JAR web uygulamalarını da destekler. Denemek için [Linux'ta App Service'e Java SE JAR dosyası dağıtma](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

`pom.xml`Güncelleştirilmiş yapılandırmayı görmek için ' i açın.

```bash
code pom.xml
```

App Service yapılandırmasını doğrudan Pod dosyanızda, gerekirse, bazı yaygın olanlar aşağıda listelenmiştir:

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
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=app-service-web-get-started-java&step=config)

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Azure App Service dağıtım işlemi, Azure CLı 'den hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI Ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) .

```azurecli
az login
```
Ardından, aşağıdaki komutu kullanarak Java uygulamanızı Azure 'a dağıtabilirsiniz:

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna gidin; örneğin `http://<webapp>.azurewebsites.net/`.

![Azure App Service 'de çalışan örnek uygulama](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

**Tebrikler!** Windows üzerinde App Service için ilk Java uygulamanızı dağıttınız.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Java ile Azure SQL veritabanı 'na bağlanma](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Java ile MySQL için Azure DB 'ye bağlanma](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Java ile PostgreSQL için Azure DB 'ye bağlanma](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Java geliştiricileri için Azure kaynakları](/java/azure/)

> [!div class="nextstepaction"]
> [Özel etki alanı eşleme](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Azure için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugins)
