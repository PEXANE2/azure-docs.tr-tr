---
title: Azure klasik dağıtım modeli
description: Artık Kaynak Yöneticisi modeli tarafından değiştirilen klasik dağıtım modeli, VM 'Ler ve sanal makine ölçek kümeleri için genel bir vCPU kota sınırı uygular.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: afdb8b2b677321239f240e74d49711195ac186c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763916"
---
# <a name="classic-deployment-model"></a>Klasik dağıtım modeli

Klasik dağıtım modeli, eski nesil Azure dağıtım modelidir. Sanal makineler ve sanal makine ölçek kümeleri için genel bir vCPU kota sınırı uygular. Klasik dağıtım modelinin artık kullanılması önerilmez ve artık Kaynak Yöneticisi modeli tarafından değiştirildi.

Bu iki dağıtım modeli ve Kaynak Yöneticisi kullanmanın avantajları hakkında daha fazla bilgi edinmek için, bkz. [Kaynak Yöneticisi ve klasik dağıtım](../../azure-resource-manager/management/deployment-models.md).

Yeni bir abonelik oluşturulduğunda, buna varsayılan bir vCPU kotası atanır. Klasik dağıtım modeli kullanılarak yeni bir sanal makine dağıtılırsa, tüm bölgelerde yeni ve var olan vCPU kullanımının toplamı, klasik dağıtım modeli için onaylanan vCPU kotasını aşmamalıdır.

Kotalar hakkında daha fazla bilgi edinmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).

Klasik dağıtım modeli için vCPU kota sınırı artışı isteyebilirsiniz. Azure portal **Yardım + Destek** ya da **kullanım + kotalar** kullanın.

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-help--support"></a>Yardım + destek kullanarak abonelik düzeyindeki VM Serisi vCPU kotası artışına göre istek

Azure portal **Yardım + Destek** kullanarak bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

1. [Azure Portal](https://portal.azure.com) menüsünde **Yardım + Destek**' i seçin.

   ![Azure portal yardım + destek ' i seçin](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. **Yeni destek isteği**’ni seçin.

   ![Azure portal yeni bir destek isteği oluşturun](./media/resource-manager-core-quotas-request/new-support-request.png)

1. **Sorun türü**' nde **hizmet ve abonelik sınırları (kotalar)** öğesini seçin.

   ![Sorun türü olarak kotaları seçin](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   ![Kotayı artırmak için abonelik seçin](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. **Kota türü**Için, **işlem-VM (çekirdekler-vCPU 'lar) abonelik sınırı artışları**' ni seçin.

   ![Arttırmak için kota türünü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. **İleri ' yi seçin:** **Sorun ayrıntılarını**açma çözümleri. Ek bilgi sağlamak için **Ayrıntılar sağla** ' yı seçin.

   ![İsteğiniz hakkında yardımcı olacak ayrıntılar sağlayın](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. **Kota ayrıntıları**' nda, **Klasik** ' i seçin ve bir **konum**seçin.

   ![Dağıtım modeli ve konumu dahil olmak üzere ayrıntıları ekleme](./media/resource-manager-core-quotas-request/quota-details-classic.png)

1. **SKU ailesi**için, arttırmak üzere bir veya daha fazla SKU ailesinden birini seçin.

   ![Arttırabileceğiniz SKU ailesini belirtin](./media/resource-manager-core-quotas-request/sku-family-classic.png)

1. Abonelikte istediğiniz yeni limitleri girin. Bir satırı kaldırmak için **SKU ailesinden** SKU 'nun seçimini kaldırın veya atma "X" simgesini seçin. Her SKU ailesi için bir kota girdikten sonra, destek isteğine devam etmek için Kaydet ve **Kota ayrıntılarına** **devam et** ' i seçin.

   ![Yeni sınırları iste](./media/resource-manager-core-quotas-request/new-limits-classic.png)

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-usage--quotas"></a>Kullanım + kota kullanarak abonelik düzeyindeki VM Serisi vCPU kotası artışına göre istek

Azure portal **kullanım + kotaları** kullanarak bir destek isteği oluşturmak için aşağıdaki yönergeleri izleyin.

1. [Azure Portal](https://portal.azure.com), **abonelikleri**arayıp seçin.

   ![Azure portal abonelikler 'e git](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Kotasını artırmak istediğiniz aboneliği seçin.

   ![Değiştirilecek aboneliği seçin](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. **Kullanım + kotalar**' ı seçin.

   ![Bir abonelik için kullanım ve kotaları seçin](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. Sağ üst köşede **istek artışı**' nı seçin.

   ![Kotayı artırmak için seçin](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. **Hesaplama-VM (çekirdek-vCPU) abonelik sınırı** , **Kota türü**olarak artar.

   ![Kota türünü seçin](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. **İleri ' yi seçin:** **Sorun ayrıntılarını**açma çözümleri. Ek bilgi sağlamak için **Ayrıntılar sağla** ' yı seçin.

   ![İsteğiniz için ayrıntıları sağlayın](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. **Kota ayrıntıları**' nda, **Klasik** ve bir **konum**seçin.

   ![Dağıtım modeli ve konumu dahil olmak üzere Kota ayrıntılarını seçin](./media/resource-manager-core-quotas-request/quota-details-classic.png)

1. Bir artış için bir veya daha fazla SKU ailesi seçin.

   ![Artış için SKU ailesini seçin](./media/resource-manager-core-quotas-request/sku-family-classic.png)

1. Abonelikte istediğiniz yeni limitleri girin. Bir satırı kaldırmak için **SKU ailesinden** SKU 'nun seçimini kaldırın veya atma "X" simgesini seçin. Her SKU ailesi için bir kota girdikten sonra, destek isteğine devam etmek için Kaydet ve **Kota ayrıntılarına** **devam et** ' i seçin.

   ![Yeni kota girin](./media/resource-manager-core-quotas-request/new-limits-classic.png)

