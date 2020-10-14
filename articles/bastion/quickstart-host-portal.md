---
title: "Hızlı başlangıç: bir VM 'den savunma Konağı oluşturma ve özel IP adresi aracılığıyla bağlanma"
titleSuffix: Azure Bastion
description: Bu hızlı başlangıç makalesinde, bir sanal makineden Azure savunma ana bilgisayarı oluşturmayı ve özel bir IP adresi kullanarak güvenli bir şekilde bağlamayı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: quickstart
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 6f451e7b115c00bc7b2cf350e00b9f704ab1d29f
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019061"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>Hızlı başlangıç: özel IP adresi ve Azure savunma kullanarak bir sanal makineye bağlanma

Bu hızlı başlangıç makalesinde, Azure savunma ve Azure portal kullanarak tarayıcınızla bir sanal makineye nasıl bağlanabilmeniz gösterilmektedir. Azure VM 'nizden Azure portal sanal ağınıza bir savunma dağıtımı yapabilirsiniz. Savunma 'yı dağıttıktan sonra, Azure portal kullanarak VM 'ye özel IP adresi aracılığıyla bağlanabilirsiniz. VM 'nizin genel bir IP adresi veya özel bir yazılıma ihtiyacı yoktur. VNet 'iniz için doğrudan sanal ağınız için bir savunma ana bilgisayarı oluşturmanın avantajlarından biri, ayarların çoğunun sizin için önceden doldurulduğu bir avantajdır.

Hizmet sağlandıktan sonra, RDP/SSH deneyimi aynı sanal ağdaki tüm sanal makineler için kullanılabilir. Azure savunma hakkında daha fazla bilgi için bkz. [Azure](bastion-overview.md)savunma nedir?.

## <a name="prerequisites"></a><a name="prereq"></a>Ön koşullar

* Bir sanal ağ.
* Sanal ağdaki bir Windows sanal makinesi.
* Aşağıdaki gerekli roller:
  * Sanal makinede okuyucu rolü.
  * Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü.

* Bağlantı noktaları: VM 'ye bağlanmak Için VM 'de aşağıdaki bağlantı noktalarının açık olması gerekir:
  * Gelen bağlantı noktaları: RDP (3389)

### <a name="example-values"></a>Örnek değerler

|**Ad** | **Değer** |
| --- | --- |
| Ad |  TestVNet1-savunma |
| Sanal ağ |  TestVNet1 (VM 'ye bağlı) |
| + Alt ağ adı | AzureBastionSubnet |
| AzureBastionSubnet adresleri |  10.1.254.0/27 |
| Genel IP adresi |  Yeni oluştur |
| Genel IP adresi adı | VNet1BastionPIP  |
| Genel IP adresi SKU 'SU |  Standart  |
| Atama  | Statik |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Savunma Konağı oluşturma

Azure portal var olan bir sanal makineyi kullanarak bir savunma ana bilgisayarı oluşturduğunuzda, sanal makinenize ve/veya sanal ağınıza karşılık gelen farklı ayarlar otomatik olarak varsayılan olarak değişir.

1. [Azure portalını](https://portal.azure.com) açın. Sanal makinenize gidip **Bağlan**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/vm-settings.png" alt-text="sanal makine ayarları" lightbox="./media/quickstart-host-portal/vm-settings.png":::
1. Açılan menüden, savunma ' **yı seçin.**
1. **Testvm 'de | Bağlan sayfasında**, **kullan**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/select-bastion.png" alt-text="sanal makine ayarları" border="false":::

1. Savunma sayfasında **,** aşağıdaki ayarlar alanlarını doldurun:

   * **Ad**: savunma konağını adlandırın.
   * **Alt ağ**: sanal ağınızdaki, savunma kaynağına dağıtılacak alt ağ. Alt ağın **AzureBastionSubnet**adıyla oluşturulması gerekir. Ad, Azure 'un savunma kaynağını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az/27 veya daha büyük (/27,/26,/25 vb.) bir alt ağ kullanın.
   
      * **Alt ağ yapılandırmasını Yönet**' i seçin.
      * **AzureBastionSubnet**seçin.
      * Gerekirse, CıDR gösteriminde adres aralığını ayarlayın. Örneğin, 10.1.254.0/27.
      * Diğer ayarları ayarlamamayın. Alt ağ değişikliklerini kabul etmek ve kaydetmek için **Tamam** ' ı seçin veya herhangi bir değişiklik yapmak istemiyorsanız sayfanın en üstünde **x** ' i seçin.
1. Savunma **sayfasına dönmek** ve değer belirtmeye devam etmek için tarayıcınızda geri düğmesine tıklayın.
   * **Genel IP adresi adı**: genel IP adresi kaynağının adı.
   * **Genel IP adresi**: Bu, RDP/SSH 'ye erişilebilen savunma KAYNAĞıNıN genel IP adresidir (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun.
1. Savunma konağını oluşturmak için **Oluştur** ' u seçin. Azure ayarlarınızı doğrular ve ardından Konağı oluşturur. Ana bilgisayar ve kaynakları, oluşturulması ve dağıtılması yaklaşık 5 dakika sürer.

   :::image type="content" source="./media/quickstart-host-portal/validate.png" alt-text="sanal makine ayarları":::

## <a name="connect"></a><a name="connect"></a>Bağlan

Sanal ağa bağlantı dağıtıldıktan sonra ekran bağlan sayfasına dönüşür.

1. Sanal makineniz için Kullanıcı adını ve parolayı yazın. Ardından **Bağlan**' ı seçin.

   :::image type="content" source="./media/quickstart-host-portal/connect-vm.png" alt-text="sanal makine ayarları":::
1. Bu sanal makineyle savunma aracılığıyla RDP bağlantısı, bağlantı noktası 443 ve savunma hizmeti kullanılarak doğrudan Azure portal (HTML5 üzerinden) açılır.

   :::image type="content" source="./media/quickstart-host-portal/connected.png" alt-text="sanal makine ayarları":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağ ve sanal makineler kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **arama** kutusuna kaynak grubunuzun adını girip arama sonuçlarından seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Kaynak **grubunun adını yazıp** **Sil**' i seçerek kaynak grubunuzu girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal ağınız için bir savunma ana bilgisayarı oluşturdunuz ve ardından savunma ana bilgisayarı aracılığıyla güvenli bir şekilde sanal makineye bağlanırsınız. Daha sonra, bir sanal makine ölçek kümesine bağlanmak istiyorsanız aşağıdaki adımla devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Azure savunma kullanarak bir sanal makine ölçek kümesine bağlanma](bastion-connect-vm-scale-set.md)
