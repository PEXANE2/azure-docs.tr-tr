---
title: Windows Azure Konuk Aracısı sorunlarını giderme
description: Windows Azure Konuk aracısının sorun gidermesi çalışmıyor sorunları
services: virtual-machines-windows
ms.service: virtual-machines-windows
author: dkdiksha
manager: dcscontentpm
editor: ''
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/07/2020
ms.author: genli
ms.openlocfilehash: c3295365859ad3291a95b616cccc6fa265237a01
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88264049"
---
# <a name="troubleshooting-windows-azure-guest-agent"></a>Windows Azure Konuk Aracısı sorunlarını giderme

Windows Azure Konuk Aracısı, bir sanal makine (VM) aracısıdır. Bu, sanal makinenin 168.63.129.16 IP adresinde doku denetleyicisi (VM 'nin barındırıldığı temeldeki fiziksel sunucu) ile iletişim kurmasını sağlar. Bu, iletişimi kolaylaştıran bir sanal genel IP adresidir. Daha fazla bilgi için bkz. [IP adresi 168.63.129.16 nedir](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16).

 Şirket içinden Azure 'a geçirilmiş veya özelleştirilmiş bir görüntü kullanılarak oluşturulan VM 'de Windows Azure Konuk Aracısı yüklü değil. Bu senaryolarda, VM aracısını el ile yüklemelisiniz. VM aracısının nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure sanal makine aracısına genel bakış](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows).

Windows Azure Konuk Aracısı başarıyla yüklendikten sonra, VM 'de Services. msc dosyasında listelenen aşağıdaki hizmetleri görebilirsiniz:
 
- Windows Azure Konuk Aracısı hizmeti
- Telemetri hizmeti
- RD aracı hizmeti

**Windows Azure Konuk Aracısı hizmeti**: Bu hizmet, WAppAgent. log dosyasındaki tüm oturumdan sorumlu hizmettir. Bu hizmet çeşitli uzantıları ve konuktan konağa iletişimin yapılandırılmasından sorumludur. 

**Telemetri hizmeti**: Bu hizmet, sanal makinenin telemetri verilerini arka uç sunucusuna göndermekten sorumludur.

**RD Aracısı hizmeti**: Bu hizmet, Konuk aracısının yüklenmesinden sorumludur. Saydam yükleyici, Ayrıca, Konuk aracısının diğer bileşenlerini ve hizmetlerini yükseltmesine yardımcı olan RD aracısının bir bileşenidir. Rdadgent, Konuk VM 'den fiziksel sunucudaki ana bilgisayar aracısına sinyal göndermekten de sorumludur.

> [!NOTE]
> VM Konuk aracısının 2.7.41491.971 sürümünden başlayarak telemetri bileşeni Rdadgent hizmetine dahil edilmiştir, bu nedenle bu telemetri hizmetini yeni oluşturulan VM 'lerde listelenmiş görmeyebilirsiniz.

## <a name="checking-agent-status-and-version"></a>Aracı durumu ve sürümü denetleniyor

Azure portal ' de VM özellikleri sayfasına gidin ve **aracı durumunu**kontrol edin. Windows Azure Konuk Aracısı düzgün çalışıyorsa durum, **Ready**olarak gösterilir. VM Aracısı, **Ready** durumunda değilse, Azure Portal Uzantılar ve **Çalıştır komutu** çalışmaz.

## <a name="troubleshooting-vm-agent-that-is-in-not-ready-status"></a>Hazırlama durumunda olan VM Aracısı sorunlarını giderme

### <a name="step-1-check-whether-the-windows-azure-guest-agent-service-is-installed"></a>1. adım Microsoft Azure Konuk Aracısı hizmeti 'nin yüklü olup olmadığını denetleyin

