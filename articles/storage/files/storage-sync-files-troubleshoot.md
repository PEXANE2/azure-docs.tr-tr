---
title: Sorun Giderme Azure Dosya Eşitlemi | Microsoft Dokümanlar
description: Azure Dosya Eşitlemenile ilgili sık karşılaşılan sorunları giderin.
author: jeffpatt24
ms.service: storage
ms.topic: conceptual
ms.date: 1/22/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: d46f513fccf9921d4cf47835bc9d5be4c6ffe241
ms.sourcegitcommit: 515482c6348d5bef78bb5def9b71c01bb469ed80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80607485"
---
# <a name="troubleshoot-azure-file-sync"></a>Azure Dosya Eşitleme ile ilgili sorunları giderme
Kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmek ve şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu korumak için Azure Dosya Eşitlemeyi'ni kullanın. Azure Dosya Eşitleme, Windows Server’ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS dahil olmak üzere verilerinize yerel olarak erişmek için Windows Server'da kullanılabilen tüm protokolleri kullanabilirsiniz. Dünya çapında ihtiyacınız olduğu kadar önbelleke sahip olabilirsiniz.

Bu makale, Azure Dosya Eşitleme dağıtımınızda karşılaşabileceğiniz sorunları gidermenize ve çözmenize yardımcı olmak için tasarlanmıştır. Ayrıca, konunun daha derin bir şekilde araştırılması gerektiğinde, sistemden önemli günlüklerin nasıl toplandığını da açıklıyoruz. Sorunuzun cevabını göremiyorsanız, aşağıdaki kanallardan (artan sırada) bizimle iletişime geçebilirsiniz:

