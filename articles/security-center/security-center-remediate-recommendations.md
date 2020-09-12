---
title: Azure Güvenlik Merkezi 'ndeki önerileri düzeltin | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'ndeki kaynaklarınızın korunması ve güvenlik ilkeleriyle uyumlu olması için önerilerin nasıl düzeltileceği açıklanır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: memildin
ms.openlocfilehash: 4bad3227e08c0fbe0d280967e45bbef9d477e1b3
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569144"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde düzeltme önerileri

Öneriler, kaynaklarınızın daha iyi güvenliğini sağlamaya yönelik öneriler sağlar. Öneri bölümünde belirtilen düzeltme adımlarını izleyerek bir öneri uygulıyordu.

## <a name="remediation-steps"></a>Düzeltme adımları <a name="remediation-steps"></a>

Tüm önerileri inceledikten sonra, hangisi önce düzeltileceğine karar verin. Öncelikle ne yapacaklarınızı önceliklendirmeye yardımcı olması için [güvenli puan etkisini](security-center-recommendations.md#monitor-recommendations) kullanmanızı öneririz.

1. Listeden öneri ' e tıklayın.

1. **Düzeltme adımları** bölümündeki yönergeleri izleyin. Her öneri kendi yönerge kümesine sahiptir. Aşağıdaki ekran görüntüsünde, uygulamaları yalnızca HTTPS üzerinden trafiğe izin verecek şekilde yapılandırmaya yönelik düzeltme adımları gösterilmektedir.

    ![Öneri ayrıntıları](./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png)

1. Tamamlandıktan sonra, düzeltme başarılı olursa bildirim görüntülenir.

## <a name="quick-fix-remediation"></a>Hızlı düzeltme düzeltme<a name="one-click"></a>

Hızlı düzeltme, birden fazla kaynak üzerinde bir öneriyi hızlıca düzeltmenizi sağlar. Yalnızca belirli öneriler için kullanılabilir. Hızlı düzeltme, düzeltmeyi basitleştirir ve ortamınızın güvenliğini iyileştirmek için güvenli puanınızı hızla artırmanıza olanak sağlar.

Hızlı düzeltme düzeltmesini uygulamak için:

1. **Hızlı düzelme** sahip öneriler listesinden! etiket ' e tıklayın.

    [![Hızlı düzelme 'yi seçin!](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png)](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png#lightbox)

1. **Sağlıksız kaynaklar** sekmesinden, öneriyi uygulamak istediğiniz kaynakları seçin ve **Düzelt**' e tıklayın.

    > [!NOTE]
    > Listelenen bazı kaynaklar devre dışı bırakılabilir, çünkü bunları değiştirmek için uygun izinlere sahip değilsiniz.

1. Onay kutusunda düzeltme ayrıntılarını ve etkilerini okuyun.

    ![Hızlı çözüm](./media/security-center-remediate-recommendations/security-center-one-click-fix-view.png)

    > [!NOTE]
    > Bu etkiler, **Düzelt**' i tıkladıktan sonra açılan **kaynakları Düzelt** penceresindeki gri kutusunda listelenir. Hızlı düzeltme düzeltmesine devam eden değişikliklerin ne olduğunu listeler.

1. Gerekirse ilgili parametreleri ekleyin ve düzeltmeyi onaylayın.

    > [!NOTE]
    > **Sağlıklı kaynaklar** sekmesindeki kaynakları görmek için düzeltme tamamlandıktan sonra işlem birkaç dakika sürebilir. Düzeltme eylemlerini görüntülemek için [etkinlik günlüğünü](#activity-log)kontrol edin.

1. Tamamlandıktan sonra, düzeltme başarılı olursa bildirim görüntülenir.

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>Etkinlik günlüğünde düzeltme günlüğü hızlı düzeltme <a name="activity-log"></a>

Düzeltme işlemi, kaynak üzerinde yapılandırmayı uygulamak için bir şablon dağıtımı veya REST PATCH API çağrısı kullanır. Bu işlemler [Azure etkinlik günlüğünde](../azure-resource-manager/management/view-activity-logs.md)günlüğe kaydedilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, güvenlik merkezi 'nde önerilerin nasıl düzeltileceği gösterildi. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md) -Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
