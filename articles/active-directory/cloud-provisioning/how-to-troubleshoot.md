---
title: Azure AD Connect bulut sağlama sorunlarını giderme
description: Bu belge, bulut sağlama aracısında oluşabilecek sorunları nasıl giderebileceğinizi açıklar.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4100886e0a24fa961b9085bd507ae3f4ebfdd6eb
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74794243"
---
# <a name="cloud-provisioning-troubleshooting"></a>Bulut sağlama sorunlarını giderme

Bulut sağlama birçok farklı şeyi dokunduğunda birçok farklı bağımlılığı vardır.  Doğal olarak, bu çeşitli sorunlara göre daha fazla soruna izin verebilir.  Bu belge, bu sorunları gidermeye başlamak için tasarlanmıştır.  Bu belge sizi odaklanmanız gereken tipik alanlara, ek bilgilerin nasıl toplantığına ve sorunları izlemek için kullanılabilecek çeşitli tekniklerin tanıtılyacaktır.  


## <a name="common-troubleshooting-areas"></a>Ortak sorun giderme alanı

|Adı|Açıklama|
|-----|-----|
|[Aracı sorunları](#agent-issues)|Aracının doğru şekilde yüklendiğini ve Azure AD ile iletişim kurduğunu doğrulayın.|
|[Nesne eşitleme sorunları](#object-synchronization-issues)|Nesne eşitleme sorunlarını gidermek için sağlama günlüklerini kullanın.|
|[Karantina sorunlarını sağlama](#provisioning-quarantined-issues)|Sağlama karantina sorununu ve bunların nasıl düzeltileceğini anlayın.|


## <a name="agent-issues"></a>Aracı sorunları
Aracıda doğrulamak istediğiniz ilk şey şunlardır:


-  mi yüklendi?
-  Aracı yerel olarak çalışıyor mu?
-  Aracı portalda mı?
-  Aracı sağlıklı olarak işaretlendi mi?  

Bu öğeler Azure portal ve aracıyı çalıştıran yerel sunucu üzerinde doğrulanabilir.

### <a name="azure-portal-agent-verification"></a>Aracı doğrulama Azure portal

Aracının Azure tarafından görüleceği ve sağlıklı olduğundan emin olmak için şu adımları izleyin:

1. Azure Portal’da oturum açın.
2. Sol tarafta **Azure Active Directory**' ı seçin, **Azure AD Connect** ' a tıklayın ve ardından **yönetimi sağlama (Önizleme)** seçeneğini belirleyin.
3.  **Azure AD sağlama (Önizleme)** ekranında **tüm aracıları gözden geçir**' e tıklayın.
 Azure AD sağlama](media/how-to-install/install7.png) ![</br>
 
4. **Şirket içi sağlama aracıları ekranında** , yüklediğiniz aracıları görürsünüz.  Söz konusu aracının orada olduğunu ve **sağlıklı**olarak işaretlendiğinden emin olun.
 ![sağlama aracıları](media/how-to-install/install8.png)</br>

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama

Azure 'un 443 numaralı bağlantı noktasını dinlediğini ve aracınızın onunla iletişim kurabildiğini doğrulamak için aşağıdaki aracı kullanabilirsiniz:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Bu test, aracılarınızın 443 numaralı bağlantı noktası üzerinden Azure ile iletişim kurabildiğini doğrular.  Bir tarayıcı açın ve aracının yüklendiği sunucudan yukarıdaki URL 'ye gidin.
 ![Hizmetler](media/how-to-install/verify2.png)

### <a name="on-the-local-server"></a>Yerel sunucuda

Aracının çalıştığını doğrulamak için şu adımları izleyin:

1.  Aracının yüklü olduğu sunucuda, hizmete giderek veya Start/Run/Services. msc ' ye giderek **Hizmetleri** açın.
2.  **Hizmetler**' in altında **Microsoft Azure AD aracı Güncelleştirici** ' ı bağlama ve **Microsoft Azure AD Connect sağlama aracısının** aynı ve **çalışır**durumda olduğundan emin olun.
 ![Hizmetler](media/how-to-troubleshoot/troubleshoot1.png)

### <a name="common-agent-installation-issues"></a>Ortak aracı yükleme sorunları

Aşağıda bazı yaygın aracı yükleme sorunları ve tipik çözümlemenin ne olduğu aşağıda verilmiştir.

#### <a name="agent-failed-to-start"></a>Aracı başlatılamadı

Aşağıdakileri belirten bir hata iletisi alırsanız:

**' Mikro Sfoft Azure AD Connect sağlama Aracısı ' hizmeti başlatılamadı.  Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın.** 

Bu, genellikle izinleri yükleyicinin (NT SERVICE\AADConnectProvisioningAgent) tarafından oluşturulan yerel NT hizmeti "oturum açma hesabına" uygulanmasını önleyen bir grup ilkesi nedeniyle, hizmeti başlatmak için bu izinler gereklidir.

Bu sorunu çözmek için aşağıdaki adımları kullanın:

1.  Yönetici hesabıyla sunucuda oturum açma
2.  Hizmetlere giderek veya Start/Run/Services. msc ' ye giderek **Hizmetleri** açın.
3.  **Hizmetler** ' in altında **Microsoft Azure AD sağlama aracısına Bağlan** ' a çift tıklayın
4. Sekmesinde, "oturum açma hesabı" nı bir etki alanı yöneticisi olarak değiştirin ve hizmeti yeniden başlatın. 

 ![Hizmetler](media/how-to-troubleshoot/troubleshoot3.png)

#### <a name="agent-times-out-or-certificate-is-invalid"></a>Aracı zaman aşımı veya sertifika geçersiz

Aracıyı kaydetmeye çalışıyorsanız aşağıdaki hataları alabilirsiniz.

 ![Hizmetler](media/how-to-troubleshoot/troubleshoot4.png)

Bu, genellikle aracının karma kimlik hizmetine bağlanamamasından kaynaklanır ve HTTP proxy yapılandırmasını gerektirir.  Bu sorunu çözmek için bir giden proxy yapılandırın. 

Sağlama Aracısı giden ara sunucu kullanımını destekler. Bunu, **C:\Program Files\Microsoft Azure AD Connect sağlama Agent\AADConnectProvisioningAgent.exe.config**aracı yapılandırma dosyasını düzenleyerek yapılandırabilirsiniz. Aşağıdaki satırları, kapatma `</configuration>` etiketinden hemen önceki dosyanın sonuna doğru bir şekilde ekleyin.
[Proxy-Server] ve [proxy-port] değişkenlerini ara sunucu adı ve bağlantı noktası değerlerinizle değiştirin.

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

Bulut sağlama aracısını yüklerken aşağıdaki hatayı görebilirsiniz.

Bunun nedeni genellikle aracının yerel PowerShell yürütme ilkeleri nedeniyle PowerShell kayıt betikleri yürütmesine neden olur.

Bu sorunu gidermek için, sunucudaki PowerShell yürütme ilkelerini değiştirin. Makine ve Kullanıcı ilkelerindeki "tanımsız" veya "RemoteSigned" olması gerekir. "Kısıtlamasız" ise bu hatayı görürsünüz.  Daha fazla bilgi için bkz. [PowerShell yürütme ilkeleri](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6). 

### <a name="log-files"></a>Günlük dosyaları

Varsayılan olarak, aracı çok az hata iletisi ve yığın izleme bilgilerini yayar. Bu izleme günlüklerini şu klasörde bulabilirsiniz: **C:\programdata\microsoft\azure AD Connect sağlama programı \ izleme**

Aracıyla ilgili sorunları gidermeye yönelik ek ayrıntılar toplamak için aşağıdaki adımları kullanın.

1. Hizmeti durdurma **Microsoft Azure AD sağlama aracısını bağlama**
2. Özgün yapılandırma dosyasının bir kopyasını oluşturun: **C:\Program Files\Microsoft Azure AD Connect sağlama Agent\AADConnectProvisioningAgent.exe.config**
3. Mevcut < System. Diagnostics > bölümünü aşağıdaki ile değiştirin ve tüm izleme iletileri **ProvAgentTrace. log** dosyasına gider

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
4. Hizmeti başlatma **Microsoft Azure AD sağlama aracısına bağlanma**
5. Dosya ve hata ayıklama sorunlarını ayıklamak için aşağıdaki komutu kullanabilirsiniz: 
    ```
    Get-Content “C:/ProgramData/Microsoft/Azure AD Connect Provisioning Agent/Trace/ProvAgentTrace.log” -Wait
    ```
## <a name="object-synchronization-issues"></a>Nesne eşitleme sorunları

Aşağıdaki bölümde, nesne eşitlemeyle ilgili sorun giderme hakkında bilgiler yer almaktadır.

### <a name="provisioning-logs"></a>Sağlama günlükleri

Azure portal, sağlama günlükleri, nesne eşitleme sorunlarını izlemeye ve sorun gidermenize yardımcı olmak için kullanılabilir.  Günlükleri görüntülemek için **Günlükler**' i seçin.
 ![sağlama günlükleri](media/how-to-troubleshoot/log1.png)

Sağlama günlükleri, şirket içi AD ortamınız ve Azure arasında eşitlenen nesnelerin durumu hakkında çok fazla bilgi sağlar.

 ![Sağlama günlükleri](media/how-to-troubleshoot/log2.png)

Görünümü belirli sorunlar, tarihler vb. üzerinde sıfıra filtrelemek için sayfanın üst kısmındaki açılan listeleri kullanabilirsiniz.  Tek bir olaya çift tıklamak ek ayrıntılı bilgi sağlayacaktır.

 ![Sağlama günlükleri](media/how-to-troubleshoot/log3.png)

Bu bilgiler, ayrıntılı adımlar ve eşitleme sorununun nerede oluştuğunu sağlayacaktır.  Bu nedenle, sorunun tam olarak bulunduğu ve belirleyebilmenizi sağlar.


## <a name="provisioning-quarantined-issues"></a>Karantinaya alınan sorunları sağlama

Bulut sağlama, yapılandırmanızın sistem durumunu izler ve sağlıksız nesneleri "Karantina" durumuna koyar. Hedef sisteme karşı yapılan çağrıların çoğu veya hepsi bir hata nedeniyle sürekli olarak başarısız olursa, örneğin, geçersiz yönetici kimlik bilgileri gibi, sağlama işi karantinada olarak işaretlenir.

 ![Ada](media/how-to-troubleshoot/quarantine1.png)

Duruma tıklayarak, karantinaya alma hakkında ek bilgi görebilirsiniz.  Hata kodunu ve iletiyi de elde edebilirsiniz.

 ![Ada](media/how-to-troubleshoot/quarantine2.png)

### <a name="resolving-a-quarantine"></a>Karantina çözme

- Sağlama işini yeniden başlatmak için Azure portal kullanın. Aracı Yapılandırma sayfasında **yeniden başlatma sağlama**' yı seçin.

  ![Ada](media/how-to-troubleshoot/quarantine3.png)

- [Sağlama işini yeniden başlatmak](https://docs.microsoft.com/graph/api/synchronization-synchronizationjob-restart?view=graph-rest-beta&tabs=http)için Microsoft Graph kullanın. Yeniden başlatdıklarınız üzerinde tam denetime sahip olacaksınız. Et 'ları kaldırmayı seçebilirsiniz (karantina durumuna göre tahakkuk eden Emanet sayacını yeniden başlatmak için), karantinayı temizleyebilir (uygulamayı karantinadan kaldırmak için) veya filigranları temizleyebilirsiniz. Aşağıdaki isteği kullanın:
 
  `POST /servicePrincipals/{id}/synchronization/jobs/{jobId}/restart`

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)



