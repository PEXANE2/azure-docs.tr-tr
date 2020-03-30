---
title: Azure Günlük Analizi aracısını yönetme
description: Bu makalede, genellikle bir makinede dağıtılan Log Analytics Windows veya Linux aracısının yaşam döngüsü sırasında gerçekleştireceğiniz farklı yönetim görevleri açıklanmaktadır.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/14/2019
ms.openlocfilehash: 4d0ceacd37748e9761903d02fd7e052d70b10e15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275106"
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Windows ve Linux için Log Analytics aracısını yönetme ve bakımını yapma

Azure Monitor'da Log Analytics Windows veya Linux aracısının ilk dağıtımından sonra aracıyı yeniden yapılandırmanız, yükseltmeniz veya kullanım ömrü nde emeklilik aşamasına ulaşmışsa bilgisayardan kaldırmanız gerekebilir. Bu rutin bakım görevlerini el ile veya otomasyon yoluyla kolayca yönetebilirsiniz, bu da hem operasyonel hatayı hem de giderleri azaltır.

## <a name="upgrading-agent"></a>Aracıyı yükseltme

Windows ve Linux için Log Analytics aracısı, VM'nin çalıştırılan dağıtım senaryosuna ve ortamına bağlı olarak el ile veya otomatik olarak en son sürüme yükseltilebilir. Aracıyı yükseltmek için aşağıdaki yöntemler kullanılabilir.

| Ortam | Kurulum Yöntemi | Yükseltme yöntemi |
|--------|----------|-------------|
| Azure VM | Windows/Linux için Log Analytics aracısı VM uzantısı | Azure Kaynak Yöneticisi şablonunuzu, özellik *otomatikYükseltmeMinorVersion'u* **false**olarak ayarlayarak devre dışı bırakmak üzere yapılandırmadığınız sürece aracı otomatik olarak otomatik olarak yükseltilir. |
| Özel Azure VM görüntüleri | Windows/Linux için Log Analytics aracısının manuel olarak yüklenmesi | VM'leri aracının en yeni sürümüne güncellemenin, Windows yükleyici paketini veya Linux'un kendi kendine ayıklama ve yüklenebilir kabuk komut paketi paketini çalıştıran komut satırından gerçekleştirilmesi gerekir.|
| Azure Olmayan VM'ler | Windows/Linux için Log Analytics aracısının manuel olarak yüklenmesi | VM'leri aracının en yeni sürümüne güncellemenin, Windows yükleyici paketini veya Linux'un kendi kendine ayıklama ve yüklenebilir kabuk komut paketi paketini çalıştıran komut satırından gerçekleştirilmesi gerekir. |

### <a name="upgrade-windows-agent"></a>Windows aracıyı yükseltme 

Windows VM'deki aracıyı Log Analytics VM uzantısı kullanılarak yüklenmemiş en son sürüme güncellemek için Komut İstemi, komut dosyası\<veya\>diğer otomasyon çözümlerinden veya MMASetup platformu .msi Kurulum Sihirbazı'nı kullanarak çalışırsınız.  

Aşağıdaki adımları gerçekleştirerek Windows aracısının en son sürümünü Log Analytics çalışma alanınızdan indirebilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Günlük Analizi çalışma alanlarını**seçin.

3. Günlük Analizi çalışma alanları listenizde çalışma alanını seçin.

4. Günlük Analizi çalışma alanınızda **Gelişmiş ayarları**seçin, ardından **Bağlı Kaynaklar'ı**ve son olarak **Windows Sunucularını**seçin.

5. Windows **Sunucuları** sayfasından, Windows işletim sisteminin işlemci mimarisine bağlı olarak indirmek için uygun Windows Agent sürümünü **karşıdan** yükleyin' i seçin.

