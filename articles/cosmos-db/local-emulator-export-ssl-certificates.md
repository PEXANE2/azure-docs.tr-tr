---
title: Azure Cosmos DB Öykünücüsü sertifikalarını dışarı aktarma
description: Windows sertifika deposu kullanmayan diller ve çalışma zamanları geliştirilirken, TLS/SSL sertifikalarını dışarı ve yönetmeye ihtiyaç duyarsınız. Bu gönderi adım adım yönergeler verir.
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
author: deborahc
ms.author: dech
ms.openlocfilehash: c72dbf24df850d8b0f7e5f26a873b78f5664c9e0
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82200948"
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Azure Cosmos DB Öykünücü sertifikalarını Java, Python ve Node.js ile kullanmak için dışarı aktarma

[**Öykünücüyü İndirin**](https://aka.ms/cosmosdb-emulator)

Azure Cosmos DB öykünücüsü, TLS bağlantılarının kullanımı da dahil olmak üzere geliştirme amacıyla Azure Cosmos DB hizmetine taklit eden yerel bir ortam sağlar. Bu gönderi, kendi [sertifika deposunu](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) kullanan Java gibi Windows sertifika depolama alanı ile tümleştirilebilen diller ve çalışma zamanları için TLS/SSL sertifikalarının nasıl dışa aktarılacağını ve [Tlssocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback)kullanan Node. js ' ye sahip olan anahtar ve Node. [js ' ye](https://docs.python.org/2/library/ssl.html) sahip olan Python ' i kullanır. [Geliştirme ve test için Azure Cosmos DB Emulator kullanma](./local-emulator.md) konusunda öykünücü hakkında daha fazla bilgi edinebilirsiniz.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Sertifikalar döndürülüyor
> * TLS/SSL sertifikası dışarı aktarılıyor
> * Java, Python ve Node.js’de sertifika kullanmayı öğrenme

## <a name="certification-rotation"></a>Sertifika döndürme

Azure Cosmos DB Yerel Öykünücüsündeki sertifikalar öykünücü ilk kez çalıştırıldığında oluşturulur. İki sertifika vardır. Yerel öykünücüye bağlanmak için bir tane ve öykünücü içindeki gizli dizileri yönetmek için bir tane. Dışarı aktarmak istediğiniz sertifika, "DocumentDBEmulatorCertificate" kısa adına sahip bağlantı sertifikasıdır.

İki sertifika da aşağıda Windows Tepsisinde çalışan Azure Cosmos DB Öykünücüsü’nde gösterildiği gibi **Verileri Sıfırla**’ya tıklanarak yeniden oluşturulabilir. Sertifikaları yeniden oluşturursanız ve bunları Java sertifika deposuna yüklediyseniz ya da başka bir yerde kullandıysanız sertifikaları güncelleştirmeniz gerekir, aksi takdirde uygulamanız artık yerel öykünücüye bağlanamaz.

![Azure Cosmos DB yerel öykünücüsü sıfırlama verileri](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-tlsssl-certificate"></a>Azure Cosmos DB TLS/SSL sertifikasını dışarı aktarma

1. Certlm.msc’yi çalıştırarak Windows Sertifika yöneticisini çalıştırın ve Kişisel->Sertifikalar klasörüne gidip **DocumentDbEmulatorCertificate** kısa adına sahip sertifikayı açın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. **Ayrıntılar**’a ve ardından **Tamam**’a tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. **Dosyaya Kopyala...** seçeneğini belirleyin.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. **Hayır, özel anahtarı dışarı aktarma**’ya tıklayın ve **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. **Base-64 ile kodlanmış X.509 (.CER)** seçeneğine ve ardından **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Sertifikaya bir ad verin. Bu durumda **documentdbemulatorcert** dosyasını seçin ve **İleri**’ye tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. **Son**'a tıklayın.

    ![Azure Cosmos DB yerel öykünücüsü dışarı aktarma adımı 8](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a>Java içinde sertifika kullanma

Java istemcisini kullanan Java uygulamalarını veya MongoDB uygulamalarını çalıştırırken, bu sertifikayı, `-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>"` bayrakları geçirmeden Java varsayılan sertifika deposuna yüklemek daha kolaydır. Örneğin, dahil edilen Java demo uygulaması (`https://localhost:8081/_explorer/index.html`) varsayılan sertifika deposuna bağlıdır.

X.509 sertifikasını varsayılan Java sertifika deposuna içeri aktarmak için, [Java CA Sertifika Deposuna Sertifika Ekleme](https://docs.microsoft.com/azure/java-add-certificate-ca-store) bölümündeki yönergeleri izleyin. Anahtar aracını çalıştırırken %JAVA_HOME% dizininde çalışacağınıza dikkat edin.

"CosmosDBEmulatorCertificate" TLS/SSL sertifikası yüklendikten sonra uygulamanız, yerel Azure Cosmos DB öykünücüsünün bağlanabilmesi ve kullanabilmesi gerekir. Sorun yaşamaya devam ederseniz [SSL/TLS Bağlantılarında hata ayıklama](https://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) makalesindeki yönergeleri izleyin. Sertifika %JAVA_HOME%/jre/lib/security/cacerts deposuna yüklenmemiş olabilir. Örneğin birden çok Java sürümü yüklüyse, uygulamanız güncelleştirdiğinizden farklı bir cacerts deposu kullanıyor olabilir.

## <a name="how-to-use-the-certificate-in-python"></a>Python içinde sertifika kullanma

Varsayılan olarak, SQL API 'SI için [Python SDK (sürüm 2.0.0 veya üzeri)](sql-api-sdk-python.md) , yerel öykünücüsüne bağlanırken TLS/SSL sertifikasını denemez ve kullanamaz. Ancak TLS doğrulamasını kullanmak istiyorsanız, [Python yuva sarmalayıcıları](https://docs.python.org/2/library/ssl.html) belgelerindeki örnekleri izleyebilirsiniz.

## <a name="how-to-use-the-certificate-in-nodejs"></a>Node.js içinde sertifika kullanma

Varsayılan olarak, SQL API 'SI için [Node. js SDK (sürüm 1.10.1 veya üzeri)](sql-api-sdk-node.md) , yerel öykünücüye bağlanılırken TLS/SSL sertifikasını denemez ve kullanamaz. Ancak TLS doğrulamasını kullanmak isterseniz [Node. js belgelerindeki](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback)örnekleri izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Döndürülen sertifikalar
> * TLS/SSL sertifikası verildi
> * Java, Python ve Node.js’de sertifika kullanmayı öğrendiniz

Şimdi Azure Cosmos DB hakkında daha fazla bilgi için kavramlar bölümüne geçebilirsiniz. 

> [!div class="nextstepaction"]
>[Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)