1. [Azure Depolama Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
2. [Azure Dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).
3. Microsoft Desteği. **Azure portalında, Yardım** sekmesinde yeni bir destek isteği oluşturmak için Yardım + **destek** düğmesini seçin ve ardından Yeni **destek isteğini**seçin.

## <a name="im-having-an-issue-with-azure-file-sync-on-my-server-sync-cloud-tiering-etc-should-i-remove-and-recreate-my-server-endpoint"></a>Sunucumda Azure Dosya Eşitlemi ile ilgili bir sorun yaşıyorum (eşitleme, bulut katmanlama, vb.). Sunucu bitiş noktamı kaldırmalı ve yeniden oluşturmalı mıyım?
[!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]

## <a name="agent-installation-and-server-registration"></a>Aracı yükleme ve sunucu kaydı
<a id="agent-installation-failures"></a>**Sorun giderme aracı yükleme hataları**  
Azure Dosya Eşitleme aracısı yüklemesi başarısız olursa, yükseltilmiş bir komut isteminde aracı yükleme sırasında günlüğe kaydetmeyi açmak için aşağıdaki komutu çalıştırın:

```
StorageSyncAgent.msi /l*v AFSInstaller.log
```

Yükleme hatasının nedenini belirlemek için installer.log'u gözden geçirin.

<a id="agent-installation-on-DC"></a>**Etkin Dizin Etki Alanı Denetleyicisi'nde aracı yükleme başarısız olur**  
Eşitleme aracısını, PDC rol sahibinin Windows Server 2008 R2 veya işletim sistemi sürümünün altında olduğu bir Active Directory etki alanı denetleyicisine yüklemeye çalışırsanız, eşitleme aracısının yüklenemediği soruna ulaşabilirsiniz.

Çözmek için, PDC rolünü Windows Server 2012 R2 veya daha yeni çalıştıran başka bir etki alanı denetleyicisine aktarın, ardından eşitlemeyi yükleyin.

<a id="parameter-is-incorrect"></a>**Windows Server 2012 R2'de bir ses birimine erişim hatasıyla başarısız olur: Parametre yanlış**  
Windows Server 2012 R2'de bir sunucu bitiş noktası oluşturduktan sonra, ses düzeyine erişirken aşağıdaki hata oluşur:

driveletter:\ erişilemez.  
Parametre yanlış.

Çözmek için, Windows Server 2012 R2 için en son güncelleştirmeleri yükleyin ve sunucuyu yeniden başlatın.

<a id="server-registration-missing-subscriptions"></a>**Sunucu Kaydı tüm Azure Aboneliklerini listelemiyor**  
ServerRegistration.exe kullanarak bir sunucu kaydederken, Azure Aboneliği açılır listesini tıklattığınızda abonelikler eksik olur.

ServerRegistration.exe şu anda çok kiracılı ortamları desteklemediğinden bu sorun oluşur. Bu sorun gelecekteki bir Azure Dosya Eşitleme aracısı güncelleştirmesinde giderilir.

Bu sorunu çözmek için sunucuyu kaydetmek için aşağıdaki PowerShell komutlarını kullanın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<guid>" -TenantID "<guid>"
Register-AzureRmStorageSyncServer -SubscriptionId "<guid>" -ResourceGroupName "<string>" -StorageSyncServiceName "<string>"
```

<a id="server-registration-prerequisites"></a>**Sunucu Kaydı aşağıdaki mesajı görüntüler: "Ön koşullar eksik"**  
Az veya AzureRM PowerShell modülü PowerShell 5.1'e yüklenmezse bu ileti görüntülenir. 

> [!Note]  
> ServerRegistration.exe PowerShell 6.x'i desteklemez. Sunucuyu kaydetmek için PowerShell 6.x'teki Register-AzStorageSyncServer cmdlet'i kullanabilirsiniz.

Az veya AzureRM modüllerini PowerShell 5.1'e yüklemek için aşağıdaki adımları gerçekleştirin:

1. **Powershell'i** yükseltilmiş bir komut isteminden yazın ve enter tuşuna basın.
2. Belgeleri izleyerek en son Az veya AzureRM modüllerini yükleyin:
    - [Az modülü (.NET 4.7.2 gerektirir)](https://go.microsoft.com/fwlink/?linkid=2062890)
    - [AzureRM modülü]( https://go.microsoft.com/fwlink/?linkid=856959)
3. ServerRegistration.exe'yi çalıştırın ve sunucuyu Depolama Eşitleme Hizmetine kaydettirmek için sihirbazı tamamlayın.

<a id="server-already-registered"></a>**Sunucu Kaydı aşağıdaki mesajı görüntüler: "Bu sunucu zaten kayıtlı"** 

!["Sunucu zaten kayıtlıdır" hata iletisi ile Sunucu Kaydı iletişim kutusunun ekran görüntüsü](media/storage-sync-files-troubleshoot/server-registration-1.png)

Sunucu daha önce bir Depolama Eşitleme Hizmeti'ne kayıtlıysa, bu ileti görüntülenir. Sunucunun geçerli Depolama Eşitleme Hizmeti'nden kaydını çıkarmak ve ardından yeni bir Depolama Eşitleme Hizmetine kaydolmak için, [Azure Dosya Eşitlemi ile bir sunucunun kaydını açma'da](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service)açıklanan adımları tamamlayın.

Sunucu, Depolama Eşitleme Hizmeti'ndeki **Kayıtlı sunucular** altında listelenmemişse, kaydını çıkarmak istediğiniz sunucuda aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Sunucu bir kümenin parçasıysa, küme kaydını kaldırmak için isteğe bağlı *Reset-StorageSyncServer -CleanClusterRegistration* parametresini kullanabilirsiniz.

<a id="web-site-not-trusted"></a>**Bir sunucu kaydettirdiğimde, çok sayıda "web sitesi güvenilir değil" yanıtları görüyorum. Neden?**  
Bu sorun, sunucu kaydı sırasında **Gelişmiş Internet Explorer Güvenlik** ilkesi etkinleştirildiğinde oluşur. **Gelişmiş Internet Explorer Güvenlik** ilkesini nasıl doğru devre dışı bırakabilirsiniz hakkında daha fazla bilgi [How to deploy Azure File Sync](storage-sync-files-deployment-guide.md)için [bkz.](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync)

<a id="server-registration-missing"></a>**Sunucu, Azure portalında kayıtlı sunucular altında listelenmez**  
Bir sunucu Depolama Eşitleme Hizmeti için **Kayıtlı sunucular** altında listelenmemişse:
1. Kaydolmak istediğiniz sunucuda oturum açın.
2. Dosya Gezgini'ni açın ve ardından Depolama Eşitlemesi Aracısı yükleme dizinine gidin (varsayılan konum C:\Program Files\Azure\StorageSyncAgent'dır). 
3. ServerRegistration.exe'yi çalıştırın ve sunucuyu Depolama Eşitleme Hizmetine kaydettirmek için sihirbazı tamamlayın.

## <a name="sync-group-management"></a>Eşitleme grubu yönetimi
<a id="cloud-endpoint-using-share"></a>**Bulut uç noktası oluşturma, bu hata ile başarısız olur: "Belirtilen Azure FileShare zaten farklı bir CloudEndpoint tarafından kullanımda"**  
Azure dosya paylaşımı zaten başka bir bulut uç noktasının kullanımında olduğunda bu hata oluşur. 

Bu iletiyi görürseniz ve Şu anda Azure dosya paylaşımı bulut bitiş noktası tarafından kullanılmaz, Azure dosya paylaşımındaki Azure Dosya Eşitleme meta verilerini temizlemek için aşağıdaki adımları tamamlayın:

> [!Warning]  
> Şu anda bulut bitiş noktası tarafından kullanılmakta olan bir Azure dosya paylaşımındaki meta verilerin silmesi, Azure Dosya Eşitleme işlemlerinin başarısız olmasını sağlar. 

1. Azure portalında Azure dosya paylaşımınıza gidin.  
2. Azure dosya paylaşımını sağ tıklatın ve ardından **meta verileri edit'i**seçin.
3. **SyncService'e**sağ tıklayın ve sonra **Sil'i**seçin.

<a id="cloud-endpoint-authfailed"></a>**Bulut uç noktası oluşturma başarısız olur, bu hata ile: "AuthorizationFailed"**  
Bu hata, kullanıcı hesabınızda bulut bitiş noktası oluşturmak için yeterli haklara sahip değilse oluşur. 

Bulut bitiş noktası oluşturmak için kullanıcı hesabınızda aşağıdaki Microsoft Yetkilendirme izinleri olması gerekir:  
* Oku: Rol tanımı alın
* Yazma: Özel rol tanımı oluşturma veya güncelleştirme
* Oku: Rol ataması alın
* Yazma: Rol ataması oluşturma

Aşağıdaki yerleşik roller, gerekli Microsoft Yetkilendirme izinlerine sahiptir:  
* Sahip
* Kullanıcı Erişimi Yöneticisi

Kullanıcı hesabı rolünün gerekli izinlere sahip olup olmadığını belirlemek için:  
1. Azure portalında **Kaynak gruplarını**seçin.
2. Depolama hesabının bulunduğu kaynak grubunu seçin ve ardından **Access denetimi 'ni (IAM)** seçin.
3. Rol **atamaları** sekmesini seçin.
4. Kullanıcı hesabınız için **Rolü** (örneğin, Sahibi veya Katılımcısı) seçin.
5. Kaynak **Sağlayıcı** listesinde **Microsoft Yetkilendirme'yi**seçin. 
    * **Rol ataması** **Okuma** ve **Yazma** izinleri olmalıdır.
    * **Rol tanımında** **Okuma** ve **Yazma** izinleri olmalıdır.

<a id="-2134375898"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2134375898 veya 0x80c80226)**  
Sunucu uç noktası yolu sistem birimindeyse ve bulut katmanlama etkinleştirildiyse bu hata oluşur. Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.

<a id="-2147024894"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2147024894 veya 0x80070002)**  
Belirtilen sunucu uç noktası yolu geçerli değilse bu hata oluşur. Belirtilen sunucu uç noktası yolunun yerel olarak eklenmiş bir NTFS birimi olduğunu doğrulayın. Not, Azure Dosya Eşitlemesi eşlenen sürücüleri sunucu bitiş noktası yolu olarak desteklemez.

<a id="-2134375640"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2134375640 veya 0x80c80328)**  
Bu hata, belirtilen sunucu uç noktası yolu NTFS birimi değilse oluşur. Belirtilen sunucu uç noktası yolunun yerel olarak eklenmiş bir NTFS birimi olduğunu doğrulayın. Not, Azure Dosya Eşitlemesi eşlenen sürücüleri sunucu bitiş noktası yolu olarak desteklemez.

<a id="-2134347507"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2134347507 veya 0x80c8710d)**  
Azure Dosya Eşitleme, sıkıştırılmış Sistem Birim Bilgisi klasörü bulunan birimlerde sunucu uç noktalarını desteklemediğinden bu hata oluşur. Bu sorunu çözmek için Sistem Birim Bilgisi klasörünün sıkıştırmasını açın. Sistem Birim Bilgisi klasörü birimde sıkıştırılmış durumdaki tek klasörse aşağıdaki adımları izleyin:

1. [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec) aracını indirin.
2. Sistem hesabı altında çalışan bir komut istemi başlatmak için yükseltilmiş bir komut istemi aşağıdaki komutu çalıştırın: **PsExec.exe -i -s -d cmd**
3. Sistem hesabı altında çalışan komut isteminde şu komutu yazın ve Enter tuşuna basın:   
    **cd /d "sürücü harfi:\Sistem Hacim Bilgileri"**  
    **kompakt /u /s**

<a id="-2134376345"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2134376345 veya 0x80C80067)**  
Sunucu başına sunucu uç noktası sınırına ulaşılırsa bu hata oluşur. Azure Dosya Eşitleme şu anda sunucu başına en çok 30 sunucu uç noktasını destekler. Daha fazla bilgi için Bkz. [Azure Dosya Eşitleme ölçeği hedefleri.](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#azure-file-sync-scale-targets)

<a id="-2134376427"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2134376427 veya 0x80c80015)**  
Belirtilen sunucu uç noktası yolunu başka bir sunucu uç noktası zaten eşitliyorsa bu hata oluşur. Azure Dosya Eşitleme birden çok sunucu uç noktasının aynı dizini veya birimi eşitlemesini desteklemez.

<a id="-2160590967"></a>**Sunucu bitiş noktası oluşturma, bu hata ile başarısız olur: "MgmtServerJobFailed" (Hata kodu: -2160590967 veya 0x80c80077)**  
Sunucu bitiş noktası yolu alt katmanlı dosyalar içeriyorsa bu hata oluşur. Bir sunucu bitiş noktası yakın zamanda kaldırıldıysa, artık katmanlı dosya temizleme tamamlanana kadar bekleyin. Bir Olay Kimliği 6662, yetim katmanlı dosya temizleme başladıktan sonra Telemetri olay günlüğüne kaydedilir. Bir Olay Kimliği 6661, yedek katmanlı dosya temizleme tamamlandıktan ve yol kullanılarak bir sunucu bitiş noktası yeniden oluşturulduktan sonra günlüğe kaydedilir. Bir Event ID 6661 günlüğe kaydedildikten sonra sunucu bitiş noktası oluşturma başarısız olursa, katmanlı dosyalarda belgelenen adımları gerçekleştirerek bir sunucu bitiş noktası bölümü [sildikten sonra sunucuda erişilemez.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint)

<a id="-2134347757"></a>**Sunucu bitiş noktası silme bu hata ile başarısız olur: "MgmtServerJobExpired" (Hata kodu: -2134347757 veya 0x80c87013)**  
Sunucu çevrimdışıysa veya ağ bağlantısı yoksa bu hata oluşur. Sunucu artık kullanılabilir değilse portalda sunucunun kaydını kaldırın; bu işlem sunucu uç noktalarını siler. Sunucu uç noktalarını silmek için, [Azure Dosya Eşitlemi ile bir sunucunun kaydını açma'da](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service)açıklanan adımları izleyin.

<a id="server-endpoint-provisioningfailed"></a>**Sunucu uç noktası özellikleri sayfasını açamıyor veya bulut katmanlama ilkesini güncelleştiremiyor**  
Sunucu bitiş noktasındaki bir yönetim işlemi başarısız olursa bu sorun oluşabilir. Sunucu uç noktası özellikleri sayfası Azure portalında açılmıyorsa, sunucudan PowerShell komutlarını kullanarak sunucu bitiş noktasını güncelleştirmek bu sorunu giderebilir. 

```powershell
# Get the server endpoint id based on the server endpoint DisplayName property
Get-AzStorageSyncServerEndpoint `
    -ResourceGroupName myrgname `
    -StorageSyncServiceName storagesvcname `
    -SyncGroupName mysyncgroup | `
Tee-Object -Variable serverEndpoint

# Update the free space percent policy for the server endpoint
Set-AzStorageSyncServerEndpoint `
    -InputObject $serverEndpoint
    -CloudTiering `
    -VolumeFreeSpacePercent 60
```
<a id="server-endpoint-noactivity"></a>**Sunucu bitiş noktası "Etkinlik Yok" veya "Beklemede" sistem durumu vardır ve kayıtlı sunucular bıçak taki sunucu durumu "Çevrimdışı görünür"**  

Depolama Eşitleme Monitörü işlemi (AzureStorageSyncMonitor.exe) çalışmıyorsa veya sunucu Azure Dosya Eşitleme hizmetine erişemiyorsa bu sorun oluşabilir.

Portalda "Çevrimdışı görünür" olarak gösterilen sunucuda, sunucunun Azure Dosya Eşitleme hizmetine neden erişemediğini belirlemek için Telemetri etkinlik günlüğündeki (Uygulamalar ve Hizmetler\Microsoft\FileSync\Agent in Event Viewer altında bulunan) Event ID 9301'e bakın. 

- **GetNextJob durumla tamamlanırsa: 0** günlüğe kaydedilirse, sunucu Azure Dosya Eşitleme hizmetiyle iletişim kurabilir. 
    - Sunucuda Görev Yöneticisi'ni açın ve Depolama Eşitleme İzleyicisi (AzureStorageSyncMonitor.exe) işleminin çalıştığından emin olun. İşlem çalışmıyorsa önce sunucuyu yeniden başlatmayı deneyin. Sunucunun yeniden başlatılması sorunu çözmezse en son Azure Dosya Eşitleme [aracısı sürümüne](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes) yükseltin. 

- **GetNextJob durumla tamamlanırsa: -2134347756** günlüğe kaydedilirse, sunucu bir güvenlik duvarı veya proxy nedeniyle Azure Dosya Eşitleme hizmetiyle iletişim kuramaz. 
    - Sunucu bir güvenlik duvarının arkasındaysa 443 numaralı bağlantı noktası üzerinden giden bağlantılara izin verildiğinden emin olun. Güvenlik duvarı trafiği belirli etki alanlarıyla sınırlıyorsa, Güvenlik Duvarı [belgelerinde](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#firewall) listelenen etki alanlarının erişilebilir olduğunu onaylayın.
    - Sunucu bir proxy'nin arkasındaysa, Proxy [belgelerindeki](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#proxy)adımları izleyerek makine genelinde veya uygulamaya özgü proxy ayarlarını yapılandırın.
    - Hizmet uç noktalarına ağ bağlantısını denetlemek için Test-StorageSyncNetworkConnectivity cmdlet'i kullanın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#test-network-connectivity-to-service-endpoints)

- **GetNextJob durumla tamamlanırsa: -2134347764** günlüğe kaydedilirse, sunucu süresi dolmuş veya silinmiş bir sertifika nedeniyle Azure Dosya Eşitleme hizmetiyle iletişim kuramaz.  
    - Kimlik doğrulaması için kullanılan sertifikayı sıfırlamak için sunucuda aşağıdaki PowerShell komutunu çalıştırın:
    ```powershell
    Reset-AzStorageSyncServerCertificate -ResourceGroupName <string> -StorageSyncServiceName <string>
    ```
<a id="endpoint-noactivity-sync"></a>**Sunucu bitiş noktası "Etkinlik Yok" sistem durumu vardır ve kayıtlı sunucular bıçak sunucu durumu "Çevrimiçi"**  

"Etkinlik Yok" sunucu bitiş noktası durumu, sunucu bitiş noktasının son iki saat içinde eşitleme etkinliğini günlüğe kaydetmediğini gösterir.

Sunucudaki geçerli eşitleme etkinliğini denetlemek için geçerli [eşitleme oturumunun ilerlemesini nasıl izleyebilirim?](#how-do-i-monitor-the-progress-of-a-current-sync-session)

Sunucu bitiş noktası, bir hata veya yetersiz sistem kaynakları nedeniyle eşitleme etkinliğini birkaç saat süreyle günlüğe kaydedebilirsiniz. En son Azure Dosya Eşitleme [aracısı sürümünün](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes) yüklü olduğunu doğrulayın. Sorun devam ederse, bir destek isteği açın.

> [!Note]  
> Kayıtlı sunucular bıçak üzerinde sunucu durumu "Çevrimdışı Görünür," Sunucu bitiş noktasında belgelenen adımları gerçekleştirmek ["Hiçbir Etkinlik" veya "Bekleyen" bir sistem durumu vardır ve kayıtlı sunucular bıçak sunucu durumu "Çevrimdışı görünür" bölümü.](#server-endpoint-noactivity)

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Doğrudan Azure dosya paylaşımımda veya portal üzerinden bir dosya oluşturduysam, dosyanın eşitleme grubundaki sunucularla eşitlemesi ne kadar sürer?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="serverendpoint-pending"></a>**Sunucu uç noktası durumu birkaç saat beklemede durumda**  
Bulut bitiş noktası oluşturur ve veri içeren bir Azure dosya paylaşımı kullanırsanız bu sorun beklenir. Dosyaların bulut ve sunucu bitiş noktaları arasında eşitlemesi için Azure dosya paylaşımındaki değişiklikleri tarayan değişiklik numaralandırma işi tamamlanmalıdır. İşi tamamlama süresi, Azure dosya paylaşımındaki ad alanının boyutuna bağlıdır. Değişiklik numaralandırma işi tamamlandıktan sonra sunucu uç noktası durumu güncelleştirilmelidir.

### <a name="how-do-i-monitor-sync-health"></a><a id="broken-sync"></a>Eşitleme durumunu nasıl izleyebilirim?
# <a name="portal"></a>[Portal](#tab/portal1)
Her eşitleme grubunda, tamamlanan son eşitleme oturumlarının durumunu görmek için tek tek sunucu uç noktalarını ayrıntılı olarak açabilirsiniz. Yeşil Sistem Durumu sütunu ve 0 eşitleme olmayan dosyalar eşitlemenin beklendiği gibi çalıştığını gösterir. Bu durumda değilse, sık kullanılan eşitleme hataları listesi ve eşitlenmemiş dosyaların nasıl işleyeceğiniz için aşağıya bakın. 

![Azure portalının ekran görüntüsü](media/storage-sync-files-troubleshoot/portal-sync-health.png)

# <a name="server"></a>[Sunucu](#tab/server)
Sunucunun telemetri günlüklerine gidin, bu da Olay Görüntüleyicisi'nde `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`bulunabilir. Olay 9102 tamamlanmış bir eşitleme oturumuna karşılık gelir; eşitlemenin en son durumu için, ID 9102 ile en son olayı arayın. SyncDirection, bu oturumun bir yükleme veya indirme olup olmadığını söyler. HResult 0 ise, eşitleme oturumu başarılı oldu. Sıfır olmayan Bir HResult eşitleme sırasında bir hata olduğu anlamına gelir; sık karşılaşılan hataların listesi için aşağıya bakın. PerItemErrorCount 0'dan büyükse, bu bazı dosya veya klasörlerin düzgün eşitlenmemiş olduğu anlamına gelir. 0'dan daha büyük bir PerItemErrorCount'a sahip olmak mümkündür.

Aşağıda başarılı bir yükleme örneği verilmiştir. Kısaltma için, her 9102 olayında yer alan değerlerin yalnızca bazıları aşağıda listelenmiştir. 

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: 0, 
SyncFileCount: 2, SyncDirectoryCount: 0,
AppliedFileCount: 2, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0,
TransferredFiles: 2, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Tersine, başarısız bir yükleme şu şekilde görünebilir:

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: -2134364065,
SyncFileCount: 0, SyncDirectoryCount: 0, 
AppliedFileCount: 0, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0, 
TransferredFiles: 0, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Bazen eşitleme oturumları genel olarak başarısız olur veya sıfır olmayan bir PerItemErrorCount'a sahiptir, ancak yine de bazı dosyaların başarıyla eşitlemesi ile ilerleme kaydedin. Bu, oturumun ne kadarının başarılı olduğunu gösteren Applied* alanlarında (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount ve AppliedSizeBytes) görülebilir. Bir satırda başarısız olan ancak artan bir Uygulamalı* sayısı olan birden çok eşitleme oturumu görürseniz, destek biletini açmadan önce yeniden denemek için eşitleme süresi vermelisiniz.

---

### <a name="how-do-i-monitor-the-progress-of-a-current-sync-session"></a>Geçerli eşitleme oturumunun ilerleme durumunu nasıl izleyebilirim?
# <a name="portal"></a>[Portal](#tab/portal1)
Eşitleme grubunuz içinde, söz konusu sunucu bitiş noktasına gidin ve geçerli eşitleme oturumunda yüklenen veya indirilen dosya sayısını görmek için Eşitleme Etkinliği bölümüne bakın. Bu durumun yaklaşık 5 dakika gecikeceğini ve eşitleme oturumunuzun bu süre içinde tamamlanacak kadar küçük sayılacağına dikkat edin, portalda bildirilemeyebilir. 

# <a name="server"></a>[Sunucu](#tab/server)
Sunucudaki telemetri günlüğündeki en son 9302 olayına bakın (Olay Görüntüleyicisi'nde, Uygulamalar ve Hizmetler Günlükleri\Microsoft\FileSync\Agent\Telemetry adresine gidin). Bu olay, geçerli eşitleme oturumunun durumunu gösterir. TotalItemCount kaç dosyanın eşitlendiğini, AppliedItemCount şimdiye kadar eşitlenen dosya sayısını ve PerItemErrorCount eşitleme başarısız olan dosya sayısını gösterir (bununla nasıl başa çıkış için aşağıya bakın).

```
Replica Sync Progress. 
ServerEndpointName: <CI>sename</CI>, SyncGroupName: <CI>sgname</CI>, ReplicaName: <CI>rname</CI>, 
SyncDirection: Upload, CorrelationId: {AB4BA07D-5B5C-461D-AAE6-4ED724762B65}. 
AppliedItemCount: 172473, TotalItemCount: 624196. AppliedBytes: 51473711577, 
TotalBytes: 293363829906. 
AreTotalCountsFinal: true. 
PerItemErrorCount: 1006.
```
---

### <a name="how-do-i-know-if-my-servers-are-in-sync-with-each-other"></a>Sunucularımın eşitlenmiş olup olmadığını nasıl anlayabilirim?
# <a name="portal"></a>[Portal](#tab/portal1)
Belirli bir eşitleme grubundaki her sunucu için şunları unutmayın:
- Hem yükleme hem de karşıdan yükleme için Son Deneme Eşitleme için zaman damgaları yenidir.
- Durum hem yükleme hem de indirme için yeşildir.
- Etkinliği Eşitleme alanı, eşitlemek için çok az dosya kaldığını veya hiç dosya kalmadığını gösterir.
- Eşitlemeyen Dosyalar alanı hem yükleme hem de indirme için 0'dır.

# <a name="server"></a>[Sunucu](#tab/server)
Her sunucu için telemetri olay günlüğünde 9102 olayla işaretlenmiş tamamlanmış eşitleme oturumlarına bakın `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`(Olay Görüntüleyicisi'nde gidin). 

1. Herhangi bir sunucuda, en son yükleme ve indirme oturumlarının başarıyla tamamlandığından emin olmak istersiniz. Bunu yapmak için, HResult ve PerItemErrorCount'ın hem yükleme hem de indirme için 0 olup olmadığını denetleyin (SyncDirection alanı belirli bir oturumun yükleme veya indirme oturumu olup olmadığını gösterir). Yakın zamanda tamamlanmış bir eşitleme oturumu görmüyorsanız, büyük miktarda veri eklediyseniz veya değiştirseniz beklenen bir eşitleme oturumunun şu anda devam ediyor olma olasılığının yüksek olduğunu unutmayın.
2. Bir sunucu bulutla tamamen güncel olduğunda ve her iki yönde eşitlenecek bir değişiklik olmadığında, boş eşitleme oturumları görürsünüz. Bunlar, tüm Eşitleme* alanlarının (SyncFileCount, SyncDirCount, SyncTombstoneCount ve SyncSizeBytes) sıfır olduğu yükleme ve indirme olaylarıyla gösterilir, yani eşitlenebilmek için hiçbir şey yoktur. Bu boş eşitleme oturumlarının yüksek karmaşalı sunucularda oluşmayabileceğini, çünkü eşitlemek için her zaman yeni bir şey olduğunu unutmayın. Eşitleme etkinliği yoksa, her 30 dakikada bir oluşmalıdır. 
3. Tüm sunucular bulutla güncelse, yani son yükleme ve indirme oturumları boş eşitleme oturumlarıysa, sistemin bir bütün olarak senkronize olduğunu makul bir kesinlik ile söyleyebilirsiniz. 
    
Azure dosya paylaşımınızda doğrudan değişiklik yaptıysanız, Azure Dosya Eşitlemesi'nin her 24 saatte bir gerçekleşen değişiklik numaralandırması çalışana kadar bu değişikliği algılamayacağını unutmayın. Bir sunucu, doğrudan Azure dosya paylaşımında yapılan son değişiklikleri kaçırdığında bulutla güncel olduğunu söyleyebilir. 

---

### <a name="how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing"></a>Belirli dosya veya klasörlerin eşitlenip eşitlenmediğini nasıl görebilirim?
Sunucudaki PerItemErrorCount veya portaldaki Eşitlenmemiş Dosyalar Eşitleme sayısı belirli bir eşitleme oturumu için 0'dan büyükse, bazı öğelerin eşitleme yapmadığı anlamına gelir. Dosya ve klasörlerin eşitlemesini engelleyen özellikler olabilir. Bu özellikler kalıcı olabilir ve eşitlenmeye devam etmek için açık eylem gerektirebilir, örneğin desteklenmeyen karakterleri dosya veya klasör adından kaldırmak gibi. Bunlar da geçici olabilir, yani dosya veya klasör otomatik olarak eşitleme devam edecektir; örneğin, açık tutamaçları olan dosyalar, dosya kapatıldığında eşitleme yle otomatik olarak devam eder. Azure Dosya Eşitleme altyapısı böyle bir sorunu algıladığında, şu anda düzgün eşitlenmeyen öğeleri listelemek için ayrıştırılabilen bir hata günlüğü üretilir.

Bu hataları görmek için, açık tutamaçları, desteklenmeyen karakterler veya diğer sorunlar nedeniyle eşitlenemeyen dosyaları tanımlamak için **FileSyncErrorsReport.ps1** PowerShell komut dosyasını (Azure Dosya Eşitleme aracısının aracı yükleme dizininde bulunur) çalıştırın. ItemPath alanı, kök eşitleme diziniyle ilişkili olarak dosyanın konumunu bildirir. Düzeltme adımları için aşağıdaki yaygın eşitleme hatalarılistesine bakın.

> [!Note]  
> FileSyncErrorsReport.ps1 komut dosyası "Dosya hatası bulunamadı" döndürür veya eşitleme grubu için öğe başına hataları listelemiyorsa, bunun nedeni aşağıdakilerden biri olur:
>
>- Neden 1: Tamamlanan son eşitleme oturumunda madde başına hatalar yoktu. Portal, Eşitlenmeyen 0 Dosyaları göstermek için yakında güncelleştirilmelidir. 
>   - PerItemErrorCount'ın 0 olduğunu doğrulamak için Telemetri olay günlüğündeki [Event ID 9102'yi](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) kontrol edin. 
>
>- Neden 2: Öğe başına çok fazla hata nedeniyle sarılmış sunucudaki ItemResults olay günlüğü ve olay günlüğü artık bu eşitleme grubu için hatalar içermez.
>   - Bu sorunu önlemek için ItemResults olay günlüğü boyutunu artırın. ItemResults olay günlüğü Olay Görüntüleyicisi'nde "Uygulamalar ve Hizmetler Günlükleri\Microsoft\FileSync\Agent" altında bulunabilir. 

#### <a name="troubleshooting-per-filedirectory-sync-errors"></a>Dosya/dizin eşitleme hataları başına sorun giderme
**ItemResults günlüğü - madde başına eşitleme hataları**  

| HRESULT | HRESULT (ondalık) | Hata dizesi | Sorun | Düzeltme |
|---------|-------------------|--------------|-------|-------------|
| 0x80070043 | -2147942467 | ERROR_BAD_NET_NAME | Sunucudaki katmanlı dosyaya erişilemez. Sunucu uç noktası silinmeden önce katmanlanmış dosya geri çağrılmıyorsa bu hata oluşur. | Bu sorunu gidermek için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) |
| 0x80c80207 | -2134375929 | ECS_E_SYNC_CONSTRAINT_CONFLICT | Bağımlı bir klasör henüz eşitlenmedi, çünkü dosya veya dizin değişikliği henüz eşitlenemez. Bağımlı değişiklikler eşitlendikten sonra bu öğe eşitlenir. | Eylem gerekmiyor. Hata birkaç gün devam ederse, bağımlı klasörün neden henüz eşitlenmediğini belirlemek için FileSyncErrorsReport.ps1 PowerShell komut dosyasını kullanın. |
| 0x80c80284 | -2134375804 | ECS_E_SYNC_CONSTRAINT_CONFLICT_SESSION_FAILED | Bağımlı bir klasör henüz eşitlenmedi ve eşitleme oturumu başarısız olduğundan dosya veya dizin değişikliği henüz eşitlenemez. Bağımlı değişiklikler eşitlendikten sonra bu öğe eşitlenir. | Eylem gerekmiyor. Hata devam ederse, eşitleme oturumu hatasını araştırın. |
| 0x8007007b | -2147024773 | ERROR_INVALID_NAME | Dosya veya dizin adı geçersizdir. | Söz konusu dosyayı veya dizini yeniden adlandırın. Daha fazla bilgi için [desteklenmeyen karakterleri işleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) ye bakın. |
| 0x80c80255 | -2134375851 | ECS_E_XSMB_REST_INCOMPATIBILITY | Dosya veya dizin adı geçersizdir. | Söz konusu dosyayı veya dizini yeniden adlandırın. Daha fazla bilgi için [desteklenmeyen karakterleri işleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) ye bakın. |
| 0x80c80018 | -2134376424 | ECS_E_SYNC_FILE_IN_USE | Kullanımda olduğu için dosya eşitlenemez. Dosya artık kullanımda olmadığında eşitlenir. | Eylem gerekmiyor. Azure Dosya Eşitlemi, açık tutamaçları olan dosyaları eşitlemek için sunucuda günde bir kez geçici bir VSS anlık görüntüsü oluşturur. |
| 0x80c8031d | -2134375651 | ECS_E_CONCURRENCY_CHECK_FAILED | Dosya değişti, ancak değişiklik henüz eşitleme tarafından algılanmadı. Eşitleme, bu değişiklik algılandıktan sonra kurtarır. | Eylem gerekmiyor. |
| 0x80070002 | -2147024894 | ERROR_FILE_NOT_FOUND | Dosya silindi ve eşitleme değişikliğinden haberdar değil. | Eylem gerekmiyor. Degistirme algılaması dosyanın silindiğini tespit ettiğinde bu hatayı günün günü |
| 0x80070003 | -2147942403 | ERROR_PATH_NOT_FOUND | Öğe hedefte zaten silindiğinden ve eşitleme değişikliğinden haberdar olmadığından, dosya veya dizin silinemez. | Eylem gerekmiyor. Eşitleme, değişiklik algılaması hedefte çalıştığında ve eşitleme öğenin silinmiş olduğunu algıladıktan sonra bu hatayı günlüğe kaydetmeyi durdurur. |
| 0x80c80205 | -2134375931 | ECS_E_SYNC_ITEM_SKIP | Dosya veya dizin atlandı, ancak bir sonraki eşitleme oturumu sırasında eşitlenecek. Öğe indirirken bu hata bildirilirse, dosya veya dizin adı büyük olasılıkla geçersizdir. | Dosya yı yüklerken bu hata bildirilirse işlem gerekmez. Dosya yı karşıdan yüklediğinizde hata bildirilirse, söz konusu dosyayı veya dizini yeniden adlandırın. Daha fazla bilgi için [desteklenmeyen karakterleri işleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#handling-unsupported-characters) ye bakın. |
| 0x800700B7 | -2147024713 | ERROR_ALREADY_EXISTS | Öğe hedefte zaten var olduğundan ve eşitleme değişikliğinfarkında olmadığından, dosya veya dizin oluşturulamıyor. | Eylem gerekmiyor. Eşitleme, değişiklik algılaması hedefte çalıştığında ve eşitleme bu yeni öğeden haberdar olduğunda bu hatayı günlüğe kaydetmeyi durdurur. |
| 0x80c8603e | -2134351810 | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED | Azure dosya paylaşımı sınırına ulaşıldığından dosya eşitlenemiyor. | Bu sorunu gidermek için bkz. Sorun giderme kılavuzundaki Azure dosya paylaşımı depolama sınırı bölümüne [ulaştınız.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#-2134351810) |
| 0x80c8027C | -2134375812 | ECS_E_ACCESS_DENIED_EFS | Dosya desteklenmeyen bir çözüm (NTFS EFS gibi) tarafından şifrelenir. | Dosyanın şifresini çözün ve desteklenen bir şifreleme çözümü kullanın. Desteklenen çözümleri listesi için planlama kılavuzunun [Şifreleme çözümleri](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#encryption) bölümüne bakın. |
| 0x80c80283 | -2160591491 | ECS_E_ACCESS_DENIED_DFSRRO | Dosya, yalnızca okunan bir çoğaltma klasöründe bulunur. | Dosya, yalnızca okunan bir çoğaltma klasöründe bulunur. Azure Dosya Eşitleme DFS-R salt okunur çoğaltma klasörlerindeki sunucu uç noktalarını desteklemez. Daha fazla bilgi için [planlama kılavuzuna](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#distributed-file-system-dfs) bakın. |
| 0x80070005 | -2147024891 | ERROR_ACCESS_DENIED | Dosya bekleyen bir silme durumu vardır. | Eylem gerekmiyor. Tüm açık dosya tutamaçları kapatıldıktan sonra dosya silinir. |
| 0x80c86044 | -2134351804 | ECS_E_AZURE_AUTHORIZATION_FAILED | Depolama hesabındaki güvenlik duvarı ve sanal ağ ayarları etkin olduğundan ve sunucunun depolama hesabına erişimi olmadığı için dosya eşitlenemez. | Dağıtım kılavuzundaki [Yapılandırma güvenlik duvarı ve sanal ağ ayarları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings) bölümünde belgelenen adımları izleyerek Sunucu IP adresini veya sanal ağı ekleyin. |
| 0x80c80243 | -2134375869 | ECS_E_SECURITY_DESCRIPTOR_SIZE_TOO_LARGE | Güvenlik tanımlayıcı boyutu 64 KiB sınırını aştığından dosya eşitlenemez. | Bu sorunu çözmek için dosyadaki erişim denetimi girdilerini (ACE) kaldırarak güvenlik açıklayıcısının boyutunu küçültün. |
| 0x8000ffff | -2147418113 | E_unexpected | Beklenmeyen bir hata nedeniyle dosya eşitlenemez. | Hata birkaç gün devam ederse, lütfen bir destek örneği açın. |
| 0x80070020 | -2147024864 | ERROR_SHARING_VIOLATION | Kullanımda olduğu için dosya eşitlenemez. Dosya artık kullanımda olmadığında eşitlenir. | Eylem gerekmiyor. |
| 0x80c80017 | -2134376425 | ECS_E_SYNC_OPLOCK_BROKEN | Dosya eşitleme sırasında değiştirildi, bu nedenle yeniden eşitlemesi gerekir. | Eylem gerekmiyor. |
| 0x80070017 | -2147024873 | ERROR_CRC | CrC hatası nedeniyle dosya eşitlenemez. Katmanlı bir dosya, sunucu bitiş noktasını silmeden önce geri çağrılmazsa veya dosya bozuksa bu hata oluşabilir. | Bu sorunu gidermek [Tiered files are not accessible on the server after deleting a server endpoint](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) için bkz. Oprhaned katmanlı dosyaları kaldırdıktan sonra hata oluşmaya devam ederse, ses üzerinde [chkdsk](https://docs.microsoft.com/windows-server/administration/windows-commands/chkdsk) çalıştırın. |
| 0x80c80200 | -2134375936 | ECS_E_SYNC_CONFLICT_NAME_EXISTS | En fazla çakışma dosyası sayısına ulaşıldığından dosya eşitlenemez. Azure Dosya Eşitlemi, dosya başına 100 çakışma dosyalarını destekler. Dosya çakışmaları hakkında daha fazla bilgi edinmek için Azure Dosya Eşitleme [SSS'sine](https://docs.microsoft.com/azure/storage/files/storage-files-faq#afs-conflict-resolution)bakın. | Bu sorunu gidermek için çakışma dosyalarının sayısını azaltın. Çakışma dosyalarının sayısı 100'den az olduğunda dosya eşitlenir. |

#### <a name="handling-unsupported-characters"></a>Desteklenmeyen karakterleri işleme
**FileSyncErrorsReport.ps1** PowerShell komut dosyası desteklenmeyen karakterler (hata kodu 0x8007007b veya 0x80c80255) nedeniyle hataları gösterirse, ilgili dosya adlarından hatalı karakterleri kaldırmanız veya yeniden adlandırmanız gerekir. PowerShell büyük olasılıkla bu karakterlerin çoğu standart görsel kodlama ya da soru işaretleri veya boş dikdörtgenler olarak yazdıracaktır. [Değerlendirme Aracı](storage-sync-files-planning.md#evaluation-cmdlet) desteklenmeyen karakterleri tanımlamak için kullanılabilir.

Aşağıdaki tablo, Azure Dosya Eşitlemesi'nin henüz desteklemediği tüm unicode karakterlerini içerir.

| Karakter kümesi | Karakter sayısı |
|---------------|-----------------|
| <ul><li>0x000009D (osc işletim sistemi komutu)</li><li>0x00000090 (dcs cihaz kontrol dizesi)</li><li>0x000008F (ss3 tek vites üç)</li><li>0x00000081 (yüksek sekizli ön ayarlı)</li><li>0x0000007F (del silme)</li><li>0x000008D (ri ters hat beslemesi)</li></ul> | 6 |
| 0x0000FD0 - 0x0000FDEF (Arapça sunum formları-a) | 32 |
| 0x0000FFF0 - 0x0000FFFF (özel) | 16 |
| <ul><li>0x0001FFFE - 0x0001FFFF = 2 (karaktersiz)</li><li>0x0002FFFE - 0x0002FFFF = 2 (karaktersiz)</li><li>0x0003FFFE - 0x0003FFFF = 2 (karaktersiz)</li><li>0x0004FFFE - 0x0004FFFF = 2 (karaktersiz)</li><li>0x0005FFFE - 0x0005FFFF = 2 (karaktersiz)</li><li>0x0006FFFE - 0x0006FFFF = 2 (karaktersiz)</li><li>0x0007FFFE - 0x0007FFFF = 2 (karaktersiz)</li><li>0x0008FFFE - 0x0008FFFF = 2 (karaktersiz)</li><li>0x0009FFFE - 0x0009FFFF = 2 (karaktersiz)</li><li>0x000AFFFE - 0x000AFFFF = 2 (karaktersiz)</li><li>0x000BFFFE - 0x000BFFFF = 2 (karaktersiz)</li><li>0x000CFFFE - 0x000CFFFF = 2 (karakterdışı)</li><li>0x000DFFFE - 0x000DFFFF = 2 (karaktersiz)</li><li>0x000EFFFE - 0x000EFFFF = 2 (tanımsız)</li><li>0x000FFFFE - 0x000FFFFFF = 2 (ek özel kullanım alanı)</li></ul> | 30 |
| 0x0010FFFE, 0x0010FFFF | 2 |

### <a name="common-sync-errors"></a>Sık eşitleme hataları
<a id="-2147023673"></a>**Eşitleme oturumu iptal edildi.**  

| | |
|-|-|
| **HRESULT** | 0x800704c7 |
| **HRESULT (ondalık)** | -2147023673 | 
| **Hata dizesi** | ERROR_CANCELLED |
| **Düzeltme gerekli** | Hayır |

Eşitleme oturumları, sunucunun yeniden başlatılması veya güncellenmesi, VSS anlık görüntüleri vb. gibi çeşitli nedenlerle başarısız olabilir. Bu hata izleme gerektiriyor gibi görünse de, birkaç saatlik bir süre boyunca devam etmediği sürece bu hatayı yok saymak güvenlidir.

<a id="-2147012889"></a>**Hizmetle bağlantı kurulamadı.**    

| | |
|-|-|
| **HRESULT** | 0x80072ee7 |
| **HRESULT (ondalık)** | -2147012889 | 
| **Hata dizesi** | WININET_E_NAME_NOT_RESOLVED |
| **Düzeltme gerekli** | Evet |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134376372"></a>**Kullanıcı isteği hizmet tarafından azaltıldı.**  

| | |
|-|-|
| **HRESULT** | 0x80c8004c |
| **HRESULT (ondalık)** | -2134376372 |
| **Hata dizesi** | ECS_E_USER_REQUEST_THROTTLED |
| **Düzeltme gerekli** | Hayır |

Hiçbir eylem gerekli değildir; sunucu tekrar deneyecektir. Bu hata birkaç saat devam ederse destek isteği oluşturun.

<a id="-2134364043"></a>**Değişiklik algılaması geri yükleme sonrası tamamlanana kadar eşitleme engellenir**  

| | |
|-|-|
| **HRESULT** | 0x80c83075 |
| **HRESULT (ondalık)** | -2134364043 |
| **Hata dizesi** | ECS_E_SYNC_BLOCKED_ON_CHANGE_DETECTION_POST_RESTORE |
| **Düzeltme gerekli** | Hayır |

İşlem yapmanız gerekmez. Azure Yedekleme kullanılarak bir dosya veya dosya paylaşımı (bulut bitiş noktası) geri yüklendiğinde, Azure dosya paylaşımında değişiklik algılaması tamamlanana kadar eşitleme engellenir. Geri yükleme tamamlandıktan hemen sonra değişiklik algılama çalıştırılır ve çalışma süresi dosya paylaşımındaki dosyaların sayısına bağlıdır.

<a id="-2147216747"></a>**Eşitleme veritabanı boşolduğundan eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80041295 |
| **HRESULT (ondalık)** | -2147216747 |
| **Hata dizesi** | SYNC_E_METADATA_INVALID_OPERATION |
| **Düzeltme gerekli** | Hayır |

Normalde bir yedekleme uygulaması VSS anlık görüntüsü oluşturduğunda ve eşitleme veritabanının yüklemesi kaldırıldığında bu hata oluşur. Bu hata birkaç saat devam ederse destek isteği oluşturun.

<a id="-2134364065"></a>**Eşitleme, bulut bitiş noktasında belirtilen Azure dosya paylaşımına erişemez.**  

| | |
|-|-|
| **HRESULT** | 0x80c8305f |
| **HRESULT (ondalık)** | -2134364065 |
| **Hata dizesi** | ECS_E_EXTERNAL_STORAGE_ACCOUNT_AUTHORIZATION_FAILED |
| **Düzeltme gerekli** | Evet |

Azure dosya paylaşımı veya bunu barındıran depolama hesabı artık var olmadığından Azure Dosya Eşitleme aracısı Azure dosya paylaşımına erişemediği için bu hata oluştu. Aşağıdaki adımları uygulayarak bu hataya giderebilirsiniz:

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Azure dosya paylaşımının var olduğundan emin olun.](#troubleshoot-azure-file-share)
3. [Azure Dosya Eşitlemesini'nin depolama hesabına erişebilmesini sağlayın.](#troubleshoot-rbac)
4. [Depolama hesabında güvenlik duvarı ve sanal ağ ayarlarının (etkinleştirildiyse) düzgün yapılandırıldığını doğrulayın](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings)

<a id="-2134351804"></a>**İstek bu işlemi gerçekleştirmek için yetkili olmadığından eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c86044 |
| **HRESULT (ondalık)** | -2134351804 |
| **Hata dizesi** | ECS_E_AZURE_AUTHORIZATION_FAILED |
| **Düzeltme gerekli** | Evet |

Bu hata, Azure Dosya Eşitleme aracısının Azure dosya paylaşımına erişme yetkisine sahip olmaması nedeniyle oluşur. Aşağıdaki adımları uygulayarak bu hataya giderebilirsiniz:

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Azure dosya paylaşımının var olduğundan emin olun.](#troubleshoot-azure-file-share)
3. [Depolama hesabında güvenlik duvarı ve sanal ağ ayarlarının (etkinleştirildiyse) düzgün yapılandırıldığını doğrulayın](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings)
4. [Azure Dosya Eşitlemesini'nin depolama hesabına erişebilmesini sağlayın.](#troubleshoot-rbac)

<a id="-2134364064"></a><a id="cannot-resolve-storage"></a>**Kullanılan depolama hesabı adı çözülemedi.**  

| | |
|-|-|
| **HRESULT** | 0x80C83060 |
| **HRESULT (ondalık)** | -2134364064 |
| **Hata dizesi** | ECS_E_STORAGE_ACCOUNT_NAME_UNRESOLVED |
| **Düzeltme gerekli** | Evet |

1. Depolama DNS adını sunucudan çözebileceğinizden denetleyin.

    ```powershell
    Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 443
    ```
2. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
3. [Depolama hesabında güvenlik duvarı ve sanal ağ ayarlarının (etkinleştirildiyse) düzgün yapılandırıldığını doğrulayın](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings)

<a id="-2134364022"></a><a id="storage-unknown-error"></a>**Depolama hesabına erişirken bilinmeyen bir hata oluştu.**  

| | |
|-|-|
| **HRESULT** | 0x80c8308a |
| **HRESULT (ondalık)** | -2134364022 |
| **Hata dizesi** | ECS_E_STORAGE_ACCOUNT_UNKNOWN_ERROR |
| **Düzeltme gerekli** | Evet |

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Depolama hesabında güvenlik duvarı ve sanal ağ ayarlarının (etkinleştirildiyse) düzgün yapılandırıldığını doğrulayın](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings)

<a id="-2134364014"></a>**Depolama hesabı kilitli olduğu için eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c83092 |
| **HRESULT (ondalık)** | -2134364014 |
| **Hata dizesi** | ECS_E_STORAGE_ACCOUNT_LOCKED |
| **Düzeltme gerekli** | Evet |

Depolama hesabının salt okunur [kaynak kilidi](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources)olduğundan bu hata oluşur. Bu sorunu gidermek için, depolama hesabındaki salt okunur kaynak kilidini kaldırın. 

<a id="-1906441138"></a>**Eşitleme veritabanı ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x8e5e044e |
| **HRESULT (ondalık)** | -1906441138 |
| **Hata dizesi** | JET_errWriteConflict |
| **Düzeltme gerekli** | Evet |

Bu hata, Azure Dosya Eşitlemi tarafından kullanılan dahili veritabanıyla ilgili bir sorun olduğunda oluşur. Bu sorun oluştuğunda, bir destek isteği oluşturun ve bu sorunu çözmenize yardımcı olmak için sizinle iletişime geçelim.

<a id="-2134364053"></a>**Sunucuya yüklenen Azure Dosya Eşitleme aracısı sürümü desteklenmez.**  

| | |
|-|-|
| **HRESULT** | 0x80C8306B |
| **HRESULT (ondalık)** | -2134364053 |
| **Hata dizesi** | ECS_E_AGENT_VERSION_BLOCKED |
| **Düzeltme gerekli** | Evet |

Sunucuda yüklü olan Azure Dosya Eşitleme aracısının sürümü desteklenmiyorsa bu hata oluşur. Bu sorunu gidermek için [desteklenen aracı sürümüne yükseltin.]( https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#supported-versions) [upgrade]( https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#upgrade-paths)

<a id="-2134351810"></a>**Azure dosya paylaşımı depolama sınırına ulaştınız.**  

| | |
|-|-|
| **HRESULT** | 0x80c8603e |
| **HRESULT (ondalık)** | -2134351810 |
| **Hata dizesi** | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED |
| **Düzeltme gerekli** | Evet |

Bu hata, Azure dosya paylaşımına kota uygulandığında veya kullanım Azure dosya paylaşımının sınırlarını aştığında ortaya çıkabilen Azure dosya paylaşımı depolama sınırına ulaşılması durumunda oluşur. Daha fazla bilgi için, [Azure dosya paylaşımı için geçerli sınırlara](storage-files-scale-targets.md)bakın.

1. Depolama Eşitleme Hizmeti içindeki eşitleme grubuna gidin.
2. Eşitleme grubu içindeki bulut bitiş noktasını seçin.
3. Açılan bölmedeki Azure dosya paylaşım adını not edin.
4. Bağlantılı depolama hesabını seçin. Bu bağlantı başarısız olursa, başvurulan depolama hesabı kaldırıldı.

    ![Depolama hesabına bağlantı içeren bulut uç nokta ayrıntı bölmesini gösteren ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

5. Dosya paylaşımları listesini görüntülemek için **Dosyalar'ı** seçin.
6. Bulut bitiş noktası tarafından başvurulan Azure dosya paylaşımı için satırın sonundaki üç noktayı tıklatın.
7. **Kullanım** değerinin **Kota**'nın altında kaldığını doğrulayın. Alternatif bir kota belirtilmedikçe, kota [Azure dosya paylaşımının maksimum boyutuyla](storage-files-scale-targets.md)eşleşecektir.

    ![Azure dosya paylaşım özelliklerinin ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-limit-reached-1.png)

Paylaşım doluysa ve kota ayarlanmadıysa, bu sorunu çözmenin olası bir yolu geçerli sunucu uç noktasının her alt klasörünü kendi ayrı eşitleme gruplarında kendi sunucu uç noktası yapmaktır. Bu şekilde her alt klasör ayrı bir Azure dosya paylaşımına eşitlenir.

<a id="-2134351824"></a>**Azure dosya paylaşımı bulunamıyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c86030 |
| **HRESULT (ondalık)** | -2134351824 |
| **Hata dizesi** | ECS_E_AZURE_FILE_SHARE_NOT_FOUND |
| **Düzeltme gerekli** | Evet |

Azure dosya paylaşımı erişilemez olduğunda bu hata oluşur. Sorunu gidermek için:

1. [Depolama hesabının var olduğunu doğrulayın.](#troubleshoot-storage-account)
2. [Azure dosya paylaşımının var olduğundan emin olun.](#troubleshoot-azure-file-share)

Azure dosya paylaşımı silindiyse, yeni bir dosya paylaşımı oluşturmanız ve ardından eşitleme grubunu yeniden oluşturmanız gerekir. 

<a id="-2134364042"></a>**Bu Azure aboneliği askıya alınırken eşitleme duraklatıldı.**  

| | |
|-|-|
| **HRESULT** | 0x80C83076 |
| **HRESULT (ondalık)** | -2134364042 |
| **Hata dizesi** | ECS_E_SYNC_BLOCKED_ON_SUSPENDED_SUBSCRIPTION |
| **Düzeltme gerekli** | Evet |

Azure aboneliği askıya alındığında bu hata oluşur. Azure aboneliği geri yüklendiğinde eşitleme yeniden etkinleştirilir. Azure aboneliğim neden devre dışı bırakıldı ve daha fazla bilgi için [aboneliğimi nasıl yeniden etkinleştirebilirim?](../../cost-management-billing/manage/subscription-disabled.md)

<a id="-2134364052"></a>**Depolama hesabının bir güvenlik duvarı veya yapılandırılmış sanal ağlar vardır.**  

| | |
|-|-|
| **HRESULT** | 0x80c8306c |
| **HRESULT (ondalık)** | -2134364052 |
| **Hata dizesi** | ECS_E_MGMT_STORAGEACLSNOTSUPPORTED |
| **Düzeltme gerekli** | Evet |

Depolama hesabı güvenlik duvarından veya depolama hesabının bir sanal ağa ait olmasından dolayı Azure dosya paylaşımına erişilemediğinde bu hata oluşur. Depolama hesabındaki güvenlik duvarı nın ve sanal ağ ayarlarının doğru şekilde yapılandırıldığı doğrulayın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings) 

<a id="-2134375911"></a>**Eşitleme veritabanı ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80219 |
| **HRESULT (ondalık)** | -2134375911 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_WRITE_LOCK_TIMEOUT |
| **Düzeltme gerekli** | Hayır |

Bu hata genellikle kendiliğinden çözülür ve şunlar varsa gerçekleşebilir:

* Eşitleme grubundaki sunucular arasında çok sayıda dosya değişikliği.
* Tek tek dosyalarda ve dizinlerde çok sayıda hata.

Bu hata birkaç saatten uzun süre devam ederse, bir destek isteği oluşturun ve bu sorunu çözmenize yardımcı olmak için sizinle iletişime geçelim.

<a id="-2146762487"></a>**Sunucu güvenli bir bağlantı kurmak için başarısız oldu. Bulut hizmeti beklenmeyen bir sertifika aldı.**  

| | |
|-|-|
| **HRESULT** | 0x800b0109 |
| **HRESULT (ondalık)** | -2146762487 |
| **Hata dizesi** | CERT_E_UNTRUSTEDROOT |
| **Düzeltme gerekli** | Evet |

Bu hata, kuruluşunuz TLS sonlandırma proxy'si kullanıyorsa veya kötü amaçlı bir varlık sunucunuzla Azure Dosya Eşitleme hizmeti arasındaki trafiği engellenebiliyorsa olabilir. Bunun beklendiğinden eminseniz (kuruluşunuz TLS sonlandırma proxy'si kullandığından), bir kayıt defteri geçersiz kılınarak sertifika doğrulamasını atlarsınız.

1. SkipVerifyingPinnedRootCertificate kayıt defteri değerini oluşturun.

    ```powershell
    New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Azure\StorageSync -Name SkipVerifyingPinnedRootCertificate -PropertyType DWORD -Value 1
    ```

2. Kayıtlı sunucudaki eşitleme hizmetini yeniden başlatın.

    ```powershell
    Restart-Service -Name FileSyncSvc -Force
    ```

Bu kayıt defteri değerini ayarlayarak, Azure Dosya Eşitleme aracısı, sunucu ve bulut hizmeti arasında veri aktarırken yerel olarak güvenilen TLS/SSL sertifikasını kabul eder.

<a id="-2147012894"></a>**Hizmetle bağlantı kurulamadı.**  

| | |
|-|-|
| **HRESULT** | 0x80072ee2 |
| **HRESULT (ondalık)** | -2147012894 |
| **Hata dizesi** | WININET_E_TIMEOUT |
| **Düzeltme gerekli** | Evet |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134375680"></a>**Kimlik doğrulamayla ilgili bir sorun nedeniyle eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80300 |
| **HRESULT (ondalık)** | -2134375680 |
| **Hata dizesi** | ECS_E_SERVER_CREDENTIAL_NEEDED |
| **Düzeltme gerekli** | Evet |

Bu hata normalde sunucu saati yanlış olduğunda oluşur. Sunucu sanal bir makinede çalışıyorsa, ana bilgisayardaki sürenin doğru olduğunu doğrulayın.

<a id="-2134364040"></a>**Eşitleme, sertifikanın sona ermesi nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c83078 |
| **HRESULT (ondalık)** | -2134364040 |
| **Hata dizesi** | ECS_E_AUTH_SRV_CERT_EXPIRED |
| **Düzeltme gerekli** | Evet |

Kimlik doğrulaması için kullanılan sertifikanın süresi dolduğundan bu hata oluştu.

Sertifikanın süresinin dolduğunu onaylamak için aşağıdaki adımları uygulayın:  
1. MMC sertifikalarını açın, Bilgisayar Hesabı'nı seçin ve Sertifikalar (Yerel Bilgisayar)\Kişisel\Sertifikalar'a gidin.
2. İstemci kimlik doğrulama sertifikasının süresinin dolup dolmadığını denetleyin.

İstemci kimlik doğrulama sertifikasının süresi dolduysa, sorunu çözmek için aşağıdaki adımları uygulayın:

1. Azure Dosya Eşitleme aracısı sürümünü 4.0.1.0 veya sonraki sürümde doğrulayın.
2. Sunucuda şu PowerShell komutunu çalıştırın: 

    ```powershell
    Reset-AzStorageSyncServerCertificate -ResourceGroupName <string> -StorageSyncServiceName <string>
    ```

<a id="-2134375896"></a>**Kimlik doğrulama sertifikası bulunamadı nedeniyle eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80228 |
| **HRESULT (ondalık)** | -2134375896 |
| **Hata dizesi** | ECS_E_AUTH_SRV_CERT_NOT_FOUND |
| **Düzeltme gerekli** | Evet |

Kimlik doğrulaması için kullanılan sertifika bulunamadığından bu hata oluştu.

Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin:

1. Azure Dosya Eşitleme aracısı sürümünü 4.0.1.0 veya sonraki sürümde doğrulayın.
2. Sunucuda şu PowerShell komutunu çalıştırın: 

    ```powershell
    Reset-AzStorageSyncServerCertificate -ResourceGroupName <string> -StorageSyncServiceName <string>
    ```

<a id="-2134364039"></a>**Kimlik doğrulama kimliği bulunamadı nedeniyle eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c83079 |
| **HRESULT (ondalık)** | -2134364039 |
| **Hata dizesi** | ECS_E_AUTH_IDENTITY_NOT_FOUND |
| **Düzeltme gerekli** | Evet |

Sunucu uç noktasını silme işlemi başarısız olduğu ve uç nokta şimdi kısmen silinmiş durumda olduğu için bu hata oluşur. Bu sorunu çözmek için sunucu uç noktasını silmeyi yeniden deneyin.

<a id="-1906441711"></a><a id="-2134375654"></a><a id="doesnt-have-enough-free-space"></a>**Sunucu bitiş noktasının bulunduğu birim, disk alanında düşüktür.**  

| | |
|-|-|
| **HRESULT** | 0x8e5e0211 |
| **HRESULT (ondalık)** | -1906441711 |
| **Hata dizesi** | JET_errLogDiskFull |
| **Düzeltme gerekli** | Evet |
| | |
| **HRESULT** | 0x80c8031a |
| **HRESULT (ondalık)** | -2134375654 |
| **Hata dizesi** | ECS_E_NOT_ENOUGH_LOCAL_STORAGE |
| **Düzeltme gerekli** | Evet |

Bu hata birimin doldurulmuş olması nedeniyle oluşur. Bu hata genellikle sunucu uç noktasının dışındaki dosyaların birimde yer kaplamasından dolayı oluşur. Ek sunucu uç noktaları ekleyerek, dosyaları farklı bir ses e taşımaya veya sunucu bitiş noktasının üzerindeki ses düzeyinin boyutunu artırarak birim de yer açın.

<a id="-2134364145"></a><a id="replica-not-ready"></a>**Hizmet henüz bu sunucu bitiş noktası ile eşitlemek için hazır değil.**  

| | |
|-|-|
| **HRESULT** | 0x80c8300f |
| **HRESULT (ondalık)** | -2134364145 |
| **Hata dizesi** | ECS_E_REPLICA_NOT_READY |
| **Düzeltme gerekli** | Hayır |

Bulut bitiş noktası, Azure dosya paylaşımında zaten var olan içerikle oluşturulduğundan bu hata oluşur. Azure Dosya Eşitlemesi, sunucu bitiş noktasının ilk eşitleme ile devam etmesine izin vermeden önce tüm içerik için Azure dosya paylaşımını tarar.

<a id="-2134375877"></a><a id="-2134375908"></a><a id="-2134375853"></a>**Eşitleme birçok tek tek dosya ile ilgili sorunlar nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c8023b |
| **HRESULT (ondalık)** | -2134375877 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_KNOWLEDGE_SOFT_LIMIT_REACHED |
| **Düzeltme gerekli** | Evet |
| | |
| **HRESULT** | 0x80c8021c |
| **HRESULT (ondalık)** | -2134375908 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_KNOWLEDGE_LIMIT_REACHED |
| **Düzeltme gerekli** | Evet |
| | |
| **HRESULT** | 0x80c80253 |
| **HRESULT (ondalık)** | -2134375853 |
| **Hata dizesi** | ECS_E_TOO_MANY_PER_ITEM_ERRORS |
| **Düzeltme gerekli** | Evet |

Dosya başına çok sayıda eşitleme hatası olduğu durumlarda, eşitleme oturumları başarısız olmaya başlayabilir. <!-- To troubleshoot this state, see [Troubleshooting per file/directory sync errors]().-->

> [!NOTE]
> Azure Dosya Eşitlemi, açık tutamaçları olan dosyaları eşitlemek için sunucuda günde bir kez geçici bir VSS anlık görüntüsü oluşturur.

<a id="-2134376423"></a>**Eşitleme, sunucu bitiş noktası yolu ile ilgili bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c80019 |
| **HRESULT (ondalık)** | -2134376423 |
| **Hata dizesi** | ECS_E_SYNC_INVALID_PATH |
| **Düzeltme gerekli** | Evet |

Yolun var olduğundan, yerel bir NTFS biriminde olduğundan ve bir düzeltme noktası veya varolan sunucu bitiş noktası olmadığından emin olun.

<a id="-2134375817"></a>**Filtre sürücüsü sürümü aracı sürümüyle uyumlu olmadığından eşitleme başarısız oldu**  

| | |
|-|-|
| **HRESULT** | 0x80C80277 |
| **HRESULT (ondalık)** | -2134375817 |
| **Hata dizesi** | ECS_E_INCOMPATIBLE_FILTER_VERSION |
| **Düzeltme gerekli** | Evet |

Yüklenmiş olan bulut katmanlama filtresi sürücüsü (StorageSync.sys) sürümü, Depolama Eşitleme Aracısı (FileSyncSvc) hizmeti ile uyumlu olmadığı için bu hata oluştu. Azure Dosya Eşitleme aracısı yükseltildiyse, yüklemeyi tamamlamak için sunucuyu yeniden başlatın. Hata oluşmaya devam ederse aracıyı kaldırın, sunucuyu yeniden başlatın ve Azure Dosya Eşitleme aracısını yeniden yükleyin.

<a id="-2134376373"></a>**Hizmet şu anda kullanılamıyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c8004b |
| **HRESULT (ondalık)** | -2134376373 |
| **Hata dizesi** | ECS_E_SERVICE_UNAVAILABLE |
| **Düzeltme gerekli** | Hayır |

Azure Dosya Eşitleme hizmeti kullanılamadığından bu hata oluşur. Azure Dosya Eşitleme hizmeti kullanılabilir olduğunda bu hata otomatik olarak çözülecektir.

<a id="-2146233088"></a>**Eşitleme bir özel durum nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80131500 |
| **HRESULT (ondalık)** | -2146233088 |
| **Hata dizesi** | COR_E_EXCEPTION |
| **Düzeltme gerekli** | Hayır |

Eşitleme özel bir durum nedeniyle başarısız olduğu için bu hata oluşur. Hata birkaç saat boyunca devam ederse, lütfen bir destek isteği oluşturun.

<a id="-2134364045"></a>**Depolama hesabı başka bir bölgeye başarısız olduğundan eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c83073 |
| **HRESULT (ondalık)** | -2134364045 |
| **Hata dizesi** | ECS_E_STORAGE_ACCOUNT_FAILED_OVER |
| **Düzeltme gerekli** | Evet |

Depolama hesabı başka bir bölgeye yük devrettiği için bu hata oluştu. Azure Dosya Eşitleme, depolama hesabı yük devretme özelliğini desteklemez. Azure Dosya Eşitleme'de bulut uç noktaları olarak kullanılan Azure dosya paylaşımlarının bulunduğu depolama hesapları yük devretmemelidir. Bunun yapılması eşitlemenin çalışmayı durdurmasına neden olur ve yeni katmanlanmış dosyalar söz konusu olduğunda beklenmedik veri kaybına da yol açabilir. Bu sorunu çözmek için depolama hesabını birincil bölgeye taşıyın.

<a id="-2134375922"></a>**Eşitleme veritabanı ile geçici bir sorun nedeniyle başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c8020e |
| **HRESULT (ondalık)** | -2134375922 |
| **Hata dizesi** | ECS_E_SYNC_METADATA_WRITE_LEASE_LOST |
| **Düzeltme gerekli** | Hayır |

Eşitleme veritabanındaki bir iç sorundan dolayı bu hata oluşur. Eşitleme yeniden denendiğinde bu hata otomatik olarak düzelecektir. Bu hata uzun bir süre devam ederse, bir destek isteği oluşturun ve bu sorunu çözmenize yardımcı olmak için sizinle iletişime geçeceğiz.

<a id="-2134364024"></a>**Azure Etkin Dizin kiracısında değişiklik nedeniyle eşitleme başarısız oldu**  

| | |
|-|-|
| **HRESULT** | 0x80c83088 |
| **HRESULT (ondalık)** | -2134364024 | 
| **Hata dizesi** | ECS_E_INVALID_AAD_TENANT |
| **Düzeltme gerekli** | Evet |

Azure Dosya Eşitleme şu anda aboneliğin farklı bir Azure Active Directory kiracısına taşınmasını desteklemediği için bu hata oluştu.
 
Bu sorunu çözmek için aşağıdaki seçeneklerden birini gerçekleştirin:

- **Seçenek 1 (önerilen)**: Aboneliği orijinal Azure Etkin Dizin kiracısına geri taşıma
- **Seçenek 2**: Geçerli eşitleme grubunu silin ve yeniden oluşturun. Sunucu uç noktasında bulutta katmanlama etkinleştirildiyse eşitleme grubunu silin ve ardından eşitleme gruplarını yeniden oluşturmadan önce [Bulutta Katmanlama bölümünde]( https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) açıklanan adımları uygulayarak yalnız bırakılan katmanlanmış dosyaları kaldırın. 

<a id="-2134364010"></a>**Güvenlik duvarı ve sanal ağ özel durumu yapılandırılmamanedeniyle eşitleme başarısız oldu**  

| | |
|-|-|
| **HRESULT** | 0x80c83096 |
| **HRESULT (ondalık)** | -2134364010 | 
| **Hata dizesi** | ECS_E_MGMT_STORAGEACLSBYPASSNOTSET |
| **Düzeltme gerekli** | Evet |

Bu hata, depolama hesabında güvenlik duvarı ve sanal ağ ayarları etkinleştirilirse ve "Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver" özel durumu işaretlenmezse oluşur. Bu sorunu çözmek için dağıtım kılavuzunun [Güvenlik duvarı ve sanal ağ ayarlarını yapılandırma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings) bölümünde açıklanan adımları izleyin.

<a id="-2147024891"></a>**Sistem Birimi Bilgileri klasöründeki izinler yanlış olduğundan eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80070005 |
| **HRESULT (ondalık)** | -2147024891 |
| **Hata dizesi** | ERROR_ACCESS_DENIED |
| **Düzeltme gerekli** | Evet |

Bu hata, NT AUTHORITY\SYSTEM hesabının sunucu uç noktasının bulunduğu birimdeki Sistem Birim Bilgisi klasörü üzerinde izinleri olmadığında oluşabilir. Tek tek dosyaların ERROR_ACCESS_DENIED ile eşitlenememesi durumunda, [dosya/dizin eşitleme hataları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#troubleshooting-per-filedirectory-sync-errors) bölümünde Sorun Giderme bölümünde belgelenen adımları gerçekleştirin.

Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin:

1. [Psexec](https://docs.microsoft.com/sysinternals/downloads/psexec) aracını indirin.
2. Sistem hesabını kullanarak komut istemi başlatmak için yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın: **PsExec.exe -i -s -d cmd** 
3. Sistem hesabı altında çalıştırılan komut isteminde şu komutu çalıştırarak NT AUTHORITY\SYSTEM hesabının Sistem Birim Bilgisi klasörüne erişimi olmadığını onaylayın: **cacls "sürücü harfi:\sistem birim bilgisi" /T /C**
4. NT AUTHORITY\SYSTEM hesabının Sistem Birim Bilgisi klasörüne erişimi yoksa şu komutu çalıştırın: **cacls  "sürücü harfi:\sistem birim bilgisi" /T /E /G "NT AUTHORITY\SYSTEM:F"**
    - 4. adım erişim reddedildi hatasıyla başarısız olursa Sistem Birim Bilgisi klasörünün sahipliğini almak için şu komutu çalıştırın ve sonra 4. adımı yineleyin: **takeown /A /R /F "sürücü harfi:\Sistem Birim Bilgisi"**

<a id="-2134375810"></a>**Azure dosya paylaşımı silinip yeniden oluşturulduğu için eşitleme başarısız oldu.**  

| | |
|-|-|
| **HRESULT** | 0x80c8027e |
| **HRESULT (ondalık)** | -2134375810 |
| **Hata dizesi** | ECS_E_SYNC_REPLICA_ROOT_CHANGED |
| **Düzeltme gerekli** | Evet |

Azure Dosya Eşitleme aynı eşitleme grubunda Azure dosya paylaşımını silmeyi ve oluşturmayı desteklemediğinden bu hata oluşur. 

Bu hatayı düzeltmek için aşağıdaki adımları uygulayarak eşitleme grubunu silin ve yeniden oluşturun:

1. Eşitleme grubundaki tüm sunucu uç noktalarını silin.
2. Bulut bitiş noktasını silin. 
3. Eşitleme grubunu silin.
4. Bir sunucu bitiş noktasında bulut katmanlama etkinleştirilmişse, Katmanlı dosyalarda belgelenen adımları gerçekleştirerek sunucudaki alt katmanlı dosyaları silin, sunucu bitiş noktası bölümü [silindikten sonra sunucuda erişilemez.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint)
5. Eşitleme grubunu yeniden oluşturun.

<a id="-2145844941"></a>**HTTP isteği yeniden yönlendirildiçünkü eşitleme başarısız oldu**  

| | |
|-|-|
| **HRESULT** | 0x80190133 |
| **HRESULT (ondalık)** | -2145844941 |
| **Hata dizesi** | HTTP_E_STATUS_REDIRECT_KEEP_VERB |
| **Düzeltme gerekli** | Evet |

Azure Dosya Eşitlemesi HTTP yeniden yönlendirmesini (3xx durum kodu) desteklemediği için bu hata oluşur. Bu sorunu gidermek için, PROXY sunucunuzda veya ağ aygıtınızda HTTP yönlendirmesini devre dışı bırakın.

<a id="-2134364027"></a>**Çevrimdışı veri aktarımı sırasında bir zaman aşımı oluştu, ancak hala devam ediyor.**  

| | |
|-|-|
| **HRESULT** | 0x80c83085 |
| **HRESULT (ondalık)** | -2134364027 |
| **Hata dizesi** | ECS_E_DATA_INGESTION_WAIT_TIMEOUT |
| **Düzeltme gerekli** | Hayır |

Bu hata, bir veri alma işlemi zaman aşımını aştığında oluşur. Eşitleme ilerleme kaydediyorsa bu hata yoksayılabilir (AppliedItemCount 0'dan büyüktür). Bkz. [Geçerli bir eşitleme oturumunun ilerlemesini nasıl izleyebilirim?](#how-do-i-monitor-the-progress-of-a-current-sync-session).

### <a name="common-troubleshooting-steps"></a>Sık karşılaşılan sorun giderme adımları
<a id="troubleshoot-storage-account"></a>**Depolama hesabının var olduğunu doğrulayın.**  
# <a name="portal"></a>[Portal](#tab/azure-portal)
1. Depolama Eşitleme Hizmeti içindeki eşitleme grubuna gidin.
2. Eşitleme grubu içindeki bulut bitiş noktasını seçin.
3. Açılan bölmedeki Azure dosya paylaşım adını not edin.
4. Bağlantılı depolama hesabını seçin. Bu bağlantı başarısız olursa, başvurulan depolama hesabı kaldırıldı.
    ![Depolama hesabına bağlantı içeren bulut uç nokta ayrıntı bölmesini gösteren ekran görüntüsü.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
# Variables for you to populate based on your configuration
$region = "<Az_Region>"
$resourceGroup = "<RG_Name>"
$syncService = "<storage-sync-service>"
$syncGroup = "<sync-group>"

# Log into the Azure account
Connect-AzAccount

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = [System.String[]]@()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the " + `
        " selected Azure Region or the region is mistyped.")
}

