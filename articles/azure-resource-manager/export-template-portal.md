---
title: Azure Resource Manager şablonu Azure portal kullanarak dışarı aktarın
description: Aboneliğinizdeki kaynaklardan bir Azure Resource Manager şablonu dışarı aktarmak için Azure portal kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: tomfitz
ms.openlocfilehash: 15f527dfe517dcb3329b8b61243d7c5054eedb56
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71959711"
---
# <a name="single-and-multi-resource-export-to-a-template-in-azure-portal"></a>Azure portal bir şablona tek ve çoklu kaynak verme

Azure Resource Manager şablonları oluşturmaya yardımcı olmak için, var olan kaynaklardan bir şablonu dışarı aktarabilirsiniz. Verdiğiniz şablon, JSON sözdizimini ve kaynaklarınızı dağıtan özellikleri anlamanıza yardımcı olur. Gelecekteki dağıtımları otomatik hale getirmek için, verdiğiniz şablonla başlayın ve senaryonuz için değiştirin.

Kaynak Yöneticisi bir şablona dışarı aktarmak için bir veya daha fazla kaynak seçmenizi sağlar. Şablonda ihtiyacınız olan kaynaklara tam olarak odaklanırsınız.

Bu makalede, Portal üzerinden şablonların nasıl dışarı aktarılacağı gösterilmektedir. [Azure CLI](manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](manage-resource-groups-powershell.md#export-resource-groups-to-templates)veya [REST API](/rest/api/resources/resourcegroups/exporttemplate)de kullanabilirsiniz.

## <a name="choose-the-right-export-option"></a>Doğru dışa aktarma seçeneğini belirleyin

Bir şablonu dışarı aktarmanın iki yolu vardır:

* **Kaynak grubundan veya kaynaktan dışarı aktarın**. Bu seçenek, mevcut kaynaklardan yeni bir şablon oluşturur. Bu şablon, kaynak grubunun geçerli durumunun "Snapshot" ' dır. Kaynak grubunun tamamını veya kaynak grubu içindeki belirli kaynakları dışarı aktarabilirsiniz.

* **Dağıtımdan veya geçmişten önce dışarı aktarın**. Bu seçenek, dağıtım için kullanılan bir şablonun tam bir kopyasını alır.

Seçtiğiniz seçeneğe bağlı olarak, içe aktarılmış şablonların farklı nitelikleri vardır.

| Kaynak grubu veya kaynaktan | Dağıtımdan veya geçmişten önce |
| --------------------- | ----------------- |
| Şablon, kaynakların geçerli durumunun anlık görüntüsüdür. Dağıtımdan sonra yaptığınız el ile yapılan tüm değişiklikleri içerir. | Şablon, dağıtım sırasında yalnızca kaynakların durumunu gösterir. Dağıtımdan sonra yaptığınız tüm el ile değişiklikler dahil edilmez. |
| Bir kaynak grubundan dışarı aktarılacak kaynakları seçebilirsiniz. | Belirli bir dağıtımın tüm kaynakları dahil edilmiştir. Bu kaynakların bir alt kümesini seçemezsiniz veya farklı bir zamanda eklenen kaynaklar ekleyebilirsiniz. |
| Şablon, genellikle dağıtım sırasında ayarladığınız bazı özellikler dahil olmak üzere, kaynakların tüm özelliklerini içerir. Şablonu yeniden kullanmadan önce bu özellikleri kaldırmak veya temizlemek isteyebilirsiniz. | Şablon yalnızca dağıtım için gereken özellikleri içerir. Şablon kullanıma hazırdır. |
| Şablon muhtemelen yeniden kullanım için gereken tüm parametreleri içermez. Çoğu özellik değeri, şablonda sabit olarak kodlanır. Diğer ortamlarda şablonu yeniden dağıtmak için, kaynakları yapılandırma özelliğini artıran parametreler eklemeniz gerekir. | Şablon, farklı ortamlarda yeniden dağıtmayı kolaylaştıran parametreler içerir. |

Şablonu bir kaynak grubu veya kaynağından dışarı aktarın, ne zaman:

* Başlangıçtaki dağıtımdan sonra yapılan kaynaklardaki değişiklikleri yakalamanız gerekir.
* Hangi kaynakların verildiğini seçmek istiyorsunuz.

Şablonu dağıtımdan önce veya geçmişten önce dışarı aktarın, ne zaman:

* Kullanımı kolay bir şablon istiyorsunuz.
* Özgün dağıtımdan sonra yaptığınız değişiklikleri eklemeniz gerekmez.

## <a name="export-template-from-resource-group"></a>Şablonu kaynak grubundan dışarı aktar

Bir kaynak grubundan bir veya daha fazla kaynağı dışarı aktarmak için:

1. Dışarı aktarmak istediğiniz kaynakları içeren kaynak grubunu seçin.

1. Kaynak grubundaki tüm kaynakları dışarı aktarmak için tümü ' nü ve ardından **şablonu dışarı aktar**' ı seçin. **Şablonu dışarı aktar** seçeneği yalnızca en az bir kaynak seçildikten sonra etkin hale gelir.

   ![Tüm kaynakları dışarı aktar](./media/export-template-portal/select-all-resources.png)

1. Dışarı aktarma için belirli kaynakları seçmek üzere bu kaynakların yanındaki onay kutularını seçin. Ardından, **şablonu dışarı aktar**' ı seçin.

   ![Dışarı aktarılacak kaynakları seçin](./media/export-template-portal/select-resources.png)

1. İçe aktarılmış şablon görüntülenir ve indirilebilir.

   ![Şablonu göster](./media/export-template-portal/show-template.png)

## <a name="export-template-from-resource"></a>Şablonu kaynaktan dışarı aktar

Bir kaynağı dışarı aktarmak için:

1. Dışarı aktarmak istediğiniz kaynağı içeren kaynak grubunu seçin.

1. Dışarı aktarılacak kaynağı seçin.

   ![Kaynak seçin](./media/export-template-portal/select-link-resource.png)

1. Bu kaynak için sol bölmedeki **şablonu dışarı aktar** ' ı seçin.

   ![Kaynağı dışarı aktar](./media/export-template-portal/export-single-resource.png)

1. İçe aktarılmış şablon görüntülenir ve indirilebilir. Şablon yalnızca tek bir kaynağı içerir.

## <a name="export-template-before-deployment"></a>Dağıtımdan önce şablonu dışarı aktar

1. Dağıtmak istediğiniz Azure hizmetini seçin.

1. Yeni hizmet için değerleri girin.

1. Doğrulamayı geçirdikten sonra, ancak dağıtımı başlatmadan önce **Otomasyon için bir şablon indir**' i seçin.

   ![Şablonu indir](./media/export-template-portal/download-before-deployment.png)

1. Şablon görüntülenir ve indirilebilir.

   ![Şablonu göster](./media/export-template-portal/show-template-before-deployment.png)

## <a name="export-template-after-deployment"></a>Dağıtımdan sonra şablonu dışarı aktar

Mevcut kaynakları dağıtmak için kullanılan şablonu dışarı aktarabilirsiniz. Aldığınız şablon tam olarak dağıtım için kullanılan bir şablondur.

1. Dışarı aktarmak istediğiniz kaynak grubunu seçin.

1. **Dağıtımlar**altındaki bağlantıyı seçin.

   ![Dağıtım geçmişini seçin](./media/export-template-portal/select-deployment-history.png)

1. Dağıtım geçmişinden dağıtımlardan birini seçin.

   ![Dağıtım seçin](./media/export-template-portal/select-details.png)

1. **Şablon**seçin. Bu dağıtım için kullanılan şablon görüntülenir ve indirilmek üzere kullanılabilir.

   ![Şablon seç](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](manage-resource-groups-powershell.md#export-resource-groups-to-templates)veya [REST API](/rest/api/resources/resourcegroups/exporttemplate)ile şablonları dışarı aktarmayı öğrenin.
- Kaynak Yöneticisi Şablon sözdizimini öğrenmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](./resource-group-authoring-templates.md).
- Şablon geliştirmeyi öğrenmek için [adım adım öğreticiler](/azure/azure-resource-manager/)bölümüne bakın.
- Azure Resource Manager şablonu şemalarını görüntülemek için bkz. [şablon başvurusu](/azure/templates/).
