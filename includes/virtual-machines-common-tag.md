---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: f6bd574c83d309ce6d6f54fdb1c7d23cb713420d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73182297"
---
## <a name="tagging-a-virtual-machine-through-templates"></a>Şablonlar aracılığıyla Sanal Makineetiketleme
İlk olarak, şablonlar aracılığıyla etiketleme bakalım. [Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) etiketleri aşağıdaki kaynaklara yerleştirir: Bilgi İşlem (Sanal Makine), Depolama (Depolama Hesabı) ve Ağ (Genel IP Adresi, Sanal Ağ ve Ağ Arabirimi). Bu şablon bir Windows VM içindir, ancak Linux VM'leri için uyarlanabilir.

[Şablon bağlantısından](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) **Azure'a Dağıt** düğmesini tıklatın. Bu, bu şablonu dağıtabileceğiniz [Azure portalına](https://portal.azure.com/) yönlendirecektir.

![Etiketlerle basit dağıtım](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Bu şablon aşağıdaki etiketleri içerir: *Bölüm*, *Uygulama*ve *Created By*. Farklı etiket adları istiyorsanız bu etiketleri doğrudan şablona ekleyebilir/edinebilirsiniz.

![Şablondaki Azure etiketleri](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Gördüğünüz gibi, etiketler bir üst üste (:) ile ayrılmış anahtar/değer çiftleri olarak tanımlanır. Etiketler bu biçimde tanımlanmalıdır:

        "tags": {
            "Key1" : "Value1",
            "Key2" : "Value2"
        }

Şablon dosyasını seçtiğiniz etiketlerle düzenlemeyi bitirdikten sonra kaydedin.

Ardından, **Parametreleri Edit** bölümünde, etiketlerinizin değerlerini doldurabilirsiniz.

![Azure portalında Etiketleri Edin](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Bu şablonu etiket değerlerinizle dağıtmak için **Oluştur'u** tıklatın.

## <a name="tagging-through-the-portal"></a>Portal üzerinden etiketleme
Kaynaklarınızı etiketlerle oluşturduktan sonra portaldaki etiketleri görüntüleyebilir, ekleyebilir ve silebilirsiniz.

Etiketlerinizi görüntülemek için etiketler simgesini seçin:

![Azure portalındaki etiketler simgesi](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Kendi Anahtar/Değer çiftinizi tanımlayarak portalüzerinden yeni bir etiket ekleyin ve kaydedin.

![Azure portalına yeni Etiket ekleme](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Yeni etiketiniz artık kaynağınızın etiketleri listesinde görünmelidir.

![Azure portalında kaydedilen Yeni Etiket](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

