---
title: Azure klasik dağıtım modeli
description: Artık Kaynak Yöneticisi modeli nin yerini alan Klasik dağıtım modeli, VM'ler ve sanal makine ölçek kümeleri için genel vCPU kota sınırını uygular.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: a3d5106cafc1d3bfe77f3e42e85cedb668fc4fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76835630"
---
# <a name="classic-deployment-model"></a>Klasik dağıtım modeli

Klasik dağıtım modeli, eski nesil Azure dağıtım modelidir. Sanal makineler ve sanal makine ölçek kümeleri için küresel bir vCPU kota sınırı uygular. Klasik dağıtım modeli artık önerilmez ve artık Kaynak Yöneticisi modeli tarafından yerini adatır.

Bu iki dağıtım modeli ve Kaynak Yöneticisi kullanmanın avantajları hakkında daha fazla bilgi edinmek için [Kaynak Yöneticisi ve klasik dağıtım](../../azure-resource-manager/management/deployment-models.md)bölümüne bakın.

Yeni bir abonelik oluşturulduğunda, varsayılan vCPUs kotası atanır. Klasik dağıtım modeli kullanılarak yeni bir sanal makine dağıtılsa, tüm bölgelerde ki yeni ve mevcut vCPU kullanımının toplamı Klasik dağıtım modeli için onaylanan vCPU kotasını aşmamalıdır.

Kotalar hakkında daha fazla bilgi edinmek için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md)bölümüne bakın.

Klasik dağıtım modeli için vCPU kota sınırında bir artış talep edebilirsiniz. Azure portalında **Yardım + destek** veya Kullanım + **kotaları** kullanın.

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-help--support"></a>Yardım + destek kullanarak abonelik düzeyinde VM serisi vCPU kota artışı başına istek

Azure portalında **Yardım + destek** kullanarak bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

1. Azure [portalı](https://portal.azure.com) menüsünden **Yardım + destek'i**seçin.

   ![Azure portalında Yardım + destek'i seçin](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. **Yeni destek isteği**’ni seçin.

   ![Azure portalında yeni bir destek isteği oluşturma](./media/resource-manager-core-quotas-request/new-support-request.png)

1. **Sorun türünde,** Hizmet ve **abonelik limitlerini (kotalar)** seçin.

   ![Sorun türü olarak kotaları seçme](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   ![Kotayı artırmak için abonelik seçin](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. **Kota türü için,** **Bilgi İşlem -VM (cores-vCPUs) abonelik sınırı artışlarını**seçin.

   ![Artırmak için kota türünü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. **Sonraki'ni seçin:** SORUN **AYRINTILARINI**açmak için çözümler. Ek bilgi sağlamak için **ayrıntıları** sağla'yı seçin.

   ![İsteğinize yardımcı olmak için ayrıntılar sağlayın](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. **Kota ayrıntılarında,** **Klasik'i** seçin ve **Konum'u**seçin.

   ![Dağıtım modeli ve Konum dahil olmak üzere ayrıntılar ekleme](./media/resource-manager-core-quotas-request/quota-details-classic.png)

1. **SKU ailesi**için, artırmak için bir veya daha fazla SKU ailesi seçin.

   ![SKU ailesini artırmak için belirtin](./media/resource-manager-core-quotas-request/sku-family-classic.png)

1. Abonelikte istediğiniz yeni sınırları girin. Bir satırı kaldırmak için **SKU'yu SKU ailesinden** seçin veya "X" simgesini seçin. Her SKU ailesi için bir kota girdikten sonra, destek isteğine devam etmek için **Kotada** **Kaydet ve Devam** Et ayrıntılarını seçin.

   ![Yeni sınırlar isteyin](./media/resource-manager-core-quotas-request/new-limits-classic.png)

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-usage--quotas"></a>Kullanım + kotaları kullanarak abonelik düzeyinde VM serisi vCPU kota artışı başına istek

Azure portalında **Kullanım + kotalarını** kullanarak bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

1. Azure [portalında](https://portal.azure.com) **Abonelikleri**arayın ve seçin.

   ![Azure portalındaki Aboneliklere gitme](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   ![Değiştirmek için aboneliği seçin](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. **Kullanım + kotaları**seçin.

   ![Abonelik için kullanımı ve kotaları seçin](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. Sağ üst **köşede, Artış İste'yi**seçin.

   ![Kotayı artırmak için seçin](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. **Kota türü** **arttıkça Compute-VM (cores-vCPUs) abonelik sınırının arttını** seçin.

   ![Kota türünü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. **Sonraki'ni seçin:** SORUN **AYRINTILARINI**açmak için çözümler. Ek bilgi sağlamak için **ayrıntıları** sağla'yı seçin.

   ![İsteğiniz için ayrıntılı bilgi sağlayın](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. **Kota ayrıntılarında,** **Klasik** ve **Konum'u**seçin.

   ![Dağıtım modeli ve konum dahil olmak üzere kota ayrıntılarını seçme](./media/resource-manager-core-quotas-request/quota-details-classic.png)

1. Artış için bir veya daha fazla SKU ailesi seçin.

   ![Artış için SKU ailesini seçin](./media/resource-manager-core-quotas-request/sku-family-classic.png)

1. Abonelikte istediğiniz yeni sınırları girin. Bir satırı kaldırmak için **SKU'yu SKU ailesinden** seçin veya "X" simgesini seçin. Her SKU ailesi için bir kota girdikten sonra, destek isteğine devam etmek için **Kotada** **Kaydet ve Devam** Et ayrıntılarını seçin.

   ![Yeni kota girin](./media/resource-manager-core-quotas-request/new-limits-classic.png)