- Paketi denetle

    C:\WindowsAzure klasörünü bulun. Sürüm numarasını gösteren GuestAgent klasörünü görürseniz, bu, Windows Azure Konuk aracısının VM 'de yüklü olduğu anlamına gelir. Yüklü paketi de arayabilirsiniz.  SANAL makinede Microsoft Azure Konuk Aracısı yüklüyse, paket şu konuma kaydedilir: `C:\windows\OEM\GuestAgent\VMAgentPackage.zip` .
    
    VM aracısının sanal makineye dağıtılıp dağıtılmadığını denetlemek için aşağıdaki PowerShell komutunu çalıştırabilirsiniz:
    
    `Get-Az VM -ResourceGroup “RGNAME” – Name “VMNAME” -displayhint expand`
    
    Çıktıda, **Provisionvmagent** özelliğini bulun ve değerin **true**olarak ayarlandığından emin olun. Bu durumda, aracının VM 'de yüklü olduğu anlamına gelir.
    
- Hizmetleri ve süreçlerini denetleme

   Hizmetler konsoluna (Services. msc) gidin ve şu hizmetlerin durumunu denetleyin: Windows Azure Konuk Aracısı hizmeti, Rdadgent hizmeti, Windows Azure telemetri hizmeti ve Windows Azure Ağ Aracısı hizmeti.
 
    Ayrıca, aşağıdaki süreçler için Görev Yöneticisi 'Ni inceleyerek bu hizmetlerin çalışıp çalışmadığını kontrol edebilirsiniz:
       
    - WindowsAzureGuestAgent.exe: Windows Azure Konuk Aracısı hizmeti
    - WaAppAgent.exe: Rdadgent hizmeti
    - WindowsAzureNetAgent.exe: Windows Azure Ağ Aracısı hizmeti
    - WindowsAzureTelemetryService.exe: Windows Azure telemetri hizmeti

   Bu işlem ve Hizmetleri bulamıyorsanız, Windows Azure Konuk aracısının yüklü olmadığını gösterir.

- Program ve özelliği denetleyin

    Denetim Masası 'nda, Windows Azure Konuk Aracısı hizmetinin yüklenip yüklenmediğini öğrenmek için **Programlar ve Özellikler** ' e gidin.

Çalıştıran bir paket, hizmet ve işlem bulamazsanız ve Windows Azure Konuk Aracısı 'nın programlar ve Özellikler altında yüklü olduğunu görmezseniz, [Microsoft Azure Konuk Aracısı hizmetini yüklemeyi](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)deneyin. Konuk Aracısı düzgün bir şekilde yüklenmiyorsa, [VM aracısını çevrimdışı yükleyebilirsiniz](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/install-vm-agent-offline).

