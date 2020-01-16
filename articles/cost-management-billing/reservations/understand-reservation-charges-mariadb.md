---
title: MariaDB için Azure Veritabanına rezervasyon indiriminin uygulanması
description: MariaDB için Azure Veritabanına rezervasyon indiriminin uygulanması
author: kummanish
ms.author: manishku
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2019
ms.openlocfilehash: 340d395278eb924570e07d106308e63be3c2f9af
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75995526"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanına rezervasyon indiriminin uygulanması

Bir MariaDB için Azure Veritabanı ayrılmış kapasitesi satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen MariaDB sunucularına otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon yalnızca MariaDB için Azure Veritabanı işlem maliyetlerini kapsar. Depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz. 

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi ***kullanılmadığı takdirde hakkı kaybedilen*** bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.</br>

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-applied-to-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanına uygulanan indirim

MariaDB için Azure Veritabanı ayrılmış kapasitesi indirimi, çalışan MariaDB sunucularınıza saatlik olarak uygulanır. Satın aldığınız rezervasyon, çalışmakta olan MariaDB için Azure Veritabanı sunucuları tarafından gösterilen işlem kullanımı ile eşleştirilir. Saatin tamamı boyunca çalışmayan MariaDB sunucuları olursa rezervasyon, rezervasyon öznitelikleriyle eşleşen diğer MariaDB için Azure Veritabanı sunucularına otomatik olarak uygulanır. İndirim eşzamanlı olarak çalışan MariaDB için Azure Veritabanı sunucularına uygulanabilir. Rezervasyon öznitelikleriyle eşleşen ve saatin tamamı boyunca çalışan bir MariaDB sunucunuz yoksa, ilgili saat için rezervasyon indiriminden tam olarak yararlanmazsınız.

Aşağıdaki örneklerde, satın aldığınız çekirdek sayısına ve çalıştırılma zamanına bağlı olarak MariaDB için Azure Veritabanı ayrılmış kapasite indiriminin nasıl uygulanacağı gösterilmektedir.

* **Örnek 1**: bir 8 sanal çekirdek Için MariaDB ile ayrılmış kapasite Için Azure veritabanı satın alabilirsiniz. Rezervasyonun diğer öznitelikleriyle eşleşen 16 sanal çekirdekli MariaDB için Azure Veritabanı sunucusu çalıştırıyorsanız MariaDB sunucusu işlem kullanımınızın 8 sanal çekirdeği için kullandıkça öde fiyatları üzerinden ücretlendirilirsiniz ve 8 sanal çekirdek MariaDB sunucusu işlem kullanımı için bir saatlik indirimden faydalanırsınız.</br>

Bu örneklerin geri kalan kısmında satın aldığınız MariaDB için Azure Veritabanı ayrılmış kapasitesinin 16 sanal çekirdekli MariaDB için Azure Veritabanı için olduğunu ve diğer rezervasyon özniteliklerinin çalışan MariaDB sunucuları ile eşleştiğini kabul edelim.

* **Örnek 2**: her bir saat Için 8 sanal çekirdekli her bir MariaDB sunucusu Için Iki Azure veritabanı çalıştırırsınız. 8 çekirdekli iki MariaDB için Azure Veritabanı sunucusunun işlem kullanımına 16 sanal çekirdek rezervasyon indirimi uygulanır.

* **Örnek 3**: MariaDB sunucusu Için 1 16 sanal çekirdek Azure veritabanı 'nı, 1 pm ile 1:30 PM arasında çalıştırırsınız. Saat 13:30 ile 14:00 arasında başka bir 16 sanal çekirdekli MariaDB için Azure Veritabanı sunucusu çalıştırıyorsunuz. Her ikisi de rezervasyon indirimi kapsamındadır.

* **Örnek 4**: MariaDB sunucusu Için 1 16 sanal çekirdek Azure veritabanı 'nı, 1 pm ile 1:45 PM arasında çalıştırırsınız. Saat 13:30 ile 14:00 arasında başka bir 16 sanal çekirdekli MariaDB için Azure Veritabanı sunucusu çalıştırıyorsunuz. 15 dakikalık çakışma için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Rezervasyon indirimi, geri kalan süre boyunca işlem kullanımına uygulanır.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulamasını anlamak ve görüntülemek için bkz. [Azure rezervasyon kullanımınızı anlama](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bize ulaşın
Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