# Check to ensure resource group exists
$resourceGroups = [System.String[]]@()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    throw [System.Exception]::new("The provided resource group $resourceGroup does not exist.")
}

# Check to make sure the provided Storage Sync Service
# exists.
$syncServices = [System.String[]]@()

Get-AzStorageSyncService -ResourceGroupName $resourceGroup | ForEach-Object {
    $syncServices += $_.StorageSyncServiceName
}

if ($syncServices -notcontains $syncService) {
    throw [System.Exception]::new("The provided Storage Sync Service $syncService does not exist.")
}

# Check to make sure the provided Sync Group exists
$syncGroups = [System.String[]]@()

Get-AzStorageSyncGroup -ResourceGroupName $resourceGroup -StorageSyncServiceName $syncService | ForEach-Object {
    $syncGroups += $_.SyncGroupName
}

if ($syncGroups -notcontains $syncGroup) {
    throw [System.Exception]::new("The provided sync group $syncGroup does not exist.")
}

# Get reference to cloud endpoint
$cloudEndpoint = Get-AzStorageSyncCloudEndpoint `
    -ResourceGroupName $resourceGroup `
    -StorageSyncServiceName $syncService `
    -SyncGroupName $syncGroup

# Get reference to storage account
$storageAccount = Get-AzStorageAccount | Where-Object { 
    $_.Id -eq $cloudEndpoint.StorageAccountResourceId
}

if ($storageAccount -eq $null) {
    throw [System.Exception]::new("The storage account referenced in the cloud endpoint does not exist.")
}
```
---

