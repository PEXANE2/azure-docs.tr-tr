---
title: Azure API 'de FHıR için veritabanı ayarlarını yapılandırma
description: Bu makalede, FHıR için Azure API 'de veritabanı ayarlarının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 11/15/2019
ms.author: matjazl
ms.openlocfilehash: adc6fdf144927d10f811a00aa33f244cfdc25042
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871767"
---
# <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma 

FHıR için Azure API, verilerini depolamak için veritabanını kullanır. Temel alınan veritabanının performansı, hizmet sağlama sırasında veya hizmet sağlandıktan sonra veritabanı ayarlarında seçilen Istek birimlerinin (RU) sayısına bağlıdır.

Temel veritabanının performansı ayarlanırken Cosmos DB ( [Azure Cosmos DB Içindeki Istek birimlerine](https://docs.microsoft.com/azure/cosmos-db/request-units)bakın) için Azure API 'sı. 

Veritabanı için her zaman yeterli sistem kaynaklarının kullanılabilir olduğundan emin olmak için üretilen iş sağlanmalıdır. Uygulamanız için ihtiyacınız olan RUs, gerçekleştirdiğiniz işlemlere göre değişir. İşlemler basit okuma ve yazma işlemlerini daha karmaşık sorgulara göre değişebilir. 

> [!NOTE]
> Farklı işlemler farklı sayıda RU tükettiği için, yanıt üstbilgisindeki her API çağrısında tüketilen RUs gerçek sayısını döndürüyoruz. Bu şekilde, uygulamanız tarafından tüketilen RUs sayısını profilin profilini oluşturabilirsiniz.

## <a name="update-throughput"></a>Üretimi Güncelleştir
Azure portal bu ayarı değiştirmek için FHıR için Azure API 'nize gidin ve veritabanı dikey penceresini açın. Daha sonra, sağlanan aktarım hızını, performans gereksinimlerinize bağlı olarak istenen değerle değiştirin. Değeri en fazla 10.000 RU/sn olacak şekilde değiştirebilirsiniz. Daha yüksek bir değere ihtiyacınız varsa Azure desteği 'ne başvurun.

> [!NOTE] 
> Daha yüksek değer, daha yüksek bir Azure API 'SI ve hizmetin maliyeti ve daha yüksek maliyetli anlamına gelir.

![Yapılandırma Cosmos DB](media/database/database-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için RUs for Azure API 'nizi güncelleştirmeyi öğrendiniz. Daha sonra FHIR için tam olarak yönetilen bir Azure API dağıtımı:
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)