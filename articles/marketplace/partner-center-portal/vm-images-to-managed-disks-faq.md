---
title: Sanal makine (VM) görüntülerini Azure Marketi 'nde yönetilen disk depolama alanına taşııyoruz
description: Yeni Market özellikleri ve özellikleri için daha hızlı, daha güvenilir depolama ve destek sağlamak üzere Market VM görüntülerini yönetilen disk depolama alanına taşıyoruz.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 17b418ab82cc88b4368efe247b06c0c8bbb27453
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80285121"
---
# <a name="were-moving-virtual-machine-vm-images-on-azure-marketplace-to-managed-disk-storage"></a>Azure Marketi 'ndeki sanal makine (VM) görüntülerini yönetilen disk depolama alanına taşııyoruz

Yeni Market özellikleri ve özellikleri için daha hızlı, daha güvenilir depolama ve destek sağlamak üzere Market VM görüntülerini yönetilen disk depolama alanına taşıyoruz.

2 Ocak 2020 tarihinden itibaren, VM görüntülerini aşamalar halinde yönetilen disk depolama alanına taşıyacağız. İlk aşamada, yalnızca Yeni dağıtımlar olmadan veya önceki 90 gün içinde VM 'Leri çalıştıran resimleri taşıyacağız. Görüntüleri taşımadan önce, yayımcının hangi görüntülerin taşınacağını ve ne zaman taşınacağını bilmesini sağlamak için bir e-posta göndereceğiz.

Yayımcılar veya tüketiciler hiçbir işlem yapması gerekmez ve kullanıcılar etkilenmez. Market teklifleri kullanılabilir olmaya devam eder ve müşteriler, taşıma sırasında ve sonrasında bu görüntülerden yönetilen VM 'Leri dağıtabilecektir.

Sorularınız varsa lütfen [bizimle iletişime geçin](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff).

## <a name="faqs"></a>SSS

### <a name="would-the-users-of-my-vm-images-experience-an-outage"></a>VM görüntülerimin kullanıcıları kesinti yaşar mı?

VM görüntülerinin kullanıcıları kesinti yaşamacaktır. 

İlk aşamada yalnızca çalışan VM 'Leri olmayan VM görüntülerini taşıyacağız. Bu görüntüler için Kullanıcı olmadığı için herhangi bir etki olmayacaktır. Sonraki aşamalar için de kullanıcılar üzerinde hiçbir etkisi olmaz.

### <a name="how-long-does-it-take-for-the-process-to-complete"></a>İşlemin tamamlanması ne kadar sürer?

Geçişin tamamlanması 24 saate kadar sürebilir.

### <a name="do-i-need-to-take-any-action"></a>Herhangi bir eylemde olması gerekiyor mu?

Hayır. Yayımcılar veya tüketiciler için herhangi bir işlem yapması gerekmez.

### <a name="do-i-have-to-update-my-system-to-call-the-cloud-portal-apis-in-a-different-way-after-they-are-moved-to-managed-disk-storage"></a>Bulut portalı API 'Lerini, yönetilen disk depolama alanına taşındıktan sonra farklı bir şekilde çağırmak için sistemmi güncelleştirmem gerekir mi?

Hayır. Mevcut API çağrılarınız çalışmaya devam edecektir.

### <a name="would-all-my-vm-images-be-moved-to-managed-disk-at-the-same-time"></a>Tüm sanal makine görüntülerimin aynı anda yönetilen diske taşınmasını ister misiniz?

Tüm VM görüntülerinizi aynı güne taşıcağız. Taşındıktan sonra sizi bilgilendireceğiz.

### <a name="can-i-request-to-schedule-the-move-of-my-vm-images-to-a-later-time"></a>VM görüntülerimin daha sonraki bir zamanda taşınmasını isteyebilir miyim?

Görüntülerin zamanlanan tarihte taşınmasını öneririz. Ancak endişeleriniz varsa, lütfen taşımayı yeniden zamanlamak için bize ulaşın.

### <a name="can-i-publish-updates-to-my-vm-images-during-the-move"></a>Taşıma sırasında VM görüntülerimin güncelleştirmelerini yayımlayabilir miyim?

Taşıma sırasında VM görüntülerine yönelik güncelleştirmeler yapılamaz.

### <a name="will-the-publishing-process-change-after-my-vm-image-is-moved-to-managed-disk"></a>VM görüntümi yönetilen diske taşındıktan sonra yayımlama işlemi değişsin mi?

Hayır, yayımlama işlemi aynı kalacaktır. 

### <a name="can-the-publisher-move-their-offers-to-managed-disk"></a>Yayımcı teklifleri yönetilen diske taşıyabilir mi?

Hayır, yayımcılar teklifleri yönetilen diske taşıyamıyor. Bunların beklemesi gerekecektir ve görüntüleri otomatik olarak taşınır. Herhangi bir değişiklik yapmadan önce yayımcıya bildirim göndereceğiz.
