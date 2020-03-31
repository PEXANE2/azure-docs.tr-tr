---
title: 'Azure Bastion ana bilgisayar: portal oluşturma'
description: Bu makalede, portalı kullanarak bir Azure Bastion ana bilgisayarı oluşturmayı öğrenin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 14a596d78fb1f560c62013e7e439ed60d3a29b8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366152"
---
# <a name="create-an-azure-bastion-host-using-the-portal"></a>Portalı kullanarak bir Azure Bastion ana bilgisayarı oluşturma

Bu makalede, Azure portalını kullanarak bir Azure Bastion ana bilgisayarı nasıl oluşturulabileceğiniz gösterilmektedir. Azure Bastion hizmetini sanal ağınızda kullanıma saðlamaniz sonra, sorunsuz RDP/SSH deneyimi ayný sanal ağdaki tüm Sanal M'ler için kullanılabilir. Azure Bastion dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başınadır.

Portalda tüm ayarları el ile belirterek veya varolan bir VM'ye karşılık gelen ayarları kullanarak yeni bir burç ana bilgisayarı kaynağı oluşturabilirsiniz. VM ayarlarını kullanarak bir burç ana bilgisayar oluşturmak için [hızlı başlangıç](quickstart-host-portal.md) makalesine bakın. İsteğe bağlı olarak, Azure Bastion ana bilgisayar oluşturmak için [Azure PowerShell'i](bastion-create-host-powershell.md) kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bastion aşağıdaki Azure ortak bölgelerinde kullanılabilir:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Bastion konağı oluşturma

Bu bölüm, Azure portalından yeni bir Azure Bastion kaynağı oluşturmanıza yardımcı olur.

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin.

1. **Yeni** sayfada, Pazar *Ara* **alanında, Bastion**yazın, ardından arama sonuçlarına ulaşmak için **Enter'u** tıklatın.

1. Sonuçlardan **Bastion'u**tıklatın. Yayımcının *Microsoft* olduğundan ve kategorinin *Ağ olduğundan*emin olun.

1. **Burç** sayfasında, **burç oluştur** sayfasını açmak için **Oluştur'u** tıklatın.

1. Bir **burç oluştur** sayfasında, yeni bir Bastion kaynağını yapılandırın. Bastion kaynağınızın yapılandırma ayarlarını belirtin.

    ![bir burç oluşturmak](./media/bastion-create-host-portal/settings.png)

    * **Abonelik**: Yeni bir Bastion kaynağı oluşturmak için kullanmak istediğiniz Azure aboneliği.
    * **Kaynak Grubu**: Yeni Bastion kaynağının oluşturulacağı Azure kaynak grubu. Varolan bir kaynak grubunuz yoksa, yeni bir kaynak oluşturabilirsiniz.
    * **Adı**: Yeni Bastion kaynağının adı
    * **Bölge**: Kaynağın oluşturulacağı Azure ortak bölgesi.
    * **Sanal ağ**: Bastion kaynağının oluşturulacağı sanal ağ. Bu işlem sırasında portalda yeni bir sanal ağ oluşturabilir veya varolan bir sanal ağ kullanabilirsiniz. Varolan bir sanal ağ kullanıyorsanız, varolan sanal ağın Bastion alt ağ gereksinimlerini karşılamak için yeterli boş adres alanına sahip olduğundan emin olun.
    * **Alt Ağ**: Yeni Bastion ana bilgisayarı kaynağının dağıtılacayacağı sanal ağındaki alt ağ. **AzureBastionSubnet**ad değerini kullanarak bir alt ağ oluşturmanız gerekir. Bu değer, Azure'un Bastion kaynaklarını hangi alt ağa dağıtacağını bilmesini sağlar. Bu, Ağ Geçidi alt ağından farklıdır. En az /27 veya daha büyük (/27, /26 vb.) bir alt ağ kullanmanız gerekir.
    
       **AzureBastionSubnet'i** herhangi bir rota tabloları veya delegasyonları olmadan oluşturun. **AzureBastionSubnet'te**Ağ Güvenlik Grupları kullanıyorsanız, [NSG'lerle Çalışma](bastion-nsg.md) makalesine bakın.
    * **Genel IP adresi**: RDP/SSH'ye erişilen Bastion kaynağının genel IP'si (bağlantı noktası 443 üzerinden). Yeni bir genel IP oluşturun veya varolan bir IP kullanın. Ortak IP adresi, oluşturduğunuz Bastion kaynağıyla aynı bölgede olmalıdır.
    * **Genel IP adresi adı**: Genel IP adresi kaynağının adı.
    * **Genel IP adresi SKU**: Bu ayar varsayılan olarak **Standart'a**göre önceden doldurulur. Azure Bastion yalnızca Standart Genel IP SKU'yu kullanır/destekler.
    * **Atama**: Bu ayar varsayılan olarak **Statik**olarak doldurulur.

1. Ayarları belirtmeyi bitirdikten sonra **Gözden Geçir + Oluştur'u**tıklatın. Bu değerleri doğrular. Doğrulama geçtikten sonra, oluşturma işlemini başlatabilirsiniz.
1. Burç **Oluştur** sayfasında **Oluştur'u**tıklatın.
1. Dağıtımınızın devam ettiğini bildiren bir ileti görürsünüz. Kaynaklar oluşturulduğunda durum bu sayfada görüntülenir. Bastion kaynağının oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* Ek bilgi için [Bastion SSS'yi](bastion-faq.md) okuyun.

* Azure Bastion alt ağıyla Ağ Güvenlik Grupları'nı kullanmak için Bkz. [NSG'lerle Çalışma.](bastion-nsg.md)