---
title: Microsoft Azure tabanlı sanal makinenize bağlanın | Azure Marketi
description: Azure 'da oluşturulan yeni sanal makineye nasıl bağlanabileceğinizi açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 4aea624c2127c9b0a61d72b8d14929ce6f47df24
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142483"
---
# <a name="connect-to-your-azure-based-virtual-machine"></a>Azure tabanlı sanal makinenize bağlanın

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin Iş Ortağı Merkezi 'nin hareketli yönetimine başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine teknik varlıklarınızı oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) bölümündeki yönergeleri izleyin.

Bu makalede, Azure 'da oluşturduğunuz sanal makinelere (VM 'Ler) bağlanma ve bunları açma işlemleri açıklanmaktadır.  Başarıyla bağlandıktan sonra, konak sunucusunda yerel olarak oturum açmış olduğunuz gibi VM ile çalışabilirsiniz.

## <a name="connect-to-a-windows-based-vm"></a>Windows tabanlı bir VM 'ye bağlanma

Azure 'da barındırılan Windows tabanlı VM 'ye bağlanmak için uzak masaüstü istemcisini kullanacaksınız.  Windows 'un çoğu sürümü yerel olarak Uzak Masaüstü Protokolü (RDP) desteği içerir.  Diğer makineler için, [Uzak Masaüstü istemcilerindeki](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)istemciler hakkında daha fazla bilgi edinebilirsiniz.  

Aşağıdaki makalede, yerleşik Windows RDP desteğinin sanal makinenize bağlanmak için nasıl kullanılacağı anlatılmaktadır: [Windows çalıştıran bir Azure sanal makinesine bağlanma ve oturum açma](../../../virtual-machines/windows/connect-logon.md).  

>[!TIP]
> İşlem sırasında, örneğin. rdp dosyasının bilinmeyen bir yayımcıdan veya Kullanıcı kimlik bilgilerinizin doğrulanamadığı bir güvenlik uyarısı alabilirsiniz.  Bu uyarıları yoksaymak güvenlidir.


## <a name="connect-to-a-linux-based-vm"></a>Linux tabanlı bir VM 'ye bağlanma

Linux tabanlı VM 'yi bağlamak için güvenli bir kabuk Protokolü (SSH) istemcisine ihtiyacınız vardır.  Bu tartışma, ücretsiz [Putty](https://www.ssh.com/ssh/putty/) Shh terminalini kullanacaktır.

1. [Azure Portal](https://ms.portal.azure.com)gidin. **Sanal makineleri**arayın ve seçin. 
2. Bağlanmak istediğiniz VM 'yi seçin.  
3. Zaten çalışmıyorsa VM 'yi **başlatın** .
4. **Genel bakış** sayfasını açmak için VM adına tıklayın.
5. VM 'nizin genel IP adresi ve DNS adı ' nı aklınızda edin.  (Bu değerler ayarlanmamışsa, [bir ağ arabirimi oluşturmanız](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface) gerekir

   ![VM genel bakış ayarları](./media/publishvm_019.png)
 
6. PuTTY uygulamasını açın.  
7. PuTTY yapılandırması iletişim kutusunda, sanal makinenizin IP adresini veya DNS adını girin. 

   ![PuTTY Terminal ayarları](./media/publishvm_020.png)
 
8. Bir PuTTY terminali açmak için **Aç** ' a tıklayın.  
9. İstendiğinde, Linux VM hesabınızın hesap adını ve parolasını girin. 

Bağlantı sorunlarınız varsa, SSH istemcinizin belgelerine bakın. Örneğin, [Bölüm 10: ortak hata iletileri](https://www.ssh.com/ssh/putty/putty-manuals).

Sağlanan bir Linux sanal makinesine masaüstü ekleme dahil daha fazla bilgi için bkz. [Azure 'da bir LINUX VM 'sine bağlanmak Için uzak masaüstü 'Nü yüklemek ve yapılandırmak](../../../virtual-machines/linux/use-remote-desktop.md).


## <a name="stop-unused-vms"></a>Kullanılmayan VM 'Leri durdur
Bir VM çalışırken *veya boşta KALDıĞıNDA*VM barındırma için Azure faturaları.  Bu nedenle, şu anda kullanılmayan VM 'Leri durdurmak en iyi uygulamadır.  Örneğin, test, yedekleme veya kullanımdan kaldırılan VM 'Ler, kapanmaya yönelik adaylardır. Bir VM 'yi kapatmak için aşağıdaki adımları izleyin:

1. **Sanal makineler** dikey penceresinde, durdurmak istediğiniz VM 'yi seçin. 
2. Sayfanın üst kısmındaki araç çubuğunda **Durdur** düğmesine tıklayın.

   ![VM durdurma](./media/publishvm_018.png)

Azure, yalnızca VM 'deki işletim sistemini kapatmayan *, ancak*daha önce kendisi için sağlanmış olan donanım ve ağ kaynaklarını serbest bırakma adlı bir işlemdeki sanal makineyi hızlı bir şekilde sonlandırır.

Durdurulmuş bir sanal makineyi daha sonra yeniden etkinleştirmek istiyorsanız, seçin ve **Başlat** düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Uzaktan bağlandıktan sonra, [VM 'nizi yapılandırmaya](./cpp-configure-vm.md)hazırsınız demektir.