<a id="troubleshoot-azure-file-share"></a>**Azure dosya paylaşımının var olduğundan emin olun.**  
# <a name="portal"></a>[Portal](#tab/azure-portal)
1. Ana depolama hesabı sayfasına dönmek için içindekilerin sol daki tablosuna **Genel Bakış'ı** tıklatın.
2. Dosya paylaşımları listesini görüntülemek için **Dosyalar'ı** seçin.
3. Bulut bitiş noktası tarafından başvurulan dosya paylaşımının dosya paylaşımları listesinde göründüğünü doğrulayın (bunu yukarıdaki adım 1'de belirtmiş olmalısınız).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $cloudEndpoint.AzureFileShareName -and
    $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    throw [System.Exception]::new("The Azure file share referenced by the cloud endpoint does not exist")
}
```
---

<a id="troubleshoot-rbac"></a>**Azure Dosya Eşitlemesini'nin depolama hesabına erişebilmesini sağlayın.**  
# <a name="portal"></a>[Portal](#tab/azure-portal)
1. İçeriğin sol tablosunda **Access denetimi'ni (IAM)** tıklatın.
1. Depolama hesabınıza erişimi olan kullanıcılar ve uygulamalar *(hizmet ilkeleri)* listesine **Rol atamaları** sekmesini tıklatın.
1. **Microsoft.StorageSync** veya **Karma Dosya Eşitleme Hizmeti'nin** (eski uygulama adı) Okuyucu ve Veri **Erişimi** rolüyle listede göründüğünü doğrulayın. 

    ![Depolama hesabının erişim denetimi sekmesinde Karma Dosya Eşitleme Hizmeti hizmet sorumlusunun ekran görüntüsü](media/storage-sync-files-troubleshoot/file-share-inaccessible-3.png)

    **Microsoft.StorageSync** veya **Karma Dosya Eşitleme Hizmeti** listede görünmüyorsa, aşağıdaki adımları gerçekleştirin:

    - **Ekle**’ye tıklayın.
    - **Rol** alanında Okuyucu **ve Veri Erişimi'ni**seçin.
    - **Select** **alanında, Microsoft.StorageSync**yazın, rolü seçin ve **Kaydet'i**tıklatın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell    
$role = Get-AzRoleAssignment -Scope $storageAccount.Id | Where-Object { $_.DisplayName -eq "Microsoft.StorageSync" }

if ($role -eq $null) {
    throw [System.Exception]::new("The storage account does not have the Azure File Sync " + `
                "service principal authorized to access the data within the " + ` 
                "referenced Azure file share.")
}
```
---

