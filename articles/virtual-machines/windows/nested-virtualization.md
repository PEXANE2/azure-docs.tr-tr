---
title: Azure Sanal Makinelerde iç içe sanallaştırma nasıl etkinleştirilir?
description: Azure Sanal Makinelerde iç içe sanallaştırma nasıl etkinleştirilir?
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
ms.author: cynthn
ms.date: 10/09/2017
ms.topic: conceptual
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.openlocfilehash: d127d3f9e66b7d2d40aa420e2116fee17d996514
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437303"
---
# <a name="how-to-enable-nested-virtualization-in-an-azure-vm"></a>Bir Azure VM'de iç içe sanallaştırmayı etkinleştirme

İç içe sanallaştırma, birkaç Azure sanal makine ailesinde desteklenir. Bu özellik, geliştirme, test, eğitim ve gösteri ortamları gibi senaryoları desteklemede büyük esneklik sağlar.   

Bu makale, Azure VM'de Hyper-V'yi etkinleştirme ve bu konuk sanal makineye Internet bağlantısını yapılandırma yoluyla adım attırır.

## <a name="create-a-nesting-capable-azure-vm"></a>İç içe geçme özelliğine sahip bir Azure VM oluşturma

Yeni bir Windows Server 2016 Azure VM oluşturun. İç içe geçmeyi destekleyen sanal makine boyutlarının tam listesi için [Azure İşlem Birimi makalesine](acu.md)göz atın.

Bir konuk sanal makinenin taleplerini destekleyecek kadar büyük bir VM boyutu seçmeyi unutmayın. Bu örnekte, D3_v3 boyutunda bir Azure VM kullanıyoruz. 

