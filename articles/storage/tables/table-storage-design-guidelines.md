---
title: Azure depolama tablo tasarımı için yönergeler | Microsoft Docs
description: Azure Tablo hizmetinizi, okuma işlemlerini verimli bir şekilde destekleyecek şekilde tasarlayın.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: d056d29469ad9a60fceeee307aca3c0e1319283c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "61269856"
---
# <a name="guidelines-for-table-design"></a>Tablo tasarımı için yönergeler

Azure Depolama Tablo hizmeti ile kullanılmak üzere tabloları tasarlamak, ilişkisel bir veritabanı için tasarım açısından önemli bir farklılık vardır. Bu makalede, tablo hizmeti çözümünüzü daha etkili ve yazmaya yönelik olarak tasarlamaya yönelik yönergeler açıklanmaktadır.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Tablo hizmeti çözümünüzü, okuma açısından verimli olacak şekilde tasarlama

* ***Okuma ağır uygulamalarda sorgulama için tasarım.*** Tablolarınızı tasarlarken, varlıklarınızı nasıl güncelleşeceğinize ilişkin düşündüğünüzden önce kullanacağınız sorguları (özellikle gecikme süresine duyarlı olanları) düşünün. Bu, genellikle etkili ve performanslı bir çözüme neden olur.  
* ***Sorgularınızda hem PartitionKey hem de RowKey belirtin.*** Bu gibi *nokta sorguları* en verimli tablo hizmeti sorgulardır.  
* ***Varlıkların yinelenen kopyalarını depolamayı göz önünde bulundurun.*** Tablo depolaması, daha verimli sorgular sağlamak için aynı varlığı birden çok kez depolamayı (farklı anahtarlarla) göz önünde bulundurun.  
* ***Verilerinizi kabul etmeyi düşünün.*** Tablo depolaması, veri izlemeyi düşünün. Örneğin, toplama verileri sorgularının yalnızca tek bir varlığa erişmesi için, Özet varlıkları depolayın.  
* ***Bileşik anahtar değerlerini kullanın.*** Yalnızca **partitionkey** ve **rowkey**olduğunuz anahtarlar vardır. Örneğin, varlıklara alternatif anahtarlı erişim yolları sağlamak için bileşik anahtar değerlerini kullanın.  
* ***Sorgu projeksiyonu kullanın.*** Yalnızca ihtiyaç duyduğunuz alanları seçerek ağ üzerinden aktardığınız veri miktarını azaltabilirsiniz.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Tablo hizmeti çözümünüzü yazma etkin olacak şekilde tasarlama  

* ***Etkin bölümler oluşturmayın.*** İsteklerinizi dilediğiniz zaman bir noktada birden çok bölüme yaymaya olanak sağlayan anahtarlar ' ı seçin.  
* ***Trafikte ani artışlar önleyin.*** Trafiği makul bir süre boyunca düzgünleştirmek ve trafikte ani artışlar önleyin.
* ***Her varlık türü için ayrı bir tablo oluşturmanız gerekmez.*** Varlık türleri arasında Atomik işlemler gerektirdiğinde, bu birden çok varlık türünü aynı tablodaki aynı bölümde saklayabilirsiniz.
* ***Elde etmeniz gereken en yüksek aktarım hızını göz önünde bulundurun.*** Tablo hizmeti için ölçeklenebilirlik hedeflerini bilmeniz ve tasarımınızın bunları aşmanıza yol açmayacağından emin olmanız gerekir.  

Bu kılavuzu okurken, tüm bu ilkeleri uygulamaya yerleştirtirecek örnekleri görürsünüz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarımı desenleri](table-storage-design-patterns.md)
- [Sorgulama için tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
