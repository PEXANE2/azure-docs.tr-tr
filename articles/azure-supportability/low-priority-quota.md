---
title: Sanal makine kotasını spot | Microsoft Docs
description: Nokta kotası istekleri yaparak kota sınırlarını artırma
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: b087315e02a61886fa6f4ef083c75ed6b5b60cf9
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750314"
---
# <a name="spot-quota-increase-limits-for-all-vm-series"></a>Spot kota: tüm VM serileri için sınırları artır

Spot sanal makineler (VM 'Ler), farklı bir Azure kullanımı modeli sağlar. Kullandıkça Öde veya ayrılmış VM örneği dağıtımları için gerektiğinde Azure 'un VM 'Leri kaldırmasına izin vermek üzere Exchange 'de daha düşük maliyetleriniz olduğunu varsayalım. Spot VM 'Ler hakkında daha fazla bilgi için bkz. [Sanal Makine Ölçek Kümeleri Için Azure spot VM 'ler](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

Azure Resource Manager sanal makineler için iki tür vCPU kotasını destekler:
* *Kullandıkça Öde VM 'leri* ve *ayrılmış VM örnekleri* , *Standart bir vCPU kotasına*tabidir.
* *Spot VM 'ler* bir *spot vCPU kotasına*tabidir. 

*Nokta vCPU kota* türü Için kaynak yöneticisi vCPU kotaları, KULLANILABILIR tüm VM dizileri genelinde tek bir bölgesel sınır olarak zorlanır.

Yeni bir spot VM 'yi her dağıttığınızda, tüm spot sanal makine örneklerinin toplam yeni ve var olan vCPU kullanımı, onaylanan nokta vCPU kota sınırını aşmamalıdır. Spot kota aşılırsa, spot VM dağıtımına izin verilmez. 

Bu makalede, Azure portal kullanarak, nokta vCPU kota limiti artışının nasıl isteneceğini ele alınmaktadır. 

Standart vCPU kotaları hakkında daha fazla bilgi edinmek için bkz. [sanal makine vCPU kotaları](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) ve [Azure aboneliği ve hizmet limitleri](https://aka.ms/quotalimits). 

Bölgeye göre vCPU sınırını artırma hakkında bilgi edinmek için bkz. [Standart kota: bölgesel vCPU sınırı artışı](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests).

## <a name="request-a-quota-limit-increase-from-the-help--support-pane"></a>"Yardım + destek" bölmesinden bir kota sınırı artışı iste 

**Yardım + Destek** BÖLMESINDEN tüm VM serileri için bir spot kota sınırı artışı istemek üzere şunları yapın:

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

1. **Ayrıntılar** sekmesini seçin ve ardından **Sorun ayrıntıları**' nın altında, **Ayrıntılar sağla**' yı seçin ve isteğinizi işlemeye yardımcı olacak ek bilgileri girin.

   !["Ayrıntıları sağla" bağlantısı](./media/resource-manager-core-quotas-request/provide-details.png)
   
1. Sağ üst köşedeki **Kota ayrıntıları** bölmesinde şunları yapın:

   !["Kota ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/3-7.png)

   a. **Dağıtım modeli** aşağı açılan listesinde uygun modeli seçin.

   b. **Konumlar** açılan listesinde bir konum seçin. Seçilen konum için **türler**altında, **tür seçin** kutusunda, **nokta**girin. 
   
   !["Yeni destek isteği" Ayrıntılar sekmesi](./media/resource-manager-core-quotas-request/3-8.png)

    **Türler**altında, çoklu seçim desteğiyle tek bir destek durumundan hem standart hem de spot kota türleri isteyebilirsiniz. 
    
    Daha fazla bilgi için bkz. [Standart kota: VM başına seri vCPU sınırı artışı](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

   c. Bu abonelik için istediğiniz yeni kota sınırını girin. 
 
   !["Yeni vCPU sınırı" metin kutusu](./media/resource-manager-core-quotas-request/3-9.png)

1. Birden fazla konum için kota artışı istemek için, açılan listeden ek bir konum seçin ve ardından uygun bir VM türü seçin. Daha sonra ek konum için geçerli olan bir sınır girebilirsiniz.

   !["Kota ayrıntıları" bölmesindeki ek konumlar](./media/resource-manager-core-quotas-request/3-10.png)

1. Destek isteği oluşturmaya devam etmek için **Kaydet ' i seçin ve devam edin** .

## <a name="request-a-quota-limit-increase-from-the-subscriptions-pane"></a>"Abonelikler" bölmesinden bir kota sınırı artışı iste

**Abonelikler** BÖLMESINDEN tüm VM serileri için bir spot kota sınırı artışı istemek için aşağıdakileri yapın:

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

   !["Kota ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/3-2-6.png)
 
   a. **Dağıtım modeli** aşağı açılan listesinde uygun modeli seçin.

   b. **Konumlar** açılan listesinde bir konum seçin. 
   
   c. Seçilen konum için **türler**altında, **tür seçin** kutusunda, **nokta**girin.

   !["Kota ayrıntıları" bölmesi](./media/resource-manager-core-quotas-request/3-2-7.png)

   Daha fazla bilgi için bkz. [Standart kota: VM başına seri vCPU sınırı artışı](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

   d. Bu abonelik için istediğiniz yeni kota sınırını girin.

   !["Yeni vCPU sınırı" metin kutusu](./media/resource-manager-core-quotas-request/3-2-8.png)
 
1. Birden fazla konum için kota artışı istemek için, açılan listeden ek bir konum seçin ve ardından uygun bir VM türü seçin. Daha sonra ek konum için geçerli olan bir sınır girebilirsiniz.

   !["Kota ayrıntıları" bölmesindeki ek konumlar](./media/resource-manager-core-quotas-request/3-2-9.png)
 
1. Destek isteği oluşturmaya devam etmek için **Kaydet ' i seçin ve devam edin** .


