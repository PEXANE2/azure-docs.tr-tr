---
title: "QuickStart: Windows'da Java uygulaması oluşturma"
description: Windows'daki ilk Java Hello World'ünüzü birkaç dakika içinde Azure Uygulama Hizmetine dağıtın. Maven için Azure Web Uygulama Eklentisi, Java uygulamalarını dağıtmayı kolaylaştırır.
keywords: azure, uygulama hizmeti, web uygulaması, windows, java, maven, quickstart
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: ed477e3c1431048d60e4a696f59aa0593e1b3f1f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79136338"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-windows"></a>Quickstart: Windows'da Azure Uygulama Hizmeti'nde Java uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux'taki_Uygulama Hizmeti'ne dağıtmak için [bkz.](./containers/quickstart-java.md)
>

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıç, [Maven için Azure Web Uygulama Eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) ile [Azure CLI'nin](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) java web arşivi (WAR) dosyasını dağıtmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Aynı şey de IntelliJ ve Eclipse gibi popüler IES kullanılarak yapılabilir. [Eclipse Quickstart için Azure Toolkit](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app) veya Eclipse [Quickstart için Azure Toolkit'teki](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app)benzer belgelerimize göz atın.
>
![Azure Uygulama Hizmeti'nde çalışan örnek uygulama](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

Adlı `helloworld`yeni bir uygulama oluşturmak için Bulut Kabuğu isteminde aşağıdaki Maven komutunu uygulayın:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Maven eklentisini yapılandırma

Maven'dan dağıtım için, Cloud Shell'deki kod düzenleyiciyi kullanarak `helloworld` dizinindeki proje `pom.xml` dosyasını açın. 

```bash
code pom.xml
```

Sonra `pom.xml` dosyasının `<build>` öğesinin içine aşağıdaki eklenti tanımını ekleyin.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Windows         -->
    <!--*************************************************-->
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.9.0</version>
        <configuration>
            <!-- Specify v2 schema -->
            <schemaVersion>v2</schemaVersion>
            <!-- App information -->
            <subscriptionId>SUBSCRIPTION_ID</subscriptionId>
            <resourceGroup>RESOURCEGROUP_NAME</resourceGroup>
            <appName>WEBAPP_NAME</appName>
            <region>REGION</region>
            <!-- Java Runtime Stack for App Service on Windows-->
            <runtime>
                <os>windows</os>
                <javaVersion>1.8</javaVersion>
                <webContainer>tomcat 9.0</webContainer>
            </runtime>
            <deployment>
                <resources>
                    <resource>
                        <directory>${project.basedir}/target</directory>
                        <includes>
                            <include>*.war</include>
                        </includes>
                    </resource>
                </resources>
            </deployment>
        </configuration>
    </plugin>
</plugins>
```

> [!NOTE]
> Bu makalede yalnızca WAR dosyalarıyla paketlenmiş Java uygulamalarıyla çalışacağız. Eklenti ayrıca JAR web uygulamalarını da destekler. Denemek için [Linux'ta App Service'e Java SE JAR dosyası dağıtma](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).


Eklenti yapılandırmasında aşağıdaki yer tutucuları güncelleştirin:

| Yer tutucu | Açıklama |
| ----------- | ----------- |
| `SUBSCRIPTION_ID` | Uygulamanızı dağıtmak istediğiniz aboneliğin benzersiz kimliği. Varsayılan aboneliğin `az account show` kimliği, komutu kullanarak Bulut Kabuğu'ndan veya CLI'den bulunabilir. Kullanılabilir tüm abonelikler için `az account list` komutu kullanın.|
| `RESOURCEGROUP_NAME` | Uygulamanızı oluşturacak yeni kaynak grubunun adını ada. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. Bu değeri, örneğin *myResourceGroup*gibi benzersiz yeni bir kaynak grubu adı ile güncelleştirin. Bu kaynak grubunu daha sonraki bir bölümde tüm Azure kaynaklarını temizlemek için kullanacaksınız. |
| `WEBAPP_NAME` | Uygulama adı, Azure'a (WEBAPP_NAME.azurewebsites.net) dağıtıldığında uygulamanın ana bilgisayar adının bir parçası olacaktır. Bu değeri, örneğin *contoso*gibi Java uygulamanıza ev sahipliği yapacak olan yeni Uygulama Hizmeti uygulaması için benzersiz bir adla güncelleyin. |
| `REGION` | Uygulamanın barındırıldığı bir Azure bölgesi, örneğin *westus2*. Cloud Shell'den veya CLI'dan `az account list-locations` komutunu kullanarak bölgelerin bir listesini alabilirsiniz. |

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Aşağıdaki komutu kullanarak Java uygulamanızı Azure'a dağıtın:

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna gidin; örneğin `http://<webapp>.azurewebsites.net/`.

![Azure Uygulama Hizmeti'nde çalışan örnek uygulama](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

**Tebrikler!** İlk Java uygulamanızı Windows'daki Uygulama Hizmeti'ne dağıttınız.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Java ile Azure SQL veritabanına bağlanma](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Java ile MySQL için Azure DB'ye bağlanın](/azure/mysql/connect-java?toc=/azure/java/toc.json)

> [!div class="nextstepaction"]
> [Java ile PostgreSQL için Azure DB'ye bağlanın](/azure/postgresql/connect-java?toc=/azure/java/toc.json)

> [!div class="nextstepaction"]
> [Java Geliştiricileri Kaynakları için Azure](/java/azure/)

> [!div class="nextstepaction"]
> [Harita özel etki alanı](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Azure için Maven eklentileri hakkında daha fazla bilgi edinin](https://github.com/microsoft/azure-maven-plugins)
