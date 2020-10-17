---
title: "Hızlı başlangıç: Azure savunma yapılandırma ve özel IP adresi ve tarayıcı aracılığıyla bir VM 'ye bağlanma"
titleSuffix: Azure Bastion
description: Bu hızlı başlangıç makalesinde bir sanal makineden Azure savunma ana bilgisayarı oluşturmayı ve özel IP adresi aracılığıyla tarayıcınızla güvenli bir şekilde sanal makineye nasıl bağlanacağınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 10/15/2020
ms.author: cherylmc
ms.openlocfilehash: 325f39b695d80c14ed7097d071380b937458546c
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92150448"
---
# <a name="quickstart-connect-to-a-vm-securely-through-a-browser-via-private-ip-address"></a>Hızlı başlangıç: özel IP adresi aracılığıyla bir tarayıcı aracılığıyla güvenli bir şekilde VM 'ye bağlanma

Azure portal ve Azure savunma kullanarak tarayıcınız aracılığıyla bir sanal makineye (VM) bağlanabilirsiniz. Bu hızlı başlangıç makalesinde, Azure savunma 'yı VM ayarlarınıza göre yapılandırma ve ardından Portal üzerinden sanal makinenize bağlama işlemi gösterilmektedir. VM 'nin genel IP adresi, istemci yazılımı, Aracısı veya özel bir yapılandırmaya sahip olması gerekmez. Hizmet sağlandıktan sonra, RDP/SSH deneyimi aynı sanal ağdaki tüm sanal makineler için kullanılabilir. Azure savunma hakkında daha fazla bilgi için bkz. [Azure](bastion-overview.md)savunma nedir?.

## <a name="prerequisites"></a><a name="prereq"></a>Önkoşullar

