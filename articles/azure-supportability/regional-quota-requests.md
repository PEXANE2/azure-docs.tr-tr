---
title: Azure bölgesel kota artışı istekleri | Microsoft Docs
description: Bölgesel kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 132cf6ccfec5af9951f5dc6d6a3c6d3c81363d81
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850015"
---
# <a name="standard-quota-regional-vcpu-limit-increase"></a>Standart kota: bölgesel vCPU sınırı artışı 

Kaynak Yöneticisi, sanal makineler için iki tür vCPU kotasını destekler. **Kullandıkça Öde VM 'leri** ve **ayrılmış VM örnekleri** standart kotayı kullanır. **Spot VM 'Ler** spot kota kullanır. 

Kullandıkça öde ve ayrılmış VM örnekleri için standart vCPU kotası, her bölgedeki her bir abonelik için iki katmanda zorlanır.
 
İlk katman **Toplam bölgesel vCPU sayısı sınırı** (tüm VM serileri genelinde) ve ıkıncı katman **VM Serisi vCPU sayısı sınırı** (D serisi vCPU 'lar gibi). Yeni bir VM 'nin dağıtılması her zaman, bu VM Serisi için yeni ve var olan vCPU 'ların toplamı, söz konusu VM Serisi için onaylanan vCPU kotasını aşmamalıdır. Ayrıca, tüm VM dizileri genelinde dağıtılan toplam yeni ve var olan vCPU sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez. Azure portal VM Serisi için vCPU kota sınırı artışı isteyebilirsiniz. VM Serisi kotasının bir artışı toplam bölgesel vCPU sayısını aynı tutara göre otomatik olarak arttırır.

Yeni bir abonelik oluşturulduğunda, varsayılan toplam bölgesel sanal CPU 'Lar, tüm tek VM serileri için varsayılan vCPU kotaları toplamına eşit olmayabilir. Bu, dağıtmak istediğiniz her bir VM Serisi için yeterli kotaya sahip bir aboneliğe neden olabilir, ancak tüm dağıtımlar için toplam bölgesel vCPU 'Lara yetecek kadar kota elde etmez. Bu durumda, toplam bölgesel vCPU sınırını açıkça artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vCPU sayısı, bölgenin tüm VM serileri genelinde onaylanan kotanın toplamını aşamaz.

[Sanal makine vCPU kotaları sayfasında](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet limitleri](https://aka.ms/quotalimits) sayfasında standart vCPU kotaları hakkında daha fazla bilgi edinin.

**Spot VM vCPU sınırlarını** [burada](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota)artırma hakkında daha fazla bilgi edinin.

**Yardım + Destek** dikey penceresinde veya portalda **kullanımlar + kota** dikey penceresinde **Standart VM toplam bölgesel vCPU sınırına** bir artış isteğinde bulunabilir.

## <a name="request-standard-quota-regional-vcpu-limit-increase-at-subscription-level-using-the-help--support-blade"></a>Yardım + Destek dikey penceresini kullanarak abonelik düzeyinde standart kota bölgesel sanal CPU sınırı artışı iste

Azure portal Azure 'un ' yardım + destek ' dikey penceresi aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

1. https://portal.azure.com, **Yardım + Destek**' i seçin.

![Yardım + Destek](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  **Yeni destek isteği**’ni seçin. 

![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Sorun türü açılır penceresinde **hizmet ve abonelik limitleri (kotalar)** öğesini seçin.

![Sorun türü açılan liste](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Kotasını artırmanız gereken aboneliği seçin.

![Abonelik newSR 'yi seçin](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. **Kota türü** açılan kutusunda **diğer istekleri** seçin.

![Kota Türü](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. **Ayrıntılar** bölmesinde, isteğinizi işleme ve servis talebi oluşturmaya devam etme hakkında daha fazla bilgi için aşağıdaki örneğe göre ek bilgiler sağlayın. 
    1.  **Dağıtım modeli** : ' Kaynak Yöneticisi ' belirtin
    2.  **İstenen bölge** – gerekli bölgenizi belirtin, örn. Doğu ABD 2
    3.  **Yeni sınır değeri** : yeni bölge sınırı belirtin. Bu abonelik için tek tek SKU aileleri için onaylanan kotanın toplamı aşmamalıdır

![Kota ayrıntıları](./media/resource-manager-core-quotas-request/regional-details.png)

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-usages--quota-blade"></a>**Kullanımlar + kota** dikey penceresini kullanarak abonelik düzeyindeki toplam bölgesel vCPU kotası artışı iste

Azure 'un ' kullanım + kota ' dikey penceresi aracılığıyla Azure portal kullanılabilir bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

1. https://portal.azure.com, **abonelikler**' i seçin.

![Abonelikler](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. **Kullanım + kotalar** ' ı seçin

![Kullanım ve kotaları seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşede **istek artışı**' nı seçin.

![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5. **Kota türü** açılan kutusunda **diğer istekleri** seçin.

![Kota Türü](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. **Ayrıntılar** bölmesinde, isteğinizi işleme ve servis talebi oluşturmaya devam etme hakkında daha fazla bilgi için aşağıdaki örneğe göre ek bilgiler sağlayın. 
    1.  **Dağıtım modeli** : ' Kaynak Yöneticisi ' belirtin
    2.  **İstenen bölge** – gerekli bölgenizi belirtin, örn. Doğu ABD 2
    3.  **Yeni sınır değeri** : yeni bölge sınırı belirtin. Bu abonelik için tek tek SKU aileleri için onaylanan kotanın toplamı aşmamalıdır

![Kota ayrıntıları](./media/resource-manager-core-quotas-request/regional-details.png)



