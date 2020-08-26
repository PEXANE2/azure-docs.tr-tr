---
title: Azure rezervasyonu indirimini uygulama
description: Bu makale ayrılmış örnek indirimlerinin genel olarak nasıl uygulandığını anlamanıza yardımcı olur.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 03/22/2020
ms.author: banders
ms.openlocfilehash: 1c038e896a9f9fc65c3b2201b3e763e966285c56
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684686"
---
# <a name="how-a-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Bu makale ayrılmış örnek indirimlerinin genel olarak nasıl uygulandığını anlamanıza yardımcı olur. Rezervasyon indirimi, rezervasyonu satın alırken seçtiğiniz özniteliklerle eşleşen kaynak kullanımına uygulanır. Öznitelikler, eşleşen VM'lerin, SQL veritabanlarının, Azure Cosmos DB veya diğer kaynakların çalıştırıldığı kapsamı içerir. Örneğin, Batı ABD bölgesinde dört Standart D2 sanal makinesi için bir rezervasyon indirimi istiyorsanız, VM'lerin çalıştığı aboneliği seçin.

Rezervasyon indirimi “*kullanılmadığı takdirde hakkı kaybedilen*” bir özelliktir. Herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

Örneğin, daha sonra bir kaynak oluşturabilirsiniz ve az kullanılan bir eşleşen rezervasyonunuz olabilir. Rezervasyon indirimi yeni eşleşen kaynağa otomatik olarak uygulanır.

Sanal makineler, kaydınız/hesabınız içinde farklı aboneliklerde çalışıyorsa, kapsamı paylaşılan olarak seçin. Paylaşılan kapsam, rezervasyon indiriminin abonelikler arasında uygulanmasını sağlar. Bir rezervasyonu satın aldıktan sonra kapsamı değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örneklerini yönetme](manage-reserved-vm-instance.md).

Rezervasyon indirimi yalnızca, Kurumsal, Microsoft Müşteri Sözleşmesi, CSP veya kullandıkça öde tarifesine sahip aboneliklerle ilişkili kaynaklar için geçerlidir. Başka teklif türlerinin kullanıldığı bir abonelikte çalıştırılan kaynaklar rezervasyon indirimi almaz.

## <a name="when-the-reservation-term-expires"></a>Rezervasyon dönemi sona erdiğinde

Rezervasyon döneminin sonunda fatura indiriminin süresi dolar ve kaynaklar kullandıkça öde fiyatıyla faturalanır. Varsayılan olarak rezervasyonlar otomatik olarak yenilenecek şekilde ayarlanmaz. Yenileme ayarlarında ilgili seçeneği belirterek rezervasyonda otomatik yenilemeyi etkinleştirmeyi seçebilirsiniz. Otomatik yenilemeyle, mevcut rezervasyonun süresi dolduğunda onun yerine bir rezervasyon satın alınır. Varsayılan olarak yeni satın alınan rezervasyonun öznitelikleri süresi dolan rezervasyonla aynı olur; isterseniz yenileme ayarlarında faturalama sıklığını, süreyi veya miktarı değiştirebilirsiniz. Rezervasyon üzerinde ve faturalama için kullanılan abonelik üzerinde sahip erişimi olan tüm kullanıcılar yenilemeyi ayarlayabilir.  

## <a name="discount-applies-to-different-sizes"></a>Farklı boyutlar için indirim uygulanır

Bir rezervasyon satın aldığınızda, indirim aynı boyut grubundaki özniteliklere sahip diğer örneklere uygulanabilir. Bu özellik örnek boyutu esnekliği olarak bilinir. İndirim kapsamının esnekliği, rezervasyon türüne ve rezervasyonu satın alırken seçtiğiniz özniteliklere göre değişir.

Hizmet planları:

- Ayrılmış VM Örnekleri: Rezervasyonu satın alırken **Örnek boyutu esnekliği için iyileştirilmiş**'i seçerseniz, indirim kapsamı seçtiğiniz VM boyutuna bağlı olur. Rezervasyon aynı boyut serisi grubundaki sanal makine (VM) boyutlarına uygulanır. Daha fazla bilgi için bkz. [Ayrılmış VM Örnekleriyle sanal makine boyutu esnekliği](../../virtual-machines/reserved-vm-instance-size-flexibility.md).
- Azure Depolama ayrılmış kapasitesi: Standart Azure Depolama hesapları için aylık 100 TiB veya 1 PiB boyutlu birimler halinde ayrılmış kapasite satın alabilirsiniz. Azure Depolama ayrılmış kapasitesini destekleyen bölgeler hakkında bilgi için bkz. [Blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/). Azure Depolama ayrılmış kapasitesi tüm erişim katmanları (sık erişimli, seyrek erişimli ve arşiv) ve tüm çoğaltma yapılandırmaları (LRS, GRS veya ZRS) ile kullanılabilir.
- SQL Veritabanı ayrılmış kapasitesi: İndirim kapsamı, seçtiğiniz performans katmanına bağlıdır. Daha fazla bilgi için bkz. [Azure rezervasyonu indiriminin nasıl uygulandığını anlama](understand-reservation-charges.md).
- Azure Cosmos DB ayrılmış kapasitesi: İndirim kapsamı, sağlanan aktarım hızına göre değişir. Daha fazla bilgi için bkz. [Azure Cosmos DB rezervasyon indiriminin nasıl uygulandığını anlama](understand-cosmosdb-reservation-charges.md).

## <a name="how-discounts-apply-to-specific-azure-services"></a>İndirimleri belirli Azure hizmetlerine uygulama

İndirimlerin belirli bir Azure hizmetine nasıl uygulandığını öğrenmek için aşağıdaki makalelerden size uygun olanı okuyun:

- [App Service](reservation-discount-app-service-isolated-stamp.md)
- [Redis için Azure Önbelleği](understand-azure-cache-for-redis-reservation-charges.md)
- [Cosmos DB](understand-cosmosdb-reservation-charges.md)
- [MariaDB için Veritabanı](understand-reservation-charges-mariadb.md)
- [MySQL için Veritabanı](understand-reservation-charges-mysql.md)
- [PostgreSQL için Veritabanı](understand-reservation-charges-postgresql.md)
- [Databricks](reservation-discount-databricks.md)
- [Veri Gezgini](understand-azure-data-explorer-reservation-charges.md)
- [Ayrılmış Konaklar](billing-understand-dedicated-hosts-reservation-charges.md)
- [Disk Depolama](understand-disk-reservations.md)
- [Red Hat Linux Enterprise](understand-rhel-reservation-charges.md)
- [Yazılım planları](understand-suse-reservation-charges.md)
- [Depolama](understand-storage-charges.md)
- [SQL Veritabanı](understand-reservation-charges.md)
- [Azure Synapse Analytics](reservation-discount-azure-sql-dw.md)
- [Sanal makineler](../manage/understand-vm-reservation-charges.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
- [Kullandıkça öde fiyatlarına tabi aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
- [Rezervasyonlara dahil olmayan Windows yazılımı maliyetleri](reserved-instance-windows-software-costs.md)