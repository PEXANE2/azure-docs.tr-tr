---
title: Azure bölgesel vCPU kota sınırlarına bir artış iste | Microsoft Docs
description: Bölgesel kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: e262651a6e040c40dbe240ad3437eff1914aa3e5
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75896746"
---
# <a name="standard-quota-increase-limits-by-region"></a>Standart kota: bölgelere göre limitleri artırma 

Azure Resource Manager sanal makineler için iki tür vCPU kotasını destekler:
* *Kullandıkça Öde VM 'leri* ve *ayrılmış VM örnekleri* , *Standart bir vCPU kotasına*tabidir.
* *Spot VM 'ler* bir *spot vCPU kotasına*tabidir. 

Kullandıkça öde ve ayrılmış VM örnekleri için standart vCPU kotası, her bölgedeki her bir abonelik için iki katmanda zorlanır:
* İlk katman *Toplam bölgesel vCPU sayısı sınırı* (tüm VM Serisi genelinde).
* İkinci katman, *VM başına seri vCPU sayısı sınırı* (D serisi vCPU 'lar gibi).
 
Yeni bir spot VM dağıttığınızda, bu VM Serisi için yeni ve mevcut vCPU kullanımı, söz konusu VM Serisi için onaylanan vCPU kotasını aşmamalıdır. Ayrıca, tüm VM dizileri arasında dağıtılan yeni ve var olan vCPU 'ların toplam sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez. 

Azure portal kullanarak VM Serisi için vCPU kota sınırı artışını isteyebilirsiniz. VM Serisi kotasının bir artışı toplam bölgesel vCPU limitini aynı tutara göre otomatik olarak arttırır.

Yeni bir abonelik oluşturduğunuzda, varsayılan toplam bölgesel sanal CPU sayısı, tüm tek VM serileri için toplam varsayılan vCPU kotasına eşit olmayabilir. Bu tutarsızlık, dağıtmak istediğiniz her bir VM Serisi için yeterince kota içeren bir aboneliğe neden olabilir. Ancak tüm dağıtımlar için toplam bölgesel vCPU 'Lara uyum sağlamak için yeterli kota olmayabilir. Bu durumda, toplam bölgesel vCPU sayısı sınırını açıkça artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vCPU sınırı, bölgenin tüm VM serileri genelinde onaylanan toplam kotayı aşamaz.

Standart vCPU kotaları hakkında daha fazla bilgi edinmek için bkz. [sanal makine vCPU kotaları](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet limitleri](https://aka.ms/quotalimits).

Nokta VM vCPU sınırlarının artırılması hakkında daha fazla bilgi için bkz. [spot kota: tüm VM serileri için sınırları artırma](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Bir sonraki bölümde açıklandığı gibi, her iki şekilde bölgeye göre vCPU standart kota sınırınıza bir artış isteyebilirsiniz.

## <a name="request-a-quota-increase-by-region-from-the-help--support-pane"></a>"Yardım + destek" bölmesinden bölgeye göre kota artışı iste

**Yardım + Destek** bölmesinden bölgeye göre vCPU kotası artışı istemek için aşağıdakileri yapın: 

1. [Azure Portal](https://portal.azure.com)sol bölmesinde **Yardım + Destek**' i seçin.

   !["Yardım + destek" bağlantısı](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. **Yardım + Destek** bölmesinde **Yeni destek isteği**' ni seçin. 

    ![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. **Sorun türü** aşağı açılan listesinde **hizmet ve abonelik limitleri (kotalar)** öğesini seçin.

   !["Sorun türü" açılan listesi](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. **Abonelik** açılan listesinde, kotasını artırmak istediğiniz aboneliği seçin.

   !["Abonelik" açılan listesi](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. **Kota türü** aşağı açılan listesinde **diğer istekler**' i seçin.

   !["Kota türü" açılan listesi](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. **Sorun ayrıntıları** bölmesinde, **Açıklama** kutusunda, aşağıdaki ek bilgileri belirtin: 

    a. **Dağıtım modeli**için **Kaynak Yöneticisi**belirtin.  
    b. **Bölge**için gereken bölgenizi (örneğin, **Doğu ABD 2**) belirtin.  
    c. **Yeni sınır**için bölge için yeni bir vCPU sınırı belirtin. Bu değer, bu abonelik için ayrı SKU serisi için onaylanan kotaların toplamını aşmamalıdır.

    !["Sorun ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/regional-details.png)

1. Destek isteği oluşturmaya devam etmek için **Kaydet ' i seçin ve devam edin** .

## <a name="request-a-quota-increase-by-region-from-the-subscriptions-pane"></a>"Abonelikler" bölmesinden bölgeye göre kota artışı iste

**Abonelikler** bölmesinden bölgeye göre vCPU kotası artışı istemek için aşağıdakileri yapın: 

1. [Azure Portal](https://portal.azure.com)sol bölmesinde, **abonelikler**' i seçin.

   !["Abonelikler" bağlantısı](./media/resource-manager-core-quotas-request/subscriptions.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   !["Abonelikler" bölmesi](./media/resource-manager-core-quotas-request/select-subscription.png)

1. **\<abonelik adı >** sayfanızın sol bölmesinde **kullanım + kotalar**' ı seçin.

   !["Kullanım + kotalar" bağlantısı](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

1. Sağ üst köşede, **istek artışı**' nı seçin.

   ![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

1. **Kota türü** aşağı açılan listesinde **diğer istekler**' i seçin.

   !["Kota türü" açılan listesi](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. **Sorun ayrıntıları** bölmesinde, **Açıklama** kutusunda, aşağıdaki ek bilgileri belirtin: 

    a. **Dağıtım modeli**için **Kaynak Yöneticisi**belirtin.  
    b. **Bölge**için gereken bölgenizi (örneğin, **Doğu ABD 2**) belirtin.  
    c. **Yeni sınır**için bölge için yeni bir vCPU sınırı belirtin. Bu değer, bu abonelik için ayrı SKU serisi için onaylanan kotaların toplamını aşmamalıdır.

    !["Sorun ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/regional-details.png)

1. Destek isteği oluşturmaya devam etmek için **Kaydet ' i seçin ve devam edin** .

