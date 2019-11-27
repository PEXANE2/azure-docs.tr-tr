---
title: VM başına Azure vCPU kotası artış istekleri | Microsoft Docs
description: VM başına vCPU kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: ccd0c88c95ae9a752ef8ea2387bbde4f8559bc68
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74531629"
---
# <a name="standard-quota-per-vm-series-vcpu-limit-increase"></a>Standart kota: sanal makine serisi başına vCPU sınırı artışı

Kaynak Yöneticisi, sanal makineler için iki tür vCPU kotasını destekler. **Kullandıkça Öde VM 'leri ve ayrılmış VM örnekleri** standart kotayı kullanır. **Düşük öncelikli VM 'Ler** düşük öncelikli kotayı kullanır. Kullandıkça öde ve ayrılmış VM örnekleri için standart vCPU kotası her bölgedeki her bir abonelik için iki katmanda zorlanır

İlk katman **Toplam bölgesel vCPU sayısı sınırı** (tüm VM serileri genelinde) ve ıkıncı katman **VM Serisi vCPU sayısı sınırı** (Dv3-Series vCPU 'lar gibi). Yeni bir VM 'nin dağıtılması her zaman, bu VM Serisi için yeni ve var olan vCPU 'ların toplamı, söz konusu VM Serisi için onaylanan vCPU kotasını aşmamalıdır. Ayrıca, tüm VM dizileri genelinde dağıtılan toplam yeni ve var olan vCPU sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez.
Azure portal VM Serisi için vCPU kota sınırı artışı isteyebilirsiniz. VM Serisi kotasının bir artışı toplam bölgesel vCPU sayısını aynı tutara göre otomatik olarak arttırır. 

[Sanal makine vCPU kotaları sayfasında](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet limitleri sayfasında](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests)standart vCPU kotaları hakkında daha fazla bilgi edinin. 

Standart [Kota için](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests)bölgesel vCPU limitini artırma hakkında daha fazla bilgi edinin. 

**Düşük öncelıklı VM vCPU sınırlarını artırma** hakkında daha fazla bilgi [edinin.](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota)

**Yardım + Destek** dikey penceresinde ya da portalda **kullanımlar + kota** dikey penceresinde, **VM başına standart vCPU kota limitlerine** artış isteğinde bulunabilir.

## <a name="request-standard-vcpu-quota-increase-per-vm-series-at-subscription-level-using-the-help--support-blade"></a>Yardım + Destek dikey penceresini kullanarak abonelik düzeyinde her VM Serisi için standart vCPU kota artışı iste

Azure portal Azure 'un ' yardım + destek ' dikey penceresi aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin. 

Ayrıca, tek bir destek talebi aracılığıyla birden çok bölge için kota isteyebilirsiniz. Ayrıntılar için aşağıdaki 11. adıma bakın.

1. https://portal.azure.com, **Yardım + Destek**' i seçin.

   ![Yardım ve destek](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  **Yeni destek isteği**’ni seçin. 

     ![Yeni destek isteği](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Sorun türü açılır penceresinde **hizmet ve abonelik limitleri (kotalar)** öğesini seçin.

   ![Sorun türü açılan liste](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Kotasını artırmanız gereken aboneliği seçin.

   ![Abonelik newSR 'yi seçin](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. **Kota türü** açılır penceresinde **işlem-VM (çekirdekler-vCPU 'lar) abonelik sınırı artışları** ' ni seçin. 

   ![Kota türünü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. **Sorun ayrıntıları**' nda, **Ayrıntılar sağla**' ya tıklayarak isteğinizi işlemeye yardımcı olacak ek bilgiler sağlayın.

   ![Ayrıntıları belirtin](./media/resource-manager-core-quotas-request/provide-details.png)

7. **Kota ayrıntıları** panelinde, **dağıtım modeli** ' ni seçin ve bir **konum seçin.**

   ![Kota ayrıntıları DM](./media/resource-manager-core-quotas-request/1-7.png)

8. Seçilen konum için, **tür** değerini **' standart '** olarak seçin. **Tür** alanında çok seçimli destek aracılığıyla tek bir destek durumundan hem standart hem de düşük öncelikli kota türleri isteyebilirsiniz. **< > Sayfasında** **düşük öncelikli kota sınırlarını artırma** hakkında daha fazla bilgi edinin.

   ![SKU Ailesi](./media/resource-manager-core-quotas-request/1-8.png)

9. Artış gerektiren **SKU aileleri** seçin

   ![SKU Ailesi](./media/resource-manager-core-quotas-request/1-9.png)

10. Abonelikte istediğiniz yeni limitleri girin. Bir satırı kaldırmak için, SKU ailesi açılan kutusundan SKU 'nun işaretini kaldırın veya "x" simgesine tıklayın. 

   ![Yeni sınırlar](./media/resource-manager-core-quotas-request/1-10.png)

11. Birden fazla konum için kota istemek için, açılan listeden başka bir **konuma** bakabilirsiniz ve uygun VM türünü seçebilirsiniz. Bu adım, yeni konuma karşı önceki bir **konum** IÇIN seçilen SKU ailelerini önceden yükler ve istediğiniz yeni limitleri girmeniz yeterlidir.

   ![Birden çok konum](./media/resource-manager-core-quotas-request/1-11.png)
   
12. Her SKU ailesi için istenen kotayı girdikten sonra, destek isteği oluşturmaya devam etmek için kota ayrıntıları panelinde **Kaydet ve devam et** ' e tıklayın.

## <a name="request-standard-vcpu-quota-increase-per-vm-series-at-subscription-level-using-usages--quota-blade"></a>Kullanımlar + kota dikey penceresini kullanarak abonelik düzeyinde VM Serisi başına standart vCPU kota artışı iste

Azure 'un ' kullanım + kota ' dikey penceresi aracılığıyla Azure portal kullanılabilir bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

Ayrıca, tek bir destek talebi aracılığıyla **birden çok bölge Için kota isteyebilirsiniz** . Ayrıntılar için aşağıdaki 10. adıma bakın

1. https://portal.azure.com, **abonelikler**' i seçin.

   ![Abonelikler](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

   ![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3. **Kullanım + kotalar** ' ı seçin

   ![Kullanım ve kotaları seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Sağ üst köşede **istek artışı**' nı seçin.

   ![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5. **İşlem-VM (çekirdek-vCPU) abonelik sınırı** ' nı seçin, teklif türü olarak artar. 

   ![Formu doldur](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
6. **Kota ayrıntıları** panelinde, dağıtım modeli ' ni seçin ve bir konum seçin.

   ![Kota sorunu dikey penceresi](./media/resource-manager-core-quotas-request/1-1-6.png)

7. Seçilen konum için, **tür** değerini **' standart '** olarak seçin. **Tür** alanında çok seçimli destek aracılığıyla tek bir destek durumundan hem standart hem de düşük öncelikli kota türleri isteyebilirsiniz. Bu [sayfada](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota) **düşük öncelikli vCPU sınırlarını artırma** hakkında daha fazla bilgi edinin.

   ![SKU serisi seçildi](./media/resource-manager-core-quotas-request/1-1-7.png)
   
   
8. Artış gerektiren **SKU aileleri** seçin

   ![SKU serisi seçildi](./media/resource-manager-core-quotas-request/1-1-8.png)

9. Abonelikte istediğiniz yeni limitleri girin. Bir satırı kaldırmak için, SKU ailesi açılan kutusundan SKU 'nun işaretini kaldırın veya "x" simgesine tıklayın. 

   ![SKU yeni kota isteği](./media/resource-manager-core-quotas-request/1-1-9.png)
   

10. Birden fazla konum için kota istemek için, açılan listeden başka bir **konuma** bakabilirsiniz ve uygun VM türünü seçebilirsiniz. Bu adım, yeni konuma karşı önceki bir **konum** IÇIN seçilen SKU ailelerini önceden yükler ve istediğiniz yeni limitleri girmeniz yeterlidir.
   
    ![SKU yeni kota isteği](./media/resource-manager-core-quotas-request/1-1-10.png)
 
