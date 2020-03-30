---
title: Quickstart - Kaynak kodundan Bahar Bulutu uygulamanızı başlatın
description: Bu hızlı başlangıçta, Azure Bahar Bulutu uygulamanızı doğrudan kaynak kodunuzdan nasıl başlatılacağınızdan öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/30/2019
ms.author: brendm
ms.openlocfilehash: 3ab4b1729ea380671b72a9bb01740930a186d5c3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79470803"
---
# <a name="quickstart-launch-your-spring-cloud-application-from-source-code"></a>Quickstart: Kaynak kodundan Bahar Bulutu uygulamanızı başlatın

Azure Bulutu, Azure'da Bahar Bulutu tabanlı mikro hizmet uygulamalarını kolayca çalıştırmanızı sağlar.

Azure Yay Bulutu, uygulamanızı doğrudan java kaynak kodunuzdan veya önceden oluşturulmuş bir JAR'dan başlatmanızı sağlar. Bu makale, gerekli adımları size yol.

Bu hızlı başlangıcın ardından, nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet örneğini sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikrohizmet uygulaması oluşturma
> * Her microservice dağıtma
> * Başvurunuz için genel bitiş noktası atama

## <a name="prerequisites"></a>Ön koşullar

>[!Note]
> Azure İlkbahar Bulutu şu anda genel önizleme olarak sunulmaktadır. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.  Önizlemeler sırasında destek hakkında daha fazla bilgi için lütfen [SSS'mizi](https://azure.microsoft.com/support/faq/) gözden geçirin veya daha fazla bilgi edinmek için bir [Destek isteği](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) dosyalayın.

Başlamadan önce Azure aboneliğinizin gerekli bağımlılıklara sahip olduğundan emin olun:

1. [Git'i yükleyin](https://git-scm.com/)
2. [JDK 8'i yükleyin](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
3. [Maven 3.0 veya üzeri yükleme](https://maven.apache.org/download.cgi)
4. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Azure aboneliği için kaydolun](https://azure.microsoft.com/free/)

> [!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLI'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, Azure [Bulut Shell'inizi](https://shell.azure.com) shell.azure.com başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Bulut BulutU Hakkında Daha Fazla Bilgi Edinebilirsiniz

## <a name="install-the-azure-cli-extension"></a>Azure CLI uzantısını yükleme

Azure CLI için Azure İlkbahar Bulutu uzantısını aşağıdaki komutla yükleyin

```azurecli
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-using-the-azure-cli"></a>Azure CLI'yi kullanarak bir hizmet örneği sağlama

Azure CLI'ye giriş yapın ve etkin aboneliğinizi seçin. Azure İlkbahar Bulutu için beyaz listeye alınan etkin aboneliği seçtiğinizden emin olun

```azurecli
az login
az account list -o table
az account set --subscription
```

Azure İlkbahar Bulut hizmetinizi içerecek bir kaynak grubu oluşturun. [Azure Kaynak Grupları](../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinebilirsiniz.

```azurecli
az group create --location eastus --name <resource group name>
```

Azure Bahar Bulutu örneğini sağlamak için aşağıdaki komutları çalıştırın. Azure Bahar Bulutu hizmetiniz için bir ad hazırlayın. Ad 4 ile 32 karakter uzunluğunda olmalı ve yalnızca küçük harfler, sayılar ve tireler içerebilir. Hizmet adının ilk karakteri bir harf, son karakter ise bir harf veya sayı olmalıdır.

```azurecli
az spring-cloud create -n <resource name> -g <resource group name>
```

Hizmet örneğinin dağıtılması yaklaşık beş dakika sürer.

Varsayılan kaynak grup adınızı ve küme adınızı aşağıdaki komutları kullanarak ayarlayın:

```azurecli
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=provision)

## <a name="create-the-spring-cloud-application"></a>Bahar Bulutu uygulamasını oluşturma

Aşağıdaki komut, aboneliğinizde bir Bahar Bulutu uygulaması oluşturur.  Bu, uygulamamızı yükleyebileceğimiz boş bir Spring Cloud hizmeti oluşturur.

```azurecli
az spring-cloud app create -n <app-name>
```

## <a name="deploy-your-spring-cloud-application"></a>Bahar Bulutu uygulamanızı dağıtın

Uygulamanızı önceden oluşturulmuş bir JAR'dan veya Gradle veya Maven deposundan dağıtabilirsiniz.  Aşağıdaki her durum için yönergeleri bulun.

### <a name="deploy-a-built-jar"></a>Yerleşik bir JAR dağıtma

Yerel makineniz üzerine inşa edilmiş bir KAVANOZ'dan dağıtmak için, yapınızın [bir yağ KAVANOZu](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-build.html#howto-create-an-executable-jar-with-maven)ürettiğinden emin olun.

Yağ-JAR'ı etkin bir dağıtıma dağıtmak için

```azurecli
az spring-cloud app deploy -n <app-name> --jar-path <path-to-fat-JAR>
```

Yağ-JAR'ı belirli bir dağıtıma dağıtmak için

```azurecli
az spring-cloud app deployment create --app <app-name> -n <deployment-name> --jar-path <path-to-built-jar>
```

### <a name="deploy-from-source-code"></a>Kaynak kodundan dağıtma

Azure Yay Bulutu, projenizi oluşturmak için [kpack](https://github.com/pivotal/kpack) kullanır.  Kaynak kodunuzu yüklemek, projenizi kpack kullanarak oluşturmak ve hedef uygulamaya dağıtmak için Azure CLI'yi kullanabilirsiniz.

> [!WARNING]
> `main-class` Proje, `MANIFEST.MF` giriş `target` girişi olan tek bir JAR dosyası (Maven `build/libs` dağıtımları veya (Gradle dağıtımları için) oluşturmalıdır.  Girişleri olan `main-class` birden çok JAR dosyası dağıtımın başarısız olmasını sağlar.

Tek modülMa / Gradle projeleri için:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name>
```

Birden fazla modüle sahip Maven / Gradle projeleri için her modül için tekrarlayın:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name> --target-module <relative-path-to-module>
```

### <a name="show-deployment-logs"></a>Dağıtım günlüklerini göster

Kpack yapı günlüklerini aşağıdaki komutu kullanarak gözden geçirin:

```azurecli
az spring-cloud app show-deploy-log -n <app-name> [-d <deployment-name>]
```

> [!NOTE]
> Kpack günlükleri, yalnızca bu dağıtım kpack kullanılarak kaynaktan oluşturulmuşsa, en son dağıtımı gösterir.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=deploy)

## <a name="assign-a-public-endpoint-to-gateway"></a>Ağ geçidine ortak bir bitiş noktası atama

1. Uygulama **Panosu** sayfasını açın.
2. Uygulama `gateway` **Ayrıntıları** sayfasını göstermek için uygulamayı seçin.
3. Ağ geçidine ortak bir bitiş noktası atamak için **Etki Adı Ata'yı** seçin. Bu birkaç dakika olabilir. 
4. Çalışan uygulamanızı görüntülemek için tarayıcınıza atanan genel IP'yi girin.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/javae2e?tutorial=asc-source-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

> [!div class="checklist"]
> * Hizmet örneğini sağlama
> * Bir örnek için yapılandırma sunucusu ayarlama
> * Yerel olarak bir mikrohizmet uygulaması oluşturma
> * Her microservice dağıtma
> * Uygulamalar için ortam değişkenlerini düzenlemesi
> * Uygulama ağ geçidiniz için genel IP atama

> [!div class="nextstepaction"]
> [Azure Bahar Bulutu uygulamanızı dağıtıma hazırlayın](spring-cloud-tutorial-prepare-app-deployment.md)

GitHub: Azure Bahar [Bulut Örnekleri'nde](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql)daha fazla örnek mevcuttur.
