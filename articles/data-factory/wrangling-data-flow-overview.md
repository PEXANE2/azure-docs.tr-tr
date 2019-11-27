---
title: Azure Data Factory 'de wrangling veri akışları
description: Azure Data Factory 'de denetimi veri akışlarına genel bakış
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 56c2d96e6e4a5900770aaefcabb424eddb1cbde6
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74531386"
---
# <a name="what-are-wrangling-data-flows"></a>Denetimi veri akışları nelerdir?

Kuruluşların her gün büyümeye devam eden karmaşık verilerin doğru analizi için veri hazırlama ve denetimi yapması gerekir. Kuruluşların verileri çeşitli iş süreçlerinde kullanabilmesi ve değerin süresini azaltabilmesi için veri hazırlama gereklidir.

Azure Data Factory ' deki wrangling veri akışları, bulut ölçeğinde kod içermeyen veri hazırlığı yapmanıza olanak sağlar. Wrangling veri akışları [Power Query çevrimiçi](https://docs.microsoft.com/power-query/) olarak tümleştirilir ve Data Factory kullanıcıları Için Power Query M işlevlerini kullanılabilir hale getirir.

Wrangling veri akışı, Power Query çevrimiçi karma düzenleyici tarafından oluşturulan M 'yi bulut ölçek yürütmesi için Spark koduna çevirir.

Wrangling veri akışları, özellikle veri mühendisleri veya ' vatandaşlık veri tümleştiricileri ' için faydalıdır.

## <a name="use-cases"></a>Uygulama alanları

### <a name="fast-interactive-data-exploration-and-preparation"></a>Hızlı etkileşimli veri araştırması ve hazırlığı

Birden çok veri mühendisi ve vatandaşlık veri tümleştiricileri, bulut ölçeğinde veri kümelerini etkileşimli bir şekilde araştırabilir ve hazırlayabilir. Verilerin yükseminde, veri kümelerindeki verilerin çeşitliliğini, çeşitli ve hızına sahip kullanıcılar, veri kümelerini keşfetmeye ve hazırlamaya yönelik etkili bir yönteme ihtiyaç duyar. Örneğin, ' 2017 ' den beri yeni müşteriler için tüm müşteri demoonları bilgilerine sahip bir veri kümesi oluşturmanız gerekebilir. Bilinen bir hedefle eşlememeniz yok. Gölü üzerinde yayımlamadan önce bir gereksinimi karşılamak için araştırırken, wrangling ve veri kümeleri önceden alınıyor. Wrangling veri akışları genellikle daha az resmi analiz senaryosu için kullanılır. Önceden yazılmış veri kümeleri, dönüşümler ve makine öğrenimi işlemleri için aşağı akış yapmak üzere kullanılabilir.

### <a name="code-free-agile-data-preparation"></a>Kod ücretsiz çevik veri hazırlama

Vatandaşlık veri tümleştiricileri, verileri arayan ve hazırlama zamanının %60 ' inden fazlasını harcıyordu. İşlem üretkenliğini artırmak için bunu bir kod ücretsiz olarak yapmak arıyorlar. Vatandaşlık veri tümleştiricilerinin, ölçeklenebilir bir şekilde Power Query çevrimiçi gibi bilinen araçları kullanarak verileri zenginleştirmesine ve yayımlamasına olanak tanımak, büyük ölçüde üretkenliğini artırır. Azure Data Factory 'de wrangling veri akışı, vatandaşlık veri tümleştiricilerinin hataları hızla düzeltmesine, verileri standartlaştırmasına ve iş kararlarını desteklemek için yüksek kaliteli veriler oluşturmasına izin vermek üzere tanıdık Power Query çevrimiçi karma düzenleyici 'yi sağlar.

### <a name="data-validation"></a>Veri doğrulama

Herhangi bir şeyi kaldırmak ve hızlı analizler için bir şekle uymak üzere verilerinizi kod ücretsiz olarak görsel olarak tarayın.

## <a name="supported-sources"></a>Desteklenen kaynaklar

| Bağlayıcı | Veri biçimi | Kimlik doğrulaması türü |
| -- | -- | --|
| [Azure Blob Depolama](connector-azure-blob-storage.md) | CSV | Hesap anahtarı |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | CSV | Hizmet Sorumlusu |
| [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md) | CSV | Hesap anahtarı, hizmet sorumlusu |
| [Azure SQL Veritabanı](connector-azure-sql-database.md) | - | SQL kimlik doğrulaması |
| [Azure SYNAPSE Analizi](connector-azure-sql-data-warehouse.md) | - | SQL kimlik doğrulaması |

## <a name="the-mashup-editor"></a>Karma düzenleyici

Bir denetimi veri akışı oluşturduğunuzda, tüm kaynak veri kümeleri veri kümesi sorguları haline gelir ve **adfresource** klasörüne yerleştirilir. Varsayılan olarak, UserQuery ilk veri kümesi sorgusuna işaret eder. Veri kümesi sorgularının değişiklikleri desteklenmediğinden veya kalıcı hale getirilmesi nedeniyle, tüm dönüşümler UserQuery üzerinde yapılmalıdır. Sorguları yeniden adlandırma, ekleme ve silme işlemi şu anda desteklenmiyor.

![Denetimi](media/wrangling-data-flow/editor.png)

Şu anda tüm Power Query M işlevleri, yazma sırasında kullanılabilir olmasına rağmen veri denetimi için desteklenmez. Denetimi veri akışlarınızı oluştururken, bir işlev desteklenmiyorsa aşağıdaki hata iletisiyle karşılaşırsınız:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

Desteklenen Dönüşümler hakkında daha fazla bilgi için bkz. [denetimi veri akışı işlevleri](wrangling-data-flow-functions.md).

Şu anda denetimi veri akışı yalnızca bir havuza yazmayı destekler.

## <a name="next-steps"></a>Sonraki adımlar

[Bir denetimi veri akışı oluşturmayı](wrangling-data-flow-tutorial.md)öğrenin.