### <a name="how-do-i-prevent-users-from-creating-files-containing-unsupported-characters-on-the-server"></a>Kullanıcıların sunucuda desteklenmeyen karakterler içeren dosyalar oluşturmasını nasıl önlerim?
[Dosya Sunucusu Kaynak Yöneticisi (FSRM) Dosya Ekranları'nı,](https://docs.microsoft.com/windows-server/storage/fsrm/file-screening-management) adlarında desteklenmeyen karakterlerin bulunan dosyaların sunucuda oluşturulmasını engellemek için kullanabilirsiniz. Desteklenmeyen karakterlerin çoğu yazdırılamaz ve bu yüzden ilk karakter olarak onların hexadecimal gösterimleri döküm gerekir gibi PowerShell kullanarak bunu yapmak zorunda kalabilirsiniz.

Önce [New-FsrmFileGroup cmdlet](https://docs.microsoft.com/powershell/module/fileserverresourcemanager/new-fsrmfilegroup)kullanarak bir FSRM Dosya Grubu oluşturun. Bu örnek, grubu desteklenmeyen karakterlerden yalnızca ikisini içerecek şekilde tanımlar, ancak dosya grubunuza gerektiği kadar karakter ekleyebilirsiniz.

```powershell
New-FsrmFileGroup -Name "Unsupported characters" -IncludePattern @(("*"+[char]0x00000090+"*"),("*"+[char]0x0000008F+"*"))
```

Bir FSRM Dosya Grubu tanımladıktan sonra, New-FsrmFileScreen cmdlet'i kullanarak bir FSRM Dosya Ekranı oluşturabilirsiniz.

```powershell
New-FsrmFileScreen -Path "E:\AFSdataset" -Description "Filter unsupported characters" -IncludeGroup "Unsupported characters"
```

> [!Important]  
> Dosya ekranlarının yalnızca Azure Dosya Eşitlemesi tarafından desteklenmeyen karakterlerin oluşturulmasını engellemek için kullanılması gerektiğini unutmayın. Dosya ekranları diğer senaryolarda kullanılıyorsa, eşitleme dosyaları sürekli olarak Azure dosya paylaşımından sunucuya indirmeyi dener ve dosya ekranı nedeniyle engellenir ve bu da yüksek veri çıkışLarına neden olur. 

## <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulut katmanlamada hatalar için iki yol vardır:

- Dosyalar katmanlanabilir, bu da Azure Dosya Eşitlemesi'nin bir dosyayı Azure Dosyalarına katmanlaştırmayı başarısız bir şekilde denemediği anlamına gelir.
- Dosyalar geri çağırmayı başaramayabilir, bu da Azure Dosya Eşitleme dosya sistemi filtresinin (StorageSync.sys) kullanıcı katmanlı bir dosyaya erişmeye çalıştığında veri karşıdan yüklememesi anlamına gelir.

Her iki hata yolu üzerinden gerçekleşebilecek iki ana hata sınıfı vardır:

- Bulut depolama hataları
    - *Geçici depolama hizmeti kullanılabilirlik sorunları.* Daha fazla bilgi için [Azure Depolama için Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/storage/v1_2/)bakın.
    - *Erişilemeyen Azure dosya paylaşımı.* Bu hata genellikle, eşitleme grubunda bulut bitiş noktası olduğunda Azure dosya paylaşımını sildiğinizde gerçekleşir.
    - *Erişilemeyen depolama hesabı.* Bu hata genellikle depolama hesabını sildiğinizde, eşitleme grubunda bulut bitiş noktası olan bir Azure dosya paylaşımına sahipken gerçekleşir. 
- Sunucu hataları 
  - *Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys) yüklenmez.* Katmanlama/geri çağırma isteklerine yanıt verebilmek için Azure Dosya Eşitleme dosya sistemi filtresinin yüklenmesi gerekir. Filtrenin yüklenmemesi çeşitli nedenlerle olabilir, ancak en yaygın nedeni yöneticinin filtreyi el ile boşaltmasıdır. Azure Dosya Eşitlemesi dosya sistemi filtresinin düzgün çalışması için Her zaman yüklenmesi gerekir.
  - *Eksik, bozuk, ya da başka kırık tazminat noktası*. Reparse noktası, iki bölümden oluşan bir dosya üzerindeki özel bir veri yapısıdır:
    1. Azure Dosya Eşitleme dosya sistemi filtresinin (StorageSync.sys) dosyaya IO üzerinde bazı işlem yapması gerekebileceğini işletim sistemine gösteren bir reparse etiketi. 
    2. Dosya sistemine işaret eden verileri, ilgili bulut bitiş noktasındaki (Azure dosya paylaşımı) dosyanın URI'sine filtre uygulayın. 
        
       Bir telafi noktasının bozulmasının en yaygın yolu, bir yöneticinin etiketi veya verilerini değiştirmeye kalkışmasıdır. 
  - *Ağ bağlantısı sorunları.* Bir dosyayı katmanlamak veya geri çağırmak için sunucunun internet bağlantısına sahip olması gerekir.

Aşağıdaki bölümler, bulut katmanlama sorunlarını nasıl gidereceklerini ve bir sorunun bulut depolama sorunu mu yoksa sunucu sorunu mu olduğunu belirler.

### <a name="how-to-monitor-tiering-activity-on-a-server"></a>Bir sunucuda katmanlama etkinliğini izleme  
Bir sunucudaki katmanlama etkinliğini izlemek için Telemetri olay günlüğünde Event ID 9003, 9016 ve 9029'u kullanın (Etkinlik Görüntüleyicisi'nde Uygulamalar ve Hizmetler\Microsoft\FileSync\Agent altında bulunur).

