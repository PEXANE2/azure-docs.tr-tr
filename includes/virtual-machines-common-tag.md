---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 196dfdc045fd60e4a253857087177f478f50ea24
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050498"
---
## <a name="tagging-a-virtual-machine-through-templates"></a>Bir sanal makineyi şablonlar aracılığıyla etiketleme
İlk olarak, şablonlar aracılığıyla etiketleme konusuna bakalım. [Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) şu kaynaklara Etiketler koyar: Işlem (sanal makine), depolama (depolama hesabı) ve ağ (genel IP adresi, sanal ağ ve ağ arabirimi). Bu şablon bir Windows sanal makinesi içindir, ancak Linux VM 'Ler için uyarlanmıştır.

[Şablon bağlantısından](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) **Azure 'a dağıt** düğmesine tıklayın. Bu, bu şablonu dağıtabileceğiniz [Azure Portal](https://portal.azure.com/) gezirsiniz.

![Etiketlerle basit dağıtım](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Bu şablon şu etiketleri içerir: *Departman*, *uygulama*ve *tarafından oluşturulan*. Farklı etiket adları istiyorsanız bu etiketleri doğrudan şablonda ekleyebilir/düzenleyebilirsiniz.

![Bir şablonda Azure etiketleri](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Görebileceğiniz gibi, Etiketler bir iki nokta (:) ile ayrılmış olarak anahtar/değer çiftleri olarak tanımlanır. Etiketlerin bu biçimde tanımlanması gerekir:

```config
"tags": {
    "Key1" : "Value1",
    "Key2" : "Value2"
}
```

Şablon dosyasını, istediğiniz etiketlerle düzenledikten sonra kaydedin.

Ardından, **parametreleri Düzenle** bölümünde etiketlerinizin değerlerini doldurabilirsiniz.

![Azure portal etiketleri düzenle](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Bu şablonu etiket değerlerinizle dağıtmak için **Oluştur** ' a tıklayın.

## <a name="tagging-through-the-portal"></a>Portal üzerinden etiketleme
Kaynaklarınızı etiketlerle oluşturduktan sonra, portalda etiketleri görüntüleyebilir, ekleyebilir ve silebilirsiniz.

Etiketlerinizi görüntülemek için Etiketler simgesini seçin:

![Azure portal Etiketler simgesi](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Kendi anahtar/değer çiftini tanımlayarak Portal üzerinden yeni bir etiket ekleyin ve kaydedin.

![Azure portal yeni etiket ekle](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Yeni etiketlerinizin kaynağı için Etiketler listesinde görüntülenmesi gerekir.

![Yeni etiket Azure portal kaydedilir](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

