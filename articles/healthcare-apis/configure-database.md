---
title: Azure API 'de FHıR için veritabanı ayarlarını yapılandırma
description: Bu makalede, FHıR için Azure API 'de veritabanı ayarlarının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 11/15/2019
ms.author: matjazl
ms.openlocfilehash: 652445a96acfa0358211d1d97e0fcf288989d6ba
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88795788"
---
# <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma 

FHıR için Azure API, verilerini depolamak için veritabanını kullanır. Temel alınan veritabanının performansı, hizmet sağlama sırasında veya hizmet sağlandıktan sonra veritabanı ayarlarında seçilen Istek birimlerinin (RU) sayısına bağlıdır.

Temel veritabanının performansı ayarlanırken Cosmos DB ( [Azure Cosmos DB Içindeki Istek birimlerine](https://docs.microsoft.com/azure/cosmos-db/request-units)bakın) için Azure API 'sı. 

Veritabanı için her zaman yeterli sistem kaynaklarının kullanılabilir olduğundan emin olmak için üretilen iş sağlanmalıdır. Uygulamanız için ihtiyacınız olan RUs, gerçekleştirdiğiniz işlemlere göre değişir. İşlemler basit okuma ve yazma işlemlerini daha karmaşık sorgulara göre değişebilir. 

> [!NOTE]
> Farklı işlemler farklı sayıda RU tükettiği için, yanıt üstbilgisindeki her API çağrısında tüketilen RUs gerçek sayısını döndürüyoruz. Bu şekilde, uygulamanız tarafından tüketilen RUs sayısını profilin profilini oluşturabilirsiniz.

## <a name="update-throughput"></a>Üretimi Güncelleştir

Azure portal bu ayarı değiştirmek için FHıR için Azure API 'nize gidin ve veritabanı dikey penceresini açın. Daha sonra, sağlanan aktarım hızını, performans gereksinimlerinize bağlı olarak istenen değerle değiştirin. Değeri en fazla 10.000 RU/sn olacak şekilde değiştirebilirsiniz. Daha yüksek bir değere ihtiyacınız varsa Azure desteği 'ne başvurun.

Veritabanı üretimi 10.000 RU/sn 'den büyükse veya veritabanında depolanan veriler 50 GB 'tan fazlaysa, istemci uygulamanız devamlılık belirteçlerini işleyebilme yeteneğine sahip olmalıdır. Veritabanında, 10.000 RU/sn 'nin her verimlilik artışı için yeni bir bölüm oluşturulur veya depolanan veri miktarı 50 GB 'den fazla olursa. Birden çok bölüm, devamlılık belirteçleri kullanılarak Sayfalandırmaya uygulanan çok sayfalı bir yanıt oluşturur.

> [!NOTE] 
> Daha yüksek değer, daha yüksek bir Azure API 'SI ve hizmetin maliyeti ve daha yüksek maliyetli anlamına gelir.

![Yapılandırma Cosmos DB](media/database/database-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için RUs for Azure API 'nizi güncelleştirmeyi öğrendiniz. Daha sonra FHIR için tam olarak yönetilen bir Azure API dağıtımı:
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)