- Olay Kimliği 9003, sunucu bitiş noktası için hata dağıtımı sağlar. Örneğin, Toplam Hata Sayısı, Hata Kodu, vb. Not, bir olay hata kodu başına günlüğe kaydedilir.
- Olay Kimliği 9016, bir birim için gölgeleme sonuçları sağlar. Örneğin, Boş alan yüzdesi, oturumda gölgelenen dosya sayısı, hayalet başarısız dosya sayısı, vb.
- Olay Kimliği 9029, sunucu bitiş noktası için gölgeleme oturumu bilgilerini sağlar. Örneğin, oturumda denenen dosya sayısı, oturumda katmanlı dosya sayısı, zaten katmanlı dosya sayısı, vb.

### <a name="how-to-monitor-recall-activity-on-a-server"></a>Bir sunucuda geri çağırma etkinliğini izleme
Sunucudaki geri çağırma etkinliğini izlemek için, Telemetri olay günlüğünde Event ID 9005, 9006, 9009 ve 9059'u kullanın (Etkinlik Görüntüleyicisi'nde Uygulamalar ve Hizmetler\Microsoft\FileSync\Agent altında bulunur).

- Olay KIMLIĞI 9005, sunucu bitiş noktası için geri çağırma güvenilirliği sağlar. Örneğin, erişilen toplam benzersiz dosyalar, başarısız erişime sahip toplam benzersiz dosyalar vb.
- Olay Kimliği 9006, sunucu bitiş noktası için geri çağırma hatası dağıtımı sağlar. Örneğin, Toplam Başarısız İstekler, Hata Kodu, vb. Not, bir olay hata kodu başına günlüğe kaydedilir.
- Olay Kimliği 9009, sunucu bitiş noktası için geri çağırma oturumu bilgilerini sağlar. Örneğin, DurationSeconds, CountFilesRecallSucceeded, CountFilesRecallFailed, vb.
- Olay Kimliği 9059, sunucu bitiş noktası için uygulama geri çağırma dağıtımı sağlar. Örneğin, ShareId, Uygulama Adı ve TotalEgressNetworkBytes.

