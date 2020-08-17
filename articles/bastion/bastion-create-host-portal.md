---
title: 'Azure savunma Konağı oluşturma: Portal'
description: Bu makalede, bir sanal ağdaki tüm sanal makinelere RDP/SSH bağlantısı sağlayan bir Azure savunma ana bilgisayarı oluşturma hakkında bilgi edinin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 04/24/2020
ms.author: cherylmc
ms.openlocfilehash: 1e0bc9a17a12afac6d91867b19aba0f90b7f6edb
ms.sourcegitcommit: 2bab7c1cd1792ec389a488c6190e4d90f8ca503b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88270759"
---
# <a name="create-an-azure-bastion-host-using-the-portal"></a>Portalı kullanarak bir Azure savunma Konağı oluşturma

Bu makalede, Azure portal kullanarak bir Azure savunma ana bilgisayarı oluşturma işlemi gösterilmektedir. Sanal ağınızda Azure savunma hizmetini sağladığınızda, sorunsuz RDP/SSH deneyimi aynı sanal ağdaki tüm VM 'Ler tarafından kullanılabilir. Azure savunma dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başına değildir.

Portalda tüm ayarları el ile belirterek veya var olan bir VM 'ye karşılık gelen ayarları kullanarak yeni bir savunma ana bilgisayarı kaynağı oluşturabilirsiniz. VM ayarlarını kullanarak bir savunma konağı oluşturmak için [hızlı başlangıç](quickstart-host-portal.md) makalesine bakın. İsteğe bağlı olarak, Azure savunma konağı oluşturmak için [Azure PowerShell](bastion-create-host-powershell.md) kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki Azure ortak bölgelerinde kullanıma sunulmuştur:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Savunma Konağı oluşturma

Bu bölüm Azure portal yeni bir Azure savunma kaynağı oluşturmanıza yardımcı olur.

1. [Azure portalı](https://portal.azure.com) menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.

1. **Yeni** sayfada, *marketi ara* **alanında, giriş yazın ve**ardından arama sonuçlarına ulaşmak için **ENTER** ' a tıklayın.

1. Sonuçlardan, savunma ' ye **tıklayın.** Yayımcının *Microsoft* olduğundan ve kategorinin *ağ*olduğundan emin olun.

1. Savunma sayfasında **Oluştur** ' **a tıklayarak bir savunma oluştur** sayfasını açın. **Bastion**

1. Savunma **Oluştur** sayfasında, yeni bir savunma kaynağı yapılandırın. Savunma kaynağınız için yapılandırma ayarlarını belirtin.

    ![bir savunma oluşturun](./media/bastion-create-host-portal/settings.png)

    * **Abonelik**: yeni bir savunma kaynağı oluşturmak için kullanmak istediğiniz Azure aboneliği.
    * **Kaynak grubu**: yeni savunma kaynağının oluşturulacağı Azure Kaynak grubu. Mevcut bir kaynak grubunuz yoksa yeni bir tane oluşturabilirsiniz.
    * **Ad**: yeni savunma kaynağının adı
    * **Bölge**: kaynağın oluşturulacağı Azure ortak bölgesi.
    * **Sanal ağ**: savunma kaynağının oluşturulacağı sanal ağ. Bu işlem sırasında portalda yeni bir sanal ağ oluşturabilir veya var olan bir sanal ağı kullanabilirsiniz. Var olan bir sanal ağı kullanıyorsanız, var olan sanal ağın savunma alt ağ gereksinimlerine uyum sağlamak için yeterli boş adres alanı olduğundan emin olun.
    * **Alt ağ**: sanal ağınızdaki yeni savunma konağının dağıtılacağı alt ağ. Alt ağ, savunma konağına ayrılmıştır ve **AzureBastionSubnet**olarak adlandırılmalıdır. Bu alt ağ en az/27 veya daha büyük olmalıdır.
    
       **AzureBastionSubnet** , [Kullanıcı tanımlı yolları](../virtual-network/virtual-networks-udr-overview.md#custom-routes)desteklemez, ancak [ağ güvenlik gruplarını](bastion-nsg.md)destekler.
    * **Genel IP adresi**: RDP/SSH 'ye erişilecek savunma kaynağının genel IP 'si (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun veya var olan bir IP 'yi kullanın. Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.
    * **Genel IP adresi adı**: genel IP adresi kaynağının adı.
    * **Genel IP adresi SKU 'su**: Bu ayar varsayılan olarak **Standart**olarak doldurulur. Azure savunma yalnızca standart genel IP SKU 'sunu kullanır/destekler.
    * **Atama**: Bu ayar varsayılan olarak **statik**olarak doldurulur.

1. Ayarları belirtmeyi tamamladığınızda, **gözden geçir + oluştur**' a tıklayın. Bu, değerleri doğrular. Doğrulama başarılı olduktan sonra oluşturma işlemini başlatabilirsiniz.
1. Savunma **Oluştur** sayfasında **Oluştur**' a tıklayın.
1. Dağıtımınızın devam ettiğinden emin olarak bir ileti görürsünüz. Kaynaklar oluşturulduğundan bu sayfada durum görüntülenecektir. Savunma kaynağının oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* Ek bilgi için savunma [hakkında SSS](bastion-faq.md) makalesini okuyun.

* Azure savunma alt ağıyla ağ güvenlik gruplarını kullanmak için bkz. [NSG Ile çalışma](bastion-nsg.md).
