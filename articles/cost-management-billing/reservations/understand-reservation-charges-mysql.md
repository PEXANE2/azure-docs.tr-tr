---
title: Rezervasyon indirimini anlama - MySQL için Azure Veritabanı
description: MySQL için Azure Veritabanı sunucularınıza rezervasyon indiriminin nasıl uygulanacağını öğrenin.
author: mksuni
ms.author: sumuth
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 85a8c59cce74787a7b1723850915ec133b34b3b0
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240384"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanına rezervasyon indiriminin uygulanması

Bir MySQL için Azure Veritabanı ayrılmış kapasitesi satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen MySQL sunucularına otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon yalnızca MySQL için Azure Veritabanı işlem maliyetlerini kapsar. Depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi * **kullanılmadığı takdirde hakkı kaybedilen** * bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.</br>

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-applied-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanına uygulanan indirim

MySQL için Azure Veritabanı ayrılmış kapasitesi indirimi, çalışan MySQL sunucularınıza saatlik olarak uygulanır. Satın aldığınız rezervasyon, çalışmakta olan MySQL için Azure Veritabanı sunucuları tarafından gösterilen işlem kullanımı ile eşleştirilir. Saatin tamamı boyunca çalışmayan MySQL sunucuları olursa rezervasyon, rezervasyon öznitelikleriyle eşleşen diğer MySQL için Azure Veritabanı sunucularına otomatik olarak uygulanır. İndirim eşzamanlı olarak çalışan MySQL için Azure Veritabanı sunucularına uygulanabilir. Rezervasyon öznitelikleriyle eşleşen ve tam bir saat boyunca çalışan bir MySQL sunucunuz yoksa, ilgili saat için rezervasyon indiriminden tam olarak yararlanmazsınız.

Aşağıdaki örneklerde, satın aldığınız çekirdek sayısına ve çalıştırılma zamanına bağlı olarak MySQL için Azure Veritabanı ayrılmış kapasite indiriminin nasıl uygulanacağı gösterilmektedir.

_ **Örnek 1** : 8 sanal çekirdek için MySQL için Azure Veritabanı ayrılmış kapasitesi satın alabilirsiniz. Rezervasyonun diğer öznitelikleriyle eşleşen 16 sanal çekirdekli MySQL için Azure Veritabanı sunucusu çalıştırıyorsanız MySQL sunucusu işlem kullanımınızın 8 sanal çekirdeği için kullandıkça öde fiyatları üzerinden ücretlendirilirsiniz ve 8 sanal çekirdek MySQL sunucusu işlem kullanımı için bir saatlik indirimden faydalanırsınız.</br>

Bu örneklerin geri kalan kısmında satın aldığınız MySQL için Azure Veritabanı ayrılmış kapasitesinin 16 sanal çekirdekli MySQL için Azure Veritabanı için olduğunu ve diğer rezervasyon özniteliklerinin çalışan MySQL sunucuları ile eşleştiğini kabul edelim.

* **Örnek 2** : Bir saat boyunca her biri 8 sanal çekirdeğe sahip olan iki MySQL için Azure Veritabanı sunucuları çalıştırıyorsunuz. 8 çekirdekli iki MySQL için Azure Veritabanı sunucusunun işlem kullanımına 16 sanal çekirdek rezervasyon indirimi uygulanır.

* **Örnek 3** : Saat 13:00 ile 13:30 arasında 16 sanal çekirdekli MySQL için Azure Veritabanı sunucusu çalıştırıyorsunuz. Saat 13:30 ile 14:00 arasında başka bir 16 sanal çekirdekli MySQL için Azure Veritabanı sunucusu çalıştırıyorsunuz. Her ikisi de rezervasyon indirimi kapsamındadır.

* **Örnek 4** : Saat 13:00 ile 13:45 arasında 16 sanal çekirdekli MySQL için Azure Veritabanı sunucusu çalıştırıyorsunuz. Saat 13:30 ile 14:00 arasında başka bir 16 sanal çekirdekli MySQL için Azure Veritabanı sunucusu çalıştırıyorsunuz. 15 dakikalık çakışma için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Rezervasyon indirimi, geri kalan süre boyunca işlem kullanımına uygulanır.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulamasını anlamak ve görüntülemek için bkz. [Azure rezervasyon kullanımınızı anlama](./understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).