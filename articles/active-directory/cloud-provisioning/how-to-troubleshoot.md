---
title: Azure AD Connect bulut sağlama sorunlarını giderme
description: Bu makalede, bulut sağlama aracısında ortaya çıkabilecek sorunları nasıl giderebileceğiniz açıklanır.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: how-to
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 34796a435536a48100b7434ed5267802cd2d549f
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89226956"
---
# <a name="cloud-provisioning-troubleshooting"></a>Bulut sağlama sorunlarını giderme

Bulut sağlama birçok farklı şeyi dokunduğunda birçok farklı bağımlılığı vardır. Bu geniş kapsamlı kapsam, çeşitli sorunlara göre daha fazla sorun verebilir. Bu makale, bu sorunları gidermenize yardımcı olur. Üzerinde odaklanmanız, ek bilgilerin toplanması ve sorunları izlemek için kullanabileceğiniz çeşitli teknikler için tipik alanlara odaklanırsınız.


## <a name="common-troubleshooting-areas"></a>Ortak sorun giderme alanı

|Ad|Açıklama|
|-----|-----|
|[Aracı sorunları](#agent-problems)|Aracının doğru şekilde yüklendiğini ve Azure Active Directory (Azure AD) ile iletişim kuracağını doğrulayın.|
|[Nesne eşitleme sorunları](#object-synchronization-problems)|Nesne eşitleme sorunlarını gidermek için sağlama günlüklerini kullanın.|
|[Karantinaya alınan sorunları sağlama](#provisioning-quarantined-problems)|Karantina sorunlarını sağlamayı ve bunları nasıl düzelteceğinizi anlayın.|


## <a name="agent-problems"></a>Aracı sorunları
Aracıda doğrulamak istediğiniz ilk şey şunlardır:

-  Mi yüklendi?
-  Aracı yerel olarak çalışıyor mu?
-  Aracı portalda mı?
-  Aracı sağlıklı olarak işaretlendi mi?

Bu öğeler Azure portal ve aracıyı çalıştıran yerel sunucu üzerinde doğrulanabilir.

### <a name="azure-portal-agent-verification"></a>Aracı doğrulama Azure portal

Aracının Azure tarafından görüleceği ve sağlıklı olduğundan emin olmak için aşağıdaki adımları izleyin.

1. Azure portalında oturum açın.
1. Sol tarafta **Azure Active Directory**  >  **Azure AD Connect**' yı seçin. Merkezinde **Yönet sağlama (Önizleme)** öğesini seçin.
1. **Azure AD sağlama (Önizleme)** ekranında **tüm aracıları gözden geçir**' i seçin.

   ![Tüm aracıları gözden geçir](media/how-to-install/install7.png)</br>
 
1. **Şirket içi sağlama aracıları** ekranında, yüklediğiniz aracıları görürsünüz. Söz konusu aracının orada olduğunu ve *sağlıklı*olarak işaretlendiğinden emin olun.

   ![Şirket içi sağlama aracıları ekranı](media/how-to-install/install8.png)</br>

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama

Azure 'un 443 numaralı bağlantı noktasını dinlediğini ve aracınızın onunla iletişim kurabildiğini doğrulayın. 

Bu test, aracılarınızın 443 numaralı bağlantı noktası üzerinden Azure ile iletişim kurabildiğini doğrular. Bir tarayıcı açın ve aracının yüklendiği sunucudan önceki URL 'ye gidin.

![Bağlantı noktası erişilebilirliği doğrulaması](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Yerel sunucuda

Aracının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. Aracının yüklü olduğu sunucuda, bu hizmete giderek veya **Services** **Start**  >  **Run**  >  **Hizmetleri Çalıştır. msc**' yi Başlat ' a giderek hizmetler ' i açın.
1. **Hizmetler**' in altında **Microsoft Azure AD aracı güncelleştiricisi bağlama** ve **Microsoft Azure AD Connect sağlama aracısının** aynı olduğundan ve durumlarının *çalıştığından*emin olun.

   ![Hizmetler ekranı](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-problems"></a>Ortak aracı yükleme sorunları

Aşağıdaki bölümlerde bazı yaygın aracı yükleme sorunları ve tipik çözümler açıklanır.

#### <a name="agent-failed-to-start"></a>Aracı başlatılamadı

Aşağıdakileri belirten bir hata iletisi alabilirsiniz:

**' Microsoft Azure AD Connect sağlama Aracısı ' hizmeti başlatılamadı. Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın.** 

Bu sorun genellikle izinlerin yükleyicinin (NT SERVICE\AADConnectProvisioningAgent) tarafından oluşturulan yerel NT hizmeti oturum açma hesabına uygulanmasını önleyen bir grup ilkesi nedeniyle oluşur. Hizmeti başlatmak için bu izinler gereklidir.

Bu sorunu çözmek için aşağıdaki adımları izleyin.

1. Sunucuda yönetici hesabıyla oturum açın.
1. **Hizmetlere** giderek veya Hizmetleri **Start**  >  **Çalıştır**  >  **. msc**' ye giderek Hizmetleri açın.
1. **Hizmetler**' in altında, **Microsoft Azure AD sağlama Aracısı**' na çift tıklayın.
1. **Oturum aç** sekmesinde **Bu hesabı** bir etki alanı yöneticisi olarak değiştirin. Ardından hizmeti yeniden başlatın. 

   ![Oturum açma sekmesi](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>Aracı zaman aşımı veya sertifika geçersiz

Aracıyı kaydetmeye çalıştığınızda aşağıdaki hata iletisini alabilirsiniz.

![Zaman aşımı hata iletisi](media/how-to-troubleshoot/troubleshoot4.png)

Bu sorun genellikle aracının Karma Kimlik Hizmetine bağlanamamasından kaynaklanır ve bir HTTP ara sunucusu yapılandırmanız gerekir. Bu sorunu çözmek için giden bir ara sunucu yapılandırın. 

Sağlama Aracısı bir giden proxy 'nin kullanımını destekler. Bunu, *C:\Program Files\Microsoft Azure AD Connect sağlama Agent\AADConnectProvisioningAgent.exe.config*aracı yapılandırma dosyasını düzenleyerek yapılandırabilirsiniz. Aşağıdaki satırları, kapanış etiketinden hemen öncesine dosyanın sonuna doğru ekleyin `</configuration>` .
Değişkenleri `[proxy-server]` ve `[proxy-port]` proxy sunucu adı ve bağlantı noktası değerlerinizle değiştirin.

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

#### <a name="agent-registration-fails-with-security-error"></a>Aracı kaydı güvenlik hatasıyla başarısız oluyor

Bulut sağlama aracısını yüklerken bir hata iletisi alabilirsiniz.

Bu sorun genellikle aracının yerel PowerShell yürütme ilkeleri nedeniyle PowerShell kayıt betikleri yürütmesine neden olur.

Bu sorunu çözmek için sunucudaki PowerShell yürütme ilkelerini değiştirin. Makine ve kullanıcı ilkelerinin *tanımsız* veya *RemoteSigned*olarak ayarlanmış olması gerekir. *Kısıtlamasız*olarak ayarlandıklarında bu hatayı görürsünüz. Daha fazla bilgi için bkz. [PowerShell yürütme ilkeleri](/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6). 

### <a name="log-files"></a>Günlük dosyaları

Varsayılan olarak aracı mümkün olduğunca az hata iletisi ve yığın izleme bilgisi yarar. Bu izleme günlüklerini *C:\programdata\microsoft\azure AD Connect sağlama programı \ Trace*klasöründe bulabilirsiniz.

Aracıyla ilgili sorunları gidermeye yönelik ek ayrıntılar toplamak için aşağıdaki adımları izleyin.

1. Hizmeti durdurma **Microsoft Azure AD sağlama aracısını bağlama**.
1. Özgün yapılandırma dosyasının bir kopyasını oluşturun: *C:\Program Files\Microsoft Azure AD Connect sağlama Agent\AADConnectProvisioningAgent.exe.config*.
1. Var olan `<system.diagnostics>` bölümü aşağıdaki ile değiştirin ve tüm izleme Iletileri *ProvAgentTrace. log*dosyasına gider.

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
1. Hizmeti başlatın **Microsoft Azure AD sağlama aracısına bağlanın**.
1. Dosya ve hata ayıklama sorunlarını ayıklamak için aşağıdaki komutu kullanın. 
    ```
    Get-Content “C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log” -Wait
    ```
## <a name="object-synchronization-problems"></a>Nesne eşitleme sorunları

Aşağıdaki bölümde, nesne eşitlemeyle ilgili sorun giderme hakkında bilgiler yer almaktadır.

### <a name="provisioning-logs"></a>Sağlama günlükleri

Azure portal, sağlama günlükleri, nesne eşitleme sorunlarını izlemeye ve sorun gidermenize yardımcı olmak için kullanılabilir. Günlükleri görüntülemek için **Günlükler**' i seçin.

![Günlükler düğmesi](media/how-to-troubleshoot/log1.png)

Sağlama günlükleri, şirket içi Active Directory ortamınız ile Azure arasında eşitlenen nesnelerin durumu hakkında çok fazla bilgi sağlar.

![Sağlama günlükleri ekranı](media/how-to-troubleshoot/log2.png)

Sayfanın en üstündeki açılan kutuları, tarihler gibi belirli sorunlarda görünümü sıfıra filtrelemek için kullanabilirsiniz. Ek bilgi görmek için tek bir olaya çift tıklayın.

![Sağlama günlükleri açılan kutusu bilgileri](media/how-to-troubleshoot/log3.png)

Bu bilgiler, ayrıntılı adımlar ve eşitleme sorununun nerede oluştuğunu sağlar. Bu şekilde, sorunun tam olarak bulunduğu noktayı bir arada kullanabilirsiniz.


## <a name="provisioning-quarantined-problems"></a>Karantinaya alınan sorunları sağlama

Bulut sağlama, yapılandırmanızın sistem durumunu izler ve sağlıksız nesneleri karantina durumuna koyar. Hedef sisteme karşı yapılan çağrıların çoğu veya hepsi sürekli olarak hata nedeniyle başarısız olursa, örneğin, geçersiz yönetici kimlik bilgileri, sağlama işi karantinaya alındı olarak işaretlenir.

![Karantina durumu](media/how-to-troubleshoot/quarantine1.png)

Durumu seçerek, karantinaya alma hakkında daha fazla bilgi görebilirsiniz. Hata kodunu ve iletiyi de elde edebilirsiniz.

![Karantina durumu bilgileri](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolve-a-quarantine"></a>Karantinayı çözümle

- Sağlama işini yeniden başlatmak için Azure portal kullanın. Aracı yapılandırması sayfasında, **sağlamayı yeniden Başlat**' ı seçin.

  ![Sağlamayı yeniden Başlat](media/how-to-troubleshoot/quarantine3.png)

- [Sağlama işini yeniden başlatmak](/graph/api/synchronization-synchronizationjob-restart?tabs=http&view=graph-rest-beta)için Microsoft Graph kullanın. Yeniden başlatdıklarınız üzerinde tam denetime sahip olacaksınız. Şunları temizleyebilirsiniz:
  - Et, karantina durumuna doğru tahakkuk eden Emanet sayacını yeniden başlatacak.
  - Karantina, uygulamayı karantinadan kaldırma.
  - Fli. 
  
  Aşağıdaki isteği kullanın:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)