* Etkin aboneliği olan bir Azure hesabı. Yoksa, [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Savunma kullanarak bir VM 'ye tarayıcı aracılığıyla bağlanabilme için, Azure portal oturum açabiliyor olmanız gerekir.

* Bir sanal ağdaki Windows sanal makinesi. VM yoksa [hızlı başlangıç: VM oluşturma](../virtual-machines/windows/quick-create-portal.md)' yı kullanarak bir tane oluşturun.

  * Örnek değerlere ihtiyacınız varsa, bkz. belirtilen [örnek değerleri](#values).
  * Zaten bir sanal ağınız varsa, Sanal ağınızı oluştururken ağ sekmesinden seçtiğinizden emin olun.
  * Zaten bir sanal ağınız yoksa, VM 'nizi oluşturduğunuz sırada bir tane oluşturabilirsiniz.
  * Azure savunma aracılığıyla bağlanabilmek için bu VM 'nin genel IP adresine sahip olmanız gerekmez.

* Gerekli VM rolleri:
  * Sanal makinede okuyucu rolü.
  * Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü.
  
* Gerekli VM bağlantı noktaları:
  * Gelen bağlantı noktaları: RDP (3389)

### <a name="example-values"></a><a name="values"></a>Örnek değerler

Bu yapılandırmayı oluştururken aşağıdaki örnek değerleri kullanabilir veya kendi kodunuzu kullanabilirsiniz.

**Temel VNet ve VM değerleri:**

|**Ad** | **Değer** |
| --- | --- |
| Sanal makine| TestVM |
| Kaynak grubu | TestRG |
| Bölge | Doğu ABD |
| Sanal ağ | TestVNet1 |
| Adres alanı | 10.0.0.0/16 |
| Alt ağlar | Ön uç: 10.0.0.0/24 |

**Azure savunma değerleri:**

|**Ad** | **Değer** |
| --- | --- |
| Name | TestVNet1-savunma |
| + Alt ağ adı | AzureBastionSubnet |
| AzureBastionSubnet adresleri | VNet adres alanınızda/27 alt ağ maskesiyle bir alt ağ. Örneğin, 10.0.1.0/27.  |
| Genel IP adresi |  Yeni oluştur |
| Genel IP adresi adı | VNet1BastionPIP  |
| Genel IP adresi SKU 'SU |  Standart  |
| Atama  | Statik |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Savunma Konağı oluşturma

Bir savunma konağını yapılandırmanın birkaç farklı yolu vardır. Aşağıdaki adımlarda, doğrudan sanal makinenizde Azure portal bir savunma ana bilgisayarı oluşturacaksınız. Bir VM 'den bir konak oluşturduğunuzda, sanal makinenize ve/veya sanal ağınıza karşılık gelen farklı ayarlar otomatik olarak doldurulur.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Bağlanmak istediğiniz VM 'ye gidin ve ardından **Bağlan**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/vm-settings.png" alt-text="sanal makine ayarları" lightbox="./media/quickstart-host-portal/vm-settings.png":::
1. Açılan menüden, savunma ' **yı seçin.**
1. **Testvm 'de | Bağlan sayfasında**, **kullan**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="sanal makine ayarları" border="false":::

1. Savunma sayfasında **,** aşağıdaki ayarlar alanlarını doldurun:

   * **Ad**: savunma konağını adlandırın.
   * **Alt ağ**: Bu, savunma kaynağının dağıtılacağı sanal ağ adres alanıdır. Alt ağın **AzureBastionSubnet**adıyla oluşturulması gerekir. En az/27 veya daha büyük (/27,/26,/25 vb.) bir alt ağ kullanın.
   * **Alt ağ yapılandırmasını Yönet**' i seçin.
1. **Alt ağlar** sayfasında, **+ alt ağ**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/subnet.png" alt-text="sanal makine ayarları":::
    
1. **Alt ağ ekle** sayfasında, **ad**için **AzureBastionSubnet**yazın.
   * Alt ağ adres aralığı için, sanal ağ adres alanınızda bulunan bir alt ağ adresi seçin.
   * Diğer ayarları ayarlamamayın. Alt ağ değişikliklerini kabul etmek ve kaydetmek için **Tamam ' ı** seçin.

   :::image type="content" source="./media/quickstart-host-portal/add-subnet.png" alt-text="sanal makine ayarları":::
1. Savunma **sayfasına dönmek** için tarayıcınızda geri düğmesine tıklayın ve değerleri belirtmeye devam edin.
   * **Genel IP adresi**: **Yeni oluştur**olarak bırakın.
   * **Genel IP adresi adı**: genel IP adresi kaynağının adı.
   * **Atama**: varsayılan değer static ' tir. Azure savunma için dinamik bir atama kullanamazsınız.
   * **Kaynak grubu**: VM ile aynı kaynak grubu.

   :::image type="content" source="./media/quickstart-host-portal/validate.png" alt-text="sanal makine ayarları":::
1. Savunma konağını oluşturmak için **Oluştur** ' u seçin. Azure ayarlarınızı doğrular ve ardından Konağı oluşturur. Ana bilgisayar ve kaynakları, oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

## <a name="connect"></a><a name="connect"></a>Bağlan

Sanal ağa bağlantı dağıtıldıktan sonra ekran bağlan sayfasına dönüşür.

1. Sanal makineniz için Kullanıcı adını ve parolayı yazın. Ardından **Bağlan**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/connect-vm.png" alt-text="sanal makine ayarları":::
1. Bu sanal makineyle RDP bağlantısı, 443 numaralı bağlantı noktasını ve savunma hizmetini kullanarak doğrudan Azure portal (HTML5 üzerinden) açılır.

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="sanal makine ayarları":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağ ve sanal makineler kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **arama** kutusuna kaynak grubunuzun adını girip arama sonuçlarından seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Kaynak **grubunun adını yazıp** **Sil**' i seçerek kaynak grubunuzu girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal ağınız için bir savunma ana bilgisayarı oluşturdunuz ve ardından savunma aracılığıyla güvenli bir şekilde sanal makineye bağlanırsınız. Daha sonra, bir sanal makine ölçek kümesine bağlanmak istiyorsanız aşağıdaki adımla devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Azure savunma kullanarak bir sanal makine ölçek kümesine bağlanma](bastion-connect-vm-scale-set.md)
