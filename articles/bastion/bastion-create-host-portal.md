---
title: Azure savunma Konağı oluşturma | Microsoft Docs
description: Bu makalede, Azure savunma Konağı oluşturmayı öğrenin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: f907dcb4427fd07a2c212e5de91ccce5e8198960
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990428"
---
# <a name="create-an-azure-bastion-host"></a>Azure savunma Konağı oluşturma

Bu makalede, Azure portal kullanarak bir Azure savunma ana bilgisayarı oluşturma işlemi gösterilmektedir. Sanal ağınızda Azure savunma hizmetini sağladığınızda, sorunsuz RDP/SSH deneyimi aynı sanal ağdaki tüm VM 'Ler tarafından kullanılabilir. Azure savunma dağıtımı, abonelik/hesap veya sanal makine başına değil, sanal ağ başına değildir.

Portalda tüm ayarları el ile belirterek veya var olan bir VM 'ye karşılık gelen ayarları kullanarak yeni bir savunma ana bilgisayarı kaynağı oluşturabilirsiniz. İsteğe bağlı olarak, Azure [PowerShell](bastion-create-host-powershell.md) 'i kullanarak bir Azure savunma Konağı oluşturabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki Azure ortak bölgelerinde kullanıma sunulmuştur:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="createhost"></a>Savunma Konağı oluşturma-ayarları belirtme

Bu bölüm Azure portal yeni bir Azure savunma kaynağı oluşturmanıza yardımcı olur.

1. [Azure Portal](https://portal.azure.com) menüsünde veya **giriş** sayfasında, **kaynak oluştur**' u seçin.

1. **Yeni** sayfada, *marketi ara* **alanında, giriş yazın ve**ardından arama sonuçlarına ulaşmak için **ENTER** ' a tıklayın.

1. Sonuçlardan, savunma ' ye **tıklayın.** Yayımcının *Microsoft* olduğundan ve kategorinin *ağ*olduğundan emin olun.

1. Savunma sayfasında **Oluştur** ' **a tıklayarak bir savunma oluştur** sayfasını açın.

1. Savunma **Oluştur** sayfasında, yeni bir savunma kaynağı yapılandırın. Savunma kaynağınız için yapılandırma ayarlarını belirtin.

    ![bir savunma oluşturun](./media/bastion-create-host-portal/settings.png)

    * **Abonelik**: yeni bir savunma kaynağı oluşturmak için kullanmak istediğiniz Azure aboneliği.
    * **Kaynak grubu**: yeni savunma kaynağının oluşturulacağı Azure Kaynak grubu. Mevcut bir kaynak grubunuz yoksa yeni bir tane oluşturabilirsiniz.
    * **Ad**: yeni savunma kaynağının adı
    * **Bölge**: kaynağın oluşturulacağı Azure ortak bölgesi.
    * **Sanal ağ**: savunma kaynağının oluşturulacağı sanal ağ. Bu işlem sırasında portalda yeni bir sanal ağ oluşturabilir veya var olan bir sanal ağı kullanabilirsiniz. Var olan bir sanal ağı kullanıyorsanız, var olan sanal ağın savunma alt ağ gereksinimlerine uyum sağlamak için yeterli boş adres alanı olduğundan emin olun.
    * **Alt ağ**: sanal ağınızdaki yeni savunma ana bilgisayar kaynağının dağıtılacağı alt ağ. **AzureBastionSubnet**ad değerini kullanarak bir alt ağ oluşturmanız gerekir. Bu değer, Azure 'un savunma kaynaklarını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az/27 veya daha büyük (/27,/26, vb.) bir alt ağ kullanmanız gerekir.
    
       Rota tabloları veya temsilcileri olmadan **AzureBastionSubnet** oluşturun. **AzureBastionSubnet**üzerinde ağ güvenlik grupları kullanıyorsanız [Nsgs ile çalışma](bastion-nsg.md) makalesine başvurun.
    * **Genel IP adresi**: RDP/SSH 'ye erişilecek savunma kaynağının genel IP 'si (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun veya var olan bir IP 'yi kullanın. Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.
    * **Genel IP adresi adı**: genel IP adresi kaynağının adı.
    * **Genel IP adresi SKU 'su**: Bu ayar varsayılan olarak **Standart**olarak doldurulur. Azure savunma yalnızca standart genel IP SKU 'sunu kullanır/destekler.
    * **Atama**: Bu ayar varsayılan olarak **statik**olarak doldurulur.

1. Ayarları belirtmeyi tamamladığınızda, **gözden geçir + oluştur**' a tıklayın. Bu, değerleri doğrular. Doğrulama başarılı olduktan sonra oluşturma işlemini başlatabilirsiniz.
1. Savunma **Oluştur** sayfasında **Oluştur**' a tıklayın.
1. Dağıtımınızın devam ettiğinden emin olarak bir ileti görürsünüz. Kaynaklar oluşturulduğundan bu sayfada durum görüntülenecektir. Savunma kaynağının oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

## <a name="createvmset"></a>Savunma Konağı oluşturma-VM ayarlarını kullanın

Portalda var olan bir VM 'yi kullanarak bir savunma Konağı oluşturursanız, sanal makinenize ve/veya sanal ağınıza karşılık gelen farklı ayarlar otomatik olarak varsayılan olarak kullanılır.

1. [Azure portalı](https://portal.azure.com) açın. Sanal makinenize gidin ve **Bağlan**' a tıklayın.

   ![VM bağlantısı](./media/bastion-create-host-portal/vmsettings.png)
1. Sağ kenar çubuğunda, savunma ' yı ve ardından **savunma ' yi** **kullanın**.

   ![Bastion](./media/bastion-create-host-portal/vmbastion.png)
1. Savunma sayfasında, aşağıdaki ayarlar alanlarını doldurun:

   * **Ad**: oluşturmak istediğiniz savunma konağının adı.
   * **Alt ağ**: sanal ağınızdaki, savunma kaynağına dağıtılacak alt ağ. Alt ağın **AzureBastionSubnet**adıyla oluşturulması gerekir. Bu, Azure 'un savunma kaynağını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az/27 veya daha büyük (/27,/26, vb.) bir alt ağ kullanmanız gerekir. Ağ güvenlik grupları, rota tabloları veya temsilciler olmadan alt ağ oluşturun. Daha sonra **AzureBastionSubnet**üzerinde ağ güvenlik grupları kullanmayı seçerseniz bkz. [NSG ile çalışma](bastion-nsg.md).
   
     **AzureBastionSubnet**oluşturmak için **alt ağ yapılandırmasını Yönet** ' e tıklayın.  Alt ağı oluşturmak için **Oluştur** ' a tıklayın ve ardından sonraki ayarlarla devam edin.
   * **Genel IP adresi**: RDP/SSH 'ye erişilecek savunma kaynağının genel IP 'si (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun veya var olan bir IP 'yi kullanın. Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.
   * **Genel IP adresi adı**: genel IP adresi kaynağının adı.
1. Doğrulama ekranında **Oluştur**' a tıklayın. Savunma kaynağı oluşturma ve dağıtma için 5 dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Ek bilgi için savunma [hakkında SSS](bastion-faq.md) makalesini okuyun.

* Azure savunma alt ağıyla ağ güvenlik gruplarını kullanmak için bkz. [NSG Ile çalışma](bastion-nsg.md).