---
title: Azure DevTest Labs'daki laboratuvarınızdaki kotaları ve sınırları ölçeklendirin | Microsoft Dokümanlar
description: Bu makalede, Azure DevTest Labs'da laboratuvarınızı nasıl ölçeklendirebileceğiniz açıklanmaktadır. Kullanım kotalarınızı ve sınırlarınızı görüntüleyin ve artış isteğinde bulunun.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: f3299c24bc751263ccd284a4fddb492fba5dd1e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76761194"
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>DevTest Labs'da kotaları ve sınırları ölçeklendirin
DevTest Labs'da çalışırken, bazı Azure kaynaklarının DevTest Labs hizmetini etkileyebilecek belirli varsayılan sınırları olduğunu fark edebilirsiniz. Bu **sınırlarkota**olarak adlandırılır.

> [!NOTE]
> DevTest Labs hizmeti herhangi bir kota uygulamaz. Karşılaşabileceğiniz tüm kotalar, genel Azure aboneliğinin varsayılan kısıtlamalarıdır.

Kotasına ulaşana kadar her Azure kaynağını kullanabilirsiniz. Her abonelikte ayrı kotalar vardır ve kullanım abonelik başına izlenir.

Örneğin, her aboneliğin varsayılan kotası 20 çekirdektir. Yani, laboratuarınızda her biri dört çekirdekli VM oluşturuyorsanız, yalnızca beş VM oluşturabilirsiniz.

[Azure Abonelik ve Hizmet Sınırları,](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) Azure kaynakları için en yaygın kotalardan bazılarını listeler. Bir laboratuvarda en sık kullanılan ve kotalarla karşılaşabileceğiniz kaynaklar arasında VM çekirdekleri, ortak IP adresleri, ağ arabirimi, yönetilen diskler, RBAC rol ataması ve ExpressRoute devreleri yer alır.

## <a name="view-your-usage-and-quotas"></a>Kullanımınızı ve kotalarınızı görüntüleyin
Bu adımlar, belirli Azure kaynakları için aboneliğinizdeki geçerli kotaları nasıl görüntülediğinizi ve kullandığınız her kotanın yüzde kaçını nasıl göreceğinizi gösterir.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Daha Fazla Hizmet'i**seçin ve ardından listeden **Faturalandırma'yı** seçin.
1. Faturalama bıçağında bir abonelik seçin.
4. **Kullanım + kotaları**seçin.

   ![Kullanım ve kotadüğmesi](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   Kullanım + kotalar bıçak görünür, bu abonelik ve kaynak başına kullanılan kota yüzdesi kullanılabilir farklı kaynakları listeler.

   ![Kotalar ve kullanım](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Aboneliğinizde daha fazla kaynak isteme
Kota sınırına ulaşırsanız, bir kaynaktaki varsayılan sınır Azure [Abonelik ve Hizmet Sınırları'nda](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)açıklandığı gibi maksimum sınıra kadar artırılabilir.

Bu adımlar, [Azure portalı](https://go.microsoft.com/fwlink/p/?LinkID=525040)üzerinden kota artışı nın nasıl istendiğini gösterir.

1. **Daha Fazla Hizmet**seçin, **Faturalandırma'yı**seçin ve ardından **Kullanım + kotaları**seçin.
1. Kullanım + kotalar bıçak, **İstek Artır** düğmesini seçin.

   ![Artış isteği düğmesi](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. İsteği tamamlamak ve göndermek için, **Yeni destek isteği** formunun üç sekmesinde de gerekli bilgileri doldurun.

   ![İstek artış formu](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Azure Limitlerini ve Artışlarını anlamak,](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) kota artışı istemek için Azure desteğine başvurma hakkında daha fazla bilgi sağlar.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Sonraki adımlar
* [DevTest Labs Azure Kaynak Yöneticisi QuickStart şablon galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)keşfedin.
