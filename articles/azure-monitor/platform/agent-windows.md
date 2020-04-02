---
title: Windows bilgisayarlarını Azure Monitör'e bağlayın | Microsoft Dokümanlar
description: Bu makalede, diğer bulutlarda veya şirket içinde barındırılan Windows bilgisayarlarının Windows için Log Analytics aracısıyla Azure Monitor'a nasıl bağlanılacak olduğu açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/07/2019
ms.openlocfilehash: 65a6f51d0eef28ea33adcc755d3d51f1e06a5341
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528334"
---
# <a name="connect-windows-computers-to-azure-monitor"></a>Windows bilgisayarlarını Azure Monitörüne bağlama

Azure Monitor ile yerel veri merkezinizdeki veya diğer bulut ortamınızdaki sanal makineleri veya fiziksel bilgisayarları izlemek ve yönetmek için Log Analytics aracısını (Microsoft İzleme Aracısı (MMA) olarak da adlandırılır) dağıtmanız ve bir veya daha fazla Log Analytics çalışma alanıyla rapor edecek şekilde yapılandırmanız gerekir. Aracı, Azure Otomasyonu için Karma Runbook Worker rolünü de destekler.  

İzlenen bir Windows bilgisayarında, aracı Microsoft İzleme Aracısı hizmeti olarak listelenir. Microsoft İzleme Aracısı hizmeti, günlük dosyaları ve Windows olay günlüğü, performans verileri ve diğer telemetrilerden olayları toplar. Aracı, bildirdiği Azure Monitor ile iletişim kuramamasına rağmen, aracı çalıştırmaya devam eder ve toplanan verileri izlenen bilgisayarın diskinde sıraya alır. Bağlantı geri yüklendiğinde, Microsoft İzleme Aracısı hizmeti toplanan verileri hizmete gönderir.

Aracı aşağıdaki yöntemlerden biri kullanılarak yüklenebilir. Çoğu yüklemede, farklı bilgisayar gruplarını uygun şekilde yüklemek için bu yöntemlerin bir bileşimi kullanılır.  Her yöntemin kullanımıyla ilgili ayrıntılar daha sonra makalede verilmiştir.

* El ile yükleme. Kurulum, kurulum sihirbazı kullanılarak, komut satırından bilgisayarda el ile çalıştırılır veya varolan bir yazılım dağıtım aracı kullanılarak dağıtılır.
* Azure Otomasyonu İstenilen Durum Yapılandırması (DSC). Azure Otomasyonu'nda DSC'yi, ortamınızda zaten dağıtılmış olan Windows bilgisayarlar için bir komut dosyasıyla kullanma.  
* PowerShell komut dosyası.
* Azure Yığını'nda Windows'u şirket içinde çalıştıran sanal makineler için Kaynak Yöneticisi şablonu. 

