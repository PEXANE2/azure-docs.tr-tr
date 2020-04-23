---
title: Windows Server VM'ye bağlanma
description: Azure portalını ve Kaynak Yöneticisi dağıtım modelini kullanarak windows vm'ye nasıl bağlanıp oturum açabilirsiniz öğrenin.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: 92f7ba30e2a3c66218974ab65502c35139ab1f8c
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086224"
---
# <a name="how-to-connect-and-sign-on-to-an-azure-virtual-machine-running-windows"></a>Windows çalıştıran bir Azure sanal makinesine bağlanma ve oturum açma
Bir Windows masaüstü bilgisayarından Uzak Masaüstü (RDP) oturumu başlatmak için Azure portalında **Bağlan** düğmesini kullanırsınız. Önce sanal makineye bağlanırsınız, sonra oturum açın.

Bir Mac'ten Bir Windows VM'ye bağlanmak için, Mac için [Microsoft Remote Desktop](https://aka.ms/rdmac)gibi bir RDP istemcisi yüklemeniz gerekir.

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma
1. VM'ye bağlanmak için [Azure portalına](https://portal.azure.com/) gidin. **Sanal makineleri**arayın ve seçin.
2. Listeden sanal makineyi seçin.
3. Sanal makine sayfasının başında **Bağlan'ı**seçin.
4. Sanal **makineye Bağlan** sayfasında **RDP'yi**seçin ve ardından uygun **IP adresini** ve Bağlantı **Noktası numarasını**seçin. Çoğu durumda, varsayılan IP adresi ve bağlantı noktası kullanılmalıdır. **RDP Dosyasını İndir'i**seçin. VM'de tam zamanında ilke kümesi varsa, RDP dosyasını indirebilmeniz için önce erişim isteğinde bulunmak için **İstek erişim** düğmesini seçmeniz gerekir. Tam zamanında ilke hakkında daha fazla bilgi için, [tam zamanında ilkeyi kullanarak sanal makine erişimini yönet'e](../../security-center/security-center-just-in-time.md)bakın.
5. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan'ı** seçin. Dosyanın `.rdp` bilinmeyen bir yayımcıdan geldiğine dair bir uyarı alırsınız. Bu beklenen bir durumdur. Uzak **Masaüstü Bağlantısı** penceresinde devam etmek için **Bağlan'ı** seçin.
   
    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/connect-logon/rdp-warn.png)
3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Sanal makinede bir hesabın kimlik bilgilerini girin ve ardından **Tamam'ı**seçin.
   
     **Yerel hesap**: Bu genellikle sanal makineyi oluşturduğunuzda belirttiğiniz yerel hesap kullanıcı adı ve parolasIdır. Bu durumda, etki alanı sanal makinenin adıdır ve *vmadı*&#92;*kullanıcıadı* olarak girilir.  
   
    **Alan adı vm katıldı**: VM bir etki alanına aitse, kullanıcı adını *Domain*&#92;*Kullanıcı Adı*biçiminde girin. Hesabın ayrıca, Yöneticiler grubunda olması ya da VM’ye uzaktan erişim ayrıcalıkları verilmiş olması gerekir.
   
    **Etki alanı denetleyicisi**: VM bir etki alanı denetleyicisiyse, bu etki alanı için bir etki alanı yöneticisi hesabının kullanıcı adını ve parolasını girin.
4. Sanal makinenin kimliğini doğrulamak ve oturum açmayı bitirmek için **Evet'i** seçin.
   
   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Portaldaki **Bağlan** düğmesi gri renkteyse ve [Bir Ekspres Rota](../../expressroute/expressroute-introduction.md) veya Siteden Siteye VPN bağlantısı yla Azure'a bağlı değilseniz, RDP'yi kullanabilmeniz için VM'nizi genel bir IP adresi oluşturmanız ve atamanız gerekir. [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) Daha fazla bilgi için [Azure'daki Genel IP adreslerine](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)bakın.
   > 
   > 

## <a name="connect-to-the-virtual-machine-using-powershell"></a>PowerShell'i kullanarak sanal makineye bağlanın

 

PowerShell kullanıyorsanız ve Azure PowerShell modülü yüklüyse, aşağıda `Get-AzRemoteDesktopFile` gösterildiği gibi cmdlet'i kullanarak da bağlanabilirsiniz.

Bu örnek, rdp bağlantısını hemen başlatarak sizi yukarıdaki gibi benzer istemlerden geçirir.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -Launch
```

RDP dosyasını ileride kullanmak üzere de kaydedebilirsiniz.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -LocalPath "C:\Path\to\folder"
```

## <a name="next-steps"></a>Sonraki adımlar
Bağlanmada güçlük çekiyorsanız, [Sorun Giderme Uzak Masaüstü bağlantılarına](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. 

