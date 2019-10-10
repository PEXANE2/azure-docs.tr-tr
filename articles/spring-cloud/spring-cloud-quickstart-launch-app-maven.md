---
title: "Hızlı başlangıç: Maven-Azure yay bulutu 'nı kullanarak bir uygulama başlatma"
description: Maven kullanarak örnek bir uygulama başlatma
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: v-vasuke
ms.openlocfilehash: 01140e94e8d0cc47570824970801bdd0043324d7
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166510"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-using-the-maven-plug-in"></a>Hızlı başlangıç: Maven eklentisini kullanarak bir Azure Spring Cloud uygulaması başlatın

Azure yay bulutu 'nın Maven eklentisi, Azure Spring bulut hizmeti uygulamalarınızı kolayca oluşturup güncelleştirmenize olanak tanır. Bir yapılandırmayı önceden tanımlayarak, mevcut Azure yay bulut hizmetinize uygulama dağıtabilirsiniz. Bu makalede, bu özelliği göstermek için POF ölçümleri adlı örnek bir uygulama kullanıyoruz.

>[!Note]
> Bu hızlı başlangıca başlamadan önce Azure aboneliğinizin Azure Spring Cloud 'a erişimi olduğundan emin olun.  Bir önizleme hizmeti olarak, aboneliğinizin izin verilenler listenize ekleyebilmemiz için bize bize ulaşmaları istenir.  Azure Spring Cloud 'ın yeteneklerini araştırmak istiyorsanız lütfen [Bu formu doldurun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u
).

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

Bu hızlı başlangıcı tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [JDK 8 ' i yükler](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Maven 3,0 veya üstünü yükler](https://maven.apache.org/download.cgi)
4. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portal bir hizmet örneği sağlayın

1. Bir Web tarayıcısında [Azure Portal](https://portal.azure.com)açın ve hesabınızda oturum açın.

1. **Azure yay bulutu** 'nı arayın ve genel bakış sayfasına gitmek için seçin. Başlamak için **Oluştur** düğmesini seçin.

1. Aşağıdaki yönergeleri göz önünde bulundurarak formu doldurun:
    - Hizmet adı: hizmet örneğinizin adını belirtin.  Ad 4 ila 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir.  Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.
    - Abonelik: Bu kaynak için faturalandırılması istediğiniz aboneliği seçin.  Bu aboneliğin Azure yay bulutu için izin verilenler listenize eklendiğinden emin olun.
    - Kaynak grubu: yeni kaynaklar için yeni kaynak grupları oluşturmak en iyi uygulamadır.
    - Konum: hizmet örneğinizin konumunu seçin. Şu anda desteklenen konumlar Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya 'yı içerir.
    
Hizmetin dağıtılması yaklaşık 5 dakika sürer.  Hizmet örneği için **genel bakış** sayfası, dağıtıldıktan sonra görüntülenir.

## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **genel bakış** sayfasına gidin ve **yapılandırma sunucusu**' nu seçin.
1. **Varsayılan depo** bölümünde, **urı** 'yi "https://github.com/Azure-Samples/piggymetrics" olarak ayarlayın, **etiketi** "config" olarak ayarlayın ve değişikliklerinizi kaydetmek için **Uygula** ' yı seçin.

## <a name="clone-and-build-the-sample-application-repository"></a>Örnek uygulama deposunu kopyalama ve derleme

1. Aşağıdaki komutu çalıştırarak Git deposunu kopyalayın.

    ```azurecli
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Dizini değiştirin ve aşağıdaki komutu çalıştırarak projeyi derleyin.

    ```azurecli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-and-deploy-the-azure-spring-cloud-configuration"></a>Azure yay bulut yapılandırması oluşturma ve dağıtma

1. Maven 'nin Azure Spring Cloud ile çalışmasını sağlamak için, `pom.xml` veya `settings.xml` ' e aşağıdakileri ekleyin.

    ```xml
    <pluginRepositories>
      <pluginRepository>
        <id>maven.snapshots</id>
        <name>Maven Central Snapshot Repository</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <releases>
          <enabled>false</enabled>
        </releases>
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </pluginRepository>
    </pluginRepositories>
    ```

1. Aşağıdaki komutu çalıştırarak bir yapılandırma oluşturun.

    ```azurecli
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:config
    ```

    1. @No__t-0, `auth-service` ve `account-service` modüllerini seçin.

    1. Aboneliğinizi ve Azure yay bulut hizmeti kümenizi seçin.

    1. Belirtilen proje listesinden, genel erişim sağlamak için `gateway` ' a karşılık gelen sayıyı girin.
    
    1. Yapılandırmayı onaylayın.

1. Aşağıdaki komutu kullanarak uygulamaları dağıtın:

   ```azurecli
   mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:deploy
   ```

1. Önceki komutun çıktısında belirtilen URL 'YI kullanarak Plımetrik ölçümlerine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bu hızlı başlangıçta, Maven deposundan bir yay bulutu uygulaması dağıttınız.  Azure Spring Cloud hakkında daha fazla bilgi edinmek için uygulamanızı dağıtıma hazırlama öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md)
