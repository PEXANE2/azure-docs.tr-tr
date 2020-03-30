---
title: Azure Dosya Eşitlemi ile kayıtlı sunucuları yönetme | Microsoft Dokümanlar
description: Azure Dosya Eşitleme Depolama Eşitleme Hizmeti ile bir Windows Sunucusu'na nasıl kaydolun ve kaydını kaldırın öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 2656716560b981481273c3032fc0c7b1a06be8a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255099"
---
# <a name="manage-registered-servers-with-azure-file-sync"></a>Azure Dosya Eşitlemi ile kayıtlı sunucuları yönetme
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu, Windows Sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Aşağıdaki makalede, depolama eşitleme hizmeti olan bir sunucunun nasıl kaydedilip yönetilenbir sunucuyu nasıl yönetiniz gösterilebilir. Azure [Dosya Eşitlemi'nin](storage-sync-files-deployment-guide.md) uçuca nasıl dağıtılanabildiğini öğrenmek için Azure Dosya Eşitlemeyi'ni nasıl dağıtılayabilirsiniz.

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Depolama Eşitleme Hizmeti ile bir sunucuyu kaydetme/silme
Bir sunucunun Azure Dosya Eşitlemi ile kaydedilmesi, Windows Server ile Azure arasında bir güven ilişkisi kurar. Bu ilişki daha sonra sunucuda, Azure dosya paylaşımıyla eşitlenmesi gereken belirli klasörleri *(bulut bitiş noktası*olarak da bilinir) temsil eden *sunucu uç noktaları* oluşturmak için kullanılabilir. 

### <a name="prerequisites"></a>Ön koşullar
Bir sunucuyu Depolama Eşitleme Hizmetine kaydettirmek için öncelikle sunucunuzu gerekli ön koşullarla hazırlamanız gerekir:

* Sunucunuz Windows Server'ın desteklenen bir sürümünü çalıştırıyor olmalıdır. Daha fazla bilgi için Azure [Dosya Eşitleme sistem gereksinimleri ve birlikte çalışabilirlik'e](storage-sync-files-planning.md#windows-file-server-considerations)bakın.
* Bir Depolama Eşitleme Hizmetinin dağıtıldığından emin olun. Depolama Eşitleme Hizmeti'nin nasıl dağıtılanöğretilen hakkında daha fazla bilgi için [Azure Dosya Eşitleme'yi nasıl](storage-sync-files-deployment-guide.md)dağıtılayabilirsiniz.
* Sunucunun Internet'e bağlı olduğundan ve Azure'a erişilebildiğinden emin olun.
* Sunucu Yöneticisi UI'si ile yöneticiler için IE Geliştirilmiş Güvenlik Yapılandırmasını devre dışı kaldırın.
    
    ![IE Gelişmiş Güvenlik Yapılandırması ile Sunucu Yöneticisi UI vurgulanır](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Azure PowerShell modülünün sunucunuza yüklendiğinden emin olun. Sunucunuz Bir Failover Kümesi'nin üyesiyse, kümedeki her düğüm Az modülü gerektirir. Az modülünün nasıl yüklenirhakkında daha fazla bilgi [Yükleme ve Azure PowerShell'i yapılandırma](https://docs.microsoft.com/powershell/azure/install-Az-ps)da bulunabilir.

    > [!Note]  
    > Bir sunucuyu kaydetmek/kayıt dışı etmek için Az PowerShell modülünün en yeni sürümünü kullanmanızı öneririz. Az paketi daha önce bu sunucuya yüklenmişse (ve bu sunucudaki PowerShell sürümü 5.* veya daha büyükse), bu paketi güncelleştirmek için `Update-Module` cmdlet'i kullanabilirsiniz. 
* Ortamınızda bir ağ proxy sunucusu kullanıyorsanız, eşitleme aracısının kullanabilmek için sunucunuzdaki proxy ayarlarını yapılandırın.
    1. Proxy IP adresinizi ve bağlantı noktası numaranızı belirleyin
    2. Bu iki dosyayı edin:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. Yukarıdaki iki dosyada 127.0.0.1:8888'i değiştiren yukarıdaki iki dosyada şekil 1'deki (bu bölümün altında) satırları doğru IP adresine (127.0.0.1 değiştir) ve doğru bağlantı noktası numarasına (8888'i değiştirin):
    4. Komut satırı üzerinden WinHTTP proxy ayarlarını ayarlayın:
        * Proxy göster: netsh winhttp show proxy
        * Proxy ayarlayın: netsh winhttp set proxy 127.0.0.1:8888
        * Proxy sıfırlamak: netsh winhttp reset proxy
        * aracı yüklendikten sonra bu kurulum ise, o zaman eşitleme aracımızı yeniden başlatın: net stop filesyncsvc
    
```XML
    Figure 1:
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
    </system.net>
```    

### <a name="register-a-server-with-storage-sync-service"></a>Depolama Eşitleme Hizmeti ile sunucu kaydetme
Bir sunucunun Azure Dosya *Eşitleme grubunda* *sunucu bitiş noktası* olarak kullanılabilmesi için önce, bir Depolama *Eşitleme Hizmetine*kaydedilmesi gerekir. Bir sunucu aynı anda yalnızca bir Depolama Eşitleme Hizmetine kaydedilebilir.

#### <a name="install-the-azure-file-sync-agent"></a>Azure Dosya Eşitleme aracısını yükleme
1. [Azure Dosya Eşitleme aracısını indirin.](https://go.microsoft.com/fwlink/?linkid=858257)
2. Azure Dosya Eşitleme aracısını başlatın.
    
    ![Azure Dosya Eşitleme aracısı yükleyicisinin ilk bölmesi](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Microsoft Update'i kullanarak Azure Dosya Eşitleme aracısının güncelleştirmelerini etkinleştirdiğinden emin olun. Kritik güvenlik düzeltmeleri ve sunucu paketindeki özellik geliştirmeleri Microsoft Update aracılığıyla gönderildiğinden önemlidir.

    ![Azure Dosya Eşitleme aracısı yükleyicisinin Microsoft Update bölmesinde Microsoft Update'in etkinleştirildiğinden emin olun](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Sunucu daha önce kaydedilmemişse, yüklemeyi tamamladıktan hemen sonra sunucu kaydı kullanıcı ui açılır.

> [!Important]  
> Sunucu bir Failover Kümesi'nin üyesiyse, Azure Dosya Eşitleme aracısının kümedeki her düğüme yüklenmesi gerekir.

#### <a name="register-the-server-using-the-server-registration-ui"></a>Sunucu kaydı UI'sini kullanarak sunucuyu kaydetme
> [!Important]  
> Bulut Çözüm Sağlayıcısı (CSP) abonelikleri sunucu kayıt UI'sini kullanamaz. Bunun yerine PowerShell'i kullanın (bu bölümün altında).

1. Sunucu kaydı UI, Azure Dosya Eşitleme aracısının yüklemesini tamamladıktan hemen sonra başlatılmazsa, yürütülerek `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`el ile başlatılabilir.
2. Azure aboneliğinize erişmek için *Oturum Aç'ı* tıklatın. 

    ![Sunucu kaydı UI'nin açılış iletişim kutusu](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. İletişim kutusundan doğru aboneliği, kaynak grubunu ve Depolama Eşitleme Hizmetini seçin.

    ![Depolama Eşitleme Hizmeti bilgileri](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Önizlemede, işlemi tamamlamak için bir oturum açma daha gerekir. 

    ![Oturum açma iletişim kutusu](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Sunucu bir Failover Kümesi üyesiyse, her sunucunun Sunucu Kaydı'nı çalıştırması gerekir. Azure Portalı'nda kayıtlı sunucuları görüntülediğinizde, Azure Dosya Eşitlemesi her düğümü otomatik olarak aynı Failover Kümesinin üyesi olarak tanır ve uygun şekilde gruplandırAbilir.

#### <a name="register-the-server-with-powershell"></a>Sunucuyu PowerShell ile kaydedin
PowerShell üzerinden sunucu kaydı da gerçekleştirebilirsiniz. Bulut Çözüm Sağlayıcısı (CSP) abonelikleri için desteklenen tek sunucu kaydı yolu budur:

```powershell
Register-AzStorageSyncServer -ResourceGroupName "<your-resource-group-name>" -StorageSyncServiceName "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Depolama Eşitleme Hizmeti ile sunucunun kaydını kaldırın
Depolama Eşitleme Hizmeti olan bir sunucunun kaydını çıkarmak için gereken birkaç adım vardır. Bir sunucunun düzgün bir şekilde nasıl kaydını kaldırılacak bir göz atalım.

> [!Warning]  
> Bir sunucunun kaydını kaldırıp kaydederek veya bir Microsoft mühendisi tarafından açıkça talimat verilmedikçe sunucu uç noktalarını kaldırArak ve yeniden oluşturarak eşitleme, bulut katmanlama veya Azure Dosya Eşitlemesi'nin başka bir yönüyle ilgili sorunları gidermeye çalışmayın. Sunucunun kaydını kaldırmak ve sunucu uç noktalarını kaldırmak yıkıcı bir işlemdir ve sunucu uç noktalarıolan birimlerdeki katmanlı dosyalar, kayıtlı sunucu ve sunucu uç noktaları ayrıldıktan sonra Azure dosya paylaşımındaki konumlarına "yeniden bağlanmaz". eşitleme hatalarına neden olacak yeniden oluşturulur. Ayrıca, bir sunucu bitiş noktası ad alanı dışında bulunan katmanlı dosyalar kalıcı olarak kaybolabilir. Bulut katmanlama hiç etkinleştirilemese bile, katmanlı dosyalar sunucu uç noktalarında bulunabilir.

#### <a name="optional-recall-all-tiered-data"></a>(İsteğe bağlı) Tüm katmanlı verileri geri çağırma
Azure Dosya Eşitlemesi'ni kaldırdıktan sonra kullanılabilir olmak üzere katmanlı olan dosyaların (örneğin, bu bir test, ortam değil, bir üretimdir), sunucu bitiş noktalarını içeren her birimdeki tüm dosyaları hatırlayın. Tüm sunucu uç noktaları için bulut katmanlamayı devre dışı bırakın ve ardından aşağıdaki PowerShell cmdlet'i çalıştırın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Sunucu bitiş noktasını barındıran yerel birim, katmanlı tüm verileri geri çağırmak `Invoke-StorageSyncFileRecall` için yeterli boş alana sahip değilse, cmdlet başarısız olur.  

#### <a name="remove-the-server-from-all-sync-groups"></a>Sunucuyu tüm eşitleme gruplarından kaldırma
Depolama Eşitleme Hizmeti'ndeki sunucunun kaydını kaldırmadan önce, bu sunucudaki tüm sunucu uç noktalarının kaldırılması gerekir. Bu, Azure portalı üzerinden yapılabilir:

1. Sunucunuzun kayıtlı olduğu Depolama Eşitleme Hizmeti'ne gidin.
2. Depolama Eşitleme Hizmeti'ndeki her eşitleme grubunda bu sunucunun tüm sunucu uç noktalarını kaldırın. Bu, eşitleme grubu bölmesinde ilgili sunucu bitiş noktasını sağ tıklayarak gerçekleştirilebilir.

    ![Eşitleme grubundan sunucu bitiş noktasını kaldırma](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Bu da basit bir PowerShell komut dosyası ile gerçekleştirilebilir:

```powershell
Connect-AzAccount

$storageSyncServiceName = "<your-storage-sync-service>"
$resourceGroup = "<your-resource-group>"

Get-AzStorageSyncGroup -ResourceGroupName $resourceGroup -StorageSyncServiceName $storageSyncServiceName | ForEach-Object { 
    $syncGroup = $_; 
    Get-AzStorageSyncServerEndpoint -ParentObject $syncGroup | Where-Object { $_.ServerEndpointName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzStorageSyncServerEndpoint -InputObject $_ 
    } 
}
```

#### <a name="unregister-the-server"></a>Sunucunun kaydını kaldırın
Artık tüm veriler geri çağrıldığına ve sunucu tüm eşitleme gruplarından kaldırıldığına göre, sunucu kayıtsız kaçabilir. 

1. Azure portalında, Depolama Eşitleme Hizmeti'nin *Kayıtlı sunucular* bölümüne gidin.
2. Kaydını çıkarmak istediğiniz sunucuya sağ tıklayın ve "KaydıNı Kaldırın Server"ı tıklatın.

    ![Sunucunun kaydını kaldırın](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Azure Dosya Eşitlemesini veri merkezinizde iyi bir komşu olduğundan emin olun 
Azure Dosya Eşitlemi nadiren veri merkezinizde çalışan tek hizmet olacağından, Azure Dosya Eşitlemi'nin ağ ve depolama kullanımını sınırlamak isteyebilirsiniz.

> [!Important]  
> Sınırların çok düşük ayarlanması, Azure Dosya Eşitleme senkronizasyonu ve geri çağırma performansını etkiler.

### <a name="set-azure-file-sync-network-limits"></a>Azure Dosya Eşitleme ağ sınırlarını ayarlama
`StorageSyncNetworkLimit` Cmdlets'i kullanarak Azure Dosya Eşitlemi'nin ağ kullanımını azaltabilirsiniz.

> [!Note]  
> Katmanlı bir dosyaya erişildiğinde veya Invoke-StorageSyncFileRecall cmdlet kullanıldığında ağ sınırları geçerli değildir.

Örneğin, Azure Dosya Eşitleme'nin çalışma haftası boyunca 09:00 ile 17:00 (17:00) arasında 10 Mbps'den fazla kullanmamasını sağlamak için yeni bir gaz sınırı oluşturabilirsiniz: 

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

Aşağıdaki cmdlet kullanarak limitinizi görebilirsiniz:

```powershell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Ağ sınırlarını kaldırmak `Remove-StorageSyncNetworkLimit`için . Örneğin, aşağıdaki komut tüm ağ sınırlarını kaldırır:

```powershell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Windows Server depolama QoS kullanma 
Azure Dosya Eşitleme, Windows Server sanallaştırma ana bilgisayarda çalışan sanal bir makinede barındırıldığında, depolama IO tüketimini düzenlemek için Depolama QoS'unu (hizmet depolama kalitesi) kullanabilirsiniz. Depolama QoS ilkesi maksimum (veya limit, StorageSyncNetwork sınırının yukarıda nasıl uygulandığı gibi) veya minimum (veya rezervasyon) olarak ayarlanabilir. Diğer iş yükleri kullanmıyorsa, Azure Dosya Eşitlemesi'nin kullanılabilir depolama bant genişliğini kullanmak için en az ayarlanmasına olanak tanır. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview)

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
- [Azure Dosya Eşitleme ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md)
