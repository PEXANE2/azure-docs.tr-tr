---
title: Azure sanal makine oluşturmadan sonra WinRM'i yapılandırın | Azure Marketi
description: Azure tarafından barındırılan bir sanal makine oluşturulduktan sonra Windows Uzaktan Yönetim 'nin (WinRM) nasıl yapılandırılabildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: dsindona
ms.openlocfilehash: b80325594eedb87293c31de3236bb4690eb89e05
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273027"
---
# <a name="configure-winrm-after-virtual-machine-creation"></a>Sanal makine oluşturulduktan sonra WinRM'i yapılandırın

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

Bu makalede, WinRM'i HTTPS üzerinden etkinleştirmek için varolan Azure barındırılan sanal makinenin (VM) nasıl yapılandırılabildiğini açıklanmaktadır.  Bu yapılandırma yalnızca Windows tabanlı VM'ler için geçerlidir ve aşağıdaki iki adımlı işlemi gerektirir:

1. HTTPS protokolü üzerinden WinRM için bağlantı noktası trafiğini etkinleştirin.  Bu ayarı Azure portalında VM'niz için yapılandıracaksınız.
2. VM'yi, sağlanan PowerShell komut dosyalarını çalıştırarak WinRM'i etkinleştirecek şekilde yapılandırın.


## <a name="enabling-port-traffic"></a>Bağlantı noktası trafiğini etkinleştirme

HTTPS protokolü üzerindeki WinRM, Azure Marketi'nde sunulan önceden yapılandırılmış Windows VM'lerinde varsayılan olarak etkinleştirilmeyen 5986 bağlantı noktasını kullanır. Bu protokolü etkinleştirmek için, [Azure portalı](https://portal.azure.com)ile ağ güvenlik grubuna (NSG) yeni bir kural eklemek için aşağıdaki adımları kullanın.  NSG'ler hakkında daha fazla bilgi için [Güvenlik Grupları'na](https://docs.microsoft.com/azure/virtual-network/security-overview)bakın.

1. Bıçak Sanal **makineleri **   <>*vm-name* >   **> Ayarlar / Ağ**gidin.
2. Özelliklerini görüntülemek için NSG adını (bu örnekte **testvm11002)** tıklatın:

    ![Ağ güvenlik grubu özellikleri](./media/nsg-properties.png)
 
3. **Ayarlar**altında, bu bıçağı görüntülemek için **Gelen güvenlik kurallarını** seçin.
4. TCP bağlantı noktası 5986 için çağrılan `WinRM_HTTPS` yeni bir kural oluşturmak için **+Ekle'yi** tıklatın.

    ![Gelen ağ güvenliği kuralı ekleme](./media/nsg-new-rule.png)

5. Değerleri sağlamayı bitirdiğinizde **Tamam'ı** tıklatın.  Gelen güvenlik kuralları listesi aşağıdaki yeni girişleri içermelidir.

    ![Gelen ağ güvenliği kurallarının listelenmesi](./media/nsg-new-inbound-listing.png)


## <a name="configure-vm-to-enable-winrm"></a>WinRM'i etkinleştirmek için VM'yi yapılandırın 

Windows VM'nizde Windows Uzaktan Yönetim özelliğini etkinleştirmek ve yapılandırmak için aşağıdaki adımları kullanın.   

1. Azure barındırılan VM'nize Uzak Masaüstü bağlantısı kurun.  Daha fazla bilgi için [windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açabilirsiniz'](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)a bakın.  Kalan adımlar VM'nizde çalıştırılacaktır.
2. Aşağıdaki dosyaları indirin ve Bunları VM'nizdeki bir klasöre kaydedin:
    - [YapılandırmaWinRM.ps1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1)
    - [Makecert.exe](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe)
    - [winrmconf.cmd](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd)
3. **PowerShell** Konsolu'nu yüksek ayrıcalıklarla açın **(Yönetici Olarak Çalıştırın).** 
4. Gerekli parametreyi sağlayarak aşağıdaki komutu çalıştırın: VM'niz için tam nitelikli alan adı (FQDN): <br/>
   `ConfigureWinRM.ps1 <vm-domain-name>`

    ![Powershell yapılandırma komut dosyası 1](./media/powershell-file1.png)

    Bu komut dosyası, diğer iki dosyanın aynı klasörde olmasına bağlıdır.


## <a name="next-steps"></a>Sonraki adımlar

WinRM'i yapılandırdıktan sonra, [VM'nizi kurucu VHD'lerinden dağıtmaya](./cpp-deploy-vm-vhd.md)hazırsınız.
