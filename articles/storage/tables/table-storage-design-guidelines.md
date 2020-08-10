---
title: Azure depolama tablo tasarımı için yönergeler | Microsoft Docs
description: Azure depolama tablo hizmetinizi, okuma ve yazma işlemlerini verimli bir şekilde destekleyecek şekilde tasarlama yönergelerini anlayın.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: 5671cc71683061becbdbbce5d855e55cbaa7c595
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88035408"
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
