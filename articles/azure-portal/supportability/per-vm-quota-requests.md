---
title: Azure VM serisi başına vCPU kota limitlerinde artış isteğinde bulunun
description: Azure portalındaki bir VM serisi için vCPU kota sınırında artış isteğinde bulunma, toplam bölgesel vCPU sınırını aynı miktarda artırır.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: de06375dad5999a29691435317e62585a2ea7f64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843774"
---
# <a name="standard-quota-increase-limits-by-vm-series"></a>Standart kota: VM serisine göre limitleri artırın

Azure Kaynak Yöneticisi, sanal makineler için iki tür vCPU kotası destekler:

* *Gittiğiniz kadar öde VM'leri* ve *ayrılmış VM örnekleri* *standart vCPU kotasına*tabidir.
* *Spot VM'ler* *spot vCPU kotasına*tabidir.

Gittikçe öde ve ayrılmış sanal makine örnekleri için standart vCPU kotası, her bölgedeki her abonelik için iki katmanda uygulanır:

* Birinci katman, tüm VM serilerinde *toplam bölgesel vCPUs sınırıdır.*
* İkinci katman, Dv3 *serisi vCPUs'lar gibi VM serisi vCPUs başına sınırıdır.*

Yeni bir spot VM dağıttığınızda, söz konusu VM serisi için toplam yeni ve varolan vCPU kullanımı, söz konusu VM serisi için onaylanan VCPU kotasını aşmamalıdır. Ayrıca, tüm VM serilerinde dağıtılan toplam yeni ve varolan vCPUs sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez.

Azure portalını kullanarak VM serisi için vCPU kota sınırında artış talep edebilirsiniz. VM serisi kotasındaki artış, toplam bölgesel vCPU sınırını otomatik olarak aynı miktarda artırır.

Standart vCPU kotaları hakkında daha fazla bilgi edinmek için [Sanal makine vCPU kotaları](../../virtual-machines/windows/quotas.md) ve [Azure abonelik ve hizmet limitlerine](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests)bakın.

Standart kota için vCPU sınırını bölgeye göre artırma hakkında bilgi edinmek için, [Bkz. Standart kota: Bölgelere göre sınırları artırın.](regional-quota-requests.md)

Spot VM vCPU limitlerini artırma hakkında daha fazla bilgi edinmek [için Bkz. Spot kotası: Tüm VM serileri için limitleri artırın.](low-priority-quota.md)

Aşağıdaki bölümlerde açıklandığı gibi, VM serisi başına standart vCPU kota limitlerinde iki şekilde de artış talep edebilirsiniz.

## <a name="request-a-standard-quota-increase-from-help--support"></a>Yardım + destekten standart kota artışı isteğinde bulunun

**Yardım + destek**vm serisi başına standart bir vCPU kota artışı istemek için:

> [!NOTE]
> Ayrıca, tek bir destek örneği aracılığıyla birden çok bölge için kota sınırı artışı da isteyebilirsiniz. Ayrıntılar için 8.

