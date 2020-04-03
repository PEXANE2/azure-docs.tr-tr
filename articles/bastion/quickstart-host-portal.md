---
title: 'Hızlı başlatma: Özel bir IP adresi kullanarak sanal bir makineye bağlanma: Azure Bastion'
description: Bu makalede, sanal bir makineden Azure Bastion ana bilgisayarını nasıl oluşturup özel bir IP adresi kullanarak güvenli bir şekilde nasıl bağlanabilirsiniz öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: charwen
ms.openlocfilehash: a420a3253040fff8b767a81f298ede283c1d214b
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619264"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>Hızlı başlatma: Özel bir IP adresi ve Azure Kalesi kullanarak sanal makineye bağlanma

Bu hızlı başlangıç makalesi, özel bir IP adresi kullanarak sanal bir makineye nasıl bağlanabileceğinizi gösterir. Bastion üzerinden bağlandığınızda, sanal makinelerinizin genel bir IP adresine ihtiyacı yoktur. Bu makaledeki adımlar, Portaldaki sanal makineniz aracılığıyla Bastion'u sanal ağınıza dağıtmanıza yardımcı olur. Hizmet sağlandıktan sonra, RDP/SSH deneyimi aynı sanal ağdaki tüm sanal makineler tarafından kullanılabilir.

## <a name="prerequisites"></a><a name="prereq"></a>Ön koşullar

* Azure sanal ağı.
* 3389 portu açık olan sanal ağda bulunan bir Azure sanal makinesi.

### <a name="example-values"></a>Örnek değerler

|**Adı** | **Değer** |
| --- | --- |
| Adı |  VNet1Burçyasyon |
| Bölge | eastus |
| Sanal ağ |  VNet1 |
| + Subnet Adı | AzureBastionSubnet |
| AzureBastionSubnet adresleri |  10.1.254.0/27 |
| Genel IP adresi |  Yeni oluştur |
| Genel IP adresi adı | VNet1BurçPIP  |
| Genel IP adresi SKU |  Standart  |
| Atama  | Statik |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Bastion konağı oluşturma

Varolan bir sanal makineyi kullanarak portalda bir burç ana bilgisayar oluşturduğunuzda, çeşitli ayarlar sanal makinenize ve/veya sanal ağınıza karşılık gelecek şekilde otomatik olarak varsayılan hale gelir.

1. Azure [portalını](https://portal.azure.com)açın. Sanal makinenize gidin ve **ardından Bağlan'ı**tıklatın.

   ![sanal makine ayarları](./media/quickstart-host-portal/vm-settings.png)
1. Açılan dosyadan **Bastion'u**seçin.
1. Bağlan sayfasında **Bastion'u kullan'ı**seçin.

   ![burç seçin](./media/quickstart-host-portal/select-bastion.png)

1. Bastion sayfasında aşağıdaki ayarlar alanlarını doldurun:

   * **Adı**: Burç ana bilgisayarını adlandır
   * **Alt Ağ**: Bastion kaynağının dağıtılacayacağı sanal ağınuzun alt ağı. Alt ağ **AzureBastionSubnet**adı ile oluşturulmalıdır. Ad, Azure'un Bastion kaynağını hangi alt ağa dağıtacağını bilmesini sağlar. Bu, Ağ Geçidi alt ağından farklıdır. En az /27 veya daha büyük (/27, /26, /25 vb.) bir alt ağ kullanın.
   
      * **Alt net yapılandırması yönet'i**seçin, sonra + **Subnet'i**seçin.
      * Ekle alt ağ sayfasında **AzureBastionSubnet**yazın.
      * CIDR gösteriminde adres aralığını belirtin. Örneğin, 10.1.254.0/27.
      * Alt ağı oluşturmak için **Tamam'ı** seçin. Sayfanın üst kısmında, ayarların geri kalanını tamamlamak için Bastion'a geri gidin.

         ![burç ayarlarına gidin](./media/quickstart-host-portal/navigate-bastion.png)
   * **Genel IP adresi**: RDP/SSH'ye erişilen Bastion kaynağının genel IP'sidir (bağlantı noktası 443'ten fazla). Yeni bir genel IP oluşturun veya varolan bir IP kullanın. Ortak IP adresi, oluşturduğunuz Bastion kaynağıyla aynı bölgede olmalıdır.
   * **Genel IP adresi adı**: Genel IP adresi kaynağının adı.
1. Doğrulama ekranında **Oluştur'u**tıklatın. Bastion kaynağı oluşturmak ve dağıtmak için yaklaşık 5 dakika bekleyin.

   ![burç ana bilgisayar oluşturma](./media/quickstart-host-portal/bastion-settings.png)

## <a name="connect"></a><a name="connect"></a>Bağlanma

Bastion sanal ağa dağıtıldıktan sonra ekran bağlantı sayfasına değişir.

1. Sanal makinenizin kullanıcı adını ve parolasını yazın. Ardından **Bağlan'ı**seçin.

   ![bağlanmaya](./media/quickstart-host-portal/connect.png)
1. Bastion üzerinden bu sanal makineye RDP bağlantısı doğrudan Azure portalında (HTML5 üzerinden) bağlantı noktası 443 ve Bastion hizmetini kullanarak açılır.

   ![RDP bağlantısı](./media/quickstart-host-portal/443-rdp.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağı ve sanal makineleri kullanmayı bitirdiğinizde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Arama** kutusuna *TestRG1'i* girin ve arama sonuçlarından **TestRG1'i** seçin.

2. **Kaynak grubunu sil**'i seçin.

3. **KAYNAK GRUBU ADINI YAZIN** *ve* **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal ağınız için bir Bastion ana bilgisayarı oluşturdunuz ve ardından Bastion ana bilgisayarı aracılığıyla güvenli bir şekilde sanal bir makineye bağlandınız.

* Azure Bastion hakkında daha fazla bilgi edinmek için [Bastion Genel Bakış](bastion-overview.md) ve [Bastion SSS'yi](bastion-faq.md)okuyun.
* Azure Bastion alt ağıyla Ağ Güvenlik Grupları'nı kullanmak için Bkz. [NSG'lerle Çalışma.](bastion-nsg.md)
* Azure Bastion ana bilgisayar ayarlarının açıklamalarını içeren yönergeler için [Öğretici'ye](bastion-create-host-portal.md)bakın.
* Sanal makine ölçeği kümesine bağlanmak için Azure [Bastion kullanarak sanal makine ölçeği kümesine bağlan'a](bastion-connect-vm-scale-set.md)bakın.