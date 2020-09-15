---
title: Azure Data Factory içinde şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme
description: Azure Data Factory ' de şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 09/14/2020
ms.author: abnarain
ms.openlocfilehash: 1a68263598cb2cba8cc0853f5dd1be7c62dc062e
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069484"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki şirket içinde barındırılan tümleştirme çalışma zamanı için genel sorun giderme yöntemleri incelenmektedir.

## <a name="gather-self-hosted-ir-logs-from-azure-data-factory"></a>Azure Data Factory şirket içinde barındırılan IR günlüklerini toplayın

Şirket içinde barındırılan IR/paylaşılan IR üzerinde çalışan başarısız etkinlikler için Azure Data Factory hata günlüklerini görüntülemeyi ve yüklemeyi destekler. Aşağıdaki adımları izleyerek hata raporu KIMLIĞINI alabilir ve ilgili bilinen sorunları bulmak için rapor KIMLIĞINI girebilirsiniz.

1. **Etkinlik çalıştırmaları** sayfasına gidin.

1. **Hata** sütununun altında, aşağıdaki düğmeye tıklayın.

    ![Etkinlik çalıştırmaları sayfası](media/self-hosted-integration-runtime-troubleshoot-guide/activity-runs-page.png)

1. Başarısız etkinlik çalıştırması için ilgili günlükleri görürsünüz. Daha fazla yardım için **günlükleri Gönder** düğmesine tıklayın.

    ![Günlükleri Gönder](media/self-hosted-integration-runtime-troubleshoot-guide/send-logs.png)

1. Göndermek istediğiniz günlükleri seçebilirsiniz. *Şirket içinde BARıNDıRıLAN IR*için, başarısız etkinlikle ilgili günlükleri veya şirket IÇINDE barındırılan IR düğümündeki tüm günlükleri karşıya yükleyebilirsiniz. *PAYLAŞıLAN IR*için yalnızca başarısız etkinlikle ilgili günlükleri karşıya yükleyebilirsiniz.

    ![Günlükleri Seç](media/self-hosted-integration-runtime-troubleshoot-guide/choose-logs.png)

1. Günlükler karşıya yüklendiğinde, sorunu çözmeye yönelik daha fazla yardıma ihtiyacınız varsa rapor KIMLIĞININ bir kaydını saklayın.

    ![Günlükleri karşıya yükle](media/self-hosted-integration-runtime-troubleshoot-guide/upload-logs.png)

> [!NOTE]
> Günlük görüntüleme ve karşıya yükleme istekleri, tüm çevrimiçi şirket içinde barındırılan IR örneklerinde yürütülür. Lütfen tüm şirket içinde barındırılan IR örneklerinin eksik Günlükler olması durumunda çevrimiçi olduğundan emin olun. 


## <a name="self-hosted-ir-general-failure-or-error"></a>Şirket içinde barındırılan IR genel başarısızlığı veya hatası

### <a name="tlsssl-certificate-issue"></a>TLS/SSL sertifikası sorunu

#### <a name="symptoms"></a>Belirtiler

TLS/SSL sertifikası seçildikten sonra **Şirket içinde barındırılan IR Configuration Manager** -> **İntranete uzaktan erişim**'den TLS/SSL sertifikasını (gelişmiş) etkinleştirmeye çalışırken aşağıdaki hata gösteriliyor:

`Remote access settings are invalid. Identity check failed for outgoing message. The expected DNS identity of the remote endpoint was ‘abc.microsoft.com’ but the remote endpoint provided DNS claim ‘microsoft.com’. If this is a legitimate remote endpoint, you can fix the problem by explicitly specifying DNS identity ‘microsoft.com’ as the Identity property of EndpointAddress when creating channel proxy.`

Yukarıdaki durumda kullanıcı son öğe olarak "microsoft.com" bulunan sertifikayı kullanıyor.

#### <a name="cause"></a>Nedeni

Bu WCF'de bilinen bir sorundur: WCF TLS/SSL doğrulaması SAN içinde yalnızca son DNSName öğesini denetler. 

#### <a name="resolution"></a>Çözüm

Joker sertifika Azure Data Factory v2 Şirket İçinde Barındırılan IR'de desteklenir. Bu sorun normalde SSL sertifikası doğru olmadığı için oluşur. SAN'deki son DNSName doğru olmalıdır. Bunu doğrulamak için aşağıdaki adımları izleyin. 
1.  Yönetim Konsolu 'Nu açın, sertifika ayrıntılarından hem *Konu* hem de *konu alternatif adını* iki kez kontrol edin. Yukarıdaki durumda, örneğin "DNS Name = microsoft.com.com" olan *konu alternatif adındaki*son öğe geçerli değildir.
2.  Yanlış DNS adını kaldırmak için sertifika sorunu şirketine başvurun.

### <a name="concurrent-jobs-limit-issue"></a>Eşzamanlı işleri sınırlama sorunu

#### <a name="symptoms"></a>Belirtiler

Azure Data Factory kullanıcı arabiriminde eşzamanlı işlerin sınırını artırmayı denediğinizde sonsuza kadar *güncelleştiriliyormuş* gibi kilitleniyor.
Eşzamanlı işler için üst sınır 24 olarak ayarlanmış ve işlerin daha hızlı çalıştırılması için bu sayıyı artırmak istiyorsunuz. Girebileceğiniz en düşük değer 3 ve en yüksek değer de 32'dir. Bu değeri 24 ' ten 32 ' e artırmış ve *güncelleştirme* düğmesi ' nde, aşağıda gördüğünüz şekilde *güncelleştirmede* yer aldığı Kullanıcı arabiriminde anlamış olursunuz. Yenileme sonrasında müşteri değerin hala 24 olduğunu, 32'ye hiç güncelleştirilmediğini görüyor.

