---
title: Azure rezervasyonu satın almaya hazırlanma
description: Azure rezervasyonu satın almadan önce önemli noktalar hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: 1f5ca2d43356eab98cffe8414c00d97e5744739a
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235660"
---
# <a name="prepare-to-buy-a-reservation"></a>Rezervasyon satın almaya hazırlanma

Azure Rezervasyonları birçok Azure kaynağı için bir yıllık veya üç yıllık planları kabul ederek tasarruf etmenize yardımcı olur. Rezervasyon satın alma taahhüdünde bulunmadan önce, satın almanıza hazırlamak için aşağıdaki bölümleri gözden geçirdiğinizden emin olun.

## <a name="who-can-buy-a-reservation"></a>Kimler rezervasyon satın alabilir?

Bir plan satın almak için bir Kurumsal (MS-AZR-0017P veya MS-AZR-0148P) ya da Kullandıkça Öde aboneliğinde (MS-AZR-0003P veya MS-AZR-0023P) veya Microsoft Müşteri Sözleşmesi aboneliğinde abonelik sahibi rolünüz olmalıdır. Bulut çözümü sağlayıcıları, Azure Ayrılmış Sanal Makine Örnekleri satın almak için Azure portalını veya  [İş Ortağı Merkezi](/partner-center/azure-reservations) ’ni kullanabilir.

Kurumsal Anlaşma (EA) müşterileri, EA Portalındaki **Ayrılmış Örnekler Ekle** seçeneğini devre dışı bırakarak satın alma işlemlerini EA yöneticileriyle sınırlayabilir. EA yöneticilerinin bir rezervasyon satın almak için en az bir EA aboneliğinin abonelik sahibi olması gerekir. Bu seçenek, merkezi bir ekibin farklı maliyet merkezleri için rezervasyonlar satın almasını isteyen kuruluşlar için yararlıdır. Satın alma işleminden sonra merkezi ekipler, rezervasyonlara maliyet merkezi sahiplerini ekleyebilirler. Bundan sonra sahipler, rezervasyonu aboneliklerinin kapsamına alabilirler. Merkezi ekibin, rezervasyonun satın alındığı yerde abonelik sahibi erişimine sahip olması gerekmez.

Rezervasyon indirimi yalnızca Kurumsal, Bulut Çözümü Sağlayıcısı (CSP) ve Microsoft Müşteri Sözleşmesi ile satın alınan abonelikler ve kullandıkça öde tarifelerine sahip bireysel planlar ile ilişkili kaynaklar için geçerlidir.

## <a name="scope-reservations"></a>Rezervasyonları kapsama alma

Bir rezervasyonu bir abonelik veya kaynak gruplarının kapsamına alabilirsiniz. Bir rezervasyon kapsamının ayarlanması, ayırma tasarruflarının nereye uygulanacağını seçer. Rezervasyonu bir kaynak grubunun kapsamına aldığınızda rezervasyon indirimleri aboneliğin tamamı için değil yalnızca kaynak grubu için geçerli olur.

### <a name="reservation-scoping-options"></a>Rezervasyon kapsamı belirleme seçenekleri

Gereksinimlerinize bağlı olarak bir rezervasyonu kapsama almak için üç seçeneğiniz vardır:

- **Tek kaynak grubu kapsamı**—Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular.
- **Tek abonelik kapsamı**—Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.
- **Paylaşılan kapsam**—Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. Kurumsal Anlaşma müşterileri için faturalama bağlamı kayıttır. Microsoft Müşteri Sözleşmesi müşterileri için faturalama kapsamı, faturalama profilidir. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.

Kullanımınıza rezervasyon indirimleri uygulanırken, Azure rezervasyonu aşağıdaki sırayla işler:

1. Kaynak grubu kapsamındaki rezervasyonlar
2. Tek kapsam rezervasyonları
3. Paylaşılan kapsam rezervasyonları

Tek bir kaynak grubu, rezervasyonların kapsamını nasıl belirlediğinize bağlı olarak birden çok rezervasyondan rezervasyon indirimleri alabilir.

Rezervasyon satın aldıktan sonra her zaman kapsamı güncelleştirebilirsiniz. Bunu yapmak için rezervasyona gidin, **Yapılandırma**'ya tıklayın ve rezervasyon kapsamını yeniden belirleyin. Rezervasyon kapsamını yeniden belirlemek ticari bir işlem değildir. Rezervasyon süreniz değişmez. Kapsamı güncelleştirme hakkında daha fazla bilgi için bkz. [Rezervasyon satın aldıktan sonra kapsamı güncelleştirme](manage-reserved-vm-instance.md#change-the-reservation-scope).

![Bir rezervasyon kapsamı değişikliğini gösteren örnek](./media/prepare-buy-reservation/rescope-reservation-resource-group.png)

## <a name="purchase-reservations"></a>Rezervasyon satın alma

Azure portalından, API’lerden, PowerShell’den ve CLI’dan rezervasyon satın alabilirsiniz. Rezervasyon satın almaya hazır olduğunuzda aşağıdaki makalelerden size uygun olanları okuyun:

- [App Service](prepay-app-service-isolated-stamp.md)
- [Redis için Azure Önbelleği](../../azure-cache-for-redis/cache-reserved-pricing.md)
- [Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
- [Databricks](prepay-databricks-reserved-capacity.md)
- [Veri Gezgini](../../data-explorer/pricing-reserved-capacity.md)
- [Disk Depolama](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Ayrılmış Konak](../../virtual-machines/prepay-dedicated-hosts-reserved-instances.md)
- [Yazılım planları](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [Depolama](../../storage/blobs/storage-blob-reserved-capacity.md)
- [SQL Veritabanı](../../sql-database/sql-database-reserved-capacity.md)
- [SQL Veri Ambarı](prepay-sql-data-warehouse-charges.md)
- [Sanal makineler](../../virtual-machines/windows/prepay-reserved-vm-instances.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için Ayırmaları Yönetme](manage-reserved-vm-instance.md)
