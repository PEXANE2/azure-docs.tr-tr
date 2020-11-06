---
title: Azure API 'de FHıR için veritabanı ayarlarını yapılandırma
description: Bu makalede, FHıR için Azure API 'de veritabanı ayarlarının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 11/15/2019
ms.author: matjazl
ms.openlocfilehash: 3fe4118a8ecf4479732ba4073b342d3ec9f941b8
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93398207"
---
# <a name="configure-database-settings"></a>Veritabanı ayarlarını yapılandırma 

FHıR için Azure API, verilerini depolamak için veritabanını kullanır. Temel alınan veritabanının performansı, hizmet sağlama sırasında veya hizmet sağlandıktan sonra veritabanı ayarlarında seçilen Istek birimlerinin (RU) sayısına bağlıdır.

Temel veritabanının performansı ayarlanırken Cosmos DB ( [Azure Cosmos DB Içindeki Istek birimlerine](../cosmos-db/request-units.md)bakın) için Azure API 'sı. 

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

Bu makalede, FHıR için RUs for Azure API 'nizi güncelleştirmeyi öğrendiniz. Müşteri tarafından yönetilen anahtarları bir veritabanı ayarı olarak yapılandırma hakkında bilgi edinmek için:

>[!div class="nextstepaction"]
>[Müşteri tarafından yönetilen anahtarları yapılandırma](customer-managed-key.md)

Ya da FHıR için tam olarak yönetilen bir Azure API 'SI dağıtabilirsiniz:
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)