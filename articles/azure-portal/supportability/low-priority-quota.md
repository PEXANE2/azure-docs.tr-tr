---
title: Spot sanal makine kotası - Azure
description: Azure'un Gerektiğinde VM'leri kaldırmasına izin verme karşılığında daha düşük maliyetler üstlenmenize olanak tanıyan bir Azure kullanımı modeli sağlayan nokta VM'ler için kota sınırlarını artırın.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 3e6dddfc458f390f5648e23a8a2af8d714de4bf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842825"
---
# <a name="spot-quota-increase-limits-for-all-vm-series"></a>Spot kota: Tüm VM serileri için limitleri artırın

Spot sanal makineler (VM'ler), Azure kullanımının farklı bir modelini sağlar. Azure'un, istediğiniz kadar öde veya ayrılmış VM örnek dağıtımları için gerektiğinde sanal makineleri kaldırmasına izin verme karşılığında daha düşük maliyetler üstlenmenize izin verirler. Spot Sanal Ayarlar hakkında daha fazla bilgi için [sanal makine ölçek kümeleri için Azure spot VM'leri'ne](../../virtual-machine-scale-sets/use-spot.md)bakın.

Azure Kaynak Yöneticisi, sanal makineler için iki tür vCPU kotası destekler:

* *Gittiğiniz kadar öde VM'leri* ve *ayrılmış VM örnekleri* *standart vCPU kotasına*tabidir.
* *Spot VM'ler* *spot vCPU kotasına*tabidir.

Spot vCPU kota türü için, Kaynak Yöneticisi vCPU kotaları tek bir bölgesel sınır olarak kullanılabilir tüm sanal makine serileri arasında uygulanır.

Yeni bir spot VM dağıttığınızda, tüm spot VM örnekleri için toplam yeni ve varolan vCPU kullanımı onaylanan spot vCPU kota sınırını aşmamalıdır. Nokta kotası aşılırsa, nokta VM dağıtımına izin verilmez.

Bu makalede, Azure portalını kullanarak spot vCPU kota sınırında nasıl artış istenecekleri açıklanmaktadır.

Standart vCPU kotaları hakkında daha fazla bilgi edinmek için [Sanal makine vCPU kotaları](../../virtual-machines/windows/quotas.md) ve [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md)bölümüne bakın.

VCPU sınırını bölgelere göre artırma hakkında bilgi edinmek için Standart [kotaya bakın: Bölgelere göre sınırları artırın.](regional-quota-requests.md)

## <a name="request-a-quota-limit-increase-from-help--support"></a>Yardım + destekten kota sınırı artışı isteği

**Yardım + destek**kullanarak tüm sanal makine serisi için spot kota limiti artışı istemek için:

> [!NOTE]
> Ayrıca, tek bir destek örneği aracılığıyla birden çok bölge için kota sınırı artışı da isteyebilirsiniz. Ayrıntılar için 8.

1. Azure [portalı](https://portal.azure.com) menüsünden **Yardım + destek'i**seçin.

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

1. **Kota ayrıntılarında,** aşağıdaki adımları yapın:

   1. **Dağıtım modeli**için uygun modeli seçin ve **Konumlar**için bir konum seçin.

      ![Ek kota ayrıntıları sağlama](./media/resource-manager-core-quotas-request/quota-details-deployment-locations.png)

   1. Seçili konum için, **Türler**altında , **Bir tür**seç'te, **Spot'u**seçin.

      ![Nokta türünü seçin](./media/resource-manager-core-quotas-request/select-spot-type.png)

       **Türler**altında, çoklu seçim desteği aracılığıyla tek bir destek örneğinden hem standart hem de nokta kota türleri isteyebilirsiniz.

       Daha fazla bilgi için, [Bkz. Standart kota: VM serisine göre sınırları artırın.](per-vm-quota-requests.md)

   1. Bu abonelik için istediğiniz yeni kota sınırını girin.

      ![Spot VM için yeni bir kota seçin](./media/resource-manager-core-quotas-request/spot-set-new-quota.png)

1. Birden fazla konum için kota artışı istemek için **Konumlar'da**ek bir konum seçin ve ardından uygun bir VM türünü seçin. Daha sonra ek konuma uygulanan bir sınır girebilirsiniz.

   ![Kota ayrıntılarında ek konumlar belirtin](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. **Kaydet'i** seçin ve destek isteğini oluşturmaya devam edin.

## <a name="request-a-quota-limit-increase-from-the-subscriptions-pane"></a>Abonelikler bölmesinden kota sınırı artışı isteği

Abonelikler bölmesinden tüm VM **serileri** için spot kota sınırı artışı istemek için:

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

1. **Sonraki'ni seçin:** SORUN **AYRINTILARINI**açmak için çözümler. Ek bilgi girmek için **ayrıntıları sağla'yı** seçin. **Kota ayrıntılarında,** aşağıdaki bilgileri girin:

   1. **Dağıtım modeli**için uygun modeli seçin ve **Konumlar**için bir konum seçin.

      ![Kota ayrıntılarını sağlama](./media/resource-manager-core-quotas-request/quota-details-deployment-locations.png)

   1. Seçili konum için, **Türler**altında , **Bir tür**seç'te, **Spot'u**seçin.

      ![Spot türünü seçin](./media/resource-manager-core-quotas-request/select-spot-type.png)

      Daha fazla bilgi için, [Bkz. Standart kota: VM serisine göre sınırları artırın.](per-vm-quota-requests.md)

   1. Bu abonelik için istediğiniz yeni kota sınırını girin.

      ![vCPU sınırı için yeni bir değer girin](./media/resource-manager-core-quotas-request/spot-set-new-quota.png)

1. Birden fazla konum için kota artışı istemek için **Konumlar'da**ek bir konum seçin ve ardından uygun bir VM türünü seçin. Daha sonra ek konuma uygulanan bir sınır girebilirsiniz.

   ![Kota ayrıntılarında ek konumlar seçin](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. **Kaydet'i** seçin ve destek isteğini oluşturmaya devam edin.
