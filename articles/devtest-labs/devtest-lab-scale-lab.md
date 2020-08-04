---
title: Azure DevTest Labs 'de laboratuvarınızda kotaları ve sınırları ölçeklendirin | Microsoft Docs
description: Bu makalede, Azure DevTest Labs ' de laboratuvarınızı nasıl ölçekleyebileceğinizi anlatmaktadır. Kullanım kotaları ve limitlerinizi görüntüleyin ve artış isteyin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 2166eaab073e2abc24ca24494ae13eb876db1fcf
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533963"
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>DevTest Labs 'de kotaları ve sınırları ölçeklendirin
DevTest Labs 'de çalışırken, bazı Azure kaynakları için DevTest Labs hizmetini etkileyebilecek bazı varsayılan limitlerin olduğunu fark edebilirsiniz. Bu sınırlar, **Kotalar**olarak adlandırılır.

> [!NOTE]
> DevTest Labs hizmeti hiçbir kota vermez. Karşılaşabileceğiniz kotalar Genel Azure aboneliğinin varsayılan kısıtlamalardır.

Kotasına ulaşana kadar her bir Azure kaynağını kullanabilirsiniz. Her abonelikte ayrı kotalar vardır ve kullanım abonelik başına izlenir.

Örneğin, her aboneliğin varsayılan bir 20 çekirdek kotası vardır. Bu nedenle, laboratuvarınızda sanal makineleri dört çekirdekli bir şekilde oluşturuyorsanız yalnızca beş VM oluşturabilirsiniz.

[Azure aboneliği ve hizmet limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md) , Azure kaynakları için en yaygın kotaların bazılarını listeler. Bir laboratuvarda en yaygın olarak kullanılan ve kotaları ile ilgili olan kaynaklar VM çekirdekleri, genel IP adresleri, ağ arabirimi, yönetilen diskler, Azure rol ataması ve ExpressRoute devreleri dahil.

## <a name="view-your-usage-and-quotas"></a>Kullanımınızı ve kotaları görüntüleme
Bu adımlarda, belirli Azure kaynakları için aboneliğinizdeki geçerli kotaları görüntüleme ve hangi kotanın kullanıldığı her kotayı görme işlemleri gösterilmektedir.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. **Diğer hizmetler**' i seçin ve ardından listeden **faturalandırma** ' i seçin.
1. Faturalama dikey penceresinde bir abonelik seçin.
4. **Kullanım + kotalar**' ı seçin.

   ![Kullanım ve Kotalar düğmesi](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas-new.png)

   Kullanım + kotalar dikey penceresi görünür ve bu abonelikte bulunan farklı kaynakları listelemek ve kaynak başına kullanılan kotanın yüzdesi.

   ![Kotalar ve kullanım](./media/devtest-lab-scale-lab/devtestlab-view-quotas-new.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Aboneliğinizde daha fazla kaynak isteme
Bir kota Cap 'e ulaşırsanız, bir abonelikteki kaynağın varsayılan sınırı, [Azure aboneliği ve hizmet sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md)bölümünde açıklandığı gibi maksimum sınıra kadar artırılabilir.

Bu adımlarda, [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)aracılığıyla kota artışı isteme isteği gösterilmektedir.

1. **Daha fazla hizmet**seçin, **faturalandırma**' i seçin ve ardından **kullanım + kotalar**' ı seçin.
1. Kullanım + kotalar dikey penceresinde **artış iste** düğmesini seçin.

   ![Artış düğmesine iste](./media/devtest-lab-scale-lab/devtestlab-request-increase-new.png)

1. İsteği tamamlayıp göndermek için, **Yeni destek isteği** formunun üç sekmesindeki tüm gerekli bilgileri doldurun.

   ![İstek artış formu](./media/devtest-lab-scale-lab/devtestlab-support-form-new.png)

[Azure sınırlarını ve artışlarının anlaşılması](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) , kota artışı Istemek için Azure desteği 'ne başvurma hakkında daha fazla bilgi sağlar.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Sonraki adımlar
* [DevTest Labs Azure Resource Manager hızlı başlangıç şablonu galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)gezin.
