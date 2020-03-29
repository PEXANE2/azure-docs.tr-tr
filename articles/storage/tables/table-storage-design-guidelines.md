---
title: Azure depolama tablosu tasarımı için yönergeler | Microsoft Dokümanlar
description: Okuma işlemlerini verimli bir şekilde desteklemek için Azure tablo hizmetinizi tasarla.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: d056d29469ad9a60fceeee307aca3c0e1319283c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61269856"
---
# <a name="guidelines-for-table-design"></a>Tablo tasarımı için yönergeler

Azure depolama tablo hizmetinde kullanılmak üzere tablolar tasarlamak, ilişkisel bir veritabanı için tasarım hususlarından çok farklıdır. Bu makalede, tablo hizmeti çözümünüzüverimli bir şekilde okunacak ve verimli yazacak şekilde tasarlama yönergeleri açıklanmaktadır.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Tablo servis çözümünüzü okuma-verimli olacak şekilde tasarlayın

* ***Okuma ağırlıklı uygulamalarda sorgulama için tasarım.*** Tablolarınızı tasarlarken, varlıklarınızı nasıl güncelleştireceğiniz hakkında düşünmeden önce yürüteceğiniz sorguları (özellikle gecikmeye duyarlı olanları) düşünün. Bu genellikle verimli ve performant bir çözüm le sonuçlanır.  
* ***Sorgularınızda hem PartitionKey hem de RowKey'i belirtin.*** Bu gibi *nokta sorguları* en verimli tablo hizmeti sorgularıdır.  
* ***Varlıkların yinelenen kopyalarını depolamayı düşünün.*** Tablo depolama ucuz, bu nedenle daha verimli sorgular sağlamak için aynı varlığı birden çok kez (farklı anahtarlarla) depolamayı düşünün.  
* ***Verilerinizin normalinden arındırmayı düşünün.*** Tablo depolama ucuz bu yüzden verilerinizi denormalleştirme düşünün. Örneğin, özet varlıkları, toplu veri sorgularının yalnızca tek bir varlığa erişebilmeleri için depolayın.  
* ***Bileşik anahtar değerlerini kullanın.*** Sahip olduğunuz tek **anahtarlar PartitionKey** ve **RowKey'dir.** Örneğin, varlıklara alternatif anahtarlı erişim yollarını etkinleştirmek için bileşik anahtar değerlerini kullanın.  
* ***Sorgu projeksiyonu kullanın.*** Yalnızca gereksinim duyduğunuz alanları seçen sorguları kullanarak ağ üzerinden aktardığınız veri miktarını azaltabilirsiniz.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Tablo servis çözümünüzü yazma verimli olacak şekilde tasarlayın  

* ***Sıcak bölümler oluşturmayın.*** İsteklerinizi herhangi bir zamanda birden çok bölüme yaymanızı sağlayan anahtarları seçin.  
* ***Trafikte ani artışlardan kaçının.*** Trafiği makul bir süre boyunca düzleştirin ve trafikteki ani artışlardan kaçının.
* ***Her varlık türü için ayrı bir tablo oluşturma.*** Varlık türleri arasında atomik hareketler ekidirseniz, bu birden çok varlık türünü aynı tabloda aynı bölümde depolayabilirsiniz.
* ***Başarması gereken maksimum verimi göz önünde bulundurun.*** Tablo hizmetinin ölçeklenebilirlik hedeflerinin farkında olmalı ve tasarımınızın bunları aşmanıza neden olmadığından emin olmalısınız.  

Bu kılavuzu okudukça, tüm bu ilkeleri uygulamaya koyan örnekler göreceksiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo tasarımı desenleri](table-storage-design-patterns.md)
- [Sorgulama için tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için tasarım](table-storage-design-for-modification.md)
