---
title: Uygulama ve hizmet kullanılabilirliği sorunları SSS
titleSuffix: Azure Cloud Services
description: Bu makalede, Microsoft Azure Bulut Hizmetleri için uygulama ve hizmet kullanılabilirliği hakkında sık sorulan sorular listelanmaktadır.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c294d4583ba2690e1f4952441ffb43bff1459059
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386927"
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure Bulut Hizmetleri için uygulama ve hizmet kullanılabilirliği sorunları: Sık sorulan sorular (SSS)

Bu makalede, [Microsoft Azure Bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services)için uygulama ve hizmet kullanılabilirliği sorunları hakkında sık sorulan sorular yer almaktadır. Boyut bilgileri için [Bulut Hizmetleri VM Boyut sayfasına](cloud-services-sizes-specs.md) da başvurabilirsiniz.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Rolüm geri dönüştürüldü. Bulut hizmetim için herhangi bir güncelleme yapıldı mı?
Kabaca ayda bir kez, Microsoft Windows Azure PaaS VM'ler için yeni bir Konuk İşletim Sürümü yayımlar.Guest OS, ardışık hatlar üzerinde bu tür bir güncelleştirmeden yalnızca biridir. Bir sürüm diğer birçok faktörden etkilenebilir. Buna ek olarak, Azure yüz binlerce makinede çalışır. Bu nedenle, rollerinizin yeniden başlatılacak tarih ve saati tam olarak tahmin etmek mümkün değildir. Konuk İşletim Sistemi Güncelleme RSS Feed'i sahip olduğumuz en son bilgilerle güncelliyoruz, ancak bildirilen sürenin yaklaşık bir değer olduğunu göz önünde bulundurmalısınız. Bunun müşteriler için sorunlu olduğunun farkındayız ve yeniden başlatmayı sınırlamak veya tam olarak zaman alabilmek için bir plan üzerinde çalışıyoruz.

Son Konuk İşletim Sistemi güncelleştirmeleri hakkında ayrıntılı bilgi için [Azure Konuk İşletim Sistemi sürümleri ve SDK uyumluluk matrisine](cloud-services-guestos-update-matrix.md)bakın.

Konuk ve Ana Bilgisayar işletim sistemi güncelleştirmelerinin teknik ayrıntılarına ilişkin yeniden başlatmalar ve işaretçiler hakkında yararlı bilgiler için, [İşletim Sistemi Yükseltmeleri nedeniyle](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)MSDN blog gönderisi Rol Örneği Yeniden Başlatılanın bakın.

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Hizmet bir süre boşta kaldıktan sonra bulut hizmetime ilk istek neden normalden daha uzun sürüyor?
Web Sunucusu ilk isteği aldığında, önce kodu yeniden derler ve sonra isteği işler. Bu yüzden ilk istek diğerlerinden daha uzun sürüyor. Varsayılan olarak, kullanıcı nın etkinlik dışı olduğu durumlarda uygulama havuzu kapatılır. Uygulama havuzu da varsayılan olarak her 1.740 dakikada (29 saat) geri dönüşüm sağlar.

Internet Information Services (IIS) uygulama havuzları, uygulama çökmelerine, askıda kalmalara veya bellek sızıntılarına yol açabilecek kararsız durumları önlemek için düzenli olarak geri dönüştürülebilir.

Aşağıdaki belgeler, bu sorunu anlamanıza ve azaltmanıza yardımcı olur:
* [IIS için yavaş ilk yükü niçin sabitleme](https://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [IIS 7.5 web uygulaması uygulama havuzu çok yavaş geri dönüşüm sonra ilk istek](https://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

IIS'nin varsayılan davranışını değiştirmek istiyorsanız, başlangıç görevlerini kullanmanız gerekir, çünkü Web Rolü örneklerine değişiklikleri el ile uygularsanız, değişiklikler sonunda kaybolur.

Daha fazla bilgi için, [bir bulut hizmeti için başlangıç görevlerini nasıl yapılandırıp çalıştırılabilirsiniz.](cloud-services-startup-tasks.md)
