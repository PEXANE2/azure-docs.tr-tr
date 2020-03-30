---
title: Quickstart - Maven ve Jib kullanarak Java kapsayıcı görüntülerini Azure Konteyner Kayıt Defteri'ne oluşturun ve itin
description: Kapsayıcı bir Java uygulaması oluşturun ve Maven Jib eklentisini kullanarak Azure Konteyner Kayıt Defteri'ne taşıyın.
author: KarlErickson
ms.author: karler
ms.topic: quickstart
ms.date: 02/26/2020
ms.openlocfilehash: 62d63b24baab204cb029565b109ea2de768e1d80
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78165449"
---
# <a name="quickstart-build-and-push-java-container-images-to-azure-container-registry"></a>Quickstart: Java kapsayıcı görüntülerini Azure Kapsayıcı Kayıt Defteri'ne oluşturun ve itin

Bu hızlı başlangıç, maven Jib eklentisini kullanarak nasıl kapsayıcı bir Java uygulaması oluşturup Azure Konteyner Kayıt Defteri'ne taşıyabileceğinizi gösterir. Maven ve Jib'in kullanımı, bir Azure kapsayıcı kayıt defteriyle etkileşim kurmak için geliştirici aracını kullanmaya bir örnektir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği; henüz Azure aboneliğiniz yoksa [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz Azure hesabı](https://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.
* [Azure Komut Satırı Arabirimi (CLI)](/cli/azure/overview).
* Desteklenen bir Java Development Kit (JDK). Azure’da geliştirme sırasında kullanılabilecek JDK’ler hakkında daha fazla bilgi için bkz. <https://aka.ms/azure-jdks>.
* Apache's [Maven](http://maven.apache.org) yapı aracı (Sürüm 3 veya üzeri).
* [Git](https://git-scm.com) istemcisi.
* [Docker](https://www.docker.com) istemcisi.
* [ACR Docker kimlik yardımcısı.](https://github.com/Azure/acr-docker-credential-helper)

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Docker üzerinde Spring Boot'u Kullanmaya Başlama web uygulamasını oluşturma

Aşağıdaki adımlar, bir Spring Boot web uygulaması oluşturma ve yerel olarak test etme sürecinde size yol gösterecektir.

1. Komut isteminden, Docker Başlarken Örnek [Proje'de Bahar Önyüklemesini](https://github.com/spring-guides/gs-spring-boot-docker) klonlamak için aşağıdaki komutu kullanın.

   ```bash
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Tamamlanmış projenin dizinine geçin.

   ```bash
   cd gs-spring-boot-docker/complete
   ```

1. Örnek uygulamayı derlemek ve çalıştırmak için Maven'ı kullanın.

   ```bash
   mvn package spring-boot:run
   ```

1. `http://localhost:8080` adresine giderek veya aşağıdaki `curl` komutunu kullanarak web uygulamasını test edin:

   ```bash
   curl http://localhost:8080
   ```

Görüntülenen aşağıdaki iletiyi görmelisiniz: **Hello Docker World**

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Azure CLI’yi kullanarak Azure Container Registry oluşturma

Ardından, aşağıdaki adımları kullanarak bir Azure kaynak grubu ve ACR oluşturursunuz:

1. Aşağıdaki komutu kullanarak Azure hesabınızda oturum açın:

   ```azurecli
   az login
   ```

1. Kullanılacak Azure aboneliğini belirtin:

   ```azurecli
   az account set -s <subscription ID>
   ```

1. Bu öğreticide kullanılacak Azure kaynakları için bir kaynak grubu oluşturun. Aşağıdaki komutta, yer tutucuları kendi kaynak adınız ve 'de. gibi `eastus`bir konumla değiştirdiğinizden emin olun.

   ```azurecli
   az group create \
       --name=<your resource group name> \
       --location=<location>
   ```

1. Kaynak grubunda aşağıdaki komutu kullanarak özel bir Azure kapsayıcı kayıt defteri oluşturun. Yer tutucuları gerçek değerlerle değiştirdiğinden emin olun. Bu öğreticinin ilerleyen adımlarında örnek uygulama Docker görüntüsü olarak bu kayıt defterine gönderilecektir.

   ```azurecli
   az acr create \
       --resource-group <your resource group name> \
       --location <location> \
       --name <your registry name> \
       --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry-via-jib"></a>Uygulamanızı Jib aracılığıyla kapsayıcı kayıt defterine gönderme

Son olarak, proje yapılandırmanızı günceller ve resminizi oluşturmak ve dağıtmak için komut istemini kullanırsınız.

1. Aşağıdaki komutu kullanarak Azure CLI'den Azure Konteyner Kayıt Defteri'nizde oturum açın. Yer tutucuyu kendi kayıt defteri adınızı değiştirdiğinizden emin olun.

   ```azurecli
   az configure --defaults acr=<your registry name>
   az acr login
   ```

   Komut, `az configure` `az acr` komutlarla kullanılacak varsayılan kayıt defteri adını ayarlar.

1. Spring Boot uygulamanızın tamamlanmış proje dizinine (örneğin, "*C:\SpringBoot\gs-spring-boot-docker\complete*" veya "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") gidin ve *pom.xml* dosyasını metin düzenleyicide açın.

1. `<properties>` Aşağıdaki XML ile *pom.xml* dosyasındaki koleksiyonu güncelleştirin. Yer tutucuyu kayıt defteri niz ile `<jib-maven-plugin.version>` değiştirin ve değeri [jib-maven eklentisinin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)en son sürümüyle güncelleştirin.

   ```xml
   <properties>
      <docker.image.prefix><your registry name>.azurecr.io</docker.image.prefix>
      <jib-maven-plugin.version>1.8.0</jib-maven-plugin.version>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Aşağıdaki `<plugins>` örnekte gösterildiği `<plugin>` gibi, öğenin içerdiği ve girişini `jib-maven-plugin`içerebilecek şekilde *pom.xml* dosyasındaki koleksiyonu güncelleştirin. Azure için resmi olarak desteklenen JDK içeren Microsoft Kapsayıcı `mcr.microsoft.com/java/jdk:8-zulu-alpine`Kayıt Defteri'nden (MCR): temel bir resim kullandığımızı unutmayın. Resmi olarak desteklenen JDKs ile diğer MCR temel görüntüler için, [Java SE JDK](https://hub.docker.com/_/microsoft-java-jdk), [Java SE JRE](https://hub.docker.com/_/microsoft-java-jre), [Java SE Headless JRE](https://hub.docker.com/_/microsoft-java-jre-headless), ve [Java SE JDK ve Maven](https://hub.docker.com/_/microsoft-java-maven)bakın .

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>${jib-maven-plugin.version}</version>
     <configuration>
        <from>
            <image>mcr.microsoft.com/java/jdk:8-zulu-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
        </to>
     </configuration>
   </plugin>
   ```

1. Spring Boot uygulamanız için tamamlanmış proje dizinine gidin ve aşağıdaki komutu çalıştırarak görüntüyü oluşturun ve kayıt defterine gönderin:

   ```bash
   mvn compile jib:build
   ```

> [!NOTE]
>
> Güvenlik nedenleriyle, oluşturulan `az acr login` kimlik bilgisi yalnızca 1 saat için geçerlidir. *401 Yetkisiz* bir hata alırsanız, yeniden `az acr login -n <your registry name>` kimlik doğrulaması için komutu yeniden çalıştırabilirsiniz.

## <a name="verify-your-container-image"></a>Konteyner resminizi doğrulama

Tebrikler! Artık Azure destekli JDK'nın ACR'nize itilen kapsayıcı Java Uygulaması'na sahipsiniz. Artık görüntüyü Azure Uygulama Hizmeti'ne dağıtarak veya komutla yerele çekerek (yer tutucuyu değiştirme) test edebilirsiniz:

```bash
docker pull <your registry name>.azurecr.io/gs-spring-boot-docker:v1
```

## <a name="next-steps"></a>Sonraki adımlar

Microsoft destekli resmi Java temel görüntülerinin diğer sürümleri için bkz:

* [Java SE JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [Java SE JRE](https://hub.docker.com/_/microsoft-java-jre)
* [Java SE Başsız JRE](https://hub.docker.com/_/microsoft-java-jre-headless)
* [Java SE JDK ve Maven](https://hub.docker.com/_/microsoft-java-maven)

Spring ve Azure hakkında daha fazlasını öğrenmek için Azure’da Spring belge merkezinde çalışmaya devam edin.

> [!div class="nextstepaction"]
> [Azure’da Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>Ek Kaynaklar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Java Geliştiricileri için Azure](/azure/java)
* [Azure DevOps ve Java ile çalışma](/azure/devops/java)
* [Docker üzerinde Spring Boot'u Kullanmaya Başlama](https://spring.io/guides/gs/spring-boot-docker)
* [Spring Initializr](https://start.spring.io)
* [Azure App Service’e Spring Boot Uygulaması Dağıtma](/azure/java/spring-framework/deploy-spring-boot-java-app-from-container-registry-using-maven-plugin)
* [Linux üzerinde Azure Web App’e yönelik özel Docker görüntüsü kullanma](/azure/app-service-web/app-service-linux-using-custom-docker-image)
