---
title: Servise hizmet kimlik doğrulaması - Data Lake Storage Gen1 - Java SDK
description: Java ile Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması nasıl elde edilenöğrenin
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: f355da7cd9c035b4ed0845bbd374a93bfb4a7350
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73904532"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-java"></a>Java kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması

> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
>
>  

Bu makalede, Azure Veri Gölü Depolama Gen1 ile hizmetten hizmete kimlik doğrulaması yapmak için Java SDK'yı nasıl kullanacağınız hakkında bilgi edinin. Java SDK kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizin "Web" Uygulaması oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Hizmetten Hizmete kimlik doğrulamaadımlarını](data-lake-store-service-to-service-authenticate-using-active-directory.md)tamamlamış olmalısınız.

* [Maven,](https://maven.apache.org/install.html)ne kadar. Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi kullanmadan inşa etmek mümkün olsa da, bu sistemler bağımlılıkları yönetmeyi çok daha kolay hale getirir.

* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) veya [Eclipse](https://www.eclipse.org/downloads/) veya benzeri gibi bir IDE.

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

1. Komut satırından [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) veya bir IDE kullanarak Maven projesi oluşturun. IntelliJ kullanarak Java projesi oluşturma yönergeleri için [buraya](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) bakın. Eclipse kullanarak proje oluşturma yönergeleri için [buraya](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) bakın.

2. Maven **pom.xml** dosyanıza aşağıdaki bağımlılıkları ekleyin. ** \</project>** etiketinden önce aşağıdaki snippet'i ekleyin:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.2.3</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    İlk bağımlılık, maven deposundan Veri Gölü`azure-data-lake-store-sdk`Depolama Gen1 SDK () kullanmaktır. İkinci bağımlılık, bu uygulama için hangi günlük altyapısının (`slf4j-nop`) kullanılacağını belirtmektir. Data Lake Storage Gen1 SDK, log4j, Java günlük, logback, vb. gibi bir dizi popüler günlük çerçevesi arasından seçim yapmanızı sağlayan [slf4j](https://www.slf4j.org/) günlük cephesikullanır veya günlük kullanmamanızı sağlar. Bu örnekte, günlük kaydını devre dışı bırakacak ve dolayısıyla **slf4j-nop** bağlamasını kullanacağız. Uygulamanızda diğer günlük seçeneklerini kullanmak için [buraya](https://www.slf4j.org/manual.html#projectDep) bakın.

3. Aşağıdaki içeri aktarma deyimlerini uygulamanıza ekleyin.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

4. Daha önce oluşturduğunuz Active Directory Web uygulamasının alt sınıflarından `AccessTokenProvider` birini kullanarak (aşağıdaki örnek kullanır) `ClientCredsTokenProvider`belirteci almak için Java uygulamanızda aşağıdaki snippet'i kullanın. Belirteç sağlayıcısı, bellekteki belirteci almak için kullanılan kimlik bilgilerini önbelleğe alır ve süre sonu yaklaştığında belirteci otomatik olarak yeniler. Müşteri kodunuz tarafından elde edilen `AccessTokenProvider` jetonların kendi alt sınıflarınızı oluşturmak mümkündür. Şimdilik, sadece SDK sağlanan bir kullanalım.

    **BURAYI DOLDURUN** alanını, Azure Active Directory Web uygulaması için gerçek değerlerle değiştirin.

        private static String clientId = "FILL-IN-HERE";
        private static String authTokenEndpoint = "FILL-IN-HERE";
        private static String clientKey = "FILL-IN-HERE";
    
        AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);   

Data Lake Storage Gen1 SDK, Data Lake Storage Gen1 hesabıyla konuşmak için gereken güvenlik belirteçlerini yönetmenize izin veren kullanışlı yöntemler sağlar. Bununla birlikte, SDK yalnızca bu yöntemlerin kullanılmasını zorunlu kılmaz. [Azure Active Directory SDK’sını](https://github.com/AzureAD/azure-activedirectory-library-for-java) veya kendi özel kodunuzu kullanma gibi diğer belirteç edinme yöntemlerinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Java SDK kullanarak Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için son kullanıcı kimlik doğrulamasını nasıl kullanacağınızı öğrendiniz. Şimdi Data Lake Storage Gen1 ile çalışmak için Java SDK nasıl kullanılacağı hakkında konuşmak aşağıdaki makalelere bakabilirsiniz.

* [Java SDK kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-get-started-java-sdk.md)