### <a name="how-to-troubleshoot-files-that-fail-to-tier"></a>Katmana uymayan dosyaları sorun giderme
Dosyalar Azure Dosyalarına katmanla kayda geçmezse:

1. Olay Görüntüleyici'de, Uygulamalar ve Hizmetler\Microsoft\FileSync\Agent altında bulunan telemetri, operasyonel ve tanılama olay günlüklerini gözden geçirin. 
   1. Azure dosya paylaşımında dosyaların var olduğunu doğrulayın.

      > [!NOTE]
      > Bir dosyanın katmanlandırılabilmesi için azure dosya paylaşımıyla eşitlenmesi gerekir.

   2. Sunucunun internet bağlantısı olduğunu doğrulayın. 
   3. Azure Dosya Eşitleme filtresi sürücülerinin (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
       - Yükseltilmiş bir komut isteminde `fltmc`çalıştırın. StorageSync.sys ve StorageSyncGuard.sys dosya sistem filtre sürücüleri listelenen doğrulayın.

> [!NOTE]
> Bir Olay Kimliği 9003, bir dosya katmana geçmezse (hata kodu başına bir olay günlüğe kaydedilir) Telemetri olay günlüğünde saatte bir günlüğe kaydedilir. Hata kodu için düzeltme adımlarının listelanıp listelenmeyip listelenmeyini görmek için [Katmanlama hatalarını ve düzeltme](#tiering-errors-and-remediation) bölümünü denetleyin.

### <a name="tiering-errors-and-remediation"></a>Katmanlama hataları ve düzeltme

| HRESULT | HRESULT (ondalık) | Hata dizesi | Sorun | Düzeltme |
|---------|-------------------|--------------|-------|-------------|
| 0x80c86043 | -2134351805 | ECS_E_GHOSTING_FILE_IN_USE | Dosya kullanımda olduğu için katmana ulaşamadı. | Eylem gerekmiyor. Dosya artık kullanılmadığında katmanlı olacaktır. |
| 0x80c80241 | -2134375871 | ECS_E_GHOSTING_EXCLUDED_BY_SYNC | Eşitleme tarafından dışlandığı için dosya katmanlanamadı. | Eylem gerekmiyor. Eşitleme dışlama listesindeki dosyalar katmanlandırılamaz. |
| 0x80c86042 | -2134351806 | ECS_E_GHOSTING_FILE_NOT_FOUND | Dosya, sunucuda bulunmadığından katmanlandırılamamış. | Eylem gerekmiyor. Hata devam ederse, dosyanın sunucuda olup olmadığını denetleyin. |
| 0x80c83053 | -2134364077 | ECS_E_CREATE_SV_FILE_DELETED | Dosya, Azure dosya paylaşımında silindiği için katmanlandırılamamış. | Eylem gerekmiyor. Bir sonraki karşıdan yükleme eşitleme oturumu çalıştığında dosya sunucuda silinmelidir. |
| 0x80c8600e | -2134351858 | ECS_E_AZURE_SERVER_BUSY | Dosya, ağ sorunu nedeniyle katmanlandırılamamış. | Eylem gerekmiyor. Hata devam ederse, Azure dosya paylaşımına ağ bağlantısını denetleyin. |
| 0x80072ee7 | -2147012889 | WININET_E_NAME_NOT_RESOLVED | Dosya, ağ sorunu nedeniyle katmanlandırılamamış. | Eylem gerekmiyor. Hata devam ederse, Azure dosya paylaşımına ağ bağlantısını denetleyin. |
| 0x80070005 | -2147024891 | ERROR_ACCESS_DENIED | Reddedilen erişim hatası nedeniyle dosya katmana ulaşamadı. Dosya bir DFS-R salt okunur çoğaltma klasöründe bulunuyorsa, bu hata oluşabilir. | Azure Dosya Eşitleme DFS-R salt okunur çoğaltma klasörlerindeki sunucu uç noktalarını desteklemez. Daha fazla bilgi için [planlama kılavuzuna](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#distributed-file-system-dfs) bakın. |
| 0x80072efe | -2147012866 | WININET_E_CONNECTION_ABORTED | Dosya, ağ sorunu nedeniyle katmanlandırılamamış. | Eylem gerekmiyor. Hata devam ederse, Azure dosya paylaşımına ağ bağlantısını denetleyin. |
| 0x80c80261 | -2134375839 | ECS_E_GHOSTING_MIN_FILE_SIZE | Dosya boyutu desteklenen boyuttan daha az olduğundan dosya katmanlandırılamadı. | Aracı sürümü 9.0'dan küçükse, desteklenen minimum dosya boyutu 64kb'dir. Aracı sürümü 9.0 ve daha yeniyse, desteklenen minimum dosya boyutu dosya sistemi küme boyutuna (çift dosya sistemi küme boyutu) dayanır. Örneğin, dosya sistemi küme boyutu 4kb ise, minimum dosya boyutu 8kb'dir. |
| 0x80c83007 | -2134364153 | ECS_E_STORAGE_ERROR | Dosya, Azure depolama sorunu nedeniyle katmana ulaşamadı. | Hata devam ederse, bir destek isteği açın. |
| 0x800703e3 | -2147023901 | ERROR_OPERATION_ABORTED | Dosya aynı anda geri çağrıldığı için katmana ulaşamadı. | Eylem gerekmiyor. Geri çağırma tamamlandığında ve dosya artık kullanılmadığında dosya katmanlanır. |
| 0x80c80264 | -2134375836 | ECS_E_GHOSTING_FILE_NOT_SYNCED | Dosya, Azure dosya paylaşımıyla eşitlenmediği için katmanlanamadı. | Eylem gerekmiyor. Dosya, Azure dosya paylaşımıyla eşitlendikten sonra katmanlanır. |
| 0x80070001 | -2147942401 | ERROR_INVALID_FUNCTION | Bulut katmanlama filtresi sürücüsü (storagesync.sys) çalışmadığı için dosya katmanlamada başarısız oldu. | Bu sorunu gidermek için yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:`fltmc load storagesync`<br>Storagesync filtre sürücüsü fltmc komutunu çalıştırırken yüklenmezse, Azure Dosya Eşitleme aracısını kaldırın, sunucuyu yeniden başlatın ve Azure Dosya Eşitleme aracısını yeniden yükleyin. |
| 0x80070070 | -2147024784 | ERROR_DISK_FULL | Dosya, sunucu bitiş noktasının bulunduğu birimdeki disk alanının yetersizliği nedeniyle katmanlandırılamamış. | Bu sorunu gidermek için, sunucu bitiş noktasının bulunduğu birimde en az 100 MB disk alanı boşaltın. |
| 0x80070490 | -2147023728 | ERROR_NOT_FOUND | Dosya, Azure dosya paylaşımıyla eşitlenmediği için katmanlanamadı. | Eylem gerekmiyor. Dosya, Azure dosya paylaşımıyla eşitlendikten sonra katmanlanır. |
| 0x80c80262 | -2134375838 | ECS_E_GHOSTING_UNSUPPORTED_RP | Desteklenmeyen bir telafi noktası olduğu için dosya katmanlanamadı. | Dosya bir Veri Çoğaltma telafi noktasıysa, Veri Çoğaltma desteğini etkinleştirmek için [planlama kılavuzundaki](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#data-deduplication) adımları izleyin. Veri Çoğaltma dışındaki telafi noktaları olan dosyalar desteklenmez ve katmanlandırılamaz.  |
| 0x80c83052 | -2134364078 | ECS_E_CREATE_SV_STREAM_ID_MISMATCH | Dosya değiştirildiğinden katmanlanmadı. | Eylem gerekmiyor. Değiştirilen dosya Azure dosya paylaşımıyla eşitlendikten sonra dosya katmanlanır. |
| 0x80c80269 | -2134375831 | ECS_E_GHOSTING_REPLICA_NOT_FOUND | Dosya, Azure dosya paylaşımıyla eşitlenmediği için katmanlanamadı. | Eylem gerekmiyor. Dosya, Azure dosya paylaşımıyla eşitlendikten sonra katmanlanır. |
| 0x80072ee2 | -2147012894 | WININET_E_TIMEOUT | Dosya, ağ sorunu nedeniyle katmanlandırılamamış. | Eylem gerekmiyor. Hata devam ederse, Azure dosya paylaşımına ağ bağlantısını denetleyin. |
| 0x80c80017 | -2134376425 | ECS_E_SYNC_OPLOCK_BROKEN | Dosya değiştirildiğinden katmanlanmadı. | Eylem gerekmiyor. Değiştirilen dosya Azure dosya paylaşımıyla eşitlendikten sonra dosya katmanlanır. |
| 0x800705aa | -2147023446 | ERROR_NO_SYSTEM_RESOURCES | Dosya, sistem kaynaklarının yetersizliği nedeniyle katmana ulaşamadı. | Hata devam ederse, hangi uygulamanın veya çekirdek modu sürücüsünün sistem kaynaklarını tükettiğini araştırın. |



### <a name="how-to-troubleshoot-files-that-fail-to-be-recalled"></a>Geri çağrılmayan dosyaları sorun giderme  
Dosyalar geri çağrılmazsa:
1. Olay Görüntüleyici'de, Uygulamalar ve Hizmetler\Microsoft\FileSync\Agent altında bulunan telemetri, operasyonel ve tanılama olay günlüklerini gözden geçirin.
    1. Azure dosya paylaşımında dosyaların var olduğunu doğrulayın.
    2. Sunucunun internet bağlantısı olduğunu doğrulayın. 
    3. Hizmetler MMC snap-in'i açın ve Depolama Eşitleme Aracısı hizmetinin (FileSyncSvc) çalıştığını doğrulayın.
    4. Azure Dosya Eşitleme filtresi sürücülerinin (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
        - Yükseltilmiş bir komut isteminde `fltmc`çalıştırın. StorageSync.sys ve StorageSyncGuard.sys dosya sistem filtre sürücüleri listelenen doğrulayın.

> [!NOTE]
> Bir Olay Kimliği 9006, bir dosya geri çağırmayı başaramazsa (hata kodu başına bir olay günlüğe kaydedilir) Telemetri olay günlüğünde saatte bir günlüğe kaydedilir. Hata kodu için düzeltme adımlarının listelanıp listelenmedığını görmek için [Geri Çağırma hatalarını ve düzeltme](#recall-errors-and-remediation) bölümünü denetleyin.

### <a name="recall-errors-and-remediation"></a>Geri çağırma hataları ve düzeltme

| HRESULT | HRESULT (ondalık) | Hata dizesi | Sorun | Düzeltme |
|---------|-------------------|--------------|-------|-------------|
| 0x80070079 | -2147942521 | ERROR_SEM_TIMEOUT | Dosya, G/Ç zaman nedeniyle geri çağırmayı başaramadı. Bu sorun çeşitli nedenlerle oluşabilir: sunucu kaynak kısıtlamaları, zayıf ağ bağlantısı veya Azure depolama sorunu (örneğin, azaltma). | Eylem gerekmiyor. Hata birkaç saat devam ederse bir destek talebi açın. |
| 0x80070036 | -2147024842 | ERROR_NETWORK_BUSY | Dosya, ağ sorunu nedeniyle geri çağırmayı başaramadı.  | Hata devam ederse, Azure dosya paylaşımına ağ bağlantısını denetleyin. |
| 0x80c80037 | -2134376393 | ECS_E_SYNC_SHARE_NOT_FOUND | Sunucu bitiş noktası silindiği için dosya geri çağrılamadı. | Bu sorunu gidermek için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) |
| 0x80070005 | -2147024891 | ERROR_ACCESS_DENIED | Dosya, erişim reddedilen bir hata nedeniyle geri çağırmayı başaramadı. Depolama hesabındaki güvenlik duvarı ve sanal ağ ayarları etkinleştirilmişse ve sunucunun depolama hesabına erişimi yoksa bu sorun oluşabilir. | Bu sorunu gidermek için, dağıtım kılavuzundaki [Yapılandırma güvenlik duvarı ve sanal ağ ayarları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings) bölümünde belgelenen adımları izleyerek Sunucu IP adresini veya sanal ağı ekleyin. |
| 0x80c86002 | -2134351870 | ECS_E_AZURE_RESOURCE_NOT_FOUND | Azure dosya paylaşımında erişilemediği için dosya geri çağrılamadı. | Bu sorunu gidermek için, dosyanın Azure dosya paylaşımında var olduğunu doğrulayın. Dosya Azure dosya paylaşımında varsa, en son Azure Dosya Eşitleme [aracısı sürümüne](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#supported-versions)yükseltin. |
| 0x80c8305f | -2134364065 | ECS_E_EXTERNAL_STORAGE_ACCOUNT_AUTHORIZATION_FAILED | Dosya, depolama hesabına yetki verilmemesi nedeniyle geri çağrılmadı. | Bu sorunu gidermek için Azure [Dosya Eşitlemi'nin depolama hesabına erişimi olduğunu](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#troubleshoot-rbac)doğrulayın. |
| 0x80c86030 | -2134351824 | ECS_E_AZURE_FILE_SHARE_NOT_FOUND | Azure dosya paylaşımına erişilemediği için dosya geri çağrılamadı. | Dosya paylaşımının var olduğunu ve erişilebilir olduğunu doğrulayın. Dosya paylaşımı silinip yeniden oluşturulduysa, Azure dosya [paylaşımı silinip](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#-2134375810) eşitleme grubunu silmek ve yeniden oluşturmak için bölümü yeniden oluşturulduğu için Eşitleme'de belgelenen adımları gerçekleştirin. |
| 0x800705aa | -2147023446 | ERROR_NO_SYSTEM_RESOURCES | Dosya, sistem kaynaklarının yetersizliği nedeniyle geri çağrılamadı. | Hata devam ederse, hangi uygulamanın veya çekirdek modu sürücüsünün sistem kaynaklarını tükettiğini araştırın. |
| 0x8007000e | -2147024882 | ERROR_OUTOFMEMORY | Dosya yetersiz bellek nedeniyle geri çağırmak için başarısız oldu. | Hata devam ederse, düşük bellek durumuna hangi uygulamanın veya çekirdek modu sürücüsünün neden olduğunu araştırın. |
| 0x80070070 | -2147024784 | ERROR_DISK_FULL | Dosya yetersiz disk alanı nedeniyle geri çağırma başarısız oldu. | Bu sorunu gidermek için, dosyaları farklı bir ses edoğru taşıyarak birimde yer açmak, birimin boyutunu artırmak veya Invoke-StorageSyncCloudTiering cmdlet'i kullanarak dosyaları katmana zorlayarak birimde yer ayırın. |

### <a name="tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint"></a>Sunucu uç noktası silindikten sonra katmanlı dosyalara sunucuda erişilemiyor
Dosyalar bir sunucu bitiş noktasını silmeden önce geri çağrılmazsa, sunucudaki katmanlı dosyalara erişilemez hale gelir.

Katmanlı dosyalara erişilemiyorsa günlüğe kaydedilen hatalar
- Bir dosyayı eşitlerken, hata kodu -2147942467 (0x80070043 - ERROR_BAD_NET_NAME) ItemResults olay günlüğüne kaydedilir
- Bir dosyayı geri çağırırken, hata kodu -2134376393 (0x80c80037 - ECS_E_SYNC_SHARE_NOT_FOUND) Geri Çağırma Sonuçları olay günlüğüne kaydedilir

Aşağıdaki koşullara uyulursa katmanlanmış dosyalarınıza erişimi geri yüklemek mümkün olabilir:
- Sunucu uç noktası son 30 gün içinde silinmiştir
- Bulut uç noktası silinmemiştir 
- Dosya paylaşımı silinmemiştir
- Eşitleme grubu silinmemiştir

Yukarıdaki koşullara uyulmazsa, 30 gün içinde sunucuda aynı yolda aynı eşitleme grubu içinde sunucu uç noktasını yeniden oluşturarak dosyalara erişimi geri yükleyebilirsiniz. 

Yukarıdaki koşullara uyulmazsa, bu katmanlanmış dosyalar sunucuda yalnız bırakılmış olduğundan erişimi geri yüklemek mümkün olmaz. Lütfen yetim katmanlı dosyaları kaldırmak için aşağıdaki yönergeleri izleyin.

**Notlar**
- Katmanlı dosyalara sunucuda erişilemediğinde, Azure dosya paylaşımına doğrudan erişirseniz, dosyanın tamamına erişilemeye devam edebilirsiniz.
- Gelecekte alt katmanlı dosyaları önlemek için, sunucu bitiş noktasını silerken [sunucu bitiş noktasını kaldır'da](https://docs.microsoft.com/azure/storage/files/storage-sync-files-server-endpoint#remove-a-server-endpoint) belgelenen adımları izleyin.

<a id="get-orphaned"></a>**Yetim katmanlı dosyaların listesini nasıl alabilirim?** 

1. Azure Dosya Eşitleme aracısı sürümünü n5.1 veya sonraki sürüm yüklendi doğrulayın.
2. Yetim katmanlı dosyaları listelemek için aşağıdaki PowerShell komutlarını çalıştırın:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
$orphanFiles = Get-StorageSyncOrphanedTieredFiles -path <server endpoint path>
$orphanFiles.OrphanedTieredFiles > OrphanTieredFiles.txt
```
3. OrphanTieredFiles.txt çıktı dosyasını silindikten sonra yedeklemeden geri yüklenmeleri gereken durumlarda kaydedin.

<a id="remove-orphaned"></a>**Yetim katmanlı dosyalar nasıl kaldırılır?** 

*Seçenek 1: Verilen katmanlı dosyaları silme*

Bu seçenek, Windows Server'daki alt katmanlı dosyaları siler, ancak 30 gün sonra rekreasyon nedeniyle varsa veya farklı bir eşitleme grubuna bağlıysa sunucu bitiş noktasını kaldırmayı gerektirir. Dosya çakışmaları, sunucu bitiş noktası yeniden oluşturulmadan önce Windows Server veya Azure dosya paylaşımında dosyalar güncelleştirilirse oluşur.

1. Azure Dosya Eşitleme aracısı sürümünü n5.1 veya sonraki sürüm yüklendi doğrulayın.
2. Azure dosya paylaşımını ve sunucu bitiş noktası konumunu yedekleyin.
3. [Sunucu bitiş noktasını](https://docs.microsoft.com/azure/storage/files/storage-sync-files-server-endpoint#remove-a-server-endpoint)kaldır'da belgelenen adımları izleyerek eşitleme grubundaki sunucu bitiş noktasını (varsa) kaldırın.

> [!Warning]  
> Sunucu bitiş noktası Remove-StorageSyncOrphanedTieredFiles cmdlet kullanmadan önce kaldırılmazsa, sunucudaki alt katmanlı dosyayı silmek Azure dosya paylaşımındaki tüm dosyayı siler. 

4. Yetim katmanlı dosyaları listelemek için aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
$orphanFiles = Get-StorageSyncOrphanedTieredFiles -path <server endpoint path>
$orphanFiles.OrphanedTieredFiles > OrphanTieredFiles.txt
```
5. OrphanTieredFiles.txt çıktı dosyasını silindikten sonra yedeklemeden geri yüklenmeleri gereken durumlarda kaydedin.
6. Yetim katmanlı dosyaları silmek için aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
$orphanFilesRemoved = Remove-StorageSyncOrphanedTieredFiles -Path <folder path containing orphaned tiered files> -Verbose
$orphanFilesRemoved.OrphanedTieredFiles > DeletedOrphanFiles.txt
```
**Notlar** 
- Azure dosya paylaşımıyla eşitlenmemiş sunucuda değiştirilen katmanlı dosyalar silinir.
- Erişilebilen katmanlı dosyalar (yetim değil) silinmez.
- Katmanlı olmayan dosyalar sunucuda kalır.

7. İsteğe bağlı: Adım 3'te silinirse sunucu bitiş noktasını yeniden oluşturun.

*Seçenek 2: Azure dosya paylaşımını birleştirin ve sunucuda yedek olan dosyaları yerel olarak kopyalayın*

Bu seçenek, sunucu bitiş noktasının kaldırılmasını gerektirmez, ancak tüm dosyaları yerel olarak kopyalamak için yeterli disk alanı gerektirir.

1. Katmanlı dosyaları devre demalı Windows Server'da Azure dosya paylaşımını [birleştirin.](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows)
2. Yetim katmanlı dosyaları listelemek için aşağıdaki PowerShell komutlarını çalıştırın:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
$orphanFiles = Get-StorageSyncOrphanedTieredFiles -path <server endpoint path>
$orphanFiles.OrphanedTieredFiles > OrphanTieredFiles.txt
```
3. Sunucudaki orphaned katmanlı dosyaları tanımlamak için OrphanTieredFiles.txt çıktı dosyasını kullanın.
4. Azure dosya paylaşımından Windows Server'a tüm dosyayı kopyalayarak, yedek katmanlı dosyaların üzerine yazın.

### <a name="how-to-troubleshoot-files-unexpectedly-recalled-on-a-server"></a>Bir sunucuda beklenmedik bir şekilde geri çağrılan dosyaları sorun giderme  
Çok sayıda dosya okuyan virüsten koruma, yedekleme ve diğer uygulamalar, çevrimdışı atlama özelliğine saygı duymadıkları ve bu dosyaların içeriğini okumayı atlamadıkları sürece istenmeyen geri çağırmalara neden olur. Bu seçeneği destekleyen ürünler için çevrimdışı dosyaları atlamak, virüsten koruma taramaları veya yedekleme işleri gibi işlemler sırasında istenmeyen yeniden çağırma olaylarından kaçınılmasına yardımcı olabilir.

Çözümlerinin çevrimdışı dosyaları okumayı atlayacak şekilde yapılandırılması konusunda bilgi almak için yazılım satıcınızla iletişime geçin.

Dosya Gezgini'nde dosyalara göz atarken olduğu gibi diğer senaryolarda da istenmeyen geri çağırmalar oluşabilir. Dosya Gezgini'nde bulut katmanlı dosyalara sahip bir klasörün açılması, istenmeyen yeniden çağırma işlemlerine neden olabilir. Bu durumun gerçekleşme olasılığı, sunucuda virüsten koruma çözümünün etkinleştirilmiş olması halinde daha yüksektir.

> [!NOTE]
>Telemetri olay günlüğünde hangi uygulamanın geri çağırmaya neden olduğunu belirlemek için Olay Kimliği 9059'u kullanın. Bu olay, bir sunucu bitiş noktası için uygulama geri çağırma dağıtımı sağlar ve saatte bir günlüğe kaydedilir.

## <a name="general-troubleshooting"></a>Genel sorun giderme
Bir sunucuda Azure Dosya Eşitlemenle ilgili sorunlarla karşılaşırsanız, aşağıdaki adımları tamamlayarak başlayın:
1. Olay Görüntüleyici'de, telemetri, operasyonel ve tanılama olay günlüklerini gözden geçirin.
    - Uygulamalar ve Hizmetler\Microsoft\FileSync\Agent altında eşitleme, katmanlama ve geri çağırma sorunları telemetri, tanılama ve operasyonel olay günlüklerinde günlüğe kaydedilir.
    - Bir sunucunun yönetimiyle ilgili sorunlar (örneğin, yapılandırma ayarları) Uygulamalar ve Hizmetler\Microsoft\FileSync\Management altında operasyonel ve tanılama olay günlüklerinde günlüğe kaydedilir.
2. Azure Dosya Eşitleme hizmetinin sunucuda çalıştığını doğrulayın:
    - Hizmetler MMC snap-in'i açın ve Depolama Eşitleme Aracısı hizmetinin (FileSyncSvc) çalıştığını doğrulayın.
3. Azure Dosya Eşitleme filtresi sürücülerinin (StorageSync.sys ve StorageSyncGuard.sys) çalıştığını doğrulayın:
    - Yükseltilmiş bir komut isteminde `fltmc`çalıştırın. StorageSync.sys ve StorageSyncGuard.sys dosya sistem filtre sürücüleri listelenen doğrulayın.

Sorun çözülmezse, AFSDiag aracını çalıştırın:
1. AFSDiag çıktısının kaydedileceği bir dizin oluşturun (örneğin, C:\Çıktı).
    > [!NOTE]
    >AFSDiag günlükleri toplamadan önce çıktı dizinindeki tüm içeriği siler. Veri içermeyen bir çıktı konumu belirtin.
2. Yükseltilmiş bir PowerShell penceresini açın ve ardından aşağıdaki komutları çalıştırın (her komuttan sonra Enter tuşuna basın):

    ```powershell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Azure Dosya Eşitleme çekirdeği modu izleme düzeyi için **1** girin (aksi belirtilmedikçe, daha ayrıntılı izlemeler oluşturmak için) ve ardından Enter tuşuna basın.
4. Azure Dosya Eşitleme kullanıcı modu izleme düzeyi için **1** girin (aksi belirtilmedikçe, daha ayrıntılı izlemeler oluşturmak için) ve ardından Enter tuşuna basın.
5. Sorunu yeniden üretin. Bitirdikten sonra **D**girin.
6. Günlükleri ve izleme dosyalarını içeren bir .zip dosyası belirttiğiniz çıktı dizinine kaydedilir.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
- [Azure Dosyaları sık sorulan sorular](storage-files-faq.md)
- [Windows’ta Azure Dosyalar sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux'taki Azure Dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
