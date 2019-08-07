---
title: Azure savunma Konağı oluşturma | Microsoft Docs
description: Bu makalede, Azure savunma Konağı oluşturmayı öğrenin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: cherylmc
ms.openlocfilehash: 4c283e840208ab9f53d084ca17221ea2c835b794
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814149"
---
# <a name="create-an-azure-bastion-host-preview"></a>Azure savunma Konağı (Önizleme) oluşturma

Bu makalede bir Azure savunma ana bilgisayarı oluşturma işlemi gösterilmektedir. Sanal ağınızda Azure savunma hizmetini sağladığınızda, sorunsuz RDP/SSH deneyimi aynı sanal ağdaki tüm VM 'leriniz için kullanılabilir. Bu dağıtım, abonelik/hesap veya sanal makine değil sanal ağ başına yapılır.

Bir savunma ana bilgisayar kaynağı oluşturabileceğiniz iki yol vardır:

* Azure portal kullanarak bir savunma kaynağı oluşturun.
* Mevcut VM ayarlarını kullanarak Azure portal bir savunma kaynağı oluşturun.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Genel Önizleme, aşağıdaki Azure ortak bölgeleriyle sınırlandırılmıştır:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="createhost"></a>Savunma Konağı oluşturma

Bu bölüm Azure portal yeni bir Azure savunma kaynağı oluşturmanıza yardımcı olur.

1. [Azure Portal-](https://aka.ms/BastionHost)savunma Önizlemedeki giriş sayfasından **+ kaynak oluştur ' a**tıklayın. Bu önizleme için portala erişmek için sunulan bağlantıyı, normal Azure portal değil, kullandığınızdan emin olun.

1. **Yeni** sayfada, *marketi ara* alanında, giriş yazın ve ardından arama sonuçlarınaulaşmak için **ENTER** ' a tıklayın.

1. Sonuçlardan, savunma **(Önizleme)** seçeneğine tıklayın. Yayımcının *Microsoft* olduğundan ve kategorinin *ağ*olduğundan emin olun.

1. Savunma **(Önizleme)** sayfasında **Oluştur** ' a tıklayarak **bir savunma oluştur** sayfasını açın.

1. Savunma **Oluştur** sayfasında, yeni bir savunma kaynağı yapılandırın. Savunma kaynağınız için yapılandırma ayarlarını belirtin.

    ![bir savunma oluşturun](./media/bastion-create-host-portal/settings.png)

    * **Abonelik**: Yeni bir savunma kaynağı oluşturmak için kullanmak istediğiniz Azure aboneliği.
    * **Kaynak grubu**: Yeni savunma kaynağının oluşturulacağı Azure Kaynak grubu. Mevcut bir kaynak grubunuz yoksa yeni bir tane oluşturabilirsiniz.
    * **Ad**: Yeni savunma kaynağının adı
    * **Bölge**: Kaynağın oluşturulacağı Azure ortak bölgesi.
    * **Sanal ağ**: Savunma kaynağının oluşturulacağı sanal ağ. Bu işlem sırasında portalda yeni bir sanal ağ oluşturabilirsiniz. Bu durumda, mevcut bir sanal ağı kullanmak zorunda kalmazsınız. Var olan bir sanal ağı kullanıyorsanız, var olan sanal ağın savunma alt ağ gereksinimlerine uyum sağlamak için yeterli boş adres alanı olduğundan emin olun.
    * **Alt ağ**: Sanal ağınızdaki, yeni savunma ana bilgisayar kaynağının dağıtılacağı alt ağ. **AzureBastionSubnet**ad değerini kullanarak bir alt ağ oluşturmanız gerekir. Bu değer, Azure 'un savunma kaynaklarını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az bir/27 veya daha büyük alt ağın (/27,/26, vb.) bir alt ağını kullanmanız gerekir. Rota tabloları veya temsilcileri olmadan **AzureBastionSubnet** oluşturun. **AzureBastionSubnet**üzerinde ağ güvenlik grupları kullandığınızda [NSG 'ler ile çalışma](bastion-nsg.md)bölümüne bakın.
    * **Genel IP adresi**: RDP/SSH 'ye erişilecek savunma kaynağının genel IP 'si (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun veya var olan bir IP 'yi kullanın. Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.
    * **Genel IP adresi adı**: Genel IP adresi kaynağının adı.
    * **Genel IP adresi SKU 'su**: Varsayılan olarak **Standart**öğesine önceden doldurulur. Azure savunma yalnızca standart genel IP SKU 'sunu kullanır/destekler.
    * **Atama**: Varsayılan olarak **statik**olarak doldurulur.

1. Ayarları belirtmeyi tamamladığınızda, **gözden geçir + oluştur**' a tıklayın. Bu, değerleri doğrular. Doğrulama başarılı olduktan sonra oluşturma işlemini başlatabilirsiniz.
1. Savunma Oluştur sayfasında **Oluştur**' a tıklayın.
1. Dağıtımınızın devam ettiğinden emin olarak bir ileti görürsünüz. Kaynaklar oluşturulduğundan bu sayfada durum görüntülenecektir. Savunma kaynağının oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

## <a name="createvmset"></a>VM ayarlarını kullanarak bir savunma Konağı oluşturma

Portalda var olan bir VM 'yi kullanarak bir savunma Konağı oluşturursanız, sanal makinenize ve/veya sanal ağınıza otomatik olarak varsayılan ayarlar uygulanır.

1. Azure portal savunma [önizlemesinde](https://aka.ms/BastionHost)sanal makinenize gidin ve **Bağlan**' a tıklayın.

    ![VM bağlantısı](./media/bastion-create-host-portal/vmsettings.png)

1. Sağ kenar çubuğunda, savunma 'yı ve ardından savunma ' yi **kullanın**.

    ![Bastion](./media/bastion-create-host-portal/vmbastion.png)

1. Savunma sayfasında, aşağıdaki ayarlar alanlarını doldurun:

    * **Ad**: Oluşturmak istediğiniz savunma konağının adı.
    * **Alt ağ**: Sanal ağınız içindeki savunma kaynağına dağıtılacak alt ağ. Alt ağın **AzureBastionSubnet**adıyla oluşturulması gerekir. Bu, Azure 'un savunma kaynağını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. Azure savunma alt ağını oluşturmak için **alt ağ yapılandırmasını Yönet** ' e tıklayın. En az bir/27 veya daha büyük alt ağ (/27,/26, vb.) kullanmanızı kesinlikle öneririz. Herhangi bir ağ güvenlik grubu, yol tablosu veya temsilci olmadan **AzureBastionSubnet** oluşturun. Alt ağı oluşturmak için **Oluştur** ' a tıklayın ve ardından sonraki ayarlarla devam edin.

      ![Bastion](./media/bastion-create-host-portal/subnet.png)
      
    * **Genel IP adresi**: RDP/SSH 'ye erişilecek savunma kaynağının genel IP 'si (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun veya var olan bir IP 'yi kullanın. Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.
    * **Genel IP adresi adı**: Genel IP adresi kaynağının adı.
1. Doğrulama ekranında **Oluştur**' a tıklayın. Savunma kaynağının oluşturulması ve dağıtılması için yaklaşık 5 dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md) makalesini okuyun