Hizmetleri görebilir ve çalışıyorsa, sorunun çözümlenip çözümlenmediğini görmek için hizmeti yeniden başlatın. Hizmetler durdurulmuşsa, bunları başlatın ve birkaç dakika bekleyin. Ardından, **Aracı durumunun** **hazırlık**olarak raporlanıp raporlanmadığını kontrol edin. Bu hizmetlerin kilitlendiğini görürseniz, bazı üçüncü taraf süreçler bu hizmetlerin kilitlenmesine neden olabilir. Bu sorunların daha fazla giderilmesi için [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvurun.

### <a name="step-2-check-whether-auto-update-is-working"></a>2. adım otomatik güncelleştirme çalışıp çalışmadığını denetleyin

Windows Azure Konuk aracısının otomatik güncelleştirme özelliği vardır. Bu, yeni güncelleştirmeleri otomatik olarak denetler ve yükler. Otomatik güncelleştirme özelliği doğru şekilde çalışmazsa, aşağıdaki adımları kullanarak Windows Azure Konuk aracısını kaldırmayı ve yüklemeyi deneyin:

1. **Programlar ve Özellikler**'de Windows Azure Konuk Aracısı görünürse, Microsoft Azure Konuk Aracısı 'nı kaldırın.

2. Yönetici ayrıcalıklarına sahip bir komut Istemi penceresi açın.

3. Konuk Aracısı hizmetlerini durdurun. Hizmetler durmazsanız, Hizmetleri **el ile başlatılacak** şekilde ayarlamanız ve ardından VM 'yi yeniden başlatmanız gerekir.
    ```
    net stop rdagent
    net stop WindowsAzureGuestAgent
    net stop WindowsAzureTelemetryService
    ```
1. Konuk Aracısı hizmetlerini silme:
    ```
    sc delete rdagent
    sc delete WindowsAzureGuestAgent
    sc delete WindowsAzureTelemetryService
    ```
1. Altında `C:\WindowsAzure` , eski adlı bir klasör oluşturun.

1. Paket veya GuestAgent adlı klasörleri eskı klasöre taşıyın.

1. Aracı yükleme paketinin en son sürümünü [buradan](https://go.microsoft.comfwlink/?linkid=394789&clcid=0x409)indirin ve yükleyin. Yüklemeyi gerçekleştirmek için yönetici haklarına sahip olmanız gerekir.

1. Aşağıdaki komutu kullanarak Konuk aracısını yükler:

    ```
    msiexec.exe /i c:\VMAgentMSI\WindowsAzureVmAgent.2.7.<version>.fre.msi /quiet /L*v c:\VMAgentMSI\msiexec.log
    ```
    Ardından Konuk Aracısı hizmetlerinin doğru şekilde başlayıp başlamamadığını denetleyin.
 
    Konuk aracısının doğru şekilde yüklenebileceği nadir durumlarda, [VM aracısını çevrimdışı yükleyebilirsiniz](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/install-vm-agent-offline).
    

### <a name="step-3-check-whether-the-vm-can-connect-to-the-fabric-controller"></a>3. adım VM 'nin doku denetleyicisine bağlanıp bağlanamamadığını denetleyin

VM 'nin 80, 32526 ve 443 bağlantı noktalarında 168.63.129.16 'ye bağlanıp bağlanamamadığını test etmek için PsPing gibi bir araç kullanın. VM beklendiği gibi bağlanmazsa, 80, 443 ve 32526 bağlantı noktaları üzerinden giden iletişimin VM 'deki yerel güvenlik duvarınızda açık olup olmadığını denetleyin. Bu IP adresi engellenirse, VM Aracısı çeşitli senaryolarda beklenmeyen davranışları görüntüleyebilir.

## <a name="advanced-troubleshooting"></a>Gelişmiş sorun giderme

Windows Azure Konuk Aracısı sorunlarını gidermeye yönelik olaylar aşağıdaki günlük dosyalarına kaydedilir:

- C:\WindowsAzure\Logs\WaAppAgent.log
- C:\WindowsAzure\Logs\TransparentInstaller.log

Aşağıda, Microsoft Azure Konuk aracısının, bir veya daha fazla çalışmayı durdurarak **girebileceği** bazı yaygın senaryolar verilmiştir.

### <a name="agent-stuck-on-starting"></a>Aracı "başlatılıyor" durumunda takıldı

WaAppAgent günlüğünde, aracının başlangıç sürecinde takılı olduğunu ve başlayamazı görebilirsiniz.

**Günlük bilgileri**
 
[00000007] [05/28/2019 12:58:50.90] [ıNFO] WindowsAzureGuestAgent başlatılıyor. Sürüm 2.7.41491.901

**Çözümlemeleri**
 
VM hala Windows Azure Konuk aracısının eski sürümünü çalıştırıyor. C:\WindowsAzure klasöründe, aynı sürümden birkaçı dahil olmak üzere birden çok Windows Azure Konuk aracı örneğinin yüklü olduğunu fark edebilirsiniz. Birden çok aracı örneği yüklü olduğundan, sanal makine Windows Azure Konuk Aracısı 'nın en son sürümünü başlatamaz.

**Çözüm**

Windows Azure Konuk aracısını el ile kaldırın ve ardından aşağıdaki adımları izleyerek yeniden yükleyin:

1. Programlar ve Özellikler > Denetim Masası 'nı açın ve Windows Azure Konuk aracısını kaldırın.
1. Görev Yöneticisi 'ni açın ve şu hizmetleri durdurun: Windows Azure Konuk Aracısı hizmeti, Rdadgent hizmeti, Windows Azure telemetri hizmeti ve Windows Azure Ağ Aracısı hizmeti.
1. C:\WindowsAzure altında, eskı adlı bir klasör oluşturun.
1. Paket veya GuestAgent adlı klasörleri eskı klasöre taşıyın. Ayrıca, C:\WindowsAzure\logs içindeki GuestAgent klasörlerinden herhangi birini, GuestAgent_x. x. xxxxx olarak başlayan eskı klasöre taşıyın.
1. MSI aracısının en son sürümünü indirip yükleyin. Yüklemeyi gerçekleştirmek için yönetici haklarına sahip olmanız gerekir.
1. Aşağıdaki MSI komutunu kullanarak Konuk aracısını yükler:
    ```
    msiexec.exe /i c:\VMAgentMSI\WindowsAzureVmAgent.2.7.<version>.fre.msi /quiet /L*v c:\VMAgentMSI\msiexec.log`
    ```
1. Rdadgent, Windows Azure Konuk Aracısı ve Windows Azure telemetri Hizmetleri 'nin çalıştığını doğrulayın.
 
1. Windows Azure Konuk aracısının en son sürümünün çalıştığından emin olmak için WaAppAgent. log dosyasına bakın.
 
1. C:\windowsazurealtındaki eskı klasörü silin.
  
### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>Kablolu sunucu IP 'si (konak IP 'si) ile bağlantı kurulamıyor 

WaAppAgent. log ve telemetri. log dosyasında aşağıdaki hata girişlerini fark etmiş olursunuz:

**Günlük bilgileri**

```Log sample
[ERROR] GetVersions() failed with exception: Microsoft.ServiceModel.Web.WebProtocolException: Server Error: Service Unavailable (ServiceUnavailable) ---> 
System.Net.WebException: The remote server returned an error: (503) Server Unavailable.
```
```Log sample
[00000011] [12/11/2018 06:27:55.66] [WARN]  (Ignoring) Exception while fetching supported versions from HostGAPlugin: System.Net.WebException: Unable to connect to the remote server 
---> System.Net.Sockets.SocketException: An attempt was made to access a socket in a way forbidden by its access permissions 168.63.129.16:32526
at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
at System.Net.ServicePoint.ConnectSocketInternal(Boolean connectFailure, Socket s4, Socket s6, Socket& socket, IPAddress& address, ConnectSocketState status, IAsyncResult asyncResult, Exception& exception)
--- End of inner exception stack trace ---
at System.Net.WebClient.DownloadDataInternal(Uri address, WebRequest& request)
at System.Net.WebClient.DownloadString(Uri address)
at Microsoft.GuestAgentHostPlugin.Client.GuestInformationServiceClient.GetVersions()
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.HostGAPluginUtility.UpdateHostGAPluginAvailability()`
```
**Çözümlemeleri**

VM, kablolu sunucu ana bilgisayar sunucusuna ulaşamıyor.

**Çözüm**

1. Kablolu sunucuya erişilemediğinden, Uzak Masaüstü kullanarak VM 'ye bağlanın ve ardından bir internet tarayıcısından aşağıdaki URL 'ye erişmeyi deneyin: http://168.63.129.16/?comp=versions 
1. Adım 1 ' deki URL 'ye ulaşırsanız, DHCP 'nin etkin olup olmadığını ve DNS 'nin olduğunu öğrenmek için ağ arabirimini kontrol edin. Ağ arabiriminin DHCP durumunu denetlemek için şu komutu çalıştırın:  `netsh interface ip show config` .
1. DHCP devre dışıysa, sarı olan değeri arabiriminiz adına değiştirdiğinizden emin olmak için aşağıdaki işlemi çalıştırın: `netsh interface ip set address name="Name of the interface" source=dhcp` .
1. 168.63.129.16 IP adresine erişimi engelleyebilecek bir güvenlik duvarı, ara sunucu veya başka bir kaynaktan kaynaklanmış olabilecek herhangi bir sorun olup olmadığını denetleyin.
1. Windows güvenlik duvarı veya bir üçüncü taraf güvenlik duvarının 80, 443 ve 32526 bağlantı noktalarına erişimi engelleyip engellemediğini denetleyin. Bu adresin neden engellenmediği hakkında daha fazla bilgi için bkz. [IP adresi nedir 168.63.129.16](https://docs.microsoft.com/azure/virtual-network/what-is-ip-address-168-63-129-16).

### <a name="guest-agent-is-stuck-stopping"></a>Konuk Aracısı "durduruluyor" olarak takılmış  

WaAppAgent. log dosyasında aşağıdaki hata girişlerini fark edersiniz:

**Günlük bilgileri** 

```Log sample
[00000007] [07/18/2019 14:46:28.87] [WARN] WindowsAzureGuestAgent stopping.
[00000007] [07/18/2019 14:46:28.89] [INFO] Uninitializing StateExecutor with WaitForTerminalStateReachedOnEnd : True
[00000004] [07/18/2019 14:46:28.89] [WARN] WindowsAzureGuestAgent could not be stopped. Exception: System.NullReferenceException: Object reference not set to an instance of an object.
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.GoalStateExecutorBase.WaitForExtensionWorkflowComplete(Boolean WaitForTerminalStateReachedOnEnd)
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.GoalStateExecutorBase.Uninitialize(Boolean WaitForTerminalStateReachedOnEnd)
at Microsoft.WindowsAzure.GuestAgent.ContainerStateMachine.GoalStateExecutorForCloud.Uninitialize(Boolean WaitForTerminalStateReachedOnEnd)
at Microsoft.WindowsAzure.GuestAgent.AgentCore.AgentCore.Stop(Boolean waitForTerminalState)
at Microsoft.WindowsAzure.GuestAgent.AgentCore.AgentService.DoStopService()
at Microsoft.WindowsAzure.GuestAgent.AgentCore.AgentService.<>c__DisplayClass2.<OnStopProcessing>b__1()
```
**Çözümlemeleri**

Windows Azure Konuk Aracısı, durdurma sürecinde takıldı.

**Çözüm**

1. WaAppAgent.exe VM üzerinde çalıştığından emin olun. Çalışmıyorsa, rdgagent hizmetini yeniden başlatın ve beş dakika bekleyin. WaAppAgent.exe çalışırken WindowsAzureGuest.exe sürecini sonlandırın.
2. 1. adım sorunu çözmezse, yüklü olan sürümü kaldırın ve aracının en son sürümünü el ile yükleme.

### <a name="npcap-loopback-adapter"></a>Npcap geri döngü bağdaştırıcısı 

WaAppAgent. log dosyasında aşağıdaki hata girişlerini fark edersiniz:
 
```Log sample
[00000006] [06/20/2019 07:44:28.93] [INFO]  Attempting to discover fabric address on interface Npcap Loopback Adapter.
[00000024] [06/20/2019 07:44:28.93] [WARN]  Empty DHCP option data returned
[00000006] [06/20/2019 07:44:28.93] [ERROR] Did not discover fabric address on interface Npcap Loopback Adapter
```
**Çözümlemeleri**

Npcap geri döngü bağdaştırıcısı VM 'ye Wireshark tarafından yüklenir. Wireshark, ağ trafiğinin profilini oluşturmak ve paketleri çözümlemek için açık kaynaklı bir araçtır. Bu tür bir araç genellikle ağ Çözümleyicisi, ağ protokolü Çözümleyicisi veya algılayıcı olarak adlandırılır.

**Çözüm**

Npcap geri döngü bağdaştırıcısı büyük olasılıkla WireShark tarafından yüklendi. Devre dışı bırakmayı deneyin ve sonra sorunun çözümlenip çözümlenmediğini denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

"Windows Azure Konuk Aracısı çalışmıyor" sorununu gidermek için [Microsoft destek 'e başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).