>[!NOTE]
>Windows için Log Analytics aracısının yükseltilmesi sırasında, rapor için bir çalışma alanının yapılandırılmasını veya yeniden yapılandırılmayı desteklemez. Aracıyı yapılandırmak için, [çalışma alanı ekleme veya kaldırma](#adding-or-removing-a-workspace)altında listelenen desteklenen yöntemlerden birini izlemeniz gerekir.
>

#### <a name="to-upgrade-using-the-setup-wizard"></a>Kurulum Sihirbazı'nı kullanarak yükseltme yapmak için

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kurulum Sihirbazı başlatmak için **MMASetup-\<platform\>.exe** çalıştırın.

3. Kurulum Sihirbazı'nın ilk sayfasında **İleri'yi**tıklatın.

4. Microsoft **İzleme Aracısı Kurulumu** iletişim kutusunda, lisans sözleşmesini kabul etmeyi **kabul ediyorum'u** tıklatın.

5. **Microsoft Monitoring Agent Kurulumu** iletişim kutusunda **Yükselt**'t tıklayın. Durum sayfası yükseltmenin ilerleyişini görüntüler.

6. Microsoft **Monitoring Agent yapılandırması başarıyla tamamlandığında.** sayfa görünür, **Bitir'i**tıklatın.

#### <a name="to-upgrade-from-the-command-line"></a>Komut satırından yükseltmek için

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aracı yükleme dosyaları ayıklamak için, yükseltilmiş `MMASetup-<platform>.exe /c` bir komut istemi çalıştırın ve dosyaları ayıklamak için yol için ister. Alternatif olarak, bağımsız değişkenleri `MMASetup-<platform>.exe /c /t:<Full Path>`geçerek yolu belirtebilirsiniz.

3. Aşağıdaki komutu çalıştırın. Burada D:\ yükseltme günlük dosyasının konumudur.

    ```dos
    setup.exe /qn /l*v D:\logs\AgentUpgrade.log AcceptEndUserLicenseAgreement=1
    ```

### <a name="upgrade-linux-agent"></a>Linux aracıyı yükseltme 

Önceki sürümlerden yükseltme (>1.0.0-47) desteklenir. Komutla yüklemenin `--upgrade` gerçekleştirimi, aracının tüm bileşenlerini en son sürüme yükseltir.

Aracıyı yükseltmek için aşağıdaki komutu çalıştırın.

`sudo sh ./omsagent-*.universal.x64.sh --upgrade`

## <a name="adding-or-removing-a-workspace"></a>Çalışma alanı ekleme veya kaldırma

### <a name="windows-agent"></a>Windows aracısı
Bu bölümdeki adımlar, Windows aracısını yalnızca farklı bir çalışma alanına rapor etmek veya bir çalışma alanını yapılandırmasından kaldırmak için yeniden yapılandırmak değil, aynı zamanda aracıyı birden fazla çalışma alanına rapor etmek üzere yapılandırmak istediğinizde de gereklidir (genellikle çok-homing olarak adlandırılır). Windows aracısını birden çok çalışma alanı için rapor yapacak şekilde yapılandırılmak, yalnızca aracının ilk kurulumundan sonra ve aşağıda açıklanan yöntemler kullanılarak gerçekleştirilebilir.    

#### <a name="update-settings-from-control-panel"></a>Denetim Masası'ndan ayarları güncelleştir

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Açık **Kontrol Paneli**.

3. **Microsoft İzleme Aracısı'nı** seçin ve ardından **Azure Günlük Analizi** sekmesini tıklatın.

4. Çalışma alanını kaldırıyorsanız, onu seçin ve sonra **Kaldır'ı**tıklatın. Aracının raporlamayı durdurmasını istediğiniz diğer çalışma alanı için bu adımı yineleyin.

5. Çalışma alanı ekliyorsanız, **Ekle'yi** tıklatın ve **Günlük Analitiği Çalışma Alanı** ekle iletişim kutusunu tıklatın, Çalışma Alanı Kimliği ve Çalışma Alanı Anahtarını (Birincil Anahtar) yapıştırın. Bilgisayarın Azure Kamu bulutundaki bir Log Analytics çalışma alanına raporlaması gerekiyorsa, Azure Cloud açılan listesinden Azure ABD Kamu'yu seçin.

6. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

#### <a name="remove-a-workspace-using-powershell"></a>PowerShell'i kullanarak çalışma alanını kaldırma

```powershell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>PowerShell'i kullanarak Azure reklamında çalışma alanı ekleme

```powershell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>PowerShell'i kullanarak ABD Hükümeti için Azure'da bir çalışma alanı ekleme

```powershell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Aracıyı yüklemek veya yapılandırmak için komut satırını veya komut `EnableAzureOperationalInsights` dosyasını `AddCloudWorkspace` daha `RemoveCloudWorkspace`önce kullandıysanız, değiştirildi ve .
>

### <a name="linux-agent"></a>Linux aracısı
Aşağıdaki adımlar, linux aracısını farklı bir çalışma alanına kaydetmeye veya bir çalışma alanını yapılandırmasından kaldırmaya karar verirseniz nasıl yeniden yapılandırılabildiğinizi gösterir.

1. Çalışma alanına kayıtlı olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Aşağıdaki örneğe benzer bir durum döndürmelidir:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

    Durumun aracının çalıştığını da göstermesi önemlidir, aksi takdirde aracıyı yeniden yapılandırmak için aşağıdaki adımlar başarıyla tamamlanmaz.

2. Zaten bir çalışma alanına kayıtlıysa, aşağıdaki komutu çalıştırarak kayıtlı çalışma alanını kaldırın. Aksi takdirde kayıtlı değilse, bir sonraki adıma geçin.

    `/opt/microsoft/omsagent/bin/omsadmin.sh -X`

3. Farklı bir çalışma alanına kaydolmak için aşağıdaki komutu çalıştırın:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <shared key> [-d <top level domain>]`
    
4. Değişikliklerinizin yürürlüğe aktolduğunu doğrulamak için aşağıdaki komutu çalıştırın:

    `/opt/microsoft/omsagent/bin/omsadmin.sh -l`

    Aşağıdaki örneğe benzer bir durum döndürmelidir:

    `Primary Workspace: <workspaceId>   Status: Onboarded(OMSAgent Running)`

Değişikliklerin etkili olabilmesi için aracı hizmetinin yeniden başlatılması gerekmez.

## <a name="update-proxy-settings"></a>Proxy ayarlarını güncelleştir
Aracıyı dağıtımdan sonra bir proxy sunucusu veya [Log Analytics ağ geçidi](gateway.md) aracılığıyla hizmete iletecek şekilde yapılandırmak için, bu görevi tamamlamak için aşağıdaki yöntemlerden birini kullanın.

### <a name="windows-agent"></a>Windows aracısı

#### <a name="update-settings-using-control-panel"></a>Denetim Masası'nı kullanarak ayarları güncelleştirme

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Açık **Kontrol Paneli**.

3. **Microsoft İzleme Aracısı'nı** seçin ve ardından **Proxy Ayarları** sekmesini tıklatın.

4. **Ara sunucu kullan**'a tıklayıp ara sunucunun veya ağ geçidinin URL'sini ve bağlantı noktası numarasını girin. Ara sunucunuz veya Log Analytics ağ geçidiniz kimlik doğrulaması gerektiriyorsa, kimlik doğrulaması yapmak için kullanıcı adını ve parolayı yazın, ardından **Tamam**'a tıklayın.

#### <a name="update-settings-using-powershell"></a>PowerShell kullanarak ayarları güncelleştirin

Aşağıdaki örnek PowerShell kodunu kopyalayın, ortamınıza özgü bilgilerle güncelleyin ve ps1 dosya adı uzantısı ile kaydedin. Komut dosyasını, Azure Monitor'daki Log Analytics çalışma alanına doğrudan bağlanan her bilgisayarda çalıştırın.

```powershell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object. We need to determine if we
#have the right update rollup with the API we need. If not, no need to run the rest of the script.
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

$proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

if (!$proxyMethod)
{
    Write-Output 'Health Service proxy API not present, will not update settings.'
    return
}

Write-Output "Clearing proxy settings."
$healthServiceSettings.SetProxyInfo('', '', '')

$ProxyUserName = $cred.username

Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
$healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
```

### <a name="linux-agent"></a>Linux aracısı
Linux bilgisayarlarınızın bir proxy sunucusu veya Log Analytics ağ geçidi üzerinden iletişim kurması gerekiyorsa aşağıdaki adımları gerçekleştirin. Proxy yapılandırması değeri `[protocol://][user:password@]proxyhost[:port]` sözdizimine sahiptir. *proxyhost* özelliği, ara sunucunun tam etki adı alanı veya IP adresini kabul eder.

1. Aşağıdaki komutları çalıştırıp değerleri kendi ayarlarınıza göre değiştirerek `/etc/opt/microsoft/omsagent/proxy.conf` dosyasını düzenleyin.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
    ```

2. Aşağıdaki komutu çalıştırarak aracıyı yeniden başlatın:

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ```

## <a name="uninstall-agent"></a>Aracıyı kaldır
Komut satırını veya kurulum sihirbazını kullanarak Windows veya Linux aracısını kaldırmak için aşağıdaki yordamlardan birini kullanın.

### <a name="windows-agent"></a>Windows aracısı

#### <a name="uninstall-from-control-panel"></a>Denetim Panelinden Kaldır
1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. **Denetim Masası'nda**Programlar **ve Özellikler'i**tıklatın.

3. **Programlar ve Özellikler'de** **Microsoft Monitoring Agent'ı**tıklatın, **Kaldır'ı**tıklatın ve sonra **Evet'i**tıklatın.

>[!NOTE]
>Aracı Kurulum Sihirbazı, Azure portalındaki bir çalışma alanından indirilebilen **MMASetup\<platformu\>.exe'yi**çift tıklatarak da çalıştırılabilir.

#### <a name="uninstall-from-the-command-line"></a>Komut satırından kaldırma
Aracı için indirilen dosya, IExpress ile oluşturulan bağımsız bir yükleme paketidir. Aracı ve destekleyici dosyaların kurulum programı pakette bulunur ve aşağıdaki örnekte gösterilen komut satırını kullanarak düzgün bir şekilde kaldırmak için ayıklanması gerekir.

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aracı yükleme dosyaları ayıklamak için, yükseltilmiş `extract MMASetup-<platform>.exe` bir komut istemi çalıştırın ve dosyaları ayıklamak için yol için ister. Alternatif olarak, bağımsız değişkenleri `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`geçerek yolu belirtebilirsiniz. IExpress tarafından desteklenen komut satırı anahtarları hakkında daha fazla bilgi [için, IExpress için Komut satırı anahtarlarına](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) bakın ve ardından örneği gereksinimlerinize göre güncelleyin.

3. İsteyiş olarak, yazın. `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`

### <a name="linux-agent"></a>Linux aracısı
Aracıyı kaldırmak için Linux bilgisayarında aşağıdaki komutu çalıştırın. *--temizleme* bağımsız değişkeni, aracıyı ve yapılandırmasını tamamen kaldırır.

   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Operasyon Yöneticisi yönetim grubuna rapor vermek üzere aracıyı yapılandırma

### <a name="windows-agent"></a>Windows aracısı
Windows için Log Analytics aracısını bir Sistem Merkezi Operasyon Yöneticisi yönetim grubuna rapor etmek üzere yapılandırmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Açık **Kontrol Paneli**.

3. **Microsoft İzleme Aracısı'nı** ve **ardından Operations Manager** sekmesini tıklatın.

4. Operations Manager sunucularınızın Active Directory ile tümleştirmesi varsa, **AD DS'den yönetim grubu atamalarını otomatik olarak güncelleştir'i**tıklatın.

5. **Yönetim Grubu Ekle** iletişim kutusunu açmak için **Ekle'yi** tıklatın.

6. **Yönetim grubu adı** alanında, yönetim grubunuzun adını yazın.

7. Birincil **yönetim sunucusu** alanında, birincil yönetim sunucusunun bilgisayar adını yazın.

8. Yönetim **sunucu bağlantı noktası** alanında TCP bağlantı noktası numarasını yazın.

9. **Aracı Eylem Hesabı**altında, Yerel Sistem hesabını veya yerel etki alanı hesabını seçin.

10. **Yönetim Grubu Ekle** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın ve ardından **Microsoft Monitoring Agent Properties** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

### <a name="linux-agent"></a>Linux aracısı
Linux'un Bir Sistem Merkezi Operasyon Yöneticisi yönetim grubuna rapor etmesi için Log Analytics aracısını yapılandırmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

1. Dosyayı edin`/etc/opt/omi/conf/omiserver.conf`

2. Ile `httpsport=` başlayan çizginin bağlantı noktası 1270'i tanımladığından emin olun. Gibi:`httpsport=1270`

3. OMI sunucusunu yeniden başlatın:`sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Sonraki adımlar

- Linux aracısını yüklerken veya yönetirken sorunlarla [karşılaşırsanız, Sorun Giderme'yi](agent-linux-troubleshoot.md) gözden geçirin.

- Windows aracısını yüklerken veya yönetirken sorunlarla karşılaşırsanız [Windows aracısını](agent-windows-troubleshoot.md) gözden geçirin.
