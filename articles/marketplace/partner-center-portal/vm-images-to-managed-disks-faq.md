---
title: Sanal makine (VM) görüntülerini Azure Marketi'nde yönetilen disk depolamasına taşıyoruz
description: Yeni pazar özellikleri ve yetenekleri için daha hızlı, daha güvenilir depolama ve destek sağlamak için, pazar daki VM görüntülerini yönetilen disk depolamaya taşıyoruz.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 17b418ab82cc88b4368efe247b06c0c8bbb27453
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285121"
---
# <a name="were-moving-virtual-machine-vm-images-on-azure-marketplace-to-managed-disk-storage"></a>Azure Marketi'ndeki sanal makine (VM) görüntülerini yönetilen disk depolamaya taşıyoruz

Yeni pazar özellikleri ve yetenekleri için daha hızlı, daha güvenilir depolama ve destek sağlamak için, pazar daki VM görüntülerini yönetilen disk depolamaya taşıyoruz.

2 Ocak 2020'den itibaren VM görüntülerini aşamalı olarak yönetilen disk depolamasına taşıyacağız. İlk aşamada, görüntüleri yalnızca önceki 90 gün içinde yeni dağıtımlar veya çalışan VM'ler olmadan taşıyacağız. Resimlerden herhangi birini taşımadan önce, yayımcıya hangi görüntülerin taşınacağını ve ne zaman taşınacağını bildirmek için bir e-posta göndeririz.

Yayıncıların veya tüketicilerin herhangi bir işlem yapmalarına gerek yoktur ve kullanıcılar bundan etkilenmez. Pazar teklifleri kullanılabilir kalır ve müşteriler hareket sırasında ve sonrasında, bu görüntülerden yönetilen VM'leri dağıtmaya devam edecektir.

Herhangi bir sorunuz varsa, [lütfen bize ulaşın.](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff)

## <a name="faqs"></a>SSS

### <a name="would-the-users-of-my-vm-images-experience-an-outage"></a>VM görüntülerimin kullanıcıları bir kesinti yaşar mı?

VM görüntülerinin kullanıcıları bir kesinti yaşamayacaktır. 

İlk aşamada, yalnızca çalışan VM'leri olmayan VM görüntülerini taşıyacağız. Bu görüntüler için hiçbir kullanıcı olduğundan, herhangi bir etkisi olmayacaktır. Sonraki aşamalar için de, kullanıcılar üzerinde hiçbir etkisi olmayacaktır.

### <a name="how-long-does-it-take-for-the-process-to-complete"></a>İşlemin tamamlanması ne kadar sürer?

Geçişin tamamlanması 24 saat kadar sürebilir.

### <a name="do-i-need-to-take-any-action"></a>Herhangi bir işlem yapmam gerekiyor mu?

Hayır. Yayıncıların veya tüketicilerin herhangi bir işlem yapmalarına gerek yoktur.

### <a name="do-i-have-to-update-my-system-to-call-the-cloud-portal-apis-in-a-different-way-after-they-are-moved-to-managed-disk-storage"></a>Yönetilen Disk depolamasına taşındıktan sonra Bulut Portalı API'lerini farklı bir şekilde aramak için sistemimi güncelleştirmem gerekiyor mu?

Hayır. Varolan API çağrılarınız çalışmaya devam edecektir.

### <a name="would-all-my-vm-images-be-moved-to-managed-disk-at-the-same-time"></a>Tüm VM görüntülerim aynı anda Yönetilen Disk'e taşınır mı?

Tüm VM görüntülerinizi aynı gün içinde taşıyacağız. Taşındıklarında sizi bilgilendireceğiz.

### <a name="can-i-request-to-schedule-the-move-of-my-vm-images-to-a-later-time"></a>VM görüntülerimin daha sonraki bir zamana taşınmasını zamanlamayı isteyebilir miyim?

Görüntüleri planlanan tarihte taşımanızı öneririz. Ancak, endişeleriniz varsa, lütfen taşımayı yeniden planlamamız için bize ulaşın.

### <a name="can-i-publish-updates-to-my-vm-images-during-the-move"></a>Taşıma sırasında VM Görüntülerimdeki güncellemeleri yayımlayabilir miyim?

VM görüntülerinde güncelleştirmeler taşıma sırasında yapılamaz.

### <a name="will-the-publishing-process-change-after-my-vm-image-is-moved-to-managed-disk"></a>VM Görüntüm Yönetilen Disk'e taşındıktan sonra yayımlama işlemi değişecek mi?

Hayır, yayımlama işlemi aynı kalacaktır. 

### <a name="can-the-publisher-move-their-offers-to-managed-disk"></a>Yayımcı tekliflerini Yönetilen Disk'e taşıyabilir mi?

Hayır, yayıncılar tekliflerini Yönetilen Disk'e taşıyamaz. Beklemek zorunda kalacaklar ve görüntüleri otomatik olarak taşınacak. Herhangi bir değişiklik yapmadan önce yayımcıya bildirimler göndeririz.