1. Azure [portalı](https://portal.azure.com) menüsünde **Yardım + destek'i**seçin.

   ![Yardım + destek bağlantısı](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. **Yardım + destek'** te Yeni **destek isteğini**seçin.

    ![Yeni bir destek isteği oluşturma](./media/resource-manager-core-quotas-request/new-support-request.png)

1. **Sorun türü için**Hizmet ve abonelik **limitlerini (kotalar)** seçin.

   ![Sorun türü seçin](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. **Abonelik**için, kotasını artırmak istediğiniz aboneliği seçin.

   ![Artırılmış kota için abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. **Kota türü için,** **Bilgi İşlem (cores-vCPUs) abonelik sınırı artışlarını**seçin.

   ![Kota türü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. **Sonraki'ni seçin:** SORUN **AYRINTILARINI**açmak için çözümler. Ek bilgi girmek için **ayrıntıları sağla'yı** seçin.

   !["Ayrıntıları ver" bağlantısı](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. Kota **ayrıntılarında,** aşağıdaki adımları yapın:

   ![TEk kota ayrıntıları sağlayın](./media/resource-manager-core-quotas-request/quota-details-deployment-rm-locations.png)

   1. **Dağıtım modeli**için uygun modeli seçin.

   1. **Konumlar**için bir konum seçin. Seçili konum için, **Türler**altında , **Bir tür**seç'te, **Standart'ı**seçin.

      ![Kota detayları - kota türleri](./media/resource-manager-core-quotas-request/quota-details-select-standard-type.png)

      **Türler**altında, çoklu seçim desteği aracılığıyla tek bir destek örneğinden hem standart hem de nokta kota türleri isteyebilirsiniz.

      Spot kota sınırlarını artırma hakkında daha fazla bilgi için [sanal makine ölçek kümeleri için Azure spot VM'lerine](../../virtual-machine-scale-sets/use-spot.md)bakın.

   1. **Standart'ta,** artan kotalar için SKU serisini seçin.

      ![Kota detayları - SKU serisi](./media/resource-manager-core-quotas-request/quota-details-standard-select-series.png)

   1. Bu abonelik için istediğiniz yeni kota sınırlarını girin. Listenizdeki Bir SKU'yu kaldırmak için, SKU'nun yanındaki onay kutusunu temizleyin veya "X" simgesini atın'ı seçin.

      ![Yeni bir vCPU Sınırı seçin](./media/resource-manager-core-quotas-request/quota-details-standard-set-vcpu-limit.png)

1. Birden fazla konum için kota artışı istemek için **Konumlar'da**ek bir konum seçin ve ardından uygun bir VM türünü seçin. Daha sonra ek konuma uygulanan bir sınır girebilirsiniz.

   ![Kota ayrıntılarında ek konumlar belirtin](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. **Kaydet'i** seçin ve destek isteğini oluşturmaya devam edin.

## <a name="request-a-standard-quota-increase-from-subscriptions"></a>Aboneliklerden standart kota artışı isteği

**Aboneliklerden**VM serisi başına standart vCPU kota artışı istemek için:

> [!NOTE]
> Ayrıca, tek bir destek örneği aracılığıyla birden çok bölge için kota sınırı artışı da isteyebilirsiniz. Ayrıntılar için, adım 7'ye bakın.

1. Azure [portalında](https://portal.azure.com) **Abonelikleri**arayın ve seçin.

   ![Azure portal aramasındaki abonelikler](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   ![Değişiklikler için seçilecek abonelikler](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. Sol bölmede Kullanım **+ kotaları'nı**seçin.

   !["Kullanım + kotalar" bağlantısı](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. Sağ üstte, **Artış İste'yi**seçin.

   ![Kotayı artırmak için seçin](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. **Kota türü için,** **Bilgi İşlem (cores-vCPUs) abonelik sınırı artışlarını**seçin.

   ![Kota türü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. Kota **ayrıntılarında,** aşağıdaki adımları yapın:

   1. **Dağıtım modeli**için uygun modeli seçin ve **Konumlar**için bir konum seçin.

      ![Kota ayrıntılarını sağlama](./media/resource-manager-core-quotas-request/quota-details-deployment-rm-locations.png)

   1. Seçili konum için, **Türler**altında, **bir tür seçin**ve ardından **Standart'ı**seçin.

      ![Standart türünü seçin](./media/resource-manager-core-quotas-request/quota-details-select-standard-type.png)

      **Türler**altında, çoklu seçim desteği aracılığıyla tek bir destek örneğinden hem standart hem de nokta kota türleri isteyebilirsiniz.

      Spot kota sınırlarını artırma hakkında daha fazla bilgi için [sanal makine ölçek kümeleri için Azure spot VM'lerine](../../virtual-machine-scale-sets/use-spot.md)bakın.

   1. **Standart**için, kotalarını artırmak istediğiniz SKU serisini seçin.

      ![Kota detayları - SKU serisi](./media/resource-manager-core-quotas-request/quota-details-standard-select-series.png)

   1. Bu abonelik için istediğiniz yeni kota sınırlarını girin. Listenizdeki bir SKU'yu kaldırmak için SKU'nun yanındaki onay kutusunu seçin veya "X" simgesini atın'ı seçin.

      ![Yeni bir vCPU sınırı seçin](./media/resource-manager-core-quotas-request/quota-details-standard-set-vcpu-limit.png)

1. Birden fazla konum için kota artışı istemek için **Konumlar'da**ek bir konum seçin ve ardından uygun bir VM türünü seçin.

   Bu adım, önceki konumlar için seçtiğiniz SKU serisini önceden yükler. Ek seriye uygulamak istediğiniz kota sınırlarını girin.

   ![Kota ayrıntılarında ek konumlar seçin](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. **Kaydet'i** seçin ve destek isteğini oluşturmaya devam edin.
