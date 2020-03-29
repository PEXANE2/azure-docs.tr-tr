---
title: 'Son kullanıcı kimlik doğrulaması: Azure Active Directory kullanarak Azure Veri Gölü Depolama Gen1 ile Java | Microsoft Dokümanlar'
description: Java ile Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması nasıl elde edilemeyi öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8b558fca964f33d47d331e007329d1bae2626877
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60878110"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-java"></a>Java kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’sını kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>   

Bu makalede, Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması yapmak için Java SDK'yı nasıl kullanacağınız hakkında bilgi edinin. Java SDK kullanarak Data Lake Storage Gen1 ile servise hizmet kimlik doğrulaması için, [Java kullanarak Data Lake Storage Gen1 ile hizmete hizmet kimlik doğrulaması](data-lake-store-service-to-service-authenticate-java.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizini "Yerel" Uygulama Oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Son kullanıcı kimlik doğrulamaadımlarını](data-lake-store-end-user-authenticate-using-active-directory.md)tamamlamış olmalısınız.

* [Maven,](https://maven.apache.org/install.html)ne kadar. Bu eğiticide, yapı ve proje bağımlılıkları için Maven kullanılır. Maven veya Gradle gibi bir yapı sistemi olmadan derleme yapmak mümkün olsa da bu sistemler bağımlılıkların yönetilmesini çok daha kolay hale getirir.

* (İsteğe bağlı) [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) vb. bir IDE.

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
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
        import com.microsoft.azure.datalake.store.oauth2.DeviceCodeTokenProvider;

4. Daha önce oluşturduğunuz Active Directory yerel uygulamasıiçin belirteç elde etmek için `DeviceCodeTokenProvider`Java uygulamanızda aşağıdaki snippet'i kullanın. **BURADA DOLDURUN'u** Azure Active Directory yerel uygulamasının gerçek değerleriyle değiştirin.

        private static String nativeAppId = "FILL-IN-HERE";
            
        AccessTokenProvider provider = new DeviceCodeTokenProvider(nativeAppId);   

Data Lake Storage Gen1 SDK, Data Lake Storage Gen1 hesabıyla konuşmak için gereken güvenlik belirteçlerini yönetmenize izin veren kullanışlı yöntemler sağlar. Bununla birlikte, SDK yalnızca bu yöntemlerin kullanılmasını zorunlu kılmaz. [Azure Active Directory SDK’sını](https://github.com/AzureAD/azure-activedirectory-library-for-java) veya kendi özel kodunuzu kullanma gibi diğer belirteç edinme yöntemlerinden yararlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Java SDK kullanarak Azure Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için son kullanıcı kimlik doğrulamasını nasıl kullanacağınızı öğrendiniz. Artık Azure Veri Gölü Depolama Gen1 ile çalışmak için Java SDK'nın nasıl kullanılacağı hakkında aşağıdaki makalelere bakabilirsiniz.

* [Java SDK kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-get-started-java-sdk.md)


