---
title: Rezervasyon indirimini anlama - PostgreSQL için Azure Veritabanı Tek sunucu
description: PostgreSQL için Azure Veritabanı Tek sunucuya rezervasyon indiriminin nasıl uygulanacağını öğrenin.
author: kummanish
ms.author: manishku
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: d6b32df7264066daa0bc7298a04453dad4fc9937
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92147245"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure Veritabanı Tek sunucuya rezervasyon indiriminin uygulanması

Bir PostgreSQL için Azure Veritabanı Tek sunucu ayrılmış kapasitesi satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen PostgreSQL Tek sunucu veritabanlarına otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon yalnızca PostgreSQL için Azure Veritabanı Tek sunucu işlem maliyetlerini kapsar. Depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi ***kullanılmadığı takdirde hakkı kaybedilen*** bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.</br>

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-applied-to-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure Veritabanı Tek sunucuya uygulanan indirim

PostgreSQL için Azure Veritabanı Tek sunucu ayrılmış kapasite indirimi, PostgreSQL Tek sunucunuza saatlik olarak uygulanır. Satın aldığınız rezervasyon, çalışmakta olan PostgreSQL için Azure Veritabanı Tek sunucu tarafından gösterilen işlem kullanımı ile eşleştirilir. Saatin tamamı boyunca çalışmayan PostgreSQL Tek sunucuları olursa rezervasyon, rezervasyon öznitelikleriyle eşleşen diğer PostgreSQL için Azure Veritabanı Tek sunuculara otomatik olarak uygulanır. İndirim eşzamanlı olarak çalışan PostgreSQL için Azure Veritabanı Tek sunucularına uygulanabilir. Rezervasyon öznitelikleriyle eşleşen ve tam bir saat boyunca çalışan bir PostgreSQL Tek sunucunuz yoksa, ilgili saat için rezervasyon indiriminden tam olarak yararlanmazsınız.

Aşağıdaki örneklerde, satın aldığınız çekirdek sayısına ve çalıştırılma zamanına bağlı olarak PostgreSQL için Azure Veritabanı Tek sunucu ayrılmış kapasite indiriminin nasıl uygulanacağı gösterilmektedir.

**Örnek 1**: 8 sanal çekirdek için PostgreSQL için Azure Veritabanı Tek sunucu ayrılmış kapasitesi satın alabilirsiniz. Rezervasyonun diğer öznitelikleriyle eşleşen 16 sanal çekirdekli PostgreSQL için Azure Veritabanı Tek sunucu çalıştırıyorsanız PostgreSQL Tek sunucu işlem kullanımınızın 8 sanal çekirdeği için kullandıkça öde fiyatları üzerinden ücretlendirilirsiniz ve 8 sanal çekirdek PostgreSQL Tek sunucu işlem kullanımı için bir saatlik indirimden faydalanırsınız.</br>

Bu örneklerin geri kalan kısmında satın aldığınız PostgreSQL için Azure Veritabanı Tek sunucu ayrılmış kapasitesinin 16 sanal çekirdekli PostgreSQL için Azure Veritabanı Tek sunucu için olduğunu ve diğer rezervasyon özniteliklerinin çalışan PostgreSQL Tek sunucuları ile eşleştiğini kabul edelim.

* **Örnek 2**: Bir saat boyunca her biri 8 sanal çekirdeğe sahip olan iki PostgreSQL için Azure Veritabanı Tek sunucu çalıştırıyorsunuz. 8 çekirdekli iki PostgreSQL için Azure Veritabanı Tek sunucunun işlem kullanımına 16 sanal çekirdek rezervasyon indirimi uygulanır.

* **Örnek 3**: Saat 13:00 ile 13:30 arasında 16 sanal çekirdekli PostgreSQL için Azure Veritabanı Tek sunucu çalıştırıyorsunuz. Saat 13:30 ile 14:00 arasında başka bir 16 sanal çekirdekli PostgreSQL için Azure Veritabanı Tek sunucu çalıştırıyorsunuz. Her ikisi de rezervasyon indirimi kapsamındadır.

* **Örnek 4**: Saat 13:00 ile 13:45 arasında 16 sanal çekirdekli PostgreSQL için Azure Veritabanı Tek sunucu çalıştırıyorsunuz. Saat 13:30 ile 14:00 arasında başka bir 16 sanal çekirdekli PostgreSQL için Azure Veritabanı Tek sunucu çalıştırıyorsunuz. 15 dakikalık çakışma için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Rezervasyon indirimi, geri kalan süre boyunca işlem kullanımına uygulanır.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulamasını anlamak ve görüntülemek için bkz. [Azure rezervasyon kullanımınızı anlama](./understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).