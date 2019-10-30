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
ms.openlocfilehash: 5777d728f61b3ffe7d0fdaaac1391f600dca12fd
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73097832"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-by-using-the-maven-plug-in"></a>Hızlı başlangıç: Maven eklentisini kullanarak bir Azure Spring Cloud uygulaması başlatma

Azure Spring Cloud Maven eklentisiyle Azure Spring Cloud Service uygulamalarınızı kolayca oluşturabilir ve güncelleştirebilirsiniz. Bir yapılandırmayı önceden tanımlayarak, mevcut Azure yay bulut hizmetinize uygulama dağıtabilirsiniz. Bu makalede, bu özelliği göstermek için POF ölçümleri adlı örnek bir uygulama kullanırsınız.

>[!Note]
> Bu hızlı başlangıca başlamadan önce Azure aboneliğinizin Azure Spring Cloud 'a erişimi olduğundan emin olun. Bir önizleme hizmeti olarak, aboneliğinizi izin verilenler listemize ekleyebilmemiz için sizi bize ulaşmaya davet ediyoruz. Azure Spring Cloud 'ın yeteneklerini araştırmak isterseniz, [Azure yay bulutu (özel Önizleme)-Ilgi formunu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u)doldurun ve iletin.

>[!TIP]
> Azure Cloud Shell, bu makaledeki komutları çalıştırmak için kullanabileceğiniz, ücretsiz bir etkileşimli kabuktur. Git 'in en son sürümleri, Java Geliştirme Seti (JDK), Maven ve Azure CLı dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız [Azure Cloud Shell](https://shell.azure.com)başlatın. Daha fazla bilgi için bkz. [Azure Cloud Shell Genel Bakış](../cloud-shell/overview.md).

Bu hızlı başlangıcı tamamlamak için:

1. [Git’i yükleyin](https://git-scm.com/).
2. [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)' i yükler.
3. [Maven 3,0 veya üstünü yükler](https://maven.apache.org/download.cgi).
4. [Azure CLI sürüm 2.0.67 veya üstünü yükler](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
5. [Ücretsiz bir Azure aboneliğine kaydolun](https://azure.microsoft.com/free/).

## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler:

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Azure portal bir hizmet örneği sağlayın

1. Bir Web tarayıcısında [Azure Portal](https://portal.azure.com)açın ve hesabınızda oturum açın.

1. **Azure yay bulutu**'nı arayıp seçin. 
1. Genel Bakış sayfasında **Oluştur**' u seçin ve ardından aşağıdakileri yapın:  

    a. **Hizmet adı** kutusunda, hizmet örneğinizin adını belirtin. Ad 4 ile 32 karakter uzunluğunda olmalı ve yalnızca küçük harf, sayı ve kısa çizgi içermelidir. Hizmet adının ilk karakteri bir harf olmalıdır ve son karakter bir harf ya da sayı olmalıdır.  

    b. **Abonelik** açılan listesinde, bu kaynak için faturalandırılması istediğiniz aboneliği seçin. Bu aboneliğin Azure yay bulutu için izin verilenler listemize eklendiğinden emin olun.  

    c. **Kaynak grubu** kutusunda yeni bir kaynak grubu oluşturun. Yeni kaynaklar için kaynak grupları oluşturmak en iyi uygulamadır.  

    d. **Konum** açılır listesinde, hizmet örneğinizin konumunu seçin. Şu anda desteklenen konumlar Doğu ABD, Batı ABD 2, Batı Avrupa ve Güneydoğu Asya 'yı içerir.
    
Hizmetin dağıtılması yaklaşık 5 dakika sürer. Hizmet dağıtıldıktan sonra, hizmet örneği için **genel bakış** sayfası görüntülenir.

## <a name="set-up-your-configuration-server"></a>Yapılandırma sunucunuzu ayarlama

1. Hizmete **genel bakış** sayfasında, **yapılandırma sunucusu**' nu seçin.
1. **Varsayılan depo** bölümünde, **URI** 'yi **https://github.com/Azure-Samples/piggymetrics** olarak ayarlayın, **etiketi** **config**olarak ayarlayın ve ardından değişikliklerinizi kaydetmek için **Uygula** ' yı seçin.

## <a name="clone-and-build-the-sample-application-repository"></a>Örnek uygulama deposunu kopyalama ve derleme

1. Aşağıdaki komutu çalıştırarak Git deposunu kopyalayın:

    ```azurecli
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Dizini değiştirin ve aşağıdaki komutu çalıştırarak projeyi derleyin:

    ```azurecli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-and-deploy-the-azure-spring-cloud-configuration"></a>Azure yay bulut yapılandırması oluşturma ve dağıtma

1. Maven 'in Azure Spring Cloud ile çalışmasını sağlamak için, aşağıdaki kodu *POTM. xml* veya *Settings. xml* dosyanıza ekleyin.

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

1. Aşağıdaki komutu çalıştırarak bir yapılandırma oluşturun:

    ```azurecli
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:config
    ```

    a. `gateway`,`auth-service`ve `account-service`modüllerini seçin.

    b. Aboneliğinizi ve Azure yay bulut hizmeti kümenizi seçin.

    c. Belirtilen projeler listesinde, ortak erişim sağlamak için `gateway` karşılık gelen sayıyı girin.
    
    d. Yapılandırmayı onaylayın.

1. Aşağıdaki komutu kullanarak uygulamaları dağıtın:

   ```azurecli
   mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:deploy
   ```

1. Önceki komutun çıktısında belirtilen URL 'YI kullanarak, Plımetrik ölçümlere erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Maven deposundan bir yay bulutu uygulaması dağıttınız. Azure Spring Cloud hakkında daha fazla bilgi edinmek için uygulamanızı dağıtıma hazırlama hakkında öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md)
