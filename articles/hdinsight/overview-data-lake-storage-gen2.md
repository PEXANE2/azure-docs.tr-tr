---
title: HDInsight'ta Azure Veri Gölü Depolama Gen2'ye genel bakış
description: HDInsight'ta Veri Gölü Depolama Gen2'ye Genel Bakış.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: 677d8348cc230e4f64915ac5fc1e86b9b50ab5c3
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873348"
---
# <a name="azure-data-lake-storage-gen2-overview-in-hdinsight"></a>HDInsight'ta Azure Veri Gölü Depolama Gen2'ye genel bakış

Azure Veri Gölü Depolama Gen2, Azure Veri Gölü Depolama Gen1'in temel özelliklerini alır ve Azure Blob depolama alanına entegre eder. Bu özellikler arasında Hadoop, Azure Etkin Dizin (Azure AD) ve POSIX tabanlı erişim denetim listeleri (AçS) ile uyumlu bir dosya sistemi bulunur. Bu kombinasyon, Azure Veri Gölü Depolama Gen1'in performansından yararlanmanızı sağlar. Blob depolamanın katmanlama ve veri yaşam döngüsü yönetimini kullanırken.

Azure Veri Gölü Depolama Gen2 hakkında daha fazla bilgi için [bkz.](../storage/blobs/data-lake-storage-introduction.md)

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'nin temel işlevselliği

* **Hadoop ile uyumlu erişim:** Azure Veri Gölü Depolama Gen2'de, hadoop Dağıtılmış Dosya Sistemi (HDFS) ile verileri olduğu gibi yönetebilir ve erişebilirsiniz. Azure Blob Dosya Sistemi (ABFS) sürücüsü, Azure HDInsight ve Azure Databricks dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir. Veri Gölü Depolama Gen2'de depolanan verilere erişmek için ABFS'yi kullanın.

* **POSIX izinlerinin bir üst kümesi:** Data Lake Gen2'nin güvenlik modeli, ACL ve POSIX izinlerinin yanı sıra Data Lake Storage Gen2'ye özgü bazı ekstra parçalılıkları destekler. Ayarlar yönetici araçları veya Apache Hive ve Apache Spark gibi çerçeveler aracılığıyla yapılandırılabilir.

* **Maliyet etkinliği:** Data Lake Storage Gen2 düşük maliyetli depolama kapasitesi ve işlemleri sunar. Azure Blob depolama yaşam döngüleri, veriler yaşam döngüsü boyunca ilerlerken faturalandırma oranlarını ayarlayarak maliyetleri düşürmeye yardımcı olur.

* **Blob depolama araçları, çerçeveleri ve uygulamalarıyla uyumluluk:** Data Lake Storage Gen2, Blob depolama için çok çeşitli araçlar, çerçeveler ve uygulamalarla çalışmaya devam etmektedir.

* **Optimize edilmiş sürücü:** ABFS sürücüsü özellikle büyük veri analitiği için optimize edilebiyi tasarlar. İlgili REST API'leri dağıtılmış dosya sistemi (DFS) bitiş noktası, dfs.core.windows.net ile su yüzüne çıkar.

## <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Azure Veri Gölü Depolama Gen 2 için yenilikler

### <a name="managed-identities-for-secure-file-access"></a>Güvenli dosya erişimi için yönetilen kimlikler

Azure HDInsight, Azure Veri Gölü Depolama Gen2'deki dosyalara küme erişimini güvence altına almak için yönetilen kimlikleri kullanır. Yönetilen kimlikler, Azure hizmetlerine otomatik olarak yönetilen kimlik bilgileri kümesi sağlayan Azure Etkin Dizin özelliğidir. Bu kimlik bilgileri, Etkin Dizin kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanılabilir. Yönetilen kimlikleri kullanmak, kimlik bilgilerini kod veya yapılandırma dosyalarında depolamanızı gerektirmez.

Daha fazla bilgi için Azure [kaynakları için Yönetilen kimlikler'e](../active-directory/managed-identities-azure-resources/overview.md)bakın.

### <a name="azure-blob-file-system-driver"></a>Azure Blob Dosya Sistemi sürücüsü

Apache Hadoop uygulamaları yerel disk depolama dan veri okumak ve yazmak için yerli bekliyoruz. ABFS gibi hadoop dosya sistemi sürücüsü, Hadoop uygulamalarının bulut depolama yla çalışmasını sağlar. Düzenli Hadoop dosya sistemi işlemlerini taklit ederek çalışır. Sürücü, uygulamadan alınan komutları gerçek bulut depolama platformunun anladığı işlemlere dönüştürür.

Daha önce Hadoop dosya sistemi sürücüsü, tüm dosya sistemi işlemlerini istemci tarafındaki Azure Depolama REST API çağrılarına dönüştürmüştü. Ve sonra REST API çağırdı. Ancak bu istemci tarafı dönüştürme, birden çok REST API çağrısıyla, bir dosyanın yeniden adlandırılması gibi tek bir dosya sistemi çalışması için çağrı yapılmasıyla sonuçlanır. ABFS, Hadoop dosya sistemi mantığını istemci tarafından sunucu tarafına taşıdı. Azure Veri Gölü Depolama Gen2 API artık Blob API ile paralel olarak çalışır. Artık yaygın Hadoop dosya sistemi işlemleri tek bir REST API çağrısıyla yürütülebileceğinden, bu geçiş performansı artırır.

Daha fazla bilgi için [Bkz. Azure Blob Filesystem sürücüsü (ABFS): Hadoop için özel bir Azure Depolama sürücüsü.](../storage/blobs/data-lake-storage-abfs-driver.md)

### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>Azure Veri Gölü Depolama Gen 2 için URI şeması

Azure Veri Gölü Depolama Gen2, AZURE Depolama'daki dosyalara HDInsight'tan erişmek için yeni bir URI şeması kullanır:

`abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

URI şeması SSL şifreli erişim sağlar.

`<FILE_SYSTEM_NAME>`dosya sistemi Veri Gölü Depolama Gen2 yolunu tanımlar.

`<ACCOUNT_NAME>`Azure Depolama hesap adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

`<PATH>`dosya veya dizin HDFS yol adıdır.

Değerler için `<FILE_SYSTEM_NAME>` `<ACCOUNT_NAME>` ve belirtilmemişse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanın. Örneğin, HDInsight kümeleriyle birlikte gelen `hadoop-mapreduce-examples.jar` dosya, aşağıdaki yollardan biri kullanılarak yönlendirilebilir:

```
abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfs:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]
> Dosya adı `hadoop-examples.jar` HDInsight sürümleri 2.1 ve 1.6 kümelerindedir. HDInsight dışındaki dosyalarla çalışırken, çoğu yardımcı programABFS biçimini tanımadığını, bunun yerine `example/jars/hadoop-mapreduce-examples.jar`.

Daha fazla bilgi için [bkz.](../storage/blobs/data-lake-storage-introduction-abfs-uri.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
* [Azure Veri Gölü Depolama Gen1 genel bakış](./overview-data-lake-storage-gen1.md)