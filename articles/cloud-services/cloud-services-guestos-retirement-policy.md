---
title: Azure Konuk İşletim Sistemi için desteklenebilirlik ve emeklilik politikası kılavuzu | Microsoft Dokümanlar
description: Microsoft'un Bulut Hizmetleri tarafından kullanılan Azure Konuk İşletim Sistemi ile ilgili olarak neleri destekleyeceği hakkında bilgi sağlar.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 9/20/2017
ms.author: raiye
ms.openlocfilehash: fbe9b3379799fe3cf0a56d921ab257bc87606ca9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68945443"
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Guest İşletim Sistemi desteklenebilirlik ve emeklilik ilkesi
Bu sayfadaki bilgiler, Bulut Hizmetleri çalışanı ve web rolleri (PaaS) için Azure Konuk işletim sistemi[(Konuk İşletim](cloud-services-guestos-update-matrix.md)Sistemi) ile ilgilidir. Sanal Makineler (IaaS) için geçerli değildir.

Microsoft Guest [OS için](https://support.microsoft.com/gp/azure-cloud-lifecycle-faq)yayınlanmış bir destek ilkesi vardır. Şu anda okuduğunuz sayfa, ilkenin nasıl uygulandığını açıklar.

İlke,

1. Microsoft **Guest OS en az iki aile**destekleyecek. Bir aile emekli olduğunda, müşterilerin resmi emeklilik tarihinden itibaren yeni desteklenen Guest OS ailesine güncellemek için 12 ay var.
2. Microsoft **desteklenen Guest OS ailelerinin en son iki sürümü**destekleyecektir.
3. Microsoft, **Azure SDK'nın en son iki sürümüne**destek verecektir. SDK'nın bir sürümü emekli olduğunda, müşterilerin resmi emeklilik tarihinden itibaren 12 ay daha yeni bir sürüme güncelleştirilecek.

Bazen, ikiden fazla aile veya sürümler desteklenebilir. Resmi Konuk İşletim Sistemi destek bilgileri [Azure Konuk İşletim Sistemi Sürümlerinde ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'nde görünür.

## <a name="when-a-guest-os-version-is-retired"></a>Konuk İşletim Sistemi sürümü kullanımdan kaldırılınca
Yeni Konuk İşletim Sistemi **sürümleri,** en son MSRC güncelleştirmelerini dahil etmek için her ay kullanıma sunulmuştur. Düzenli aylık güncelleştirmeler nedeniyle, Guest OS sürümü normalde yayımlanmasından yaklaşık 60 gün sonra devre dışı bırakılır. Bu etkinlik, her aile için kullanılabilir en az iki Konuk İşletim Sistemi sürümü tutar.

### <a name="process-during-a-guest-os-family-retirement"></a>Misafir İşletim Sistemi aile emekliliği sırasında süreç
Emeklilik duyurulduktan sonra, müşterilerin büyük aile resmen hizmetten kaldırılmadan önce 12 aylık bir "geçiş" dönemi var. Bu geçiş süresi Microsoft'un takdirine bağlı olarak uzatılabilir. Güncelleştirmeler [Azure Konuk İşletim Sistemi Sürümleri nde ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'nde yayınlanacaktır.

Kademeli emeklilik süreci geçiş döneminin altı (6) ayında başlayacak. Bu süre zarfında:

1. Microsoft, müşterilere emekliliği bildirir.
2. Azure SDK'nın yeni sürümü emekli Guest OS ailesini desteklemez.
3. Bulut Hizmetlerinin yeni dağıtımlarına ve yeniden dağıtımlarına emekli ailede izin verilmeyecektir

Microsoft, "son kullanma tarihi" olarak bilinen geçiş döneminin son gününe kadar en son MSRC güncelleştirmelerini içeren yeni Konuk İşletim Sistemi sürümünü sunmaya devam edecektir. Son kullanma tarihinde, halen çalışmaya devam eden Bulut Hizmetleri Azure SLA kapsamında desteklenmeyen olacaktır. Microsoft, bu tarihten sonra bu hizmetleri yükseltmeye, silmeye veya durdurmaya zorlama yetkisine sahiptir.

### <a name="process-during-a-guest-os-version-retirement"></a>Konuk OS Sürümü emeklilik sırasında İşlem
Müşteriler Konuk Işletim Sistemlerini otomatik olarak güncellenecek şekilde ayarlarlarsa, Konuk İşletim Sistemi sürümleriyle başa çıkma konusunda endişelenmelerine gerek kalmaz. Onlar her zaman en son Guest OS sürümünü kullanarak olacaktır.

Konuk İşletim Sistemi Sürümleri her ay yayınlanır. Normal sürüm oranı nedeniyle, her sürümün sabit bir ömrü vardır.

60 gün ömrü nde, bir sürümü "*devre dışı bırakılır*". "Devre dışı" sürümün portaldan kaldırıldığı anlamına gelir. Sürüm artık CSCFG yapılandırma dosyasından ayarlanamaz. Varolan dağıtımlar çalışmaya bırakılır. Ancak varolan dağıtımlara yeni dağıtımlar, kod ve yapılandırma güncelleştirmeleri izin verilmeyecektir.

"Devre dışı bırakıldıktan" sonra Konuk İşletim Sistemi sürümü "süresi doluyor" ve süresi dolan tüm yüklemeler güvenlik ve güvenlik açığı sorunlarına maruz kalır. Genellikle, son kullanma tarihi toplu olarak yapılır, bu nedenle devre dışı kalma süresi sona erme değişebilir.

Hizmetlerini Konuk İşletim Sistemi'ni el ile güncelleştirecek şekilde yapılandıran müşteriler, hizmetlerinin desteklenen bir Konuk İşletim Sistemi'nde çalışmasını sağlamalıdır. Bir hizmet Konuk İşletim Sistemi'ni otomatik olarak güncelleyecek şekilde yapılandırılırsa, temel platform uyumluluğu sağlar ve en son Konuk İşletim Sistemi'ne yükseltilir.

Bu süreler, müşteri geçişlerini kolaylaştırmak için Microsoft'un takdirine bağlı olarak daha uzun süre yapılabilir. Tüm değişiklikler [Azure Konuk İşletim Sistemi Sürümleri ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'nde iletilecektir.

### <a name="notifications-during-retirement"></a>Emeklilik sırasında bildirimler
* **Aile emekliliği** <br>Microsoft blog gönderileri ve portal bildirimi kullanır. Hala emekli bir Konuk İşletim Sistemi ailesini kullanan müşteriler, atanan hizmet yöneticilerine doğrudan iletişim (e-posta, portal mesajları, telefon görüşmesi) yoluyla bildirilir. Tüm değişiklikler [Azure Konuk İşletim Sistemi Sürümleri ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'nde yayınlanacaktır.
* **Sürüm Emeklilik** <br>Tüm değişiklikler ve bunların oluştuğu tarihler, sürüm, devre dışı bırakma ve son kullanma tarihi dahil olmak üzere [Azure Konuk İşletim Sistemi Sürümleri ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'ne nakledilir. Hizmet yöneticileri, engelli bir Konuk İşletim Sistemi sürümünde veya ailesinde çalışan dağıtımları varsa e-posta alır. Bu e-postaların zamanlaması değişebilir. Genellikle devre dışı atmadan en az bir ay önce, bu zamanlama resmi bir SLA olmasa da.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**Göçün etkilerini nasıl azaltabilirim?**

Bulut Hizmetlerinizi tasarlamak için en son Guest OS ailesini kullanmanızı öneririz.

1. Yeni bir aileye geçişinizi erken planlamaya başlayın.
2. Bulut Hizmetinizi yeni aileüzerinde çalıştırmak için geçici test dağıtımları ayarlayın.
3. Konuk İşletim Sistemi sürümünüzü **Otomatik** olarak ayarlayın [(.cscfg](cloud-services-model-and-package.md#cscfg) dosyasında osVersion=* ) böylece yeni Konuk İşletim Sistemi sürümlerine geçiş otomatik olarak gerçekleşir.

**Web uygulamam işletim sistemiyle daha derin bir entegrasyon gerektiriyorsa ne olur?**

Web uygulama mimariniz işletim sisteminin temel özelliklerine bağlıysa, [başlangıç görevleri](cloud-services-startup-tasks.md) veya diğer genişletilebilirlik mekanizmaları gibi platform destekli yetenekleri kullanın. Alternatif olarak, temel işletim sistemini korumakla sorumlu olduğunuz [Azure Sanal Makineleri](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (Hizmet Olarak IaaS – Altyapı) da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
En son [Konuk İşletim Sistemi sürümlerini](cloud-services-guestos-update-matrix.md)gözden geçirin.
