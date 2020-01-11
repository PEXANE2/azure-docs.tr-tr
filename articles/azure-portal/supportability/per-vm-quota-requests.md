---
title: Azure VM Serisi başına vCPU kota sınırlarına artış iste | Microsoft Docs
description: Bu makalede VM vCPU başına kota sınırı artışına nasıl istek yapılacağı açıklanır.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: fc59ffda245834c716067bc63e6c3745fa5d23b3
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75897253"
---
# <a name="standard-quota-increase-limits-by-vm-series"></a>Standart kota: VM serisine göre limitleri artırma

Azure Resource Manager sanal makineler için iki tür vCPU kotasını destekler:
* *Kullandıkça Öde VM 'leri* ve *ayrılmış VM örnekleri* , *Standart bir vCPU kotasına*tabidir.
* *Spot VM 'ler* bir *spot vCPU kotasına*tabidir. 

Kullandıkça öde ve ayrılmış VM örnekleri için standart vCPU kotası, her bölgedeki her bir abonelik için iki katmanda zorlanır:
* İlk katman *Toplam bölgesel vCPU sayısı sınırı* (tüm VM Serisi genelinde).
* İkinci katman, *VM başına seri vCPU sayısı sınırı* (örneğin, Dv3-Series vCPU 'lar). 

Yeni bir spot VM dağıttığınızda, bu VM Serisi için yeni ve mevcut vCPU kullanımı, söz konusu VM Serisi için onaylanan vCPU kotasını aşmamalıdır. Ayrıca, tüm VM dizileri arasında dağıtılan yeni ve var olan vCPU 'ların toplam sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez.

Azure portal kullanarak VM Serisi için vCPU kota sınırı artışını isteyebilirsiniz. VM Serisi kotasının bir artışı toplam bölgesel vCPU limitini aynı tutara göre otomatik olarak arttırır. 

Standart vCPU kotaları hakkında daha fazla bilgi edinmek için bkz. [sanal makine vCPU kotaları](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet limitleri](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests). 

Standart kota için bölgeye göre vCPU sınırını artırma hakkında bilgi edinmek için bkz. [Standart kota: bölgeye göre sınırları artırma](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests). 

Nokta VM vCPU sınırlarının artırılması hakkında daha fazla bilgi için bkz. [spot kota: tüm VM serileri için sınırları artırma](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Sonraki bölümlerde açıklandığı gibi, her iki şekilde de VM Serisi başına standart vCPU kota limitlerinin artışını isteyebilirsiniz.

## <a name="request-a-standard-quota-increase-from-the-help--support-pane"></a>"Yardım + destek" bölmesinden standart bir kota artışı isteyin

**Yardım + Destek** bölmesinden VM başına standart vCPU kotası artışı istemek için aşağıdakileri yapın: 

> [!NOTE]
> Ayrıca, tek bir destek talebi aracılığıyla birden çok bölge için bir kota sınırı artışı isteyebilirsiniz. Ayrıntılar için bkz. 8. adım.

1. [Azure Portal](https://portal.azure.com)sol bölmesinde **Yardım + Destek**' i seçin.

   !["Yardım + destek" bağlantısı](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. **Yardım + Destek** bölmesinde **Yeni destek isteği**' ni seçin. 

    ![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. **Sorun türü** aşağı açılan listesinde **hizmet ve abonelik limitleri (kotalar)** öğesini seçin.

   !["Sorun türü" açılan listesi](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. **Abonelik** açılan listesinde, kotasını artırmak istediğiniz aboneliği seçin.

   !["Abonelik" açılan listesi](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. **Kota türü** aşağı açılan listesinde, **işlem-VM (çekirdek-vCPU) abonelik sınırı arttıkça**öğesini seçin. 

   !["Kota türü" açılan listesi](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. **Ayrıntılar** sekmesinde, **Sorun ayrıntıları**' nın altında, **Ayrıntılar sağla**' yı seçin ve ardından isteğinizi işlemeye yardımcı olacak ek bilgileri girin.

   !["Ayrıntıları sağla" bağlantısı](./media/resource-manager-core-quotas-request/provide-details.png)

1. Sağ üst köşedeki **Kota ayrıntıları** bölmesinde şunları yapın:

   !["Kota ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/1-7.png)

   a. **Dağıtım modeli** aşağı açılan listesinde uygun modeli seçin.

   b. **Konumlar** açılan listesinde bir konum seçin. Seçilen konum için **türler**altında, **tür seçin** kutusuna **Standart**yazın.

   !["Kota ayrıntıları" bölmesi-kota türleri](./media/resource-manager-core-quotas-request/1-8.png)

   **Türler**altında, çoklu seçim desteğiyle tek bir destek durumundan hem standart hem de spot kota türleri isteyebilirsiniz.
   
   Spot kota sınırlarını artırma hakkında daha fazla bilgi için bkz. [Sanal Makine Ölçek Kümeleri Için Azure spot VM 'ler](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   c. **Standart** aşağı açılan listesinin altında, kotalarını ARTıRMAK istediğiniz SKU serisini seçin.

   !["Kota ayrıntıları" bölmesi-SKU serisi](./media/resource-manager-core-quotas-request/1-9.png)

   d. Bu abonelik için istediğiniz yeni kota sınırlarını girin. Listenizden bir SKU 'YU kaldırmak için SKU 'nun yanındaki onay kutusunu temizleyin veya **Sil** (X) simgesini seçin. 

   !["Yeni vCPU sınırı" metin kutusu](./media/resource-manager-core-quotas-request/1-10.png)

1. Birden fazla konum için kota artışı istemek için, açılan listeden ek bir konum seçin ve ardından uygun bir VM türü seçin. Daha sonra ek konum için geçerli olan bir sınır girebilirsiniz.

   !["Kota ayrıntıları" bölmesindeki ek konumlar](./media/resource-manager-core-quotas-request/1-11.png)
   
1. Destek isteği oluşturmaya devam etmek için **Kaydet ' i seçin ve devam edin** .

## <a name="request-a-standard-quota-increase-from-the-subscriptions-pane"></a>"Abonelikler" bölmesinden standart bir kota artışı isteyin

**Abonelikler** bölmesinden VM başına standart bir vCPU kotası artışı istemek için aşağıdakileri yapın:

> [!NOTE]
> Ayrıca, tek bir destek talebi aracılığıyla birden çok bölge için bir kota sınırı artışı isteyebilirsiniz. Ayrıntılar için bkz. 7. adım.

1. [Azure Portal](https://portal.azure.com)sol bölmesinde, **abonelikler**' i seçin.

   !["Abonelikler" bağlantısı](./media/resource-manager-core-quotas-request/subscriptions.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   !["Abonelikler" bölmesi](./media/resource-manager-core-quotas-request/select-subscription.png)

1. **\<abonelik adı >** sayfanızın sol bölmesinde **kullanım + kotalar**' ı seçin.

   !["Kullanım + kotalar" bağlantısı](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

1. Sağ üst köşede, **istek artışı**' nı seçin.

   ![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

1. **Kota türü** aşağı açılan listesinde, **işlem-VM (çekirdek-vCPU) abonelik sınırı arttıkça**öğesini seçin.

   !["Kota türü" açılan listesi](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
1. Sağ üst köşedeki **Kota ayrıntıları** bölmesinde şunları yapın:

   !["Kota ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/1-1-6.png)

   a. **Dağıtım modeli** aşağı açılan listesinde uygun modeli seçin.

   b. **Konumlar** açılan listesinde bir konum seçin. 
   
   c. Seçilen konum için **türler**altında **bir tür Seç**' i seçin ve **Standart** onay kutusunu seçin.

   !["Standart" onay kutusu](./media/resource-manager-core-quotas-request/1-1-7.png)
   
   **Türler**altında, çoklu seçim desteğiyle tek bir destek durumundan hem standart hem de düşük öncelikli kota türleri isteyebilirsiniz.
   
   Spot kota sınırlarını artırma hakkında daha fazla bilgi için bkz. [Sanal Makine Ölçek Kümeleri Için Azure spot VM 'ler](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   d. **Standart** aşağı açılan listesinin altında, kotalarını ARTıRMAK istediğiniz SKU serisini seçin.

   !["Kota ayrıntıları" bölmesi-SKU serisi](./media/resource-manager-core-quotas-request/1-1-8.png)

   e. Bu abonelik için istediğiniz yeni kota sınırlarını girin. Listenizden bir SKU 'YU kaldırmak için SKU 'nun yanındaki onay kutusunu temizleyin veya **Sil** (X) öğesini seçin. 

   !["Yeni vCPU sınırı" metin kutusu](./media/resource-manager-core-quotas-request/1-1-9.png)
   
1. Birden fazla konum için kota artışı istemek için, açılan listeden ek bir konum seçin ve ardından uygun bir VM türü seçin. 

   Bu adım, önceki konumlar için seçtiğiniz SKU serisini önceden yükler. Ek seriye uygulamak istediğiniz kota sınırlarını girin.
   
   !["Kota ayrıntıları" bölmesindeki ek konumlar](./media/resource-manager-core-quotas-request/1-1-10.png)
 
1. Destek isteği oluşturmaya devam etmek için **Kaydet ' i seçin ve devam edin** .
