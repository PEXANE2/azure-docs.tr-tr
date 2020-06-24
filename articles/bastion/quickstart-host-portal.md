---
title: 'Hızlı başlangıç: özel IP adresi kullanarak bir sanal makineye bağlanma: Azure savunma'
description: Bu makalede, bir sanal makineden Azure savunma ana bilgisayarı oluşturmayı ve özel bir IP adresi kullanarak güvenli bir şekilde bağlamayı öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: quickstart
ms.date: 03/11/2020
ms.author: charwen
ms.openlocfilehash: 3c2780c8c99fd4568a7213b625ce785d3a99129c
ms.sourcegitcommit: 24f31287b6a526e23ff5b5469113522d1ccd4467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84743992"
---
# <a name="quickstart-connect-to-a-virtual-machine-using-a-private-ip-address-and-azure-bastion"></a>Hızlı başlangıç: özel IP adresi ve Azure savunma kullanarak bir sanal makineye bağlanma

Bu hızlı başlangıç makalesinde, özel bir IP adresi kullanarak bir sanal makineye nasıl bağlanabilmeniz gösterilmektedir. Savunma aracılığıyla bağlandığınızda, sanal makinelerinizin ortak bir IP adresine ihtiyacı yoktur. Bu makaledeki adımlar, portaldaki sanal makineniz aracılığıyla sanal ağınıza yönelik savunma dağıtmanıza yardımcı olur. Hizmet sağlandıktan sonra, RDP/SSH deneyimi aynı sanal ağdaki tüm sanal makineler için kullanılabilir.

## <a name="prerequisites"></a><a name="prereq"></a>Ön koşullar

* Bir Azure sanal ağı.
* Sanal ağda bulunan ve 3389 numaralı bağlantı noktası açık olan bir Azure sanal makinesi.

### <a name="example-values"></a>Örnek değerler

|**Adı** | **Değer** |
| --- | --- |
| Name |  VNet1Bastion |
| Bölge | eastus |
| Sanal ağ |  VNet1 |
| + Alt ağ adı | AzureBastionSubnet |
| AzureBastionSubnet adresleri |  10.1.254.0/27 |
| Genel IP adresi |  Yeni oluştur |
| Genel IP adresi adı | VNet1BastionPIP  |
| Genel IP adresi SKU 'SU |  Standart  |
| Atama  | Statik |

## <a name="create-a-bastion-host"></a><a name="createvmset"></a>Savunma Konağı oluşturma

Portalda var olan bir sanal makineyi kullanarak bir savunma ana bilgisayarı oluşturduğunuzda, sanal makinenize ve/veya sanal ağınıza karşılık gelen farklı ayarlar otomatik olarak varsayılan olarak değişir.

1. [Azure Portal](https://portal.azure.com)açın. Sanal makinenize gidin ve **Bağlan**' a tıklayın.

   ![sanal makine ayarları](./media/quickstart-host-portal/vm-settings.png)
1. Açılan menüden, savunma ' **yı seçin.**
1. Bağlan sayfasında, savunma **kullan**' ı seçin.

   ![Savunma seçin](./media/quickstart-host-portal/select-bastion.png)

1. Savunma sayfasında, aşağıdaki ayarlar alanlarını doldurun:

   * **Ad**: savunma konağını adlandırın
   * **Alt ağ**: sanal ağınızdaki, savunma kaynağına dağıtılacak alt ağ. Alt ağın **AzureBastionSubnet**adıyla oluşturulması gerekir. Ad, Azure 'un savunma kaynağını hangi alt ağa dağıtacağınızı bilmesini sağlar. Bu, bir ağ geçidi alt ağından farklıdır. En az/27 veya daha büyük (/27,/26,/25 vb.) bir alt ağ kullanın.
   
      * **Alt ağ yapılandırmasını Yönet**' i seçin ve **+ alt ağ**' ı seçin
      * Alt ağ Ekle sayfasında, **AzureBastionSubnet**yazın.
      * CıDR gösteriminde adres aralığını belirtin. Örneğin, 10.1.254.0/27.
      * Alt ağı oluşturmak için **Tamam ' ı** seçin. Sayfanın üst kısmında, geri dönerek ayarların geri kalanını tamamlayın.

         ![Savunma ayarlarına git](./media/quickstart-host-portal/navigate-bastion.png)
   * **Genel IP adresi**: Bu, RDP/SSH 'ye erişilebilen savunma KAYNAĞıNıN genel IP adresidir (443 numaralı bağlantı noktası üzerinden). Yeni bir genel IP oluşturun veya var olan bir IP 'yi kullanın. Genel IP adresi, oluşturmakta olduğunuz savunma kaynağıyla aynı bölgede olmalıdır.
   * **Genel IP adresi adı**: genel IP adresi kaynağının adı.
1. Doğrulama ekranında **Oluştur**' a tıklayın. Savunma kaynağı oluşturma ve dağıtma için 5 dakika bekleyin.

   ![Savunma Konağı oluştur](./media/quickstart-host-portal/bastion-settings.png)

## <a name="connect"></a><a name="connect"></a>Bağlan

Sanal ağa bağlantı dağıtıldıktan sonra ekran bağlan sayfasına dönüşür.

1. Sanal makineniz için Kullanıcı adını ve parolayı yazın. Ardından **Bağlan**' ı seçin.

   ![bağlanmaya](./media/quickstart-host-portal/connect.png)
1. Bu sanal makineyle savunma aracılığıyla RDP bağlantısı, bağlantı noktası 443 ve savunma hizmeti kullanılarak doğrudan Azure portal (HTML5 üzerinden) açılır.

   ![RDP bağlantısı](./media/quickstart-host-portal/443-rdp.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağ ve sanal makineler kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **arama** kutusuna *TestRG1* girin ve arama sonuçlarından **TestRG1** öğesini seçin.

2. **Kaynak grubunu sil**'i seçin.

3. **Kaynak grubu adını yazmak** için *TestRG1* girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal ağınız için bir savunma ana bilgisayarı oluşturdunuz ve ardından savunma ana bilgisayarı aracılığıyla güvenli bir şekilde sanal makineye bağlanırsınız.

* Azure savunma hakkında daha fazla bilgi edinmek için savunma hakkında [genel bakış](bastion-overview.md) ve savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.
* Azure savunma alt ağıyla ağ güvenlik gruplarını kullanmak için bkz. [NSG Ile çalışma](bastion-nsg.md).
* Azure savunma ana bilgisayarı ayarlarının açıklamalarını içeren yönergeler için bkz. [öğreticiye](bastion-create-host-portal.md)bakın.
* Bir sanal makine ölçek kümesine bağlanmak için bkz. [Azure savunma kullanarak bir sanal makine ölçek kümesine bağlanma](bastion-connect-vm-scale-set.md).