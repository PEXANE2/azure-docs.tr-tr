---
title: Azure bölgesel vCPU kota limitlerinde artış isteğinde bulunun
description: Azure portalındaki bir bölge için vCPU kota sınırında artış isteği nasıl alınır?
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9236989f7bbb4695db24f86a2a049f5bb58f302a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843693"
---
# <a name="standard-quota-increase-limits-by-region"></a>Standart kota: Bölgelere göre limitleri artırın

Azure Kaynak Yöneticisi, sanal makineler için iki tür vCPU kotası destekler:

* *Gittiğiniz kadar öde VM'leri* ve *ayrılmış VM örnekleri* *standart vCPU kotasına*tabidir.
* *Spot VM'ler* *spot vCPU kotasına*tabidir.

Gittikçe öde ve ayrılmış sanal makine örnekleri için standart vCPU kotası, her bölgedeki her abonelik için iki katmanda uygulanır:

* Birinci katman, tüm VM serilerinde *toplam bölgesel vCPUs sınırıdır.*
* İkinci katman, D *serisi vCPUs'lar gibi VM serisi vCPUs başına sınırıdır.*

Yeni bir spot VM dağıttığınızda, söz konusu VM serisi için toplam yeni ve varolan vCPU kullanımı, söz konusu VM serisi için onaylanan VCPU kotasını aşmamalıdır. Ayrıca, tüm VM serilerinde dağıtılan toplam yeni ve varolan vCPUs sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez.

Azure portalını kullanarak VM serisi için vCPU kota sınırında artış talep edebilirsiniz. VM serisi kotasındaki artış, toplam bölgesel vCPU sınırını otomatik olarak aynı miktarda artırır.

Yeni bir abonelik oluşturduğunuzda, varsayılan toplam bölgesel vCPUs sayısı, tüm tek tek VM serisi için varsayılan vCPU kotasına eşit olmayabilir. Bu tutarsızlık, dağıtmak istediğiniz her VM serisi için yeterli kotaya sahip bir abonelikle sonuçlanabilir. Ancak, tüm dağıtımlar için toplam bölgesel vCPUs'ları barındıracak yeterli kota olmayabilir. Bu durumda, bölgesel vCPUs toplam sayısının sınırını açıkça artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vCPU sınırı, bölge için tüm VM serilerinde onaylanan toplam kotayı aşamaz.

Standart vCPU kotaları hakkında daha fazla bilgi edinmek için [Sanal makine vCPU kotaları](../../virtual-machines/windows/quotas.md) ve [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md)bölümüne bakın.

Spot VM vCPU limitlerini artırma hakkında daha fazla bilgi edinmek [için Bkz. Spot kotası: Tüm VM serileri için limitleri artırın.](low-priority-quota.md)

VCPU standart kota sınırınızda bölgeye göre iki şekilde bir artış talep edebilirsiniz.

## <a name="request-a-quota-increase-by-region-from-help--support"></a>Yardım + destek'ten bölgeye göre kota artışı isteğinde bulunun

**Yardım + destekten**bölgeye göre vCPU kotası artışı istemek için:

1. Azure [portalı](https://portal.azure.com) menüsünden **Yardım + destek'i**seçin.

   !["Yardım + destek" bağlantısı](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. **Yardım + destek'** te Yeni **destek isteğini**seçin.

    ![Yeni destek isteği](./media/resource-manager-core-quotas-request/new-support-request.png)

1. **Sorun türü için**Hizmet ve abonelik **limitlerini (kotalar)** seçin.

   ![Sorun türü seçin](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. **Abonelik**için, kotasını artırmak istediğiniz aboneliği seçin.

   ![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. Kota **türü için** **Diğer İstekler'i**seçin.

   ![Kota türü seçin](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. **Sonraki'ni seçin:** SORUN **AYRINTILARINI**açmak için çözümler. **Açıklama'da,** aşağıdaki bilgileri sağlayın:

    1. **Dağıtım Modeli**için **Kaynak Yöneticisi'ni**belirtin.  
    1. **Bölge**için, örneğin, **Doğu ABD 2,** gerekli bölgebelirtin.  
    1. **Yeni Sınır**için, bölge için yeni bir vCPU sınırı belirtin. Bu değer, bu abonelik için tek tek SKU serisi için onaylanan kotaların toplamını aşmamalıdır.

    ![Kota isteği için ayrıntıları girin](./media/resource-manager-core-quotas-request/regional-details.png)

1. Destek isteğini oluşturmaya devam etmek için **Gözden Geçir + oluştur'u** seçin.

## <a name="request-a-quota-increase-by-region-from-subscriptions"></a>Aboneliklerden bölgeye göre kota artışı isteği

**Aboneliklerden**bölgeye göre vCPU kotası artışı istemek için:

1. Azure [portalında](https://portal.azure.com) **Abonelikleri**arayın ve seçin.

   ![Azure portalındaki Aboneliklere gitme](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   ![Değiştirmek için abonelik seçin](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. Sol bölmede Kullanım **+ kotaları'nı**seçin.

   ![Kullanım ve kotabağlantısını takip et](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. Sağ üstte, **Artış İste'yi**seçin.

   ![Kotayı artırmak için seçin](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. **Kota türünden,** **Diğer İstekler'i**seçin.

   ![Kota türünü seçin](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. **Sonraki'ni seçin:** SORUN **AYRINTILARINI**açmak için çözümler. **Açıklama** kutusunda, aşağıdaki ek bilgileri sağlayın:

    1. **Dağıtım Modeli**için **Kaynak Yöneticisi'ni**belirtin.  
    1. **Bölge**için, örneğin, **Doğu ABD 2,** gerekli bölgebelirtin.  
    1. **Yeni Sınır**için, bölge için yeni bir vCPU sınırı belirtin. Bu değer, bu abonelik için tek tek SKU serisi için onaylanan kotaların toplamını aşmamalıdır.

    ![Bilgileri ayrıntılara girin](./media/resource-manager-core-quotas-request/regional-details.png)

1. Destek isteğini oluşturmaya devam etmek için **Gözden Geçir + oluştur'u** seçin.
