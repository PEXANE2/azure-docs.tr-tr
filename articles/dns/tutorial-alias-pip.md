---
title: 'Öğretici: Azure genel IP adresine başvurmak için bir Azure DNS takma ad kaydı oluşturun'
description: Bu öğreticide Azure genel IP adresine başvurmak için bir Azure DNS diğer ad kaydını yapılandırma işlemi gösterilmektedir.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: rohink
ms.openlocfilehash: d3017d09e94040d16950598dad360fe32930c16b
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985448"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Öğretici: Azure genel IP adresine başvurmak için diğer ad kaydı yapılandırma 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ağ altyapısı oluşturma.
> * Genel IP içeren bir web sunucusu sanal makinesi oluşturun.
> * Genel IP'yi işaret eden bir takma ad kaydı oluşturun.
> * Diğer ad kaydını test etme.


Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Azure DNS’te etki alanınızı barındırma yönergeleri için bkz. [Öğretici: Azure DNS’te etki alanınızı barındırma](dns-delegate-domain-azure-dns.md).

Bu öğreticide örnek olarak contoso.com etki alanı kullanılmaktadır ancak sizin kendi etki alanı adınızı kullanmanız gerekir.

## <a name="create-the-network-infrastructure"></a>Ağ altyapısını oluşturma
İlk olarak, web sunucularınızı içine yerleştirmek için bir sanal ağ ve alt ağ oluşturun.
1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. Portalda sol üst köşeden **Kaynak oluştur**'u seçin. Arama kutusuna *kaynak grubu* yazın ve **RG-DNS-Alias-pip** adlı bir kaynak grubu oluşturun.
3. Kaynak **Oluştur** > **Sanal** > **ağ'ı**seçin.
4. **VNet-Server** adlı bir sanal ağ oluşturun. Bunu **RG-DNS-Alias-pip** kaynak grubunun içine yerleştirin ve alt ağı **SN-Web** olarak adlandırın.

## <a name="create-a-web-server-virtual-machine"></a>Web sunucusu sanal makinesi oluşturma
1. **Kaynak** > Oluştur**Windows Server 2016 VM'yi**seçin.
2. Ad için **Web-01** girin ve VM’yi **RG-DNS-Alias-TM** kaynak grubuna yerleştirin. Kullanıcı adı ve parola girip **Tamam**'ı seçin.
3. **Boyut** için 8 GB RAM'e sahip bir SKU seçin.
4. **Ayarlar** için **VNet-Servers** sanal ağını ve **SN-Web** alt ağını seçin. Ortak gelen bağlantı noktaları için **HTTP** > **HTTPS** > **RDP (3389)** seçeneğini ve ardından **Tamam'ı**seçin.
5. **Özet** sayfasında **Oluştur**'u seçin.

Bu işlemin tamamlanması birkaç dakika sürer. Sanal makine web-01-ip denilen temel bir dinamik kamu IP olacak ekli bir NIC olacaktır. Sanal makine her yeniden başlatıldığında genel IP değişecektir.

### <a name="install-iis"></a>IIS yükleme

**Web-01**’e IIS yükleyin.

1. **Web-01**’e bağlanıp oturum açın.
2. **Sunucu Yöneticisi** panosunda **Rol ve özellik ekle**’ye tıklayın.
3. Üç kez **İleri**'yi seçin. **Sunucu Rolleri** sayfasında **Web Sunucusu (IIS)** öğesini seçin.
4. **Özellik Ekle**'yi ve ardından **İleri**'yi seçin.
5. **İleri**'yi dört kez seçin ve ardından **Yükle**'yi seçin. Bu işlemin tamamlanması birkaç dakika sürer.
6. Yükleme tamamlandıktan sonra **Kapat**'ı seçin.
7. Bir web tarayıcısı açın. Varsayılan IIS web sayfasının göründüğünü doğrulamak için **localhost** sayfasına göz atın.

## <a name="create-an-alias-record"></a>Diğer ad kaydı oluşturma

Genel IP adresine işaret eden bir diğer ad kaydı oluşturun.

1. Azure DNS bölgenizi açmak için bölgeyi seçin.
2. **Kayıt kümesi**’ni seçin.
3. **Ad** metin kutusuna **web01** yazın.
4. **Tür** alanını bir **A** kaydı olarak bırakın.
5. **Diğer Ad Kayıt Kümesi** onay kutusunu seçin.
6. **Azure hizmeti seçin**’i belirleyin ve ardından **Web-01-ip** genel IP adresini seçin.

## <a name="test-the-alias-record"></a>Diğer ad kaydını test etme

1. **RG-DNS-Alias-pip** kaynak grubunda **Web-01** sanal makinesini seçin. Genel IP adresini not edin.
1. Bir web tarayıcısından Web01-01 sanal makinesinin tam etki alanı adına göz atın. Örnek: **web01.contoso.com**. Varsayılan IIS web sayfasını görürsünüz.
2. Web tarayıcısını kapatın.
3. **Web-01** sanal makinesini durdurun ve sonra yeniden başlatın.
4. Yeniden başlatıldıktan sonra, sanal makinenin yeni genel IP adresini not edin.
5. Yeni bir tarayıcı açın. Yeniden Web01-01 sanal makinesinin tam etki alanı adına göz atın. Örnek: **web01.contoso.com**.

Standart bir A kaydı yerine genel IP adresi kaynağına işaret eden bir diğer ad kaydı kullandığınız için bu işlem başarılı olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturulan kaynaklara ihtiyacınız kalmadığında **RG-DNS-Alias-pip** kaynak grubunu silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure genel IP adresine başvurmak için bir diğer ad kaydı oluşturdunuz. Azure DNS ve web uygulamaları hakkında daha fazla bilgi için web uygulaması öğreticileriyle devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Özel etki alanında web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
