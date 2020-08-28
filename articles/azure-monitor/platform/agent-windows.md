---
title: Windows bilgisayarlarına Log Analytics Aracısı 'nı yükler
description: Bu makalede, Windows için Log Analytics aracısıyla diğer bulutlarda veya şirket içinde barındırılan Windows bilgisayarlarının Azure Izleyici 'ye nasıl bağlanacağı açıklanır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/03/2020
ms.openlocfilehash: d283c2b2cdbbeb3ef4bc4e25f4288dfd95158552
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89003380"
---
# <a name="install-log-analytics-agent-on-windows-computers"></a>Windows bilgisayarlarına Log Analytics Aracısı 'nı yükler
Bu makalede, aşağıdaki yöntemleri kullanarak Log Analytics aracısının Windows bilgisayarlarına yüklenmesiyle ilgili ayrıntılar verilmektedir:

* [Kurulum sihirbazını](#install-agent-using-setup-wizard) veya [komut satırını](#install-agent-using-command-line)kullanarak el ile yükleme.
* [Azure Otomasyonu Istenen durum yapılandırması (DSC)](#install-agent-using-dsc-in-azure-automation). 

>[!IMPORTANT]
> Bu makalede açıklanan yükleme yöntemleri genellikle şirket içi veya diğer bulutlardaki sanal makineler için kullanılır. Azure sanal makineleri için kullanabileceğiniz daha verimli seçenekler için bkz. [yükleme seçenekleri](log-analytics-agent.md#installation-options) .

> [!NOTE]
> Aracıyı birden fazla çalışma alanına rapor verecek şekilde yapılandırmanız gerekiyorsa, bu, ilk kurulum sırasında yapılamaz, yalnızca [bir çalışma alanı ekleme veya kaldırma](agent-manage.md#adding-or-removing-a-workspace)bölümünde açıklandığı gibi Denetim Masası 'Ndan veya PowerShell 'den daha sonra ayarları güncelleyerek.  

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Log Analytics Aracısı tarafından desteklenen Windows sürümlerinin listesi için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md#supported-operating-systems) .

### <a name="sha-2-code-signing-support-requirement"></a>SHA-2 kod Imzalama desteği gereksinimi 
Windows Aracısı, 17 Ağustos 2020 tarihinde yalnızca SHA-2 imzalamayı kullanmaya başlayacaktır. Bu değişiklik, herhangi bir Azure hizmetinin (Azure Izleyici, Azure Otomasyonu, Azure Güncelleştirme Yönetimi, Azure Değişiklik İzleme, Azure Güvenlik Merkezi, Azure Sentinel, Windows Defender ATP) bir parçası olarak eski bir IŞLETIM sistemindeki Log Analytics aracısını kullanarak müşterileri etkiler. Aracıyı eski bir işletim sistemi sürümünde (Windows 7, Windows Server 2008 R2 ve Windows Server 2008) çalıştırmadığınız takdirde değişiklik herhangi bir müşteri eylemi gerektirmez. Eski bir işletim sistemi sürümünde çalışan müşterilerin, 17 Ağustos 2020 tarihinden önce makinelerinde aşağıdaki işlemleri yapması gerekir veya aracıların Log Analytics çalışma alanlarına veri göndermeyi durduracaktır:

1. IŞLETIM sisteminiz için en son hizmet paketini yükler. Gerekli hizmet paketi sürümleri şunlardır:
    - Windows 7 SP1
    - Windows Server 2008 SP2
    - Windows Server 2008 R2 SP1

2. [Windows ve WSUS için 2019 SHA-2 kod Imzalama desteği gereksinimi](https://support.microsoft.com/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus) bölümünde açıklandığı gibi işletim SISTEMI için SHA-2 imzalama Windows güncelleştirmelerini yükler
3. Windows aracısının en son sürümüne güncelleştirin (sürüm 10.20.18029).
4. Aracıyı [TLS 1,2 kullanacak](agent-windows.md#configure-agent-to-use-tls-12)şekilde yapılandırmak için önerilir. 

## <a name="network-requirements"></a>Ağ gereksinimleri
Bkz. Windows Aracısı için ağ gereksinimleri için [Log Analytics aracısına genel bakış](log-analytics-agent.md#network-requirements) .


   
## <a name="configure-agent-to-use-tls-12"></a>Aracıyı TLS 1,2 kullanacak şekilde yapılandırma
[TLS 1,2](/windows-server/security/tls/tls-registry-settings#tls-12) protokolü, Windows aracısı ve Log Analytics hizmeti arasındaki iletişim için yoldaki verilerin güvenliğini güvence altına aldığından emin olun. [Varsayılan olarak tls 1,2 etkin olmayan bir işletim sistemine](data-security.md#sending-data-securely-using-tls-12)yüklüyorsanız, aşağıdaki ADıMLARı kullanarak TLS 1,2 ' yi yapılandırmanız gerekir.

1. Şu kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE \System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols**
2. TLS 1,2 için **protokoller** altında bir alt anahtar oluşturma **Hklm\system\currentcontrolset\control\securityproviders\schannel\protocols\tls 1,2**
3. Daha önce oluşturduğunuz TLS 1,2 protokol sürümü alt anahtarı altında bir **istemci** alt anahtarı oluşturun. Örneğin, **Hklm\system\currentcontrolset\control\securityproviders\schannel\protocols\tls 1.2 \ Client**.
4. **Hklm\system\currentcontrolset\control\securityproviders\schannel\protocols\tls 1.2 \ Client**altında aşağıdaki DWORD değerlerini oluşturun:

    * **Etkin** [değer = 1]
    * **DisabledByDefault** [değer = 0]  

Varsayılan olarak devre dışı olduğu gibi, .NET Framework 4,6 veya üstünü güvenli şifrelemeyi destekleyecek şekilde yapılandırın. [Güçlü şifreleme](/dotnet/framework/network-programming/tls#schusestrongcrypto) , TLS 1,2 gibi daha güvenli ağ protokolleri kullanır ve güvenli olmayan protokolleri engeller. 

1. Aşağıdaki kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ . NETFramework\v4.0.30319**.  
2. Bu alt anahtar altında **1**değeriyle **Schusestrongşifre** DWORD değeri oluşturun.  
3. Şu kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE \SOFTWARE\WOW6432Node\Microsoft \\ . NETFramework\v4.0.30319**.  
4. Bu alt anahtar altında **1**değeriyle **Schusestrongşifre** DWORD değeri oluşturun. 
5. Ayarların etkili olması için sistemi yeniden başlatın. 

## <a name="install-agent-using-setup-wizard"></a>Kurulum sihirbazını kullanarak aracı yükleme
Aşağıdaki adımlar, bilgisayarınızda aracı için Kurulum Sihirbazı 'nı kullanarak Azure ve Azure Kamu bulutundaki Log Analytics aracısını yükleyip yapılandırır. Aracıyı bir System Center Operations Manager yönetim grubuna da raporlamak üzere nasıl yapılandıracağınızı öğrenmek istiyorsanız, bkz. [Aracı Kurulum Sihirbazı ile Operations Manager aracısını dağıtma](/system-center/scom/manage-deploy-windows-agent-manually#to-deploy-the-operations-manager-agent-with-the-agent-setup-wizard).

1. Log Analytics çalışma alanınızda, daha önce gidilen **Windows sunucuları** sayfasında, Windows işletim sisteminin işlemci mimarisine bağlı olarak indirmek Için uygun **Windows aracısını indir** sürümünü seçin.   
2. Aracıyı bilgisayarınıza yüklemek için Kurulum'u çalıştırın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.
3. **Lisans Koşulları** sayfasında, lisansı okuyun ve **Kabul Ediyorum**'a tıklayın.
4. **Hedef Klasör** sayfasında, varsayılan yükleme klasörünü değiştirin veya koruyun ve ardından **İleri**'ye tıklayın.
5. **Aracı Kurulum Seçenekleri** sayfasında, aracıyı Azure Log Analytics'e bağlamayı seçin ve ardından **İleri**'ye tıklayın.   
6. **Azure Log Analytics** sayfasında aşağıdakileri yapın:
   1. Daha önce kopyaladığınız **Çalışma Alanı Kimliği** ve **Çalışma Alanı Anahtarı (Birincil Anahtar)** değerlerini yapıştırın.  Bilgisayarın Azure Kamu bulutundaki bir Log Analytics çalışma alanına raporlaması gerekiyorsa, **Azure Cloud** açılan listesinden **Azure ABD Kamu**'yu seçin.  
   2. Bilgisayarın Log Analytics hizmetiyle bir ara sunucu üzerinden iletişim kurması gerekiyorsa, **Gelişmiş**'e tıklayın ve ara sunucunun URL'siyle bağlantı noktası numarasını sağlayın.  Ara sunucunuz kimlik doğrulaması gerektiriyorsa, ara sunucuyla kimlik doğrulaması yapmak için kullanıcı adını ve parolayı yazın, ardından **İleri**'ye tıklayın.  
7. Gerekli yapılandırma ayarlarını sağlamayı tamamladığınızda **İleri**'ye tıklayın.<br><br> ![Çalışma Alanı Kimliği ve Birincil Anahtarı'nı yapıştırın](media/agent-windows/log-analytics-mma-setup-laworkspace.png)<br><br>
8. **Yüklemeye Hazır** sayfasında seçimlerinizi gözden geçirin ve ardından **Yükle**'ye tıklayın.
9. **Yapılandırma başarıyla tamamlandı** sayfasında **Son**'a tıklayın.

Tamamlandığında, **Denetim Masası**'nda **Microsoft Monitoring Agent** gösterilir. Log Analytics rapor ettiğini onaylamak için, [Log Analytics için aracı bağlantısını doğrula](#verify-agent-connectivity-to-azure-monitor)' yı gözden geçirin. 

## <a name="install-agent-using-command-line"></a>Komut satırını kullanarak aracı 'nı yükler
Aracının indirilen dosyası, kendi içinde bulunan bir yükleme paketidir.  Aracının ve destekleyici dosyaların kurulum programı pakette bulunur ve aşağıdaki örneklerde gösterilen komut satırı kullanılarak düzgün bir şekilde yüklenmek üzere ayıklanmalıdır.    

>[!NOTE]
>Bir aracıyı yükseltmek istiyorsanız, Log Analytics betik oluşturma API 'sini kullanmanız gerekir. Daha fazla bilgi için bkz. [Windows ve Linux için Log Analytics aracısını yönetme ve koruma](agent-manage.md) konusuna bakın.

Aşağıdaki tabloda, Automation DSC kullanılarak dağıtıldığında de dahil olmak üzere, aracının kurulumu tarafından desteklenen belirli parametreler vurgulanmıştır.

|MMA özgü seçenekler                   |Notlar         |
|---------------------------------------|--------------|
| NOAPM=1                               | İsteğe bağlı parametre. Aracıyı .NET uygulama performansı Izleme olmadan yüklenir.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = aracıyı bir çalışma alanına rapor verecek şekilde yapılandırma                |
|OPINSIGHTS_WORKSPACE_ID                | Eklenecek çalışma alanı KIMLIĞI (GUID)                    |
|OPINSIGHTS_WORKSPACE_KEY               | Çalışma alanı ile ilk kimlik doğrulaması için kullanılan çalışma alanı anahtarı |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Çalışma alanının bulunduğu bulut ortamını belirtin <br> 0 = Azure ticari bulutu (varsayılan) <br> 1 = Azure Kamu |
|OPINSIGHTS_PROXY_URL               | Kullanılacak proxy için URI |
|OPINSIGHTS_PROXY_USERNAME               | Kimliği doğrulanmış bir ara sunucuya erişmek için Kullanıcı adı |
|OPINSIGHTS_PROXY_PASSWORD               | Kimliği doğrulanmış bir ara sunucuya erişmek için parola |

1. Aracı yükleme dosyalarını ayıklamak için, yükseltilmiş bir komut isteminden çalıştırın `MMASetup-<platform>.exe /c` ve dosyaları ayıklama yolunu sorar.  Alternatif olarak, bağımsız değişkenleri geçirerek yolu belirtebilirsiniz `MMASetup-<platform>.exe /c /t:<Full Path>` .  
2. Aracıyı sessizce yüklemek ve Azure ticari bulutundaki bir çalışma alanına raporlamak üzere yapılandırmak için, kurulum dosyalarını ayıkladığınız klasörden şunu yazın: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```

   ya da aracıyı Azure ABD kamu bulutuna bildirilecek şekilde yapılandırmak için şunu yazın: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```
    >[!NOTE]
    >*OPINSIGHTS_WORKSPACE_ID* ve *OPINSIGHTS_WORKSPACE_KEY* parametrelerinin dize değerlerinin, paket için geçerli seçenekler olarak Windows Installer bildirmek üzere çift tırnaklı şekilde kapsüllenmesi gerekir. 

## <a name="install-agent-using-dsc-in-azure-automation"></a>Azure Otomasyonu 'nda DSC 'yi kullanarak aracı 'nı yükler

Aracıyı Azure Automation DSC kullanarak yüklemek için aşağıdaki betik örneğini kullanabilirsiniz.   Bir Otomasyon hesabınız yoksa, Automation DSC kullanmadan önce gerekli bir Otomasyon hesabı oluşturmaya yönelik gereksinimleri ve adımları anlamak için bkz. [Azure Otomasyonu ile çalışmaya başlama](../../automation/index.yml) .  Automation DSC hakkında bilgi sahibi değilseniz [Automation DSC kullanmaya](../../automation/automation-dsc-getting-started.md)başlama konusunu inceleyin.

Aşağıdaki örnek, değeri tarafından tanımlanan 64 bitlik aracıyı yüklenmektedir `URI` . Ayrıca, URI değerini değiştirerek 32 bit sürümünü de kullanabilirsiniz. Her iki sürümün URI 'Leri şunlardır:

- Windows 64-bit Aracısı- https://go.microsoft.com/fwlink/?LinkId=828603
- Windows 32-bit Aracısı- https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Bu yordam ve betik örneği, bir Windows bilgisayarına zaten dağıtılan aracının yükseltilmesini desteklemez.

Aracı paketinin 32-bit ve 64 bit sürümleri farklı ürün kodlarına sahiptir ve yayımlanan yeni sürümler de benzersiz bir değer içermelidir.  Ürün kodu, bir uygulamanın veya ürünün asıl kimliği olan ve Windows Installer **ProductCode** özelliği tarafından temsil EDILEN bir GUID 'dir.  `ProductId` **MMAgent.ps1** betikteki değerin ürün kodu 32-bit veya 64-bit aracı yükleyicisi paketinden eşleşmelidir.

Ürün kodunu aracı yüklemesi paketinden doğrudan almak için, Windows yazılım geliştirme seti 'nin bir bileşeni olan Windows Installer geliştiricilerin veya Microsoft değerli bir profesyonel (MVP) tarafından yazılmış [örnek bir betikten](https://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) sonra PowerShell kullanarak [Windows SDK bileşenlerinden](/windows/win32/msi/platform-sdk-components-for-windows-installer-developers) Orca.exe kullanabilirsiniz.  Her iki yaklaşım için, önce MMASetup yükleme paketinden **MOMagent.msi** dosyasını ayıklamanız gerekir.  Bu, [komut satırını kullanarak aracıyı Install](#install-agent-using-command-line)bölümünün altındaki ilk adımda gösterilmektedir.  

1. XPSDesiredStateConfiguration DSC modülünü [https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure Automation 'a aktarın.  
2.    *OPSINSIGHTS_WS_ID* ve *OPSINSIGHTS_WS_KEY*için Azure Otomasyonu değişken varlıkları oluşturun. *OPSINSIGHTS_WS_ID* Log Analytics çalışma alanı kimliğinize ayarlayın ve *OPSINSIGHTS_WS_KEY* çalışma alanınızın birincil anahtarına ayarlayın.
3.    Betiği kopyalayın ve MMAgent.ps1 olarak kaydedin.

```powershell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

    Import-DscResource -ModuleName xPSDesiredStateConfiguration
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}

```

4. `ProductId`Komut dosyasındaki değeri, daha önce önerilen yöntemleri kullanarak aracı yüklemesi paketinin en son sürümünden ayıklanan ürün koduyla güncelleştirin. 
5. [MMAgent.ps1 yapılandırma betiğini](../../automation/automation-dsc-getting-started.md#import-a-configuration-into-azure-automation) Otomasyon hesabınıza aktarın. 
6. Yapılandırmaya [bir Windows bilgisayarı veya düğüm atayın](../../automation/automation-dsc-getting-started.md#enable-an-azure-resource-manager-vm-for-management-with-state-configuration) . 15 dakika içinde düğüm yapılandırmasını denetler ve aracı düğümüne gönderilir.

## <a name="verify-agent-connectivity-to-azure-monitor"></a>Azure Izleyici ile aracı bağlantısını doğrulama

Aracının yüklenmesi tamamlandıktan sonra, başarılı bir şekilde bağlandığının doğrulanması ve raporlama iki şekilde gerçekleştirilebilir.  

Bilgisayarın **Denetim Masası** sayfasında **Microsoft Monitoring Agent**'ı bulun.  Bunu seçin ve **Azure Log Analytics** sekmesinde aracı şöyle bir ileti görüntülemelidir: **Microsoft Monitoring Agent Microsoft Operations Management Suite hizmetine başarıyla bağlandı.**<br><br> ![MMA'nın Log Analytics'e bağlantı durumu](media/agent-windows/log-analytics-mma-laworkspace-status.png)

Azure portal basit bir günlük sorgusu da gerçekleştirebilirsiniz.  

1. Azure portal, **izleme**' yi arayıp seçin.
1. Menüde **Günlükler** ' i seçin.
1. **Günlükler** bölmesinde, sorgu alanına şunu yazın:  

    ```
    Heartbeat 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

Döndürülen arama sonuçlarında, bağlı olduğunu ve hizmete raporlanmasını belirten bilgisayar için sinyal kayıtlarını görmeniz gerekir.

## <a name="cache-information"></a>Önbellek bilgileri

Log Analytics aracıdan alınan veriler, Azure Izleyici 'ye gönderilmeden önce *C:\Program Files\Microsoft Monitoring It T\tors T\sağlık hizmeti durumunda* yerel makinede önbelleğe alınır. Aracı 20 saniyede bir karşıya yüklemeye çalışır. Başarısız olursa, işlem başarılı olana kadar üstel olarak zaman uzunluğu artacaktır. İkinci denemeden önce 30 saniye, bir sonraki, 120 saniye öncesi ve bu şekilde yeniden bağlantı başarıyla bağlanana kadar yeniden denemeler arasında 8,5 en fazla 60 saat sürer. Bu bekleme süresi, tüm aracıların eşzamanlı olarak bağlantı kurmaya engel olmak için biraz rasgeledir. En büyük arabelleğe ulaşıldığında en eski veriler atılır.

Varsayılan önbellek boyutu 50 MB 'tır, ancak en az 5 MB ve en fazla 1,5 GB arasında yapılandırılabilir. Kayıt defteri anahtarında depolanır *HKEY_LOCAL_MACHINE \System\currentcontrolset\services\healthservice\parameters\kalıcılık önbelleği en yüksek*. Değer, sayfa başına 8 KB ile sayfa sayısını temsil eder.


## <a name="next-steps"></a>Sonraki adımlar

- Aracıyı sanal makineden yeniden yapılandırma, yükseltme veya kaldırma hakkında bilgi edinmek için [Windows ve Linux için Log Analytics aracısının yönetimini ve güvenliğini](agent-manage.md) gözden geçirin.

- Aracıyı yüklerken veya yönetirken sorunlarla karşılaşırsanız [Windows Agent sorunlarını giderme](agent-windows-troubleshoot.md) konusunu gözden geçirin.