Burada Dv3 veya Ev3 serisi sanal makinelerin [here](https://azure.microsoft.com/regions/services/)bölgesel durumu görebilirsiniz.

>[!NOTE]
>
>Yeni bir sanal makine oluşturma yla ilgili ayrıntılı talimatlar için Azure [PowerShell modülüyle Windows VM'leri Oluştur ve Yönet'e](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm) bakın
    
## <a name="connect-to-your-azure-vm"></a>Azure VM'nize bağlanın

Sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

1. Sanal makine özelliklerinde **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

3. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

4. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

## <a name="enable-the-hyper-v-feature-on-the-azure-vm"></a>Azure VM'de Hyper-V özelliğini etkinleştirme
Bu ayarları el ile yapılandırabilirsiniz veya yapılandırmayı otomatikleştirmek için bir PowerShell komut dosyası sağladık.

### <a name="option-1-use-a-powershell-script-to-configure-nested-virtualization"></a>Seçenek 1: İç içe sanallaştırmayı yapılandırmak için PowerShell komut dosyası kullanın
Windows Server 2016 ana bilgisayarda iç içe sanallaştırmayı etkinleştirmek için bir PowerShell komut dosyası [GitHub'da](https://github.com/charlieding/Virtualization-Documentation/tree/live/hyperv-tools/Nested)kullanılabilir. Komut dosyası ön koşulları denetler ve ardından Azure VM'de iç içe olan sanallaştırmayı yapılandırır. Yapılandırmayı tamamlamak için Azure VM'nin yeniden başlatılması gerekir. Bu komut dosyası başka ortamlarda çalışabilir, ancak garanti edilmez. Azure'da çalışan iç içe sanallaştırma yla ilgili canlı video gösterimi ile Azure blog gönderisini göz atın! https://aka.ms/AzureNVblog.

### <a name="option-2-configure-nested-virtualization-manually"></a>Seçenek 2: İç içe sanallaştırmayı el ile yapılandırma

1. Azure VM'de PowerShell'i Yönetici olarak açın. 

2. Hyper-V özelliğini ve Yönetim Araçlarını etkinleştirin.

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

    >[!WARNING] 
    >
    >Bu komut Azure VM'yi yeniden başlatır. Yeniden başlatma işlemi sırasında RDP bağlantınızı kaybedersiniz.
    
3. Azure VM yeniden başlatıldıktan sonra RDP kullanarak VM'nize yeniden bağlanın.

## <a name="set-up-internet-connectivity-for-the-guest-virtual-machine"></a>Konuk sanal makine için internet bağlantısı ayarlama
Konuk sanal makine için yeni bir sanal ağ bağdaştırıcısı oluşturun ve Internet bağlantısını etkinleştirmek için bir NAT Ağ Geçidi yapılandırın.

### <a name="create-a-nat-virtual-network-switch"></a>NAT sanal ağ anahtarı oluşturma

1. Azure VM'de PowerShell'i Yönetici olarak açın.
   
2. Dahili bir anahtar oluşturun.

    ```powershell
    New-VMSwitch -Name "InternalNAT" -SwitchType Internal
    ```

3. Anahtarın özelliklerini görüntüleyin ve yeni bağdaştırıcı için ifIndex'e not edin.

    ```powershell
    Get-NetAdapter
    ```

    ![NetAdapter](./media/virtual-machines-nested-virtualization/get-netadapter.png)

    >[!NOTE] 
    >
    >Oluşturduğunuz sanal anahtar için "ifIndex"e dikkat edin.
    
4. NAT Ağ Geçidi için bir IP adresi oluşturun.
    
Ağ geçidini yapılandırmak için ağınız hakkında bazı bilgilere ihtiyacınız var:    
  * IPAddress - NAT Ağ Geçidi IP sanal ağ alt ağı için varsayılan ağ geçidi adresi olarak kullanmak için IPv4 veya IPv6 adresini belirtir. Genel form a.b.c.1 'dir (örneğin, "192.168.0.1"). Son konumun 0,1 olması gerekmese de, genellikle (önek uzunluğuna göre) olur. Genellikle bir RFC 1918 özel ağ adresi alanı kullanmanız gerekir. 
  * Önek Uzunluk - Alt ağ öneki uzunluğu yerel alt net boyutunu (subnet maskesi) tanımlar. Alt önek uzunluğu 0 ile 32 arasında bir toplam değer olacaktır. 0 tüm internet harita, 32 sadece bir haritalanmış IP izin verecek. Ortak değerler, NAT'ye kaç IP eklenmesi gerektiğine bağlı olarak 24 ile 12 arasında değişir. Yaygın bir Önek Uzunluğu 24 'dür -- bu 255.255.255.0'lık bir alt ağ maskesidir.
  * InterfaceIndex - **ifIndex,** önceki adımda oluşturulan sanal anahtarın arabirim dizinidir. 

    ```powershell
    New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceIndex 13
    ```

### <a name="create-the-nat-network"></a>NAT ağını oluşturma

Ağ geçidini yapılandırmak için ağ ve NAT Ağ Geçidi hakkında bilgi sağlamanız gerekir:
  * Adı - Bu NAT ağının adıdır. 
  * InternalIPInterfaceAddressPrefix - NAT altağ öneki hem yukarıdan NAT Ağ Geçidi IP öneki hem de yukarıdan NAT Subnet Önek Uzunluğu açıklar. Genel form a.b.c.0/NAT Subnet Önek Uzunluğu olacaktır. 

PowerShell'de yeni bir NAT ağı oluşturun.
```powershell
New-NetNat -Name "InternalNat" -InternalIPInterfaceAddressPrefix 192.168.0.0/24
```


## <a name="create-the-guest-virtual-machine"></a>Konuk sanal makine oluşturma

>[!IMPORTANT] 
>
>Azure konuk aracıiçlenmiş VM'lerde desteklenmez ve hem ana bilgisayarda hem de iç içe olan VM'lerde sorunlara neden olabilir. İç içe kullanılan VM'lere Azure aracısını yüklemeyin ve Azure konuk aracısını zaten yüklü olan iç içe alınmış VM'leri oluşturmak için bir resim kullanmayın.

1. Hyper-V Manager'ı açın ve yeni bir sanal makine oluşturun. Oluşturduğunuz yeni İç ağı kullanacak şekilde sanal makineyi yapılandırın.
    
    ![AğConfig](./media/virtual-machines-nested-virtualization/configure-networking.png)
    
2. Konuk sanal makineye bir işletim sistemi yükleyin.
    
    >[!NOTE] 
    >
    >VM'ye yüklemek için bir işletim sistemi için yükleme ortamına ihtiyacınız var. Bu durumda Windows 10 Enterprise kullanıyoruz.

## <a name="assign-an-ip-address-to-the-guest-virtual-machine"></a>Konuk sanal makineye IP adresi atama

Konuk sanal makineye manuel olarak statik bir IP adresi ayarlayarak veya IP adresini dinamik olarak atamak için Azure VM'de DHCP yapılandırarak konuk sanal makineye bir IP adresi atayabilirsiniz.

###  <a name="option-1-configure-dhcp-to-dynamically-assign-an-ip-address-to-the-guest-virtual-machine"></a>Seçenek 1: Konuk sanal makineye dinamik olarak bir IP adresi atamak için DHCP'yi yapılandırın
Dinamik adres ataması için ana bilgisayar sanal makinede DHCP yapılandırmak için aşağıdaki adımları izleyin.

#### <a name="install-dchp-server-on-the-azure-vm"></a>Azure VM'de DCHP Server'ı yükleme

1. Sunucu Yöneticisi'ni açın. Pano'da **rol ve özellik ekle'yi**tıklatın. Rol ve Özellik Ekleme Sihirbazı görünür.
  
2. Sihirbazda, Sunucu Rolleri sayfasına kadar **İleri'yi** tıklatın.
  
3. **DHCP Server** onay kutusunu seçmek için tıklatın, **Özellikler Ekle'yi**tıklatın ve sihirbazı tamamlayana kadar **İleri'yi** tıklatın.
  
4. **Yükle'yi**tıklatın.

#### <a name="configure-a-new-dhcp-scope"></a>Yeni bir DHCP kapsamını yapılandırma

1. DHCP Yöneticisi'ni açın.
  
2. Gezinti bölmesinde, sunucu adını genişletin, **IPv4'e**sağ tıklayın ve **Yeni Kapsam'ı**tıklatın. Yeni Kapsam Sihirbazı görünür, **İleri'yi**tıklatın.
  
3. Kapsam için bir Ad ve Açıklama girin ve **İleri'yi**tıklatın.
  
4. DCHP Sunucunuz için bir IP Aralığı tanımlayın (örneğin, 192.168.0.100 ila 192.168.0.200).
  
5. Varsayılan Ağ Geçidi sayfasına kadar **İleri'yi** tıklatın. Varsayılan Ağ Geçidi olarak daha önce oluşturduğunuz IP Adresini (örneğin, 192.168.0.1) girin ve **ardından Ekle'yi**tıklatın.
  
6. Sihirbaz tamamlanana kadar **İleri'yi** tıklatın, tüm varsayılan değerleri bırakarak, ardından **Bitir'i**tıklatın.
    
### <a name="option-2-manually-set-a-static-ip-address-on-the-guest-virtual-machine"></a>Seçenek 2: Konuk sanal makinede statik bir IP adresini el ile ayarlama
KONUK sanal makineye dinamik olarak bir IP adresi ataması için DHCP'yi yapılandırmadıysanız, statik bir IP adresi ayarlamak için aşağıdaki adımları izleyin.

1. Azure VM'de PowerShell'i Yönetici olarak açın.

2. Konuk sanal makineye sağ tıklayın ve Bağlan'ı tıklatın.

3. Konuk sanal makinede oturum açın.

4. Konuk sanal makinede Ağ ve Paylaşım Merkezi'ni açın.

5. Ağ bağdaştırıcısını, önceki bölümde oluşturduğunuz NAT ağının aralığındaki bir adres için yapılandırın.

Bu örnekte 192.168.0.0/24 aralığında bir adres kullanacaksınız.

## <a name="test-connectivity-in-guest-virtual-machine"></a>Konuk sanal makinede test bağlantısı

Konuk sanal makinede tarayıcınızı açın ve bir web sayfasına gidin.
    ![GuestVM](./media/virtual-machines-nested-virtualization/guest-virtual-machine.png)

## <a name="set-up-intranet-connectivity-for-the-guest-virtual-machine"></a>Konuk sanal makine için intranet bağlantısı ayarlama

Konuk VM'ler ve Azure VM'leri arasında saydam bağlantının nasıl etkinleştirilen yönergeleri için lütfen [bu belgeye](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization-azure-virtual-network)başvurun.
