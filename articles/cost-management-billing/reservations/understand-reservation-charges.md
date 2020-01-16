---
title: Azure SQL Veritabanları için rezervasyon indirimini anlama | Microsoft Docs
description: Çalışmakta olan Azure SQL Veritabanlarına bir rezervasyon indiriminin nasıl uygulanacağını öğrenin.
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2019
ms.author: banders
ms.openlocfilehash: f7382fc81bbb7e0f3ac61b19c1efa1e7de3e9ed7
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75995474"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-sql-databases"></a>Azure SQL Veritabanlarına rezervasyon indiriminin uygulanması

Bir Azure SQL Veritabanı ayrılmış kapasitesi satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen SQL Veritabanlarına otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon, SQL Veritabanınızın işlem maliyetlerini kapsar. Yazılım, depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz. [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) ile SQL Veritabanı için lisanslama maliyetlerini kapsayabilirsiniz.

Ayrılmış Sanal Makine Örnekleri için bkz. [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](../manage/understand-vm-reservation-charges.md).

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi “*kullanılmadığı takdirde hakkı kaybedilen*” bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

## <a name="discount-applied-to-sql-databases"></a>SQL Veritabanlarına uygulanan indirim

 SQL Veritabanı ayrılmış kapasite indirimi, çalışmakta olan SQL Veritabanlarına saat bazında uygulanır. Satın aldığınız rezervasyon, çalışmakta olan SQL Veritabanları tarafından gösterilen işlem kullanımı ile eşleştirilir. Saatin tamamı boyunca çalışmayan SQL Veritabanları olursa rezervasyon, rezervasyon öznitelikleriyle eşleşen diğer SQL veritabanlarına otomatik olarak uygulanır. Eş zamanlı olarak çalışan SQL Veritabanları için de indirim geçerli olabilir. Rezervasyon öznitelikleriyle eşleşen ve saatin tamamı boyunca çalışan bir SQL Veritabanınız yoksa, ilgili saat için rezervasyon indiriminden tam olarak yararlanmazsınız.

Aşağıdaki örneklerde, satın aldığınız çekirdek sayısına ve çalıştırılma zamanına bağlı olarak SQL Veritabanı ayrılmış kapasite indiriminin nasıl uygulanacağı gösterilmektedir.

- Senaryo 1:8 çekirdekli bir SQL veritabanı için SQL veritabanı ayrılmış kapasitesi satın alabilirsiniz. Rezervasyon özniteliklerinin geri kalanıyla eşleşen 16 çekirdekli bir SQL Veritabanı çalıştırıyorsunuz. 8 çekirdekli SQL Veritabanı işlem kullanımı için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Bir saatlik 8 çekirdekli SQL Veritabanı işlem kullanımı için rezervasyon indirimi elde edersiniz.

Bu örneklerin geri kalanında, satın aldığınız SQL Veritabanı ayrılmış kapasitesinin 16 çekirdekli SQL Veritabanı için olduğunu ve rezervasyon özniteliklerinin geri kalanının, çalışmakta olan SQL Veritabanları ile eşleştiğini varsayın.

- Senaryo 2: her bir saat için 8 çekirdekle iki SQL veritabanı çalıştırırsınız. 16 çekirdekli rezervasyon indirimi her iki 8 çekirdekli SQL Veritabanı için işlem kullanımına uygulanır.
- Senaryo 3:1 16 çekirdekli SQL veritabanını 1 PM ile 1:30 PM arasında çalıştırırsınız. 13:30’dan 14:00’a kadar başka bir 16 çekirdekli SQL Veritabanı çalıştırıyorsunuz. Her ikisi de rezervasyon indirimi kapsamındadır.
- Senaryo 4:1 16 çekirdekli SQL veritabanını 1 PM ile 1:45 PM arasında çalıştırırsınız. 13:30’dan 14:00’a kadar başka bir 16 çekirdekli SQL Veritabanı çalıştırıyorsunuz. 15 dakikalık çakışma için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Rezervasyon indirimi, geri kalan süre boyunca işlem kullanımına uygulanır.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulamasını anlamak ve görüntülemek için bkz. [Azure rezervasyon kullanımınızı anlama](understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bize ulaşın

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Rezervasyonlar nedir?](save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için rezervasyon kullanımını anlama](/partner-center/azure-reservations)
