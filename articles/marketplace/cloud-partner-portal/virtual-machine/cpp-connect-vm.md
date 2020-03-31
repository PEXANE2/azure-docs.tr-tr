---
title: Microsoft Azure tabanlı sanal makinenize bağlanın | Azure Marketi
description: Azure'da oluşturulan yeni sanal makineye nasıl bağlanılabildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 2b7eb6d321a64835254b684c8faeedc53645dffe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278067"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Azure tabanlı sanal makinenize bağlanın

Bu makalede, Azure'da oluşturduğunuz sanal makinelere (VM) nasıl bağlanAbileceğiniz ve oturum açıldığı açıklanmaktadır.  Başarılı bir şekilde bağlandıktan sonra, VM ile ana bilgisayar sunucusunda yerel olarak oturum açmış gibi çalışabilirsiniz. 

## <a name="connect-to-a-windows-based-vm"></a>Windows tabanlı bir VM'ye bağlanma

Azure'da barındırılan Windows tabanlı VM'ye bağlanmak için uzak masaüstü istemcisini kullanırsınız.  Windows'un çoğu sürümü, uzak masaüstü protokolü (RDP) için yerel olarak destek içerir.  Diğer makineler için, [Uzak Masaüstü istemcilerinde](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)istemciler hakkında daha fazla bilgi bulabilirsiniz.  

Aşağıdaki makalede, VM'nize bağlanmak için yerleşik Windows RDP desteğinin nasıl kullanılacağı ayrıntılı olarak anlatılıyor: [Windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açabilirsiniz.](../../../virtual-machines/windows/connect-logon.md)  

>[!TIP]
> İşlem sırasında ,örneğin .rdp dosyasının bilinmeyen bir yayımcıdan geldiğini veya kullanıcı kimlik bilgilerinizin doğrulanamayacağını belirten güvenlik uyarıları alabilirsiniz.  Bu uyarıları yoksaymak güvenlidir.


## <a name="connect-to-a-linux-based-vm"></a>Linux tabanlı bir VM'ye bağlanma

Linux tabanlı VM'yi bağlamak için güvenli bir kabuk protokolü (SSH) istemcisine ihtiyacınız vardır.  Bu tartışma ücretsiz [PuTTY](https://www.ssh.com/ssh/putty/) SHH terminali kullanacaktır.

1. [Azure portalına](https://ms.portal.azure.com)gidin. **Sanal makineleri**arayın ve seçin. 
2. Bağlanmak istediğiniz VM'yi seçin.  
3. Zaten çalışmıyorsa VM'yi **başlatın.**
4. **Genel Bakış** sayfasını açmak için VM'nin adını tıklayın.
5. VM'nizin Genel IP adresine ve DNS adını not edin.  (Bu değerler ayarlı değilse, [bir ağ arabirimi oluşturmanız](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface) gerekir

   ![VM Genel Bakış ayarları](./media/publishvm_019.png)
 
6. PuTTY uygulamasını açın.  
7. PuTTY Configuration iletişim kutusunda, VM'nizin IP adresini veya DNS adını girin. 

   ![PuTTY terminal ayarları](./media/publishvm_020.png)
 
8. PuTTY terminalini açmak için **Aç'ı** tıklatın.  
9. Size istendiğinde, Linux VM hesabınızın hesap adını ve parolasını girin. 

Bağlantı sorunları yaşıyorsanız, SSH istemcinizin belgelerine (örneğin [Bölüm 10: Sık hata iletileri.](https://www.ssh.com/ssh/putty/putty-manuals)

Sağlanan bir Linux VM'sine masaüstü ekleme hakkında daha fazla bilgi için, [Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü Yükle ve yapılandırma](../../../virtual-machines/linux/use-remote-desktop.md)konusuna bakın.


## <a name="stop-unused-vms"></a>Kullanılmayan VM'leri durdurma
VM çalışırken *veya boştayken*VM barındırma için Azure faturaları.  Bu nedenle, şu anda kullanılmayan VM'leri durdurmak en iyi yöntemdir.  Örneğin, test, yedekleme veya emekli VM'ler kapatma için adaylardır. VM'yi kapatmak için aşağıdaki adımları tamamlayın:

1. Sanal **makineler** bıçağında, durdurmak istediğiniz VM'yi seçin. 
2. Sayfanın üst kısmındaki araç çubuğunda **Durdur** düğmesine tıklayın.

   ![VM durdurma](./media/publishvm_018.png)

Azure, VM'nin yalnızca VM'deki işletim sistemini kapatmakla kalmıyor, aynı zamanda daha önce sağlanan donanım ve ağ kaynaklarını da serbest bırakan *deallocation*adlı bir işlemde VM'yi hızla durdurur.

Daha sonra durmuş bir VM'yi yeniden etkinleştirmek istiyorsanız, onu seçin ve **Başlat** düğmesini tıklatın.


## <a name="next-steps"></a>Sonraki adımlar

Uzaktan bağlandıktan sonra [VM'nizi yapılandırmaya](./cpp-configure-vm.md)hazırsınız.
