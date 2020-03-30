---
title: Azure AD Connect bulut sağlama sorun giderme
description: Bu makalede, bulut sağlama aracısıyla ortaya çıkabilecek sorunları nasıl giderikarşılayacakaçık.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e41be4b76245f2567015eb0ede317830120ee61a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75549494"
---
# <a name="cloud-provisioning-troubleshooting"></a>Bulut sağlama sorun giderme

Bulut sağlama birçok farklı şeye dokunur ve birçok farklı bağımlılıkları vardır. Bu geniş kapsam çeşitli sorunlara yol açabilir. Bu makale, bu sorunları gidermenize yardımcı olur. Odaklanmanız gereken tipik alanları, ek bilgileri nasıl toplayacaklarını ve sorunları izlemek için kullanabileceğiniz çeşitli teknikleri tanıtır.


## <a name="common-troubleshooting-areas"></a>Sık karşılaşılan sorun giderme alanları

|Adı|Açıklama|
|-----|-----|
|[Aracı sorunları](#agent-problems)|Aracının doğru yüklendiğinden ve Azure Etkin Dizini (Azure AD) ile iletişim kurduğunu doğrulayın.|
|[Nesne eşitleme sorunları](#object-synchronization-problems)|Nesne eşitleme sorunlarını gidermek için sağlama günlüklerini kullanın.|
|[Karantinaya alınmış sorunların sağlanması](#provisioning-quarantined-problems)|Karantina sorunlarını ve bunları nasıl düzelteceklerini anlayın.|


## <a name="agent-problems"></a>Aracı sorunları
Aracıyla doğrulamak istediğiniz ilk şeylerden bazıları şunlardır:

-  Yüklü mü?
-  Ajan yerel olarak çalışıyor mu?
-  Ajan geçitte mi?
-  Ajan sağlıklı olarak işaretlenmiş mi?

Bu öğeler Azure portalında ve aracıyı çalıştıran yerel sunucuda doğrulanabilir.

### <a name="azure-portal-agent-verification"></a>Azure portal aracısı doğrulaması

Aracının Azure tarafından görüldüğünü ve sağlıklı olduğunu doğrulamak için aşağıdaki adımları izleyin.

1. Azure Portal’da oturum açın.
1. Solda Azure **Active Directory** > Azure**AD Connect'i**seçin. Merkezde, **hükmü yönet (önizleme) seçeneğini**belirleyin.
1. Azure **AD Sağlama (önizleme)** ekranında **tüm aracıları gözden geçir'i**seçin.

   ![Tüm aracıları inceleyin](media/how-to-install/install7.png)</br>
 
1. Şirket **içi sağlama aracıları** ekranında, yüklediğiniz aracıları görürsünüz. Söz konusu aracının orada olduğunu ve *Sağlıklı*olarak işaretlendiğini doğrulayın.

   ![Şirket içi sağlama aracıları ekranı](media/how-to-install/install8.png)</br>

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama

Azure'un port 443'te dinlediğini ve aracınızın bu bağlantı noktasıyla iletişim kurabileceğini doğrulamak için aşağıdaki aracı kullanın:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Bu test, aracılarınızın 443 portu üzerinden Azure ile iletişim kurabileceğini doğrular. Bir tarayıcı açın ve aracının yüklü olduğu sunucudan önceki URL'ye gidin.

![Port ulaşılabilirliğinin doğrulanması](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Yerel sunucuda

Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. Aracı yüklü sunucuda, hizmetleri açarak ya ona yönlendirerek ya da **Start** > **Run** > **Services.msc'ye**giderek **.**
1. **Hizmetler**altında, **Microsoft Azure AD Connect Agent Updater** ve **Microsoft Azure AD Connect Provisioning Agent'ın** orada olduğundan ve durumlarının *çalışmadığından*emin olun.

   ![Hizmetler ekranı](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-problems"></a>Ortak aracı yükleme sorunları

Aşağıdaki bölümlerde bazı ortak aracı yükleme sorunları ve tipik çözümler açıklayınız.

#### <a name="agent-failed-to-start"></a>Aracı başlatılamamış

Şu durumları belirten bir hata iletisi alabilirsiniz:

**'Microsoft Azure AD Connect Provisioning Aracısı' hizmeti başlatılamamış. Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın.** 

Bu sorun genellikle, yüklemeci (NT SERVICE\AADConnectProvisioningAgent) tarafından oluşturulan yerel NT Hizmeti oturum açma hesabına uygulanmasını engelleyen bir grup ilkesinden kaynaklanır. Bu izinlerin hizmeti başlatmak için gerekli olması gerekir.

Bu sorunu gidermek için aşağıdaki adımları izleyin.

1. Yönetici hesabıyla sunucuda oturum açın.
1. **Hizmetleri** aç, ona yönlendirerek veya **Başlat** > **Hizmetleri.msc'ye****Run** > giderek.
1. **Hizmetler**altında, **Microsoft Azure AD Connect Provisioning**Aracısı'nı çift tıklatın.
1. Oturum **Açma** sekmesinde, **bu hesabı** bir etki alanı yöneticisi olarak değiştirin. Ardından hizmeti yeniden başlatın. 

   ![Giriş Sekmesi](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>Aracı nın süreleri veya sertifikası geçersiz

Aracıyı kaydetmeye çalıştığınızda aşağıdaki hata iletisini alabilirsiniz.

![Zaman-out hata iletisi](media/how-to-troubleshoot/troubleshoot4.png)

Bu sorun genellikle aracının Karma Kimlik Hizmeti'ne bağlanamamasından kaynaklanır ve bir HTTP proxy'sini yapılandırmanızı gerektirir. Bu sorunu gidermek için giden proxy'yi yapılandırın. 

Geçici aracı giden bir proxy kullanımını destekler. Aracı config dosyasını düzenleyerek yapılandırabilirsiniz *C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config*. Kapanış `</configuration>` etiketinden hemen önce dosyanın sonuna doğru aşağıdaki satırları ekleyin.
Değişkenleri `[proxy-server]` ve `[proxy-port]` proxy sunucu adınız ve bağlantı noktası değerlerinizle değiştirin.

```xml
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
                usesystemdefault="true"
                proxyaddress="http://[proxy-server]:[proxy-port]"
                bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

#### <a name="agent-registration-fails-with-security-error"></a>Aracı kaydı güvenlik hatasıyla başarısız oldu

Bulut sağlama aracısını yüklediğinizde bir hata iletisi alabilirsiniz.

Bu sorun genellikle aracının yerel PowerShell yürütme ilkeleri nedeniyle PowerShell kayıt komut dosyalarını çalıştıramamasını neden olur.

Bu sorunu gidermek için sunucudaki PowerShell yürütme ilkelerini değiştirin. Makine ve Kullanıcı ilkelerinin *Tanımsız* veya *Uzaktan İmzalı*olarak ayarlanmış olması gerekir. *Sınırsız*olarak ayarlanırlarsa, bu hatayı görürsünüz. Daha fazla bilgi için [PowerShell yürütme politikalarına](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6)bakın. 

### <a name="log-files"></a>Günlük dosyaları

Varsayılan olarak, aracı en az hata iletileri yayar ve izleme bilgilerini yığını. Bu izleme günlüklerini *C:\ProgramData\Microsoft\Azure AD Connect Provisioning Agent\Trace*klasöründe bulabilirsiniz.

Sorun giderme aracısı ile ilgili sorunlar için ek ayrıntılar toplamak için aşağıdaki adımları izleyin.

1. Hizmeti durdurun **Microsoft Azure AD Connect Provisioning Aracısı.**
1. Özgün config dosyasının bir kopyasını oluşturun: *C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\AADConnectProvisioningAgent.exe.config*.
1. Varolan `<system.diagnostics>` bölümü aşağıdakilerle değiştirin ve tüm izleme iletileri *ProvAgentTrace.log*dosyasına gider.

   ```xml
     <system.diagnostics>
         <sources>
         <source name="AAD Connect Provisioning Agent">
             <listeners>
             <add name="console"/>
             <add name="etw"/>
             <add name="textWriterListener"/>
             </listeners>
         </source>
         </sources>
         <sharedListeners>
         <add name="console" type="System.Diagnostics.ConsoleTraceListener" initializeData="false"/>
         <add name="etw" type="System.Diagnostics.EventLogTraceListener" initializeData="Azure AD Connect Provisioning Agent">
             <filter type="System.Diagnostics.EventTypeFilter" initializeData="All"/>
         </add>
         <add name="textWriterListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log"/>
         </sharedListeners>
     </system.diagnostics>
    
   ```
1. Hizmeti **başlatın Microsoft Azure AD Connect Provisioning Agent.**
1. Dosyayı takip etmek ve sorunları hata ayıklamak için aşağıdaki komutu kullanın. 
    ```
    Get-Content “C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log” -Wait
    ```
## <a name="object-synchronization-problems"></a>Nesne eşitleme sorunları

Aşağıdaki bölümde sorun giderme nesnesi eşitleme hakkında bilgi içerir.

### <a name="provisioning-logs"></a>Sağlama günlükleri

Azure portalında, madde eşitleme sorunlarını izleme ve sorun giderme de yardımcı olmak için günlükleri sağlama kullanılabilir. Günlükleri görüntülemek için **Günlükler'i**seçin.

![Günlükler düğmesi](media/how-to-troubleshoot/log1.png)

Günlükleri sağlama, şirket içi Active Directory ortamınız ile Azure arasında eşitlenen nesnelerin durumu hakkında zengin bir bilgi sağlar.

![Günlükleri sağlama ekranı](media/how-to-troubleshoot/log2.png)

Sayfanın üst kısmındaki açılır kutuları kullanarak görünümü tarihler gibi belirli sorunlarda sıfıra filtreleyebilirsiniz. Ek bilgileri görmek için tek bir olayı çift tıklatın.

![Günlükleri açılır kutu bilgilerini sağlama](media/how-to-troubleshoot/log3.png)

Bu bilgiler ayrıntılı adımlar sağlar ve eşitleme sorununun nerede oluştuğunu. Bu şekilde, sorunun tam yerini belirleyebilirsiniz.


## <a name="provisioning-quarantined-problems"></a>Karantinaya alınmış sorunların sağlanması

Bulut sağlama yapılandırmanızın durumunu izler ve sağlıksız nesneleri karantina durumuna yerleştirir. Hedef sisteme karşı yapılan çağrıların çoğu veya tümü bir hata nedeniyle sürekli olarak başarısız olursa, örneğin geçersiz yönetici kimlik bilgileri, sağlama işi karantinaolarak işaretlenir.

![Karantina durumu](media/how-to-troubleshoot/quarantine1.png)

Durumu seçerek, karantina hakkında ek bilgiler görebilirsiniz. Hata kodunu ve iletiyi de edinebilirsiniz.

![Karantina durum bilgileri](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolve-a-quarantine"></a>Karantinayı çözme

- Sağlama işini yeniden başlatmak için Azure portalını kullanın. Aracı yapılandırma sayfasında, **yeniden başlat'ı'nı**seçin.

  ![Sağlamayı yeniden başlatma](media/how-to-troubleshoot/quarantine3.png)

- Sağlama işini yeniden başlatmak için Microsoft [Graph'ı](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http)kullanın. Yeniden başlatacağınız şey üzerinde tam kontrole sahip olacaksınız. Temizlemeyi seçebilirsiniz:
  - Escrow, karantina durumuna doğru tahakkuk eden emanet sayacını yeniden başlatmak için.
  - Karantina, uygulamayı karantinadan çıkarmak için.
  - Filigran. 
  
  Aşağıdaki isteği kullanın:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)