>[!NOTE]
>Azure Güvenlik Merkezi (ASC), Microsoft İzleme Aracısı'na (Log Analytics Windows aracısı olarak da adlandırılır) bağlıdır ve dağıtımının bir parçası olarak log analytics çalışma alanına rapor verecek şekilde yükler ve yapılandırır. ASC, Log Analytics Windows aracısının aboneliğinizdeki tüm VM'lere otomatik olarak yüklenmesini sağlayan ve bunu belirli bir çalışma alanına rapor layacak şekilde yapılandıran otomatik bir sağlama seçeneği içerir. Bu seçenek hakkında daha fazla bilgi için [bkz.](../../security-center/security-center-enable-data-collection.md#auto-provision-mma)
>

Aracıyı birden fazla çalışma alanına rapor yapacak şekilde yapılandırmanız gerekiyorsa, bu işlem ilk kurulum sırasında, yalnızca daha sonra [bir çalışma alanı ekleme veya kaldırmada](agent-manage.md#adding-or-removing-a-workspace)açıklandığı gibi Denetim Masası veya PowerShell'den ayarları güncelleştirerek gerçekleştirilemez.  

Desteklenen yapılandırmayı anlamak için [desteklenen Windows işletim sistemlerini](log-analytics-agent.md#supported-windows-operating-systems) ve [ağ güvenlik duvarı yapılandırmasını](log-analytics-agent.md#firewall-requirements) inceleyin.

## <a name="obtain-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma
Windows için Log Analytics aracısını yüklemeden önce, Log Analytics çalışma alanınız için çalışma alanı kimliğine ve anahtarına ihtiyacınız vardır.  Aracıyı düzgün bir şekilde yapılandırmak ve Azure ticari ve ABD Devlet bulutu içinde Azure Monitor ile başarılı bir şekilde iletişim kurabilmesini sağlamak için bu bilgiler her yükleme yönteminden kurulum sırasında gereklidir. 

1. Azure **portalında, Log Analytics çalışma alanlarını**arayın ve seçin.
2. Log Analytics çalışma alanları listenizde, bildirilecek aracı yapılandırmak istediğiniz çalışma alanını seçin.
3. **Gelişmiş ayarlar**’ı seçin.<br><br> ![Log Analytics Gelişmiş Ayarlar](media/agent-windows/log-analytics-advanced-settings-01.png)<br><br>  
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Windows Sunucuları**’nı seçin.   
5. En sevdiğiniz düzenleyiciye, Çalışma **Alanı Kimliği'ne** ve **Birincil Anahtara**kopyalayıp yapıştırın.    
   
## <a name="configure-agent-to-use-tls-12"></a>TLS 1.2 kullanmak üzere Aracıyı yapılandır
Windows aracısı ile Log Analytics hizmeti arasındaki iletişim için [TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12) protokolünün kullanımını yapılandırmak için, aracısanal makineye yüklenmeden önce veya sonrasında etkinleştirmek için aşağıdaki adımları izleyebilirsiniz.

>[!NOTE]
>TLS 1.2'yi kullanacak şekilde Windows Server 2008 SP2 x64 çalıştıran bir VM yapılandırıyorsanız, aşağıdaki adımları gerçekleştirmeden önce aşağıdaki [SHA-2 kod imzalama destek güncelleştirmesini](https://support.microsoft.com/help/4474419/sha-2-code-signing-support-update) yüklemeniz gerekir. 
>

1. Aşağıdaki kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols**
2. TLS 1.2 **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2** için **Protokoller** altında bir alt anahtar oluşturma
3. Daha önce oluşturduğunuz TLS 1.2 protokol sürüm alt anahtarının altında bir **İstemci** alt anahtarı oluşturun. Örneğin, **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**.
4. **HKLM\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client**altında aşağıdaki DWORD değerlerini oluşturun:

    * **Etkin** [Değer = 1]
    * **Devre Dışı Bırakma [Değer** = 0]  

Varsayılan olarak devre dışı bırakıldığı gibi güvenli şifrelemeyi desteklemek için .NET Framework 4.6 veya sonraki yapılatınız. [Güçlü şifreleme](https://docs.microsoft.com/dotnet/framework/network-programming/tls#schusestrongcrypto) TLS 1.2 gibi daha güvenli ağ protokolleri kullanır ve güvenli olmayan protokolleri engeller. 

1. Aşağıdaki kayıt defteri alt anahtarını bulun: **\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft . NETFramework\v4.0.30319**.  
2. **1**değeri ile bu alt anahtar altında DWORD değeri **SchUseStrongCrypto** oluşturun.  
3. Aşağıdaki kayıt defteri alt anahtarını bulun: **HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Düğüm\Microsoft\\. NETFramework\v4.0.30319**.  
4. **1**değeri ile bu alt anahtar altında DWORD değeri **SchUseStrongCrypto** oluşturun. 
5. Ayarların etkili olması için sistemi yeniden başlatın. 

## <a name="install-the-agent-using-setup-wizard"></a>Kurulum sihirbazını kullanarak aracıyı yükleme
Aşağıdaki adımlar, bilgisayarınızdaki aracının kurulum sihirbazını kullanarak Azure ve Azure Genel bulutundaki Log Analytics aracısını yükler ve yapılandırır. Aracıyı bir Sistem Merkezi Operasyon Yöneticisi yönetim grubuna da rapor etmek üzere nasıl yapılandırabileceğinizi öğrenmek istiyorsanız, [Operasyon Yöneticisi aracısını Aracı Kurulum Sihirbazı ile dağıtma'ya](https://docs.microsoft.com/system-center/scom/manage-deploy-windows-agent-manually#to-deploy-the-operations-manager-agent-with-the-agent-setup-wizard)bakın.

1. Log Analytics çalışma alanınızda, daha önce gezindiğiniz **Windows Sunucuları** sayfasından, Windows işletim sisteminin işlemci mimarisine bağlı olarak indirilecek uygun **Windows Agent** sürümünü seçin.   
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

Tamamlandığında, **Denetim Masası**'nda **Microsoft Monitoring Agent** gösterilir. Log Analytics'e rapor bildirdiğini doğrulamak [için, Log Analytics'e aracı bağlantısını doğrula'yı](#verify-agent-connectivity-to-log-analytics)gözden geçirin. 

## <a name="install-the-agent-using-the-command-line"></a>Komut satırını kullanarak aracıyı yükleme
Aracı için indirilen dosya, bağımsız bir yükleme paketidir.  Aracı ve destekleyici dosyaların kurulum programı pakette bulunur ve aşağıdaki örneklerde gösterilen komut satırı kullanılarak düzgün bir şekilde yüklenebilmek için ayıklanması gerekir.    

>[!NOTE]
>Bir aracıyı yükseltmek istiyorsanız, Log Analytics komut dosyası API'sini kullanmanız gerekir. Daha fazla bilgi için [Windows ve Linux için Log Analytics aracısını yönetme ve koruma](agent-manage.md) konusuna bakın.

Aşağıdaki tablo, Otomasyon DSC kullanılarak dağıtıldığında da dahil olmak üzere aracı için kurulum tarafından desteklenen belirli parametreleri vurgular.

|MMA'ya özel seçenekler                   |Notlar         |
|---------------------------------------|--------------|
| NOAPM=1                               | İsteğe bağlı parametre. Aracıyı .NET Uygulama Performans İzleme olmadan yükler.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Aracıyı çalışma alanına rapor etmek üzere yapılandır                |
|OPINSIGHTS_WORKSPACE_ID                | Çalışma alanının eklenmesi için çalışma alanı kimliği (guid)                    |
|OPINSIGHTS_WORKSPACE_KEY               | Çalışma alanıyla birlikte başlangıçta kimlik doğrulaması yapmak için kullanılan çalışma alanı anahtarı |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Çalışma alanının bulunduğu bulut ortamını belirtin <br> 0 = Azure ticari bulutu (varsayılan) <br> 1 = Azure Yönetimi |
|OPINSIGHTS_PROXY_URL               | Proxy kullanmak için URI |
|OPINSIGHTS_PROXY_USERNAME               | Kimlik doğrulaması verilen bir proxy'ye erişmek için kullanıcı adı |
|OPINSIGHTS_PROXY_PASSWORD               | Kimlik doğrulaması verilen proxy'ye erişmek için parola |

1. Aracı yükleme dosyaları ayıklamak için, yükseltilmiş `MMASetup-<platform>.exe /c` bir komut istemi çalıştırın ve dosyaları ayıklamak için yol için ister.  Alternatif olarak, bağımsız değişkenleri `MMASetup-<platform>.exe /c /t:<Full Path>`geçerek yolu belirtebilirsiniz.  
2. Aracıyı sessizce yüklemek ve yükleme dosyalarını ayıkladığınız klasörden Azure ticari bulutundaki bir çalışma alanına rapor verecek şekilde yapılandırmak için: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```

   veya aracıyı Azure ABD Devlet bulutuna rapor edecek şekilde yapılandırmak için şunları yazın: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID="<your workspace ID>" OPINSIGHTS_WORKSPACE_KEY="<your workspace key>" AcceptEndUserLicenseAgreement=1
    ```
    >[!NOTE]
    >*OPINSIGHTS_WORKSPACE_ID* ve *OPINSIGHTS_WORKSPACE_KEY* parametreleri için dize değerlerinin, Windows Installer'a paket için geçerli seçenekler olarak dahil etmesini sağlamak için çift tırnak içinde kapsüllenmesi gerekir. 

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Azure Otomasyonu'nda DSC kullanarak aracıyı yükleme

Aracıyı Azure Automation DSC'yi kullanarak yüklemek için aşağıdaki komut dosyası örneğini kullanabilirsiniz.   Bir Otomasyon hesabınız yoksa, Automation DSC'yi kullanmadan önce gerekli bir Otomasyon hesabı oluşturma nın gereklerini ve adımları anlamak için [Azure Otomasyonu'na başlayın'a](/azure/automation/) bakın.  Otomasyon DSC'ye aşina değilseniz, [Otomasyon DSC ile başlarken gözden geçirin.](../../automation/automation-dsc-getting-started.md)

Aşağıdaki örnek, değerle tanımlanan 64 bit `URI` aracıyı yükler. URI değerini değiştirerek 32 bit sürümü de kullanabilirsiniz. Her iki sürüm için URI'ler şunlardır:

- Windows 64-bit aracı -https://go.microsoft.com/fwlink/?LinkId=828603
- Windows 32-bit aracı -https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Bu yordam ve komut dosyası örneği, windows bilgisayarına zaten dağıtılan aracıyı yükseltmeyi desteklemez.

Aracı paketinin 32 bit ve 64 bit sürümleri farklı ürün kodlarına sahiptir ve yeni sürümler de benzersiz bir değere sahiptir.  Ürün kodu, bir uygulamanın veya ürünün temel tanımlaması olan ve Windows Installer **ProductCode** özelliği tarafından temsil edilen bir GUID'dir.  `ProductId` **MMAgent.ps1** komut dosyasındaki değer, 32 bit veya 64 bit aracı yükleyici paketindeki ürün koduyla eşleşmeli.

Ürün kodunu doğrudan aracı yükleme paketinden almak için, Windows Yazılım Geliştirme Kiti'nin bir bileşeni olan [Windows Installer Geliştiricileri için Windows SDK Bileşenleri'nden](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx) Orca.exe'yi veya Microsoft Değerli Profesyonel (MVP) tarafından yazılmış örnek bir [komut dosyasının](https://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) ardından PowerShell'i kullanabilirsiniz.  Her iki yaklaşım için de öncelikle **MOMagent.msi** dosyasını MMASetup yükleme paketinden çıkarmanız gerekir.  Bu daha önce ilk adımda [komut satırını kullanarak aracıyı yükleyin](#install-the-agent-using-the-command-line)bölümünde gösterilir.  

1. xPSDesiredStateConfiguration DSC Modül'ünden [https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) Azure Otomasyonuna aktarın.  
1. *OPSINSIGHTS_WS_ID* ve *OPSINSIGHTS_WS_KEY*için Azure Otomasyon değişken varlıkları oluşturun. Log Analytics çalışma alanı kimliğinize *OPSINSIGHTS_WS_ID* ayarlayın ve *OPSINSIGHTS_WS_KEY* çalışma alanınızın birincil anahtarına ayarlayın.
1. Komut dosyasını kopyalayın ve MMAgent.ps1 olarak kaydedin.

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
               Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + '      OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
               DependsOn = "[xRemoteFile]OIPackage"
           }
       }
   }

   ```

4. Daha `ProductId` önce önerilen yöntemleri kullanarak aracı yükleme paketinin en son sürümünden çıkarılan ürün koduyla komut dosyasındaki değeri güncelleştirin. 
5. [MMAgent.ps1 yapılandırma komut dosyasını](../../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) Otomasyon hesabınıza aktarın. 
5. Yapılandırmaya [bir Windows bilgisayarı veya düğüm atayın.](../../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration) 15 dakika içinde, düğüm yapılandırmasını denetler ve aracı düğüme itilir.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Oturum Analizi'ne aracı bağlantısını doğrulayın

Aracının yüklenmesi tamamlandıktan sonra, başarıyla bağlandığını doğrulamak ve raporlama iki şekilde gerçekleştirilebilir.  

Bilgisayarın **Denetim Masası** sayfasında **Microsoft Monitoring Agent**'ı bulun.  Seçin ve **Azure Log Analytics** sekmesinde aracı, aşağıdakileri belirten bir ileti görüntülemelidir: **Microsoft İzleme Aracısı Microsoft Operations Management Suite hizmetine başarıyla bağlanmıştır.**<br><br> ![MMA'nın Log Analytics'e bağlantı durumu](media/agent-windows/log-analytics-mma-laworkspace-status.png)

Azure portalında basit bir günlük sorgusu da gerçekleştirebilirsiniz.  

1. Azure **portalında, Monitör'ü**arayın ve seçin.
1. Menüde **Günlükler'i** seçin.
1. **Günlükler** bölmesinde, sorgu alanı türünde:  

    ```
    Heartbeat 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

Döndürülen arama sonuçlarında, bilgisayarın bağlı olduğunu belirten ve hizmete rapor veren sinyal kayıtlarını görmeniz gerekir.   

## <a name="next-steps"></a>Sonraki adımlar

- Aracıyı sanal makineden nasıl yeniden yapılandırılabildiğini, yükseltecek veya kaldıracağız hakkında bilgi edinmek [için Windows ve Linux için Log Analytics aracısını yönetme](agent-manage.md) ve bakımını gözden geçirin.

- Aracıyı yüklerken veya yönetirken sorunlarla karşılaşırsanız [Windows aracısını](agent-windows-troubleshoot.md) gözden geçirin.
