---
title: Şablon kullanarak VM etiketleme
description: Bir şablonu kullanarak bir sanal makineyi etiketleme hakkında bilgi edinin.
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
ms.author: cynthn
author: cynthn
ms.date: 10/26/2018
ms.openlocfilehash: d1acbe82a086574a102e7897bbd3b99683c1185e
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94595041"
---
# <a name="tagging-a-vm-using-a-template"></a>Şablon kullanarak VM etiketleme


Bu makalede, Azure 'da bir sanal makinenin Kaynak Yöneticisi şablonu kullanılarak nasıl etiketleneceğini açıklanmaktadır. Etiketler, doğrudan bir kaynağa veya bir kaynak grubuna yerleştirilebilecek Kullanıcı tanımlı anahtar/değer çiftleridir. Azure Şu anda kaynak ve kaynak grubu başına en fazla 50 etiketi desteklemektedir. Etiketler, oluşturma sırasında veya var olan bir kaynağa eklenen bir kaynağa yerleştirilebilir.

[Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) şu kaynaklara Etiketler koyar: Işlem (sanal makine), depolama (depolama hesabı) ve ağ (genel IP adresi, sanal ağ ve ağ arabirimi). Bu şablon bir Windows sanal makinesi içindir, ancak Linux VM 'Ler için uyarlanmıştır.

[Şablon bağlantısından](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) **Azure 'a dağıt** düğmesine tıklayın. Bu, bu şablonu dağıtabileceğiniz [Azure Portal](https://portal.azure.com/) gezirsiniz.

![Etiketlerle basit dağıtım](./media/tag/deploy-to-azure-tags.png)

Bu şablon şu etiketleri içerir: *Departman* , *uygulama* ve *tarafından oluşturulan*. Farklı etiket adları istiyorsanız bu etiketleri doğrudan şablonda ekleyebilir/düzenleyebilirsiniz.

![Bir şablonda Azure etiketleri](./media/tag/azure-tags-in-a-template.png)

Görebileceğiniz gibi, Etiketler bir iki nokta (:) ile ayrılmış olarak anahtar/değer çiftleri olarak tanımlanır. Etiketlerin bu biçimde tanımlanması gerekir:

```config
"tags": {
    "Key1" : "Value1",
    "Key2" : "Value2"
}
```

Şablon dosyasını, istediğiniz etiketlerle düzenledikten sonra kaydedin.

Ardından, **parametreleri Düzenle** bölümünde etiketlerinizin değerlerini doldurabilirsiniz.

![Azure portal etiketleri düzenle](./media/tag/edit-tags-in-azure-portal.png)

Bu şablonu etiket değerlerinizle dağıtmak için **Oluştur** ' a tıklayın.


**Sonraki adımlar**

- Azure kaynaklarınızın etiketlenmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager genel bakış](../azure-resource-manager/management/overview.md) ve [etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/management/tag-resources.md).
- Etiketlerin Azure kaynaklarını kullanımınızı yönetmenize nasıl yardımcı olduğunu görmek için bkz. [Azure Faturanızı Anlama](../cost-management-billing/understand/review-individual-bill.md) ve [Microsoft Azure Kaynak tüketiminiz hakkında öngörüler elde](../cost-management-billing/manage/usage-rate-card-overview.md)etme.