![Durum güncelleştiriliyor](media/self-hosted-integration-runtime-troubleshoot-guide/updating-status.png)

#### <a name="cause"></a>Nedeni

Bu değer bilgisayarın Mantıksal Çekirdek ve Bellek değerlerine bağlı olduğundan ayarın bir sınırlaması vardır; bunu 24 gibi daha düşük bir değere ayarlayıp sonuca bakabilirsiniz.

> [!TIP] 
> - Mantıksal çekirdek sayısının ne olduğu ve makinenizin mantıksal çekirdek sayısını bulma hakkında daha fazla bilgi için [Bu makaleye](https://www.top-password.com/blog/find-number-of-cores-in-your-cpu-on-windows-10/)bakın.
> - Math. log ' u hesaplama hakkında daha fazla bilgi için [Bu makaleye](https://www.rapidtables.com/calc/math/Log_Calculator.html)bakın.


### <a name="self-hosted-ir-ha-ssl-certificate-issue"></a>Şirket içinde barındırılan IR HA SSL Sertifikası sorunu

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan IR iş düğümü aşağıdaki hatayı bildirdi:

`Failed to pull shared states from primary node net.tcp://abc.cloud.corp.Microsoft.com:8060/ExternalService.svc/. Activity ID: XXXXX The X.509 certificate CN=abc.cloud.corp.Microsoft.com, OU=test, O=Microsoft chain building failed. The certificate that was used has a trust chain that cannot be verified. Replace the certificate or change the certificateValidationMode. The revocation function was unable to check revocation because the revocation server was offline.`

#### <a name="cause"></a>Nedeni

SSL/TLS el sıkışmasıyla ilgili olayları işlerken, sertifika zinciri doğrulamasıyla ilgili bazı sorunlarla karşılaşabiliyoruz. 

#### <a name="resolution"></a>Çözüm

- X. 509.440 sertifika zinciri derleme hatası sorunlarını gidermek için hızlı ve sezgisel bir yol aşağıda verilmiştir.
 
    1. Doğrulanması gereken sertifikayı dışarı aktarın. Bilgisayar sertifikasını yönetme bölümüne gidin, denetlemek istediğiniz sertifikayı bulun ve **Tüm görevler** -> **Dışarı aktar**'a sağ tıklayın.
    
        ![Görevleri dışarı aktar](media/self-hosted-integration-runtime-troubleshoot-guide/export-tasks.png)

    2. İçe aktarılmış sertifikayı istemci makinesine kopyalayın. 
    3. İstemci tarafında, CMD'de aşağıdaki komutu çalıştırın. Aşağıdaki *\<certificate path>* ve *\<output txt file path>* yer tutucuları ilgili yollarla değiştirdiğinizden emin olun.
    
        ```
        Certutil -verify -urlfetch    <certificate path>   >     <output txt file path> 
        ```

        Örnek:

        ```
        Certutil -verify -urlfetch c:\users\test\desktop\servercert02.cer > c:\users\test\desktop\Certinfo.txt
        ```
    4. Çıkış txt dosyasında herhangi bir hata olup olmadığını denetleyin. Bu txt dosyasının sonunda hata özetini bulabilirsiniz.

        Örnek: 

        ![Hata Özeti](media/self-hosted-integration-runtime-troubleshoot-guide/error-summary.png)

        Günlük dosyasının sonunda aşağıda gösterildiği gibi herhangi bir hata görmüyorsanız, istemci makinesinde başarıyla oluşturulan sertifika zincirini göz önünde bulundurun.
        
        ![Günlük dosyasında hata yok](media/self-hosted-integration-runtime-troubleshoot-guide/log-file.png)      

- Sertifika dosyasında AIA, CDP ve OCSP yapılandırılmışsa. Daha sezgisel bir şekilde kontrol edebilirsiniz.
 
    1. Bir sertifikanın ayrıntılarını denetleyerek bu bilgileri alabilirsiniz.
    
        ![Sertifika ayrıntısı](media/self-hosted-integration-runtime-troubleshoot-guide/certificate-detail.png)
    1. Aşağıdaki komutu çalıştırın. *\<certificate path>* Yer tutucuyu sertifikanın ilgili yoluyla değiştirdiğinizden emin olun.
    
        ```
          Certutil   -URL    <certificate path> 
        ```
    1. Ardından **URL Alma aracı** açılır. **Al** düğmesine tıklayarak AIA, CDP ve OCSP'den sertifikaları doğrulayabilirsiniz.

        ![Alma düğmesi](media/self-hosted-integration-runtime-troubleshoot-guide/retrieval-button.png)
 
        AIA'dan sertifika "Doğrulandı" ve CDP veya OCSP'den sertifika "Doğrulandı" durumundaysa sertifika zinciri başarıyla oluşturulabilir.

        AIA ve CDP'yi alırken hata görüyorsanız ağ ekibiyle birlikte çalışarak istemci makinesinin hedef URL'ye bağlanmaya hazır olmasını sağlayın. Http yolunun veya Idap yolunun doğrulanabilir olması yeterli olacaktır.

### <a name="self-hosted-ir-could-not-load-file-or-assembly"></a>Şirket içinde barındırılan IR dosyayı veya derlemeyi yükleyemedi

#### <a name="symptoms"></a>Belirtiler

`Could not load file or assembly 'XXXXXXXXXXXXXXXX, Version=4.0.2.0, Culture=neutral, PublicKeyToken=XXXXXXXXX' or one of its dependencies. The system cannot find the file specified. Activity ID: 92693b45-b4bf-4fc8-89da-2d3dc56f27c3`
 
Örnek: 

`Could not load file or assembly 'System.ValueTuple, Version=4.0.2.0, Culture=neutral, PublicKeyToken=XXXXXXXXX' or one of its dependencies. The system cannot find the file specified. Activity ID: 92693b45-b4bf-4fc8-89da-2d3dc56f27c3`

#### <a name="cause"></a>Nedeni

İşlem izleyiciyi alırsanız aşağıdaki sonucu görebilirsiniz:

[![İşlem İzleyicisi](media/self-hosted-integration-runtime-troubleshoot-guide/process-monitor.png)](media/self-hosted-integration-runtime-troubleshoot-guide/process-monitor.png#lightbox)

> [!TIP] 
> Aşağıdaki ekran görüntüsünde gösterilen şekilde filtre ayarlayabilirsiniz.
> Bu, dll **System. ValueTuple** 'nin GAC ile ilgili klasörde veya *C:\Program Files\Microsoft Integration Runtime\4.0\Gateway*veya *c:\Program Files\Microsoft Integration Runtime\4.0\Shared* klasöründe yer aldığı konusunda bize söyler.
> Temelde dll'yi önce *GAC* klasöründen, ardından *Shared* ve son olarak da *Gateway* klasöründen yükler. Bu nedenle dll'yi yararlı olabilecek herhangi bir yola koyabilirsiniz.

![Filtreleri ayarlama](media/self-hosted-integration-runtime-troubleshoot-guide/set-filters.png)

#### <a name="resolution"></a>Çözüm

**System.ValueTuple.dll** *C:\Program Files\Microsoft Integration Runtime\4.0\Gateway\DataScan* klasöründe bulunduğunu görebilirsiniz. Sorunu çözmek **System.ValueTuple.dll** içinSystem.ValueTuple.dll*C:\Program Files\Microsoft Integration Runtime\4.0\Gateway* klasörüne kopyalayın.

Diğer eksik dosya veya derleme sorunlarını çözmek için de aynı yöntemi kullanabilirsiniz.

#### <a name="more-information"></a>Daha Fazla Bilgi

*%Windir%\Microsoft.NET\assembly* ve *%windir%\assembly* altında System.ValueTuple.dll görmenizin nedeni, .net davranışının olmasının nedenidir. 

Aşağıdaki hatadan, derleme sistemini açık bir şekilde görebilirsiniz *. ValueTuple* yok. Bu nedenle, uygulama derlemeyi *System.ValueTuple.dll*denetlemeye çalıştığında böyle bir sorun oluşur.
 
`<LogProperties><ErrorInfo>[{"Code":0,"Message":"The type initializer for 'Npgsql.PoolManager' threw an exception.","EventType":0,"Category":5,"Data":{},"MsgId":null,"ExceptionType":"System.TypeInitializationException","Source":"Npgsql","StackTrace":"","InnerEventInfos":[{"Code":0,"Message":"Could not load file or assembly 'System.ValueTuple, Version=4.0.2.0, Culture=neutral, PublicKeyToken=XXXXXXXXX' or one of its dependencies. The system cannot find the file specified.","EventType":0,"Category":5,"Data":{},"MsgId":null,"ExceptionType":"System.IO.FileNotFoundException","Source":"Npgsql","StackTrace":"","InnerEventInfos":[]}]}]</ErrorInfo></LogProperties>`
 
GAC hakkında daha fazla bilgi için [Bu makaleye](https://docs.microsoft.com/dotnet/framework/app-domains/gac)bakın.


### <a name="how-to-audit-self-hosted-ir-key-missing"></a>Şirket içinde barındırılan IR eksik anahtarını izleme

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan tümleştirme çalışma zamanı anahtar olmadan aniden çevrimdışı duruma geliyor, Olay Günlüğünde şu hata iletisi gösteriliyor: `Authentication Key is not assigned yet`

![Kimlik doğrulama anahtarı eksik](media/self-hosted-integration-runtime-troubleshoot-guide/key-missing.png)

#### <a name="cause"></a>Nedeni

- Portalda Şirket içinde barındırılan IR düğümü veya mantıksal Şirket içinde barındırılan IR silinmiş.
- Temiz bir kaldırma işlemi yapılmış.

#### <a name="resolution"></a>Çözüm

Yukarıdaki nedenlerin hiçbiri geçerli değilse, şu klasöre gidebilirsiniz: *%ProgramData%\microsoft\data Transfer\DataManagementGateway*ve **yapılandırma** adlı dosyanın silinip silinmediğini kontrol edebilirsiniz. Silinmişse, [buradaki](https://www.netwrix.com/how_to_detect_who_deleted_file.html) yönergeleri izleyerek dosyayı kimin sildiğini denetleyin.

![Yapılandırma dosyasını denetle](media/self-hosted-integration-runtime-troubleshoot-guide/configurations-file.png)


### <a name="cannot-use-self-hosted-ir-to-bridge-two-on-premises-data-stores"></a>Şirket içindeki iki veri deposu arasında köprü oluşturmak için Şirket içinde barındırılan IR kullanılamıyor

#### <a name="symptoms"></a>Belirtiler

Hem kaynak hem de hedef veri deposunda Şirket içinde barındırılan IR'ler oluşturulduktan sonra bir kopyalamayı bitirmek için iki IR'yi birbirine bağlamak istiyorsunuz. Veri depoları farklı VNET 'lerde yapılandırılmışsa veya ağ geçidi mekanizmasını anlamadıklarında, şunun gibi hatalarla karşılaşmanız gerekir: *kaynak sürücüsü hedef IR 'de bulunamıyor*; *hedef IR tarafından kaynağa erişilemiyor*.
 
#### <a name="cause"></a>Nedeni

Şirket içinde barındırılan IR, her veri deposu için yüklenmesi gereken bir istemci aracısı olarak değil kopyalama etkinliğinin merkezi düğümü olarak belirlenmiş.
 
Yukarıdaki durumda her veri deposu için bağlı hizmet aynı IR ile oluşturulmalı ve IR ağ üzerinden her iki veri deposuna da erişebilmelidir. IR'nin kaynak veri deposu, hedef veri deposu için veya üçüncü bir makinede yüklenmiş olması fark etmez; iki bağlı hizmet farklı IR'lerle oluşturulduysa ama aynı kopyalama etkinliğinde kullanılıyorsa, hedef IR kullanılır ve hedef IR makinesine her iki veri deposu için de sürücülerin yüklenmesi gerekir.

#### <a name="resolution"></a>Çözüm

Hedef IR'de hem kaynak hem de hedef sürücülerini yükleyin ve bunun kaynak veri deposuna erişebildiğinden emin olun.
 
İki veri deposu arasındaki ağdan trafik geçemiyorsa (örneğin iki VNET'te yapılandırıldılarsa), IR yüklü olduğunda bile bir etkinlikteki kopyalamayı bitiremezler. Böyle bir durumda her biri bir VNET'te olmak üzere iki IR ile iki kopyalama etkinliği oluşturabilirsiniz: 1 IR 1. veri deposundan Azure Blob Depolama'ya ve diğeri de Azure Blob Depolama'dan 2. veri deposuna kopyalamak için. Bu yöntem iki bağlantısız veri deposunu bağlayan bir köprü oluşturmak için IR'yi kullanma gereksiniminin simülasyonunu sağlayabilir.


### <a name="credential-sync-issue-causes-credential-lost-from-ha"></a>Kimlik bilgileri eşitleme sorunu HA'dan kimlik bilgilerinin kaybolmasına neden oluyor

#### <a name="symptoms"></a>Belirtiler

Veri kaynağı "XXXXXXXXXX" kimlik bilgisi, yükü şöyle olan geçerli Integration Runtime düğümünden silinmiş: "Azure portaldaki bağlantı hizmetini sildiğinizde veya görevin yükü yanlış olduğunda, lütfen kimlik bilginizle yeni bir bağlantı hizmeti oluşturun".

#### <a name="cause"></a>Nedeni

Şirket içinde barındırılan IR'niz iki düğümlü HA modunda oluşturulmuş ama kimlik bilgilerini eşitleme durumunda değiller. Diğer bir deyişle dağıtıcı düğümünde depolanan kimlik bilgileri diğer çalışan düğümlerine eşitlenmiyor. Dağıtıcı düğümünden çalışan düğümüne herhangi bir yük devretme gerçekleşirse ama kimlik bilgileri yalnızca önceki dağıtıcı düğümünde bulunuyorsa, kimlik bilgilerine erişmeye çalışılırken görev başarısız olur ve yukarıdaki hatayla karşılaşırsınız.

#### <a name="resolution"></a>Çözüm

Bu sorundan kaçınmanın tek yolu her iki düğümünde kimlik bilgilerini eşitleme durumunda olduğundan emin olmaktır. Aksi takdirde, yeni dağıtıcı için kimlik bilgilerini yeniden girmeniz gerekir.


### <a name="cannot-choose-the-certificate-due-to-private-key-missing"></a>Özel anahtar eksik olduğundan sertifika seçilemiyor

#### <a name="symptoms"></a>Belirtiler

1.  PFX dosyasını sertifika depolama alanına aktarın.
2.  IR Configuration Manager kullanıcı arabirimi üzerinden sertifikayı seçerken aşağıdaki hatayla karşılaşıyorsunuz:

    ![Özel anahtar eksik](media/self-hosted-integration-runtime-troubleshoot-guide/private-key-missing.png)

#### <a name="cause"></a>Nedeni

- Kullanıcı hesabının ayrıcalık düzeyi düşük ve özel anahtara erişemiyor.
- Sertifika imza olarak oluşturuldu ama anahtar değişimi olarak oluşturulmadı.

#### <a name="resolution"></a>Çözüm

1.  Kullanıcı arabirimini çalıştırmak için özel anahtara erişebilen ayrıcalıklı bir hesap kullanın.
2.  Sertifikayı içeri aktarmak için aşağıdaki komutu çalıştırın:
    
    ```
    certutil -importpfx FILENAME.pfx AT_KEYEXCHANGE
    ```


## <a name="self-hosted-ir-setup"></a>Şirket içinde barındırılan IR kurulumu

### <a name="the-integration-runtime-registration-error"></a>Integration Runtime kayıt hatası 

#### <a name="symptoms"></a>Belirtiler

Bazen, aşağıdaki gibi nedenlerle şirket içinde barındırılan IR 'yi farklı bir hesapta çalıştırmak istiyoruz:
- Şirket ilkesi, hizmet hesabına izin vermez.
- Bazı kimlik doğrulaması gereklidir.

Hizmet panelinde hizmet hesabını değiştirdikten sonra, Integration Runtime çalışmayı durdurduğunu fark edebilirsiniz.

![IR kayıt hatası](media/self-hosted-integration-runtime-troubleshoot-guide/ir-registration-error.png)

#### <a name="cause"></a>Nedeni

Yalnızca hizmet hesabına verilen birçok kaynak vardır. Hizmet hesabını başka bir hesapla değiştirirken, tüm bağımlı kaynakların izni aynı kalır.

#### <a name="resolution"></a>Çözüm

Hatayı denetlemek için Integration Runtime olay günlüğüne gidin.

![IR olay günlüğü](media/self-hosted-integration-runtime-troubleshoot-guide/ir-event-log.png)

Hata, *UnauthorizedAccessException*' dan sonra gösteriyorsa, aşağıdaki yönergeleri izleyin:


1. Windows hizmeti panelinde *Diahostservice* oturum açma hizmeti hesabını denetleyin.

    ![Oturum açma hizmeti hesabı](media/self-hosted-integration-runtime-troubleshoot-guide/logon-service-account.png)

2. Oturum açma hizmeti hesabının şu klasör üzerinde R/W iznine sahip olup olmadığını denetleyin: *%ProgramData%\microsoft\datatransfer\datamanagementgateway*.

    - Varsayılan olarak, hizmet oturum açma hesabı değiştirilmediyseniz, R/W iznine sahip olmalıdır.

        ![Hizmet izni](media/self-hosted-integration-runtime-troubleshoot-guide/service-permission.png)

    - Hizmet oturum açma hesabını değiştirdiyseniz, sorunu azaltmak için aşağıdaki adımları izleyin:
        1. Temizle geçerli şirket içinde barındırılan IR 'yi kaldırın.
        1. Şirket içinde barındırılan IR bitlerini yükler.
        1. Hizmet hesabını değiştirmek için aşağıdaki yönergeleri izleyin: 
            1. Selfhosted IR yükleme klasörüne gidin, klasöre geçin: *Microsoft Integration Runtime\4.0\Shared*.
            1. Yükseltilmiş ayrıcalık kullanarak bir komut satırı başlatın. *\<user>* Ve *\<password>* kendi Kullanıcı adınızı ve parolanızı değiştirin ve ardından aşağıdaki komutu çalıştırın:
                       
                ```
                dmgcmd.exe -SwitchServiceAccount "<user>" "<password>"
                ```
            1. LocalSystem hesabına geçmek istiyorsanız, bu hesap için doğru biçimi kullandığınızdan emin olun. Doğru biçimin bir örneği aşağıda verilmiştir:

                ```
                dmgcmd.exe -SwitchServiceAccount "NT Authority\System" ""
                ```         
                Biçimi aşağıda gösterildiği **gibi kullanmayın:**

                ```
                dmgcmd.exe -SwitchServiceAccount "LocalSystem" ""
                ```              
            1. Alternatif olarak, yerel sistem yöneticiden daha yüksek ayrıcalıklara sahip olduğundan, bunu "Hizmetler" de doğrudan değiştirebilirsiniz.
            1. IR hizmeti oturum açma hesabı için yerel/etki alanı kullanıcısı ' nı kullanabilirsiniz.            
        1. Integration Runtime kaydedin.

Hata şöyle görünüyorsa: *' Integration Runtime Service ' (DIAHostService) hizmeti başlatılamadı. Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın*, aşağıdaki yönergeleri izleyin:

1. Windows hizmeti panelinde *Diahostservice* oturum açma hizmeti hesabını denetleyin.
   
    ![Oturum açma hizmeti hesabı](media/self-hosted-integration-runtime-troubleshoot-guide/logon-service-account.png)

2. Windows hizmetini başlatmak için oturum açma hizmeti hesabının **hizmet olarak oturum** aç iznine sahip olup olmadığını denetleyin:

    ![Hizmet olarak oturum aç](media/self-hosted-integration-runtime-troubleshoot-guide/logon-as-service.png)

#### <a name="more-information"></a>Daha Fazla Bilgi

Büyük/küçük bir çözünürlükte iki desenden hiçbiri uygulamanızda yoksa, aşağıdaki Windows olay günlüklerini toplamayı deneyin: 
- Uygulama ve hizmet günlükleri-> Integration Runtime
- Windows günlükleri-> uygulaması

### <a name="cannot-find-register-button-to-register-a-self-hosted-ir"></a>Kendi kendine barındırılan bir IR kaydetmek için Kaydet düğmesi bulunamıyor    

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan bir IR kaydı sırasında Configuration Manager Kullanıcı arabiriminde **Kaydet** düğmesi bulunamadı.

![Kaydolma düğmesi yok](media/self-hosted-integration-runtime-troubleshoot-guide/no-register-button.png)

#### <a name="cause"></a>Nedeni

*Integration Runtime 3,0*' nin yayımlanmasından sonra, bir temizleyici ve daha güvenli bir ortamı etkinleştirmek için mevcut bir Integration Runtime düğümündeki **Kaydet** düğmesi kaldırılmıştır. Herhangi bir Integration Runtime'a (çevrimiçi veya değil) düğüm kaydedildiyse, bu düğümü başka bir Integration Runtime'a yeniden kaydetmek için önceki düğümün yüklemesini kaldırmalı ve sonra düğümü yükleyip kaydetmelisiniz.

#### <a name="resolution"></a>Çözüm

1. Mevcut Integration Runtime kaldırmak için Denetim Masası 'na gidin.

    > [!IMPORTANT] 
    > Aşağıdaki işlemde Evet ' i seçin. Kaldırma işlemi sırasında verileri değiştirmeyin.

    ![Verileri silme](media/self-hosted-integration-runtime-troubleshoot-guide/delete-data.png)

1. Tümleştirme çalışma zamanı yükleyicisi MSI yoksa, en son Integration Runtime indirmek için [indirme merkezi](https://www.microsoft.com/en-sg/download/details.aspx?id=39717) ' ne gidin.
1. MSI 'yi yükleyip Integration Runtime kaydedin.


### <a name="unable-to-register-the-self-hosted-ir-due-to-localhost"></a>Şirket içinde barındırılan IR localhost nedeniyle kaydedilemedi    

#### <a name="symptoms"></a>Belirtiler

Get_LoopbackIpOrName, şirket içinde barındırılan IR yeni bir makineye kaydedilemiyor.

**Hata Ayıkla:** Çalışma zamanı hatası oluştu.
' Microsoft. DataTransfer. DIAgentHost. DataSourceCache ' için tür başlatıcısı özel durum oluşturdu.
Veritabanı araması sırasında kurtarılamaz bir hata oluştu.
 
**Özel durum ayrıntısı:** System. TypeInitializationException: ' Microsoft. DataTransfer. DIAgentHost. DataSourceCache ' için tür başlatıcısı özel durum oluşturdu. ---> sistemi .net. Sockets. SocketException: System .net. DNS. GetAddrInfo (dize adı) konumundaki bir veritabanı araması sırasında kurtarılamaz bir hata oluştu.

#### <a name="cause"></a>Nedeni

Bu sorun genellikle localhost çözümlenirken meydana gelir.

#### <a name="resolution"></a>Çözüm

Dosyayı barındırmak ve bu sorunu çözmek için localhost 127.0.0.1 kullanın.


### <a name="self-hosted-setup-failed"></a>Şirket içinde barındırılan kurulum başarısız oldu    

#### <a name="symptoms"></a>Belirtiler

Mevcut bir IR kaldırılamaz veya yeni bir IR yükleyemez ya da mevcut bir IR 'yi yeni bir IR ile yükseltebilirsiniz.

#### <a name="cause"></a>Nedeni

Yükleme Windows Installer hizmetine bağlıdır. Yükleme sorununa neden olabilecek değişken nedenleri vardır:
- Yeterli disk alanı yok
- İzinlerin bulunmaması
- NT hizmeti bir nedenden dolayı kilitlenmiş
- CPU kullanımı çok yüksek
- MSI dosyası yavaş bir ağ konumunda barındırılıyor
- Bazı sistem dosyalarına veya kayıt defterlerine istem dışı olarak dokunulmadı


## <a name="self-hosted-ir-connectivity-issues"></a>Şirket içinde barındırılan IR bağlantı sorunları

### <a name="self-hosted-integration-runtime-cant-connect-to-cloud-service"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı bulut hizmetine bağlanamıyor

#### <a name="symptoms"></a>Belirtiler

![Şirket içinde barındırılan IR bağlantı sorunu](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni 

Şirket içinde barındırılan tümleştirme çalışma zamanı Data Factory hizmetine bağlanamaz (arka uç). Bu sorun genellikle güvenlik duvarındaki ağ ayarlarından kaynaklanır.

#### <a name="resolution"></a>Çözüm

1. Integration Runtime hizmetinin çalışıp çalışmadığını denetleyin.
    
   ![Şirket içinde barındırılan IR hizmeti çalıştırma durumu](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Hizmet çalışıyorsa Adım 3 ' e gidin.

1. Şirket içinde barındırılan tümleştirme çalışma zamanı (varsayılan ayar) üzerinde yapılandırılmış bir proxy yoksa, şirket içinde barındırılan tümleştirme çalışma zamanının yüklendiği makinede aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > Data Factory konumunuza bağlı olarak hizmet URL 'SI farklılık gösterebilir. Hizmet URL 'sini **ADF Kullanıcı arabirimi**  >  **bağlantıları**  >  **tümleştirme çalışma zamanları**altında bulabilirsiniz  >  **Şirket içinde barındırılan IR**  >  **düğümlerini**düzenleme  >  **hizmeti URL 'lerini**düzenleyin.
            
    Beklenen yanıt aşağıda verilmiştir:
            
    ![PowerShell komut yanıtı](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Beklenen yanıtı almazsanız, durumunuza uygun şekilde aşağıdaki yöntemlerden birini kullanın:
            
    * "Uzak ad çözümlenemedi" iletisini alırsanız, bir etki alanı adı sistemi (DNS) sorunu vardır. Bu sorunu çözmesi için ağ ekibinize başvurun.
    * "SSL/TLS sertifikası güvenilir değil" iletisini alırsanız, için sertifikasının makinede güvenilir olup olmadığını denetleyin https://wu2.frontend.clouddatahub.net/ ve ardından Sertifika Yöneticisi 'ni kullanarak ortak sertifikayı yükler. Bu eylem sorunu azaltmalıdır.
    * **Windows**  >  **Olay Görüntüleyicisi (Günlükler)**  >  **uygulama ve hizmet günlükleri**  >  **Integration Runtime** gidin ve DNS, güvenlik duvarı kuralı veya şirket ağı ayarlarından kaynaklanan tüm hataları kontrol edin. (Böyle bir hata bulursanız bağlantıyı zorla kapatın.) Her şirket ağ ayarlarını özelleştirdiğinden, bu sorunları gidermek için ağ ekibinize başvurun.

1. Şirket içinde barındırılan tümleştirme çalışma zamanında "proxy" yapılandırılmışsa, proxy sunucunuzun hizmet uç noktasına erişebileceğini doğrulayın. Örnek bir komut için bkz. [PowerShell, Web istekleri ve proxy 'ler](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies).    
                
    ```powershell
    $user = $env:username
    $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
    Settings').ProxyServer
    $pwd = Read-Host "Password?" -assecurestring
    $proxy = new-object System.Net.WebProxy
    $proxy.Address = $webproxy
    $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
    $proxy.credentials = $account
    $url = "https://wu2.frontend.clouddatahub.net/"
    $wc = new-object system.net.WebClient
    $wc.proxy = $proxy
    $webpage = $wc.DownloadData($url)
    $string = [System.Text.Encoding]::ASCII.GetString($webpage)
    $string
    ```

Beklenen yanıt aşağıda verilmiştir:
            
![PowerShell komut yanıtı 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Ara sunucu konuları:
> *    Proxy sunucusunun Güvenli Alıcılar listesine alınması gerekip gerekmediğini denetleyin. Bu durumda, [Bu etki alanlarının](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) Güvenli Alıcılar listesinde olduğundan emin olun.
> *    "Wu2.frontend.clouddatahub.net/" TLS/SSL sertifikasının proxy sunucusunda güvenilir olup olmadığını denetleyin.
> *    Proxy üzerinde Active Directory kimlik doğrulaması kullanıyorsanız, hizmet hesabını "Integration Runtime hizmeti" olarak proxy 'ye erişebilen kullanıcı hesabı olarak değiştirin.

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Hata iletisi: şirket içinde barındırılan tümleştirme çalışma zamanı düğümü/mantıksal dolgu, etkin olmayan/"çalışıyor (sınırlı)" durumunda

#### <a name="cause"></a>Nedeni 

Şirket içinde barındırılan tümleşik çalışma zamanı düğümü, aşağıdaki ekran görüntüsünde gösterildiği gibi **etkin olmayan** bir duruma sahip olabilir:

![Etkin olmayan şirket içinde barındırılan IR düğümü](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Bu davranış, düğümler birbirleriyle iletişim kuramıyorsa oluşur.

#### <a name="resolution"></a>Çözüm

1. Düğüm tarafından barındırılan VM 'de oturum açın. **Uygulama ve hizmet günlükleri**altında  >  **Integration Runtime**, Olay Görüntüleyicisi açın ve tüm hata günlüklerini filtreleyin.

1. Hata günlüğünde aşağıdaki hatayı içerip içermediğini kontrol edin: 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. Bu hatayı görürseniz, komut satırında aşağıdakileri çalıştırın: 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. Aşağıdaki hatayı alıyorsanız, bu sorunu gidermeye yönelik yardım için BT bölümünüze başvurun. Başarılı bir şekilde Telnet 'i başardıktan sonra, tümleştirme çalışma zamanı düğümü durumu ile ilgili sorunlar yaşıyorsanız Microsoft Desteği başvurun.
        
   ![Komut satırı hatası](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. Hata günlüğünde şunları içerip içermediğini kontrol edin:

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. Sorunu çözmek için aşağıdaki yöntemlerden birini veya her ikisini deneyin:
    - Tüm düğümleri aynı etki alanına yerleştirin.
    - Barındırılan tüm VM 'nin ana bilgisayar dosyalarındaki ana bilgisayar eşlemesine IP 'yi ekleyin.

### <a name="connectivity-issue-between-self-hosted-ir-and-data-factory-or-self-hosted-ir-and-data-sourcesink"></a>Şirket içinde barındırılan IR ve Data Factory veya şirket içinde barındırılan IR ve veri kaynağı/havuz arasında bağlantı sorunu

Ağ bağlantısı sorununu gidermek için, ağ izlemesini nasıl toplayacağınızı, nasıl kullanacağınızı anlamanız gerektiğini ve NetMon 'u şirket içinde barındırılan IR 'den gerçek zamanlı olarak uygulamadan önce [Netmon izlemesini nasıl analiz](#how-to-analyze-netmon-trace) edeceğinizi bilmeniz gerekir.

#### <a name="symptoms"></a>Belirtiler

Bazen, şirket içinde barındırılan IR ve Data Factory arasındaki bağlantı sorunlarını giderirken: 

![HTTP isteği başarısız oldu](media/self-hosted-integration-runtime-troubleshoot-guide/http-request-error.png)

Ya da şirket içinde barındırılan IR ve veri kaynağı/havuz arasında aşağıdaki hatalarla karşılaşacağız:

`Copy failed with error:Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server: ‘IP address’`

`One or more errors occurred. An error occurred while sending the request. The underlying connection was closed: An unexpected error occurred on a receive. Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host. An existing connection was forcibly closed by the remote host Activity ID.`

#### <a name="resolution"></a>Çözüm:

Yukarıdaki sorunlar hakkında daha fazla bilgi için aşağıdaki yönergelere bakın:

Netmon izlemesini alın ve daha fazla analiz edin.
- İlk olarak, bir filtreyi sunucudan istemci tarafına herhangi bir sıfırlamayı görmek için ayarlayabilirsiniz. Aşağıdaki örnekte, sunucu tarafının Data Factory sunucu olduğunu görebilirsiniz.

    ![Data Factory sunucusu](media/self-hosted-integration-runtime-troubleshoot-guide/data-factory-server.png)

- Sıfırlama paketini aldığınızda, TCP 'yi izleyerek konuşmayı bulabilirsiniz.

    ![Konuşmayı bul](media/self-hosted-integration-runtime-troubleshoot-guide/find-conversation.png)

- Daha sonra filtreyi kaldırarak istemci ve Data Factory sunucusu arasında dönüştürme yapabilirsiniz.

    ![Konuşmayı al](media/self-hosted-integration-runtime-troubleshoot-guide/get-conversation.png)
- Toplanan Netmon izlemesini temel alan TTL (TimeToLive) toplamının 64 olduğunu söyleyebilir. [Bu makalede](https://packetpushers.net/ip-time-to-live-and-hop-limit-basics/) BAHSEDILEN **varsayılan TTL ve atlama sınırı değerlerine** göre (aşağıda ayıklanan gibi), paketin sıfırlamasına ve bağlantısının kesilmesine neden olan Linux sistem olduğunu görebiliriz.

    Varsayılan TTL ve atlama sınırı değerleri, farklı işletim sistemleri arasında farklılık gösterir, aşağıda varsayılan değer verilmiştir:
    - Linux Kernel 2,4 (Circa 2001): TCP, UDP ve ıCMP için 255
    - Linux Kernel 4,10 (2015): TCP, UDP ve ıCMP için 64
    - Windows XP (2001): TCP, UDP ve ıCMP için 128
    - Windows 10 (2015): TCP, UDP ve ıCMP için 128
    - Windows Server 2008: TCP, UDP ve ıCMP için 128
    - Windows Server 2019 (2018): TCP, UDP ve ıCMP için 128
    - macOS (2001): TCP, UDP ve ıCMP için 64

    ![TTL 61](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-61.png)
    
    Ancak, yukarıdaki örnekte 64 yerine 61 olarak gösterilir. Bu, ağ paketi hedefe ulaştığında yönlendiriciler/ağ cihazları gibi farklı atlamalar arasında geçmesi gerekir. Yönlendirici/ağ cihazlarının sayısı son TTL 'ye kesilecek.
    Bu durumda, sıfırlama 'nın TTL 64 ile Linux sisteminden gönderileolabileceğini görebiliriz.

- Sıfırlama cihazının nereden olabileceğini onaylamak için, kendinden konak IR 'den dördüncü atlamayı denetliyoruz.
 
    *Linux sistem A 'dan TTL 64 ile ağ paketi-> B TTL 64 eksi 1 = 63-> C TTL 63, eksi 1 = 62-> TTL 62 eksi 1 = 61 kendinden konak IR*

- İdeal durumda, TTL 128 olur, bu da Windows sisteminin Data Factory çalıştırdığı anlamına gelir. Aşağıdaki örnekte gösterildiği gibi, *128 – 107 = 21 atlamaları*, TCP 3 el sıkışması sırasında paketin Data Factory 21 ' den kendınden konak IR 'ye gönderildiği anlamına gelir.
 
    ![TTL 107](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-107.png)

    Bu nedenle, dördüncü atlamanın kendinden konak IR 'den ne olduğunu denetlemek için ağ ekibine ihtiyacınız vardır. Bu, Linux sistemi olarak güvenlik duvarıdır, bu nedenle cihazın TCP 3 el sıkışması sonrasında paketi sıfırlamalarında herhangi bir günlüğe bakın. Ancak, araştırmanın nerede olduğundan emin değilseniz, bu paketi hangi cihazın sıfırlayabileceğinizi ve bağlantısının kesilmesine neden olduğunu anlamak için sorunlu süre içinde kendinden konak IR ve güvenlik duvarından alınan Netmon izlemesini almayı deneyin. Bu durumda, ileride ilerlemek için ağ ekibinize de ihtiyacınız vardır.

### <a name="how-to-analyze-netmon-trace"></a>Netmon izlemesini çözümleme

> [!NOTE] 
> Aşağıdaki yönerge, Netmon Trace için geçerlidir. Netmon Trace Şu anda destek dışında olduğundan, Wireshark ' den aynı şekilde yararlanabilirsiniz.

Netmon Trace ile Telnet **8.8.8.8 888** ' i topladığınızda, izlemeyi aşağıdaki şekilde görmeniz gerekir:

![Netmon izleme 1](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-1.png)

![Netmon Trace 2](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-2.png)
 

Bu, **888**numaralı bağlantı noktasına bağlı olarak **8.8.8.8** sunucu tarafında TCP bağlantısı yapamayacağı anlamına gelir; bu nedenle, burada iki **synyeniden aktarım** ek paketi görürsünüz. Kaynak **self-konak2** , ilk pakette **8.8.8.8** ile bağlantı kuramadı, çünkü bağlantı kurmak için devam edecektir.

> [!TIP]
> - **Yükleme filtresi**  ->  **Standart filtre**  ->  **adresleri**  ->  **IPv4 adresleri**' ne tıklayabilirsiniz.
> - Giriş **IPv4. Address = = 8.8.8.8** as Filter ve **Uygula**' ya tıklayın. Bundan sonra, yalnızca yerel makineden hedef **8.8.8.8**iletişimi görürsünüz.

![adresleri filtrele 1](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-1.png)
        
![adresleri filtrele 2](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-2.png)

Aşağıda, iyi bir senaryonun nasıl görüneceğine ilişkin bir örnek gösterilmektedir. 

- Telnet **8.8.8.8 53** herhangi bir sorun olmadan düzgün ÇALıŞıYORSA, TCP 3 el sıkışma olduğunu görebilir ve oturum, TCP 4 el sıkışma ile biter.

    ![iyi senaryo örneği 1](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-1.png)
     
    ![iyi senaryo örneği 2](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-2.png)

- Yukarıdaki TCP 3 el sıkışma temelinde, aşağıdaki iş akışı ' na bakabilirsiniz:

    ![TCP 3 el sıkışma iş akışı](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-3-handshake-workflow.png)
 
- Oturumu tamamlaması için TCP 4 el sıkışması ve iş akışı aşağıdaki gibi gösterilir:

    ![TCP 4 el sıkışma](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake.png)

    ![TCP 4 el sıkışma iş akışı](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake-workflow.png) 


## <a name="self-hosted-ir-sharing"></a>Şirket içinde barındırılan IR paylaşımı

### <a name="share-self-hosted-ir-from-a-different-tenant-is-not-supported"></a>Şirket içinde barındırılan IR 'yi farklı bir kiracıdan paylaşma desteklenmez 

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan IR 'yi Azure Data Factory kullanıcı arabiriminden paylaşmaya çalışırken diğer veri fabrikaları (farklı kiracılarda) fark edebilirsiniz, ancak şirket içinde barındırılan IR 'yi farklı kiracılardaki veri fabrikaları arasında paylaşamaz.

#### <a name="cause"></a>Nedeni

Şirket içinde barındırılan IR, çapraz kiracılar paylaştırılamaz.


## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için aşağıdaki kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Data Factory için yığın taşma Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [Veri akışları eşleme performans Kılavuzu](concepts-data-flow-performance.md)
