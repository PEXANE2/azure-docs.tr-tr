---
title: Spot kota | Microsoft Docs
description: Spot kota istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 09c9d7940314b691e6351353e6a0076510fdcb13
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850576"
---
# <a name="spot-quota-limit-increase-for-all-vm-series"></a>Spot kota: tüm VM serileri için sınır artışı

Spot sanal makineler farklı bir Azure kullanımı modeli sağlar ve Azure 'un Kullandıkça Öde veya ayrılmış VM örnek dağıtımları için gereken bir VM 'yi kaldırmasına izin vermek için daha düşük bir maliyet sunar. [Burada](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot)spot VM 'ler hakkında daha fazla bilgi edinin.

Kaynak Yöneticisi, sanal makineler için iki tür vCPU kotasını destekler. **Kullandıkça Öde VM 'leri ve ayrılmış VM örnekleri** standart kotayı kullanır. **Spot VM 'Ler** spot kota kullanır. 

**Spot kota** türü Için kaynak yöneticisi vCPU kotaları, KULLANILABILIR tüm VM dizileri genelinde tek bir bölgesel sınır olarak zorlanır.

Yeni bir spot VM 'nin dağıtılması her zaman, tüm spot VM örnekleri için yeni ve var olan vCPU 'ların toplamı, onaylanan nokta vCPU kota sınırını aşmamalıdır. Spot kota aşılırsa, spot VM dağıtımına izin verilmez. Azure portal noktadan itibaren sanal CPU kota sınırı artışı isteyebilirsiniz. 

Sanal makine vCPU kotaları sayfasında ve Azure aboneliği ve hizmet limitleri sayfasında standart vCPU kotaları hakkında daha fazla bilgi edinin. Bu [sayfada](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests)standart kota Için bölgesel vCPU sınırını artırma hakkında daha fazla bilgi edinin.

Artık, **Yardım + Destek** dikey penceresinde veya portalda **kullanımlar + kota** dikey penceresinde **Tüm VM dizileri için nokta kotası limitlerine** bir artış isteyebilirsiniz.

## <a name="request-spot-quota-limit-increase-for-all-vm-series-per-subscription-using-the-help--support-blade"></a>Yardım + Destek dikey penceresini kullanarak abonelik başına tüm VM serileri için istek noktası kota sınırı artışı

Azure portal Azure 'un ' yardım + destek ' dikey penceresi aracılığıyla bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

Ayrıca, tek bir destek talebi aracılığıyla **birden çok bölge Için kota isteyebilirsiniz** . Ayrıntılar için aşağıdaki 10. adıma bakın. 


1. https://portal.azure.com, **Yardım + Destek**' i seçin.

   ![Yardım + Destek](./media/resource-manager-core-quotas-request/helpsupport.png)
 
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
   
7.  **Kota ayrıntıları*** panelinde **dağıtım modeli** ' ni seçin ve bir **konum**seçin.

![Ayrıntıları belirtin](./media/resource-manager-core-quotas-request/3-7.png)

8. Seçilen konum için **' nokta '** olarak **tür** değeri ' ni seçin. **Tür** alanında çok seçimli destek aracılığıyla tek bir destek durumundan hem standart hem de spot kota türleri isteyebilirsiniz. Bu [SAYFADAKI](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests) **VM serileri başına standart kotayı artırma** hakkında daha fazla bilgi edinin.

![Ayrıntıları belirtin](./media/resource-manager-core-quotas-request/3-8.png)

9.  Abonelikte istediğiniz yeni limiti girin. 
 
 ![Ayrıntıları belirtin](./media/resource-manager-core-quotas-request/3-9.png)

10. Birden fazla konum için kota istemek için, açılan listeden başka bir konuma bakabilirsiniz ve uygun VM türünü seçebilirsiniz. Daha sonra istediğiniz yeni limitleri girebilirsiniz.

![Ayrıntıları belirtin](./media/resource-manager-core-quotas-request/3-10.png)

11. İstenen kotayı girdikten sonra, destek isteği oluşturmaya devam etmek için kota ayrıntıları panelinde Kaydet ' e tıklayın **ve devam edin** .

## <a name="request-spot-quota-limit-increase-for-all-vm-series-per-subscription-using-usages--quota-blade"></a>Kullanımlar + kota dikey penceresini kullanarak abonelik başına tüm VM serileri için istek noktası kota sınırı artışı

Azure 'un ' kullanım + kota ' dikey penceresi aracılığıyla Azure portal kullanılabilir bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

Ayrıca, tek bir destek talebi aracılığıyla **birden çok bölge Için kota isteyebilirsiniz** . Ayrıntılar için aşağıdaki adım 9 ' a bakın. 

1.  https://portal.azure.com, **abonelikler**' i seçin.

 ![Abonelikler](./media/resource-manager-core-quotas-request/subscriptions.png)

2.  Kotasını artırmanız gereken aboneliği seçin.

 ![Abonelik seçme](./media/resource-manager-core-quotas-request/select-subscription.png)

3.  **Kullanım + kotalar**' ı seçin.

 ![Kullanım ve kotaları seçin](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4.  Sağ üst köşede **istek artışı** ' nı seçin.

   ![İstek artışı](./media/resource-manager-core-quotas-request/request-increase.png)

5.  **İşlem-VM (çekirdek-vCPU) abonelik sınırı** ' nı seçin, teklif türü olarak artar.

  ![Formu doldur](./media/resource-manager-core-quotas-request/select-quota-type.png)

6.  **Kota ayrıntıları** panelinde, dağıtım modeli ' ni seçin ve bir konum seçin.

  ![Formu doldur](./media/resource-manager-core-quotas-request/3-2-6.png)
 
7.  Seçilen konum için **' nokta '** olarak **tür** değeri ' ni seçin. **Tür** alanında çok seçimli destek aracılığıyla tek bir destek durumundan hem standart hem de spot kota türleri isteyebilirsiniz. Bu [SAYFADAKI](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests) **VM serileri başına standart kotayı artırma** hakkında daha fazla bilgi edinin.

  ![Formu doldur](./media/resource-manager-core-quotas-request/3-2-7.png)
 
8.  Abonelikte istediğiniz yeni limiti girin.

  ![Formu doldur](./media/resource-manager-core-quotas-request/3-2-8.png)
 
9.  Birden fazla konum için kota istemek için, açılan listeden başka bir **konuma** bakabilirsiniz ve uygun VM türünü seçebilirsiniz. Daha sonra istediğiniz yeni limitleri girebilirsiniz.

  ![Formu doldur](./media/resource-manager-core-quotas-request/3-2-9.png)
 
10. İstenen kotayı girdikten sonra, destek isteği oluşturmaya devam etmek için kota ayrıntıları panelinde Kaydet ' e tıklayın **ve devam edin** .


