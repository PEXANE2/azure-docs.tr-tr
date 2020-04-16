---
title: Azure Veri Fabrikası'nda veri akışları
description: Azure Veri Fabrikası'nda çekişmeli veri akışlarına genel bakış
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 71df574602518fdc252d9d86859ed772a17be443
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81408967"
---
# <a name="what-are-wrangling-data-flows"></a>Çekişmeli veri akışları nelerdir?

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Kuruluşların her gün büyümeye devam eden karmaşık verilerin doğru analizi için veri hazırlama ve çekişme ler yapmaları gerekir. Kuruluşların verileri çeşitli iş süreçlerinde kullanabilmesi ve değer verme süresini kısaltması için veri hazırlama gereklidir.

Azure Veri Fabrikası'ndaki veri akışlarının düzenlenmesi, bulut ölçeğinde yinelemeli olarak kodsuz veri hazırlama yapmanıza olanak tanır. Wrangling veri akışları [Power Query Online](https://docs.microsoft.com/power-query/) ile entegre ve Güç Sorgusu M işlevleri veri fabrikası kullanıcıları için kullanılabilir hale getirir.

Wrangling veri akışı, Power Query Online Mashup Düzenleyicisi tarafından oluşturulan M'yi bulut ölçeği yürütme için kıvılcım koduna çevirir.

Çekişmeli veri akışları özellikle veri mühendisleri veya 'vatandaş veri entegratörleri' için yararlıdır.

## <a name="use-cases"></a>Uygulama alanları

### <a name="fast-interactive-data-exploration-and-preparation"></a>Hızlı etkileşimli veri arama ve hazırlama

Birden çok veri mühendisi ve vatandaş veri entegratörleri bulut ölçeğinde veri kümelerini etkileşimli olarak keşfedebilir ve hazırlayabilir. Veri göllerinde veri hacminin, çeşitliliğinin ve hızının artmasıyla, kullanıcılar veri kümelerini keşfetmek ve hazırlamak için etkili bir yola ihtiyaç duyarlar. Örneğin, '2017'den beri yeni müşteriler için tüm müşteri demografik bilgilerine sahip' bir veri kümesi oluşturmanız gerekebilir. Bilinen bir hedefin haritasını çıkarmadın. Gölde yayımlamadan önce bir gereksinimi karşılamak için veri kümelerini keşfediyor, çarpıtıyor ve hazırlıyorsunuz. Çekişmeli veri akışları genellikle daha az resmi analiz senaryoları için kullanılır. Hazır veri kümeleri, akış aşağı dönüşümleri ve makine öğrenimi işlemleri yapmak için kullanılabilir.

### <a name="code-free-agile-data-preparation"></a>Kodsuz çevik veri hazırlama

Vatandaş veri entegratörleri zamanlarının %60'ından fazlasını veri aramak ve hazırlamakla geçirirler. Operasyonel üretkenliği artırmak için bunu kodsuz bir şekilde yapmak istiyorlar. Vatandaş veri entegratörlerinin Power Query Online gibi bilinen araçları kullanarak verileri ölçeklenebilir bir şekilde zenginleştirmesine, şekillendirmesine ve yayımlayabilmelerine izin vermek, üretkenliklerini önemli ölçüde artırır. Azure Veri Fabrikası'nda veri akışını nrangling, vatandaş veri entegratörlerinin hataları hızlı bir şekilde düzeltmesine, verileri standartlaştırmasına ve iş kararlarını desteklemek için yüksek kaliteli veriler üretmesine olanak tanıyan tanıdık Power Query Online mashup düzenleyicisine olanak tanır.

### <a name="data-validation"></a>Veri doğrulama

Herhangi bir aykırılıkları, anormallikleri kaldırmak ve hızlı analitik için bir şekle uygun hale getirmek için verilerinizi kodsuz bir şekilde görsel olarak tarar.

## <a name="supported-sources"></a>Desteklenen kaynaklar

| Bağlayıcı | Veri biçimi | Kimlik doğrulaması türü |
| -- | -- | --|
| [Azure Blob Depolama](connector-azure-blob-storage.md) | CSV, Parke | Hesap Anahtarı |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | CSV | Hizmet Sorumlusu |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | CSV, Parke | Hesap Anahtarı, Servis Sorumlusu |
| [Azure SQL Veritabanı](connector-azure-sql-database.md) | - | SQL kimlik doğrulaması |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) | - | SQL kimlik doğrulaması |

## <a name="the-mashup-editor"></a>Mashup editörü

Bir çekişme li veri akışı oluşturduğunuzda, tüm kaynak veri kümeleri veri kümesi sorguları olur ve **ADFResource** klasörüne yerleştirilir. Varsayılan olarak, UserQuery ilk veri kümesi sorgusunu gösterir. Dataset sorgularında yapılan değişiklikler desteklenmedikçe ve kalıcı olarak kullanılmadığı için tüm dönüşümler UserQuery'de yapılmalıdır. Sorguları yeniden adlandırma, ekleme ve silme şu anda desteklenmez.

![Çekişme](media/wrangling-data-flow/editor.png)

Şu anda tüm Power Query M işlevleri, yazarlık sırasında kullanılabilir olmasına rağmen veri çekişmesi için desteklenmemektedir. Çekişmeli veri akışlarınızı yaparken, bir işlev desteklenmezse aşağıdaki hata iletisi istenir:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

Desteklenen dönüşümler hakkında daha fazla bilgi için, [veri akışı işlevleri nin çekişmesi](wrangling-data-flow-functions.md)ne bakın.

Şu anda çekişmeli veri akışı yalnızca bir lavaboya yazmayı destekler.

## <a name="next-steps"></a>Sonraki adımlar

[Nasıl çekişmeli bir veri akışı oluşturabilirsiniz](wrangling-data-flow-tutorial.md)öğrenin.