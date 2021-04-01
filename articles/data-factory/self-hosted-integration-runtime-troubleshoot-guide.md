---
title: Azure Data Factory içinde şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme
description: Azure Data Factory ' de şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme hakkında bilgi edinin.
author: lrtoyou1223
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 01/25/2021
ms.author: lle
ms.openlocfilehash: 2cb0e0870b32270340e37d54dc54a43b22ee014a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100376471"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki şirket içinde barındırılan tümleştirme çalışma zamanı (IR) için genel sorun giderme yöntemleri incelenmektedir.

## <a name="gather-self-hosted-ir-logs-from-azure-data-factory"></a>Azure Data Factory şirket içinde barındırılan IR günlüklerini toplayın

Şirket içinde barındırılan bir IR veya paylaşılan IR üzerinde çalışan başarısız etkinlikler için, Azure Data Factory hata günlüklerini görüntülemeyi ve yüklemeyi destekler. Hata raporu KIMLIĞINI almak için buradaki yönergeleri izleyin ve ilgili bilinen sorunları aramak için rapor KIMLIĞINI girin.

1. Data Factory, işlem **hattı çalıştırmaları**' nı seçin.

1. **Etkinlik çalıştırmaları** altında, **hata** sütununda, aşağıdaki ekran görüntüsünde gösterildiği gibi etkinlik günlüklerini göstermek için vurgulanan düğmeyi seçin:

    !["Tüm işlem hattı çalıştırmaları" bölmesindeki "etkinlik çalıştırmaları" bölümünün ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/activity-runs-page.png)

    Etkinlik günlükleri, başarısız etkinlik çalıştırması için görüntülenir.

    ![Başarısız etkinliğin etkinlik günlüklerinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/send-logs.png) 
    
1. Daha fazla yardım için **günlükleri Gönder**' i seçin.
 
   **Şirket içinde barındırılan tümleştirme çalışma zamanı (IR) günlüklerini Microsoft** penceresi açılarak paylaşma.

    !["Şirket içinde barındırılan tümleştirme çalışma zamanı (IR) günlüklerini Microsoft 'a paylaşma" penceresinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/choose-logs.png)

1. Hangi günlükleri göndermek istediğinizi seçin. 
    * Şirket içinde *barındırılan BIR IR* için, başarısız etkinlikle ilgili günlükleri veya şirket IÇINDE barındırılan IR düğümündeki tüm günlükleri karşıya yükleyebilirsiniz. 
    * Paylaşılan bir *IR* için yalnızca başarısız etkinlikle ilgili günlükleri karşıya yükleyebilirsiniz.

1. Günlükler karşıya yüklendiğinde, sorunu gidermek için daha fazla yardıma ihtiyacınız varsa, daha sonra kullanmak üzere rapor KIMLIĞININ bir kaydını saklayın.

    ![IR günlükleri için karşıya yükleme ilerleme durumu penceresinde, görünen rapor KIMLIĞININ ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/upload-logs.png)

> [!NOTE]
> Günlük görüntüleme ve karşıya yükleme istekleri, tüm çevrimiçi şirket içinde barındırılan IR örneklerinde yürütülür. Herhangi bir günlük eksikse, tüm şirket içinde barındırılan IR örneklerinin çevrimiçi olduğundan emin olun. 


## <a name="self-hosted-ir-general-failure-or-error"></a>Şirket içinde barındırılan IR genel başarısızlığı veya hatası

### <a name="out-of-memory-issue"></a>Bellek yetersiz sorunu

#### <a name="symptoms"></a>Belirtiler

Bağlı IR veya şirket içinde barındırılan IR ile bir arama etkinliği çalıştırmayı denediğinizde bir OutOfMemoryException (OOM) hatası oluşur.

#### <a name="cause"></a>Nedeni

IR makinesi kopan yüksek bellek kullanımı yaşıyorsa yeni bir etkinlik OOM hatası oluşturabilir. Sorun, büyük bir eş zamanlı etkinliğin oluşmasına neden olmuş olabilir ve hata tasarıma göre yapılır.

#### <a name="resolution"></a>Çözüm

IR düğümünde kaynak kullanımını ve eşzamanlı etkinlik yürütmesini denetleyin. Tek bir IR düğümünde aynı anda çok fazla yürütmeyi önlemek için etkinlik çalıştırmalarının dahili ve tetikleme süresini ayarlayın.

### <a name="concurrent-jobs-limit-issue"></a>Eşzamanlı işleri sınırlama sorunu

#### <a name="symptoms"></a>Belirtiler

Azure Data Factory arabiriminden eşzamanlı iş sınırını arttırmaya çalıştığınızda, işlem *güncelleştirme* durumunda askıda kalır.

Örnek senaryo: en fazla eşzamanlı iş değeri şu anda 24 olarak ayarlanmıştır ve işlerin daha hızlı çalışabilmesi için sayıyı artırmak istiyorsunuz. Girebileceğiniz en küçük değer 3 ' ü ve en büyük değer 32 ' dir. 24 ile 32 arasında bir değer artırırsınız ve sonra **Güncelleştir** düğmesini seçersiniz. Aşağıdaki ekran görüntüsünde gösterildiği gibi işlem, durumu *güncelleştirme* sırasında takılmış olur. Sayfayı yenilerseniz de değer 24 olarak görüntülenir. Beklenen şekilde 32 'e güncelleştirilmedi.

![Tümleştirme çalışma zamanının düğümler bölmesinin ekran görüntüsü, işlemi "güncelleştirme" durumunda kalmış olarak görüntülüyor.](media/self-hosted-integration-runtime-troubleshoot-guide/updating-status.png)

#### <a name="cause"></a>Nedeni

Eşzamanlı iş sayısı sınırı bilgisayarın mantıksal çekirdeğinin ve belleğine bağlıdır. Değeri, 24 gibi bir değere ayarlamayı deneyin ve ardından sonucu görüntüleyin.

> [!TIP] 
> -    Mantıksal çekirdek sayısı hakkında daha fazla bilgi edinmek ve makinenizin mantıksal çekirdek sayısını belirleme hakkında daha fazla bilgi için bkz. [Windows 10 ' da CPU 'inizdeki çekirdek sayısını bulmanın dört yolu](https://www.top-password.com/blog/find-number-of-cores-in-your-cpu-on-windows-10/).
> -    Math. log ' un nasıl hesaplanacağını öğrenmek için, [logaritma hesaplayıcısına](https://www.rapidtables.com/calc/math/Log_Calculator.html)gidin.


### <a name="self-hosted-ir-high-availability-ha-ssl-certificate-issue"></a>Şirket içinde barındırılan IR yüksek kullanılabilirlik (HA) SSL sertifikası sorunu

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan IR çalışma düğümü şu hatayı raporladı:

"Birincil düğümden paylaşılan durumlar alınamadı net. TCP://abc.cloud.corp.Microsoft.com: 8060/ExternalService. svc/. Etkinlik KIMLIĞI: XXXXX X. 509.440 sertifikası CN = ABC. Cloud. Corp. Microsoft. com, OU = test, O = Microsoft zinciri oluşturma başarısız oldu. Kullanılan sertifikanın doğrulanamayan bir güven zinciri vardır. Sertifikayı değiştirin veya certificateValidationMode değerini değiştirin. İptal sunucusu çevrimdışı olduğundan, iptal işlevi iptal etmeyi denetleyemedi. "

#### <a name="cause"></a>Nedeni

Bir SSL/TLS anlaşması ile ilgili olan durumları işlerken, sertifika zinciri doğrulamayla ilgili bazı sorunlarla karşılaşabilirsiniz. 

#### <a name="resolution"></a>Çözüm

- İşte bir X. 509.440 sertifika zinciri derleme başarısızlığının giderilmesi için hızlı ve sezgisel bir yol:
 
    1. Doğrulanması gereken sertifikayı dışarı aktarın. Bunu yapmak için aşağıdakileri yapın:
    
       a. Windows 'ta **Başlat**' ı seçin, **sertifikaları** yazmaya başlayın ve ardından **bilgisayar sertifikalarını Yönet**' i seçin.
       
       b. Dosya Gezgini ' nde, sol bölmede, denetlemek istediğiniz sertifikayı arayın, sağ tıklayın ve **Tüm görevler**  >  **dışarı aktar**' ı seçin.
    
        !["Bilgisayar sertifikalarını yönetme" bölmesindeki bir sertifika için "tüm görevler" > "dışarı aktarma" ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/export-tasks.png)

    2. İçe aktarılmış sertifikayı istemci makinesine kopyalayın. 
    3. İstemci tarafında, bir komut Istemi penceresinde aşağıdaki komutu çalıştırın. *\<certificate path>* Ve ' nin *\<output txt file path>* gerçek yollarla değiştirilmesini unutmayın.
    
        ```
        Certutil -verify -urlfetch    <certificate path>   >     <output txt file path> 
        ```

        Örnek:

        ```
        Certutil -verify -urlfetch c:\users\test\desktop\servercert02.cer > c:\users\test\desktop\Certinfo.txt
        ```
    4. Çıkış TXT dosyasında hata olup olmadığını denetleyin. Hata özetini TXT dosyasının sonunda bulabilirsiniz.

        Örnek: 

        ![TXT dosyasının sonundaki hata özetinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/error-summary.png)

        Günlük dosyasının sonunda, aşağıdaki ekran görüntüsünde gösterildiği gibi bir hata görmüyorsanız, Sertifika zincirinin istemci makinede başarıyla derlendiğine göz önünde bulundurun.
        
        ![Hata olmadığını gösteren bir günlük dosyasının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/log-file.png)      

- Sertifika dosyasında bir AIA (yetkili bilgi erişimi), CDP (CRL dağıtım noktası) veya OCSP (çevrimiçi sertifika durumu Protokolü) dosya adı uzantısı yapılandırılmışsa, daha sezgisel bir şekilde kontrol edebilirsiniz:
 
    1. Aşağıdaki ekran görüntüsünde gösterildiği gibi sertifika ayrıntılarını denetleyerek bu bilgileri alın:
    
        ![Sertifika ayrıntılarının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/certificate-detail.png)
    
    1. Aşağıdaki komutu çalıştırın. *\<certificate path>* Sertifikanın gerçek yoluyla değiştirdiğinizden emin olun.
    
        ```
          Certutil   -URL    <certificate path> 
        ```
    
        URL alma aracı açılır. 
        
    1. AIA, CDP ve OCSP dosya adı uzantılarına sahip sertifikaları doğrulamak için **Al**' ı seçin.

        ![URL alma aracının ve Al düğmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/retrieval-button.png)
 
        AIA 'dan sertifika durumu *doğrulanırsa* ve CDP veya OCSP 'deki sertifika durumu *doğrulandıktan* sonra sertifika zincirini başarıyla oluşturdunuz.

        AIA veya CDP 'yi almaya çalıştığınızda başarısız olursa, istemci makinesini hedef URL 'ye bağlamaya hazırlamak için ağ ekibinizle birlikte çalışın. HTTP yolu veya hafif Dizin Erişim Protokolü (LDAP) yolu doğrulanırsa bu yeterli olacaktır.

### <a name="self-hosted-ir-could-not-load-file-or-assembly"></a>Şirket içinde barındırılan IR dosyayı veya derlemeyi yükleyemedi

#### <a name="symptoms"></a>Belirtiler

Aşağıdaki hata iletisini alırsınız:

"Dosya veya derleme ' XXXXXXXXXXXXXXXX, sürüm = 4.0.2.0, Culture = neutral, PublicKeyToken = XXXXXXXXX ' veya bunun bağımlılıklarından biri yüklenemedi. Sistem belirtilen dosyayı bulamıyor. Etkinlik KIMLIĞI: 92693b45-B4BF-4FC8-89da-2d3dc56f27c3 "
 
Daha belirli bir hata iletisi aşağıda verilmiştir: 

"Dosya veya derleme ' System. ValueTuple, sürüm = 4.0.2.0, Culture = neutral, PublicKeyToken = XXXXXXXXX ' veya bağımlılıklarından biri yüklenemedi. Sistem belirtilen dosyayı bulamıyor. Etkinlik KIMLIĞI: 92693b45-B4BF-4FC8-89da-2d3dc56f27c3 "

#### <a name="cause"></a>Nedeni

Işlem Izleyicisi 'nde, aşağıdaki sonucu görebilirsiniz:

[![Işlem Izleyicisinde yol listesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/process-monitor.png)](media/self-hosted-integration-runtime-troubleshoot-guide/process-monitor.png#lightbox)

> [!TIP] 
> Işlem Izleyicisi 'nde, aşağıdaki ekran görüntüsünde gösterildiği gibi filtreler ayarlayabilirsiniz.
>
> Yukarıdaki hata iletisinde, DLL System. ValueTuple 'nin ilgili *genel derleme önbelleği* (GAC) klasöründe, *C:\Program Files\Microsoft Integration Runtime\4.0\Gateway* klasöründe veya *c:\Program Files\Microsoft Integration Runtime\4.0\Shared* klasöründe yer aldığı açıklanıyor.
>
> Temel olarak, işlem DLL dosyasını önce *GAC* klasöründen, sonra *paylaşılan* klasörden ve son olarak *ağ geçidi* klasöründen yükler. Bu nedenle, DLL 'yi faydalı olan herhangi bir yoldan yükleyebilirsiniz.

<br>

![DLL filtrelerini listeleyerek "Işlem Izleyicisi filtresi" sayfasının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/set-filters.png)

#### <a name="resolution"></a>Çözüm

*System.ValueTuple.dll* dosyasını *C:\Program Files\Microsoft Integration Runtime\4.0\Gateway\DataScan* klasöründe bulabilirsiniz. Sorunu çözmek için *System.ValueTuple.dll* dosyasını *C:\Program Files\Microsoft Integration Runtime\4.0\Gateway* klasörüne kopyalayın.

Aynı yöntemi, diğer eksik dosya veya derleme sorunlarını gidermek için de kullanabilirsiniz.

#### <a name="more-information-about-this-issue"></a>Bu sorun hakkında daha fazla bilgi

*%Windir%\Microsoft.NET\assembly* ve *%windir%\assembly* altında *System.ValueTuple.dll* görmenizin nedeni bunun .net davranışıdır. 

Aşağıdaki hatada, *System. ValueTuple* derlemesinin eksik olduğunu net bir şekilde görebilirsiniz. Bu sorun, uygulama *System.ValueTuple.dll* derlemeyi denetlemeye çalıştığında ortaya çıkar.
 
" \<LogProperties> \<ErrorInfo> [{" Code ": 0," Message ":" ' Npgsql. poolmanager ' için tür başlatıcısı bir özel durum oluşturdu. "," EventType ": 0," Category ": 5," Data ": {} ," MsgID ": null," ExceptionType ":" System. TypeInitializationException "," kaynak ":" npgsql "," StackTrace ":" "," ınnereventinfos ": [{" Code ": 0," Message ":" dosya veya derleme ' System. ValueTuple, sürüm = 4.0.2.0, Culture = neutral, PUBLICKEYTOKEN = xxxxxxxxx ' veya bağımlılıklarından biri yüklenemedi. Sistem belirtilen dosyayı bulamıyor. "," EventType ": 0," Kategori ": 5," veri ": {} ," MsgId":null,"ExceptionType":"System. IO. FileNotFoundException "," kaynak ":" Npgsql "," StackTrace ":" "," ınnereventınfos ": []}]}] \</ErrorInfo> \</LogProperties> "
 
GAC hakkında daha fazla bilgi için bkz. [genel derleme önbelleği](/dotnet/framework/app-domains/gac).


### <a name="self-hosted-integration-runtime-authentication-key-is-missing"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı kimlik doğrulama anahtarı eksik

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan tümleştirme çalışma zamanı, bir kimlik doğrulama anahtarı olmadan aniden çevrimdışı duruma geçer ve olay günlüğü şu hata iletisini görüntüler: 

"Kimlik doğrulama anahtarı henüz atanmadı"

![Tümleştirme çalışma zamanı olay bölmesinin, kimlik doğrulama anahtarının henüz atanmadığını gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/key-missing.png)

#### <a name="cause"></a>Nedeni

- Azure portal içinde şirket içinde barındırılan IR düğümü veya mantıksal olarak barındırılan bir IR silindi.
- Temiz bir kaldırma işlemi gerçekleştirildi.

#### <a name="resolution"></a>Çözüm

Yukarıdaki nedenlerin hiçbiri geçerli değilse, *yapılandırmaların* dosyasının silinip silinmediğini görmek için *%ProgramData%\microsoft\data Transfer\DataManagementGateway* klasörüne gidebilirsiniz. Silinmişse, [Windows dosya sunucularınızda bir dosyayı kimin silindiğini algılayan](https://www.netwrix.com/how_to_detect_who_deleted_file.html)Netwrix makalesindeki yönergeleri izleyin.

![Yapılandırma dosyasını denetlemek için olay günlüğü ayrıntılar bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/configurations-file.png)


### <a name="cant-use-self-hosted-ir-to-bridge-two-on-premises-datastores"></a>İki şirket içi veri deposunu köprülemek için kendi kendine barındırılan IR kullanılamaz

#### <a name="symptoms"></a>Belirtiler

Hem kaynak hem de hedef veri depoları için şirket içinde barındırılan IRS oluşturduktan sonra, iki IRS 'yi bir kopyalama etkinliğini tamamlayacak şekilde bağlamak istersiniz. Veri depoları farklı sanal ağlarda yapılandırılmışsa veya veri depoları ağ geçidi mekanizmasını anladıysanız, aşağıdaki hatalardan birini alırsınız: 

* "Kaynak sürücüsü hedef IR 'de bulunamıyor"
* "Kaynak hedef IR tarafından erişilemez"
 
#### <a name="cause"></a>Nedeni

Şirket içinde barındırılan IR, her bir veri deposu için yüklenmesi gereken bir istemci aracısına değil, kopyalama etkinliğinin merkezi bir düğümü olarak tasarlanmıştır.
 
Bu durumda, aynı IR ile her bir veri deposu için bağlı hizmeti oluşturmanız gerekir ve IR ağ üzerinden her iki veri deposuna de erişebilmelidir. IR 'nin kaynak veri deposuna veya hedef veri deposuna ya da üçüncü bir makineye yüklenip yüklenmediğini hiçbir zaman değildir. Farklı IRS ile iki bağlı hizmet oluşturulmuşsa, ancak aynı kopyalama etkinliğinde kullanılırsa, hedef IR kullanılır ve hedef IR makinesindeki her iki veri deposu için de sürücüleri yüklemeniz gerekir.

#### <a name="resolution"></a>Çözüm

Hedef IR üzerindeki kaynak ve hedef veri depoları için sürücüleri yükler ve kaynak veri deposuna erişebilmesini sağlayın.
 
Trafik iki veri deposu arasında ağ üzerinden geçemezse (örneğin, iki sanal ağda yapılandırılmışsa), bir etkinliğin kopyalanmasını, bu da yüklü IR ile birlikte bitiremeyebilirsiniz. Kopyalamayı tek bir etkinlikte tamamlayamıyorum, her biri bir VENT halinde iki IRS ile iki adet kopyalama etkinliği oluşturabilirsiniz: 
* Veri deposu 1 ' den Azure Blob depolamaya bir IR kopyalama
* Azure Blob depolama alanındaki başka bir IR veri deposu 2 ' ye kopyalayın. 

Bu çözüm, iki bağlantısı kesilen veri deposunu bağlayan bir köprü oluşturmak için IR kullanma gereksiniminin benzetimini yapar.


### <a name="credential-sync-issue-causes-credential-loss-from-ha"></a>Kimlik bilgisi eşitleme sorunu, HA 'dan kimlik bilgisi kaybına neden oluyor

#### <a name="symptoms"></a>Belirtiler

Veri kaynağı kimlik bilgisi "XXXXXXXXXX", geçerli tümleştirme çalışma zamanı düğümünden yük ile silinirse, aşağıdaki hata iletisini alırsınız:

"Azure portal bağlantı hizmetini sildiğinizde veya görevde yanlış yük varsa, lütfen kimlik bilgilerinizi yeniden kullanarak yeni bağlantı hizmeti oluşturun."

#### <a name="cause"></a>Nedeni

Şirket içinde barındırılan IR, iki düğüm ile HA modunda oluşturulmuştur, ancak düğümler kimlik bilgileri eşitleme durumunda değildir. Bu, dağıtıcı düğümünde depolanan kimlik bilgilerinin diğer çalışan düğümleriyle eşitlenmediği anlamına gelir. Dağıtıcı düğümünden çalışan düğümüne herhangi bir yük devretme olursa ve kimlik bilgileri yalnızca önceki dağıtıcı düğümünde mevcutsa, kimlik bilgilerine erişmeye çalışırken görev başarısız olur ve yukarıdaki hatayı alırsınız.

#### <a name="resolution"></a>Çözüm

Bu sorunu önlemenin tek yolu, iki düğümün kimlik bilgileri eşitleme durumunda olduğundan emin olmak içindir. Bunlar eşitlenmemişse, yeni dağıtıcı için kimlik bilgilerini yeniden girmeniz gerekir.


### <a name="cant-choose-the-certificate-because-the-private-key-is-missing"></a>Özel anahtar eksik olduğundan sertifika seçemezsiniz

#### <a name="symptoms"></a>Belirtiler

* Sertifika deposuna bir PFX dosyası aldınız.
* Sertifikayı IR Configuration Manager kullanıcı arabiriminden seçtiğinizde, aşağıdaki hata iletisini aldınız:

   "İntranet iletişim şifreleme modu değiştirilemedi. ' ' Sertifikası, \<*certificate name*> anahtar değişimi yeteneğine sahip bir özel anahtara sahip olmayabilir veya işlem özel anahtar için erişim haklarına sahip olmayabilir. Lütfen ayrıntılı bilgi için iç özel duruma bakın. "

    ![Integration Runtime Configuration Manager ayarları bölmesinin ekran görüntüsü, "özel anahtar eksik" hata iletisi görüntülüyor.](media/self-hosted-integration-runtime-troubleshoot-guide/private-key-missing.png)

#### <a name="cause"></a>Nedeni

- Kullanıcı hesabının düşük ayrıcalıklı düzeyi vardır ve özel anahtara erişemez.
- Sertifika, anahtar değişimi olarak değil, imza olarak üretildi.

#### <a name="resolution"></a>Çözüm

* Kullanıcı arabirimini çalıştırmak için özel anahtara erişim için uygun ayrıcalıklara sahip bir hesap kullanın.  
* Aşağıdaki komutu çalıştırarak sertifikayı içeri aktarın:
    
    ```
    certutil -importpfx FILENAME.pfx AT_KEYEXCHANGE
    ```

## <a name="self-hosted-ir-setup"></a>Şirket içinde barındırılan IR kurulumu

### <a name="integration-runtime-registration-error"></a>Tümleştirme çalışma zamanı kayıt hatası 

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan bir IR 'yi aşağıdaki nedenlerden biri için farklı bir hesapta çalıştırmak isteyebilirsiniz:
- Şirket ilkesi, hizmet hesabına izin vermez.
- Bazı kimlik doğrulaması gereklidir.

Hizmet bölmesinde hizmet hesabını değiştirdikten sonra, tümleştirme çalışma zamanının çalışmayı durdurduğunu ve aşağıdaki hata iletisini alırsınız:

"Integration Runtime (şirket içinde barındırılan) düğümü kayıt sırasında bir hatayla karşılaştı. Integration Runtime (şirket içinde barındırılan) ana bilgisayar hizmetine bağlanılamıyor. "

![IR kayıt hatası gösteren Integration Runtime Configuration Manager penceresinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/ir-registration-error.png)

#### <a name="cause"></a>Nedeni

Birçok kaynak yalnızca hizmet hesabına verilir. Hizmet hesabını başka bir hesapla değiştirdiğinizde, tüm bağımlı kaynakların izinleri değişmeden kalır.

#### <a name="resolution"></a>Çözüm

Hatayı denetlemek için Integration Runtime olay günlüğüne gidin.

![IR olay günlüğü 'nün bir çalışma zamanı hatası oluştuğunu gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/ir-event-log.png)

* Olay günlüğündeki hata "UnauthorizedAccessException" ise şunları yapın:

    1. Windows hizmeti panelinde *Diahostservice* oturum açma hizmeti hesabını denetleyin.

        ![Oturum açma hizmeti hesabı Özellikler bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/logon-service-account.png)

    1. Oturum açma hizmeti hesabının *%ProgramData%\microsoft\datatransfer\datamanagementgateway* klasörü için okuma/yazma izinlerine sahip olup olmadığını denetleyin.

        - Varsayılan olarak, hizmet oturum açma hesabı değiştirilmediyse, okuma/yazma izinlerine sahip olmalıdır.

            ![Hizmet izinleri bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/service-permission.png)

        - Hizmet oturum açma hesabını değiştirdiyseniz, aşağıdakileri yaparak sorunu azaltabilirsiniz:
 
            a. Geçerli şirket içinde barındırılan IR 'nin temiz bir yüklemesini gerçekleştirin.   
            b. Şirket içinde barındırılan IR bitlerini yükler.  
            c. Aşağıdakileri yaparak hizmet hesabını değiştirin:  

             i. Şirket içinde barındırılan IR yükleme klasörüne gidin ve *Microsoft Integration Runtime\4.0\Shared* klasörüne geçin.  
             ii. Yükseltilmiş ayrıcalıkları kullanarak bir komut Istemi penceresi açın. *\<user>* Ve *\<password>* kendi Kullanıcı adınız ve parolanızla değiştirin ve ardından aşağıdaki komutu çalıştırın:   
                `dmgcmd.exe -SwitchServiceAccount "<user>" "<password>"`  
             iii. LocalSystem hesabına geçmek istiyorsanız, bu hesap için doğru biçimi kullandığınızdan emin olun: `dmgcmd.exe -SwitchServiceAccount "NT Authority\System" ""`  
                Bu *biçimi kullanmayın:*`dmgcmd.exe -SwitchServiceAccount "LocalSystem" ""`     
             IV. İsteğe bağlı olarak, yerel sistem yöneticiden daha yüksek ayrıcalıklara sahip olduğundan, bunu "Hizmetler" de doğrudan değiştirebilirsiniz.  
             v. IR hizmeti oturum açma hesabı için bir yerel/etki alanı kullanıcısı kullanabilirsiniz.            

            d. Tümleştirme çalışma zamanını kaydedin.

* Hata "hizmet ' Integration Runtime hizmeti ' (DIAHostService) başlatılamadı. Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip olduğunuzu doğrulayın, "şunları yapın:

    1. Windows hizmeti panelinde *Diahostservice* oturum açma hizmeti hesabını denetleyin.
    
        ![Hizmet hesabı için "oturum aç" bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/logon-service-account.png)

    1. Windows hizmetini başlatmak için oturum açma hizmeti hesabının **hizmet olarak oturum** açma izni olup olmadığını denetleyin:

        !["Hizmet olarak oturum aç" Özellikler bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/logon-as-service.png)

#### <a name="more-information"></a>Daha fazla bilgi

Büyük/küçük çözüm desenlerinden hiçbiri geçerliyse, aşağıdaki Windows olay günlüklerini toplamayı deneyin: 
- Uygulama ve hizmet günlükleri > Integration Runtime
- Windows günlükleri > uygulaması

### <a name="cant-find-the-register-button-to-register-a-self-hosted-ir"></a>Kendi kendine barındırılan bir IR kaydetmek için Kaydet düğmesi bulunamıyor    

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan bir IR kaydettiğinizde, Configuration Manager bölmesinde **Kaydet** düğmesi gösterilmez.

![Tümleştirme çalışma zamanı düğümünün kaydettirilmediği bir ileti gösteren Configuration Manager bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/no-register-button.png)

#### <a name="cause"></a>Nedeni

Integration Runtime 3,0 sürümünden itibaren, bir temizleyici ve daha güvenli bir ortamı etkinleştirmek için mevcut tümleştirme çalışma zamanı düğümlerinde **Kaydet** düğmesi kaldırılmıştır. Bir düğüm bir tümleştirme çalışma zamanına kaydedilmişse, çevrimiçi olup olmadığı, önceki düğümü kaldırarak başka bir tümleştirme çalışma zamanına yeniden kaydedin ve düğümü yükleyip kaydedin.

#### <a name="resolution"></a>Çözüm

1. Denetim Masası 'nda, mevcut tümleştirme çalışma zamanını kaldırın.

    > [!IMPORTANT] 
    > Aşağıdaki işlemde **Evet**' i seçin. Kaldırma işlemi sırasında verileri değiştirmeyin.

    ![Tümleştirme çalışma zamanından tüm Kullanıcı verilerini silmek için "Evet" düğmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/delete-data.png)

1. Integration Runtime Installer MSI dosyanız yoksa, en son tümleştirme çalışma zamanını indirmek için [indirme merkezi](https://www.microsoft.com/en-sg/download/details.aspx?id=39717) ' ne gidin.
1. MSI dosyasını yükleyip tümleştirme çalışma zamanını kaydettirin.


### <a name="unable-to-register-the-self-hosted-ir-because-of-localhost"></a>Localhost nedeniyle Şirket içinde barındırılan IR kaydedilemiyor    

#### <a name="symptoms"></a>Belirtiler

Get_LoopbackIpOrName kullandığınızda şirket içinde barındırılan IR 'yi yeni bir makineye kaydedemedik.

**Hata Ayıkla:** Çalışma zamanı hatası oluştu.
'Microsoft.DataTransfer.DIAgentHost.DataSourceCache' tür başlatıcısı özel durum döndürdü.
Veritabanı araması sırasında kurtarılamaz bir hata oluştu.
 
**Özel durum ayrıntısı:** System. TypeInitializationException: ' Microsoft. DataTransfer. DIAgentHost. DataSourceCache ' için tür başlatıcısı özel durum oluşturdu. ---> sistemi .net. Sockets. SocketException: System .net. DNS. GetAddrInfo (dize adı) konumundaki bir veritabanı araması sırasında kurtarılamaz bir hata oluştu.

#### <a name="cause"></a>Nedeni

Bu sorun genellikle localhost çözümlendiğinde oluşur.

#### <a name="resolution"></a>Çözüm

Dosyayı barındırmak ve sorunu çözmek için 127.0.0.1 localhost IP adresini kullanın.

### <a name="self-hosted-setup-failed"></a>Şirket içinde barındırılan kurulum başarısız oldu    

#### <a name="symptoms"></a>Belirtiler

Mevcut bir IR 'yi kaldıramıyorsunuz, yeni bir IR yükleyemiyor veya mevcut bir IR 'yi yeni bir IR 'ye yükseltemedik.

#### <a name="cause"></a>Nedeni

Integration Runtime yüklemesi Windows Installer hizmetine bağlıdır. Aşağıdaki nedenlerden dolayı yükleme sorunlarıyla karşılaşabilirsiniz:
- Yeterli kullanılabilir disk alanı yok.
- İzinlerin bulunmaması.
- Windows NT hizmeti kilitli.
- CPU kullanımı çok yüksek.
- MSI dosyası yavaş bir ağ konumunda barındırılır.
- Bazı sistem dosyalarına veya kayıt defterlerine istem dışı olarak dokunulmadı.

### <a name="the-ir-service-account-failed-to-fetch-certificate-access"></a>IR hizmeti hesabı sertifika erişimini getiremedi

#### <a name="symptoms"></a>Belirtiler

Microsoft Integration Runtime Configuration Manager aracılığıyla şirket içinde barındırılan bir IR yüklediğinizde, güvenilir bir sertifika yetkilisi (CA) içeren bir sertifika oluşturulur. İki düğüm arasındaki iletişimi şifrelemek için sertifika uygulanamadı ve aşağıdaki hata iletisi görüntülenir: 

"Intranet iletişim şifreleme modu değiştirilemedi: Integration Runtime hizmet hesabına ' ' sertifikası erişimi verilemedi \<*certificate name*> . Hata kodu 103 "

![Hata iletisini görüntüleyen ekran görüntüsü "... Integration Runtime Service Account sertifikası erişimi sağlanamadı.](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-account-certificate-error.png)

#### <a name="cause"></a>Nedeni

Sertifika, henüz desteklenmeyen anahtar depolama sağlayıcısı (KSP) depolama alanını kullanıyor. Güncel, şirket içinde barındırılan IR yalnızca şifreleme hizmeti sağlayıcısı (CSP) depolamayı destekler.

#### <a name="resolution"></a>Çözüm

Bu durumda CSP sertifikalarını kullanmanızı öneririz.

**Çözüm 1** 

Sertifikayı içeri aktarmak için aşağıdaki komutu çalıştırın:

`Certutil.exe -CSP "CSP or KSP" -ImportPFX FILENAME.pfx`

![Sertifikayı içeri aktarmaya yönelik certutil komutunun ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/use-certutil.png)

**Çözüm 2** 

Sertifikayı dönüştürmek için aşağıdaki komutları çalıştırın:

`openssl pkcs12 -in .\xxxx.pfx -out .\xxxx_new.pem -password pass: <EnterPassword>`
`openssl pkcs12 -export -in .\xxxx_new.pem -out xxxx_new.pfx`

Dönüştürmeden önce ve sonra:

![Sertifika dönüşümden önceki sonucun ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/before-certificate-change.png)

![Sertifika dönüşümden sonraki sonucun ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/after-certificate-change.png)

### <a name="self-hosted-integration-runtime-version-5x"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı sürüm 5. x
Azure Data Factory şirket içinde barındırılan tümleştirme çalışma zamanının 5. x sürümüne yükseltme için, **.NET Framework Runtime 4.7.2** veya üzeri bir sürüm gerekir. İndirme sayfasında, en son 4. x sürümü ve en son iki 5. x sürümü için karşıdan yükleme bağlantıları bulacaksınız. 

Azure Data Factory v2 müşterileri için:
- Otomatik güncelleştirme açık ise ve .NET Framework çalışma zamanını 4.7.2 veya sonraki bir sürüme yükseltmişseniz, şirket içinde barındırılan tümleştirme çalışma zamanı otomatik olarak en son 5. x sürümüne yükseltilir.
- Otomatik güncelleştirme açık ise ve .NET Framework çalışma zamanını 4.7.2 veya sonraki bir sürüme yükseltmemiş olmanız durumunda, şirket içinde barındırılan tümleştirme çalışma zamanı, en son 5. x sürümüne otomatik olarak yükseltilmez. Şirket içinde barındırılan tümleştirme çalışma zamanı geçerli 4. x sürümünde kalır. Portalda ve şirket içinde barındırılan tümleştirme çalışma zamanı istemcisinde .NET Framework çalışma zamanı yükseltmesi için bir uyarı görebilirsiniz.
- Otomatik Güncelleştirme kapalıysa ve .NET Framework çalışma zamanını 4.7.2 veya sonraki bir sürüme yükselttiyseniz, en son 5. x sürümünü el ile indirebilir ve makinenize yükleyebilirsiniz.
- Otomatik Güncelleştirme kapalıysa ve .NET Framework çalışma zamanını 4.7.2 veya sonraki bir sürüme yükseltmemiş olursunuz. Şirket içinde barındırılan Integration Runtime 5. x 'i el ile yüklemeye ve anahtarı kaydetmeye çalıştığınızda, önce .NET Framework çalışma zamanı sürümünüzü yükseltmeniz gerekir.


Azure Data Factory v1 müşterileri için:
- Şirket içinde barındırılan tümleştirme çalışma zamanı 5. X Azure Data Factory v1 'yi desteklemez.
- Şirket içinde barındırılan tümleştirme çalışma zamanı, otomatik olarak en son 4. x sürümüne yükseltilir. Ve 4. x ' in en son sürümü sona ermez. 
- Şirket içinde barındırılan tümleştirme çalışma zamanı 5. x 'i el ile yüklemeye çalışırsanız ve anahtarı kaydettiğinizde, şirket içinde barındırılan tümleştirme çalışma zamanı 5. x Azure Data Factory v1 'yi desteklemez.


## <a name="self-hosted-ir-connectivity-issues"></a>Şirket içinde barındırılan IR bağlantı sorunları

### <a name="self-hosted-integration-runtime-cant-connect-to-the-cloud-service"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı bulut hizmetine bağlanamıyor

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan tümleştirme çalışma zamanını kaydettirmeye çalıştığınızda Configuration Manager aşağıdaki hata iletisini görüntüler:

"Integration Runtime (şirket içinde barındırılan) düğümü kayıt sırasında bir hatayla karşılaştı."

!["Integration Runtime (şirket içinde barındırılan) düğümü kayıt sırasında bir hatayla karşılaştı" iletisi ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni 

Şirket içinde barındırılan IR, Azure Data Factory hizmet arka ucuna bağlanamaz. Bu sorun genellikle güvenlik duvarındaki ağ ayarlarından kaynaklanır.

#### <a name="resolution"></a>Çözüm

1. Integration Runtime hizmetinin çalışıp çalışmadığını denetleyin. Varsa 2. adıma gidin.
    
   ![Şirket içinde barındırılan IR hizmetinin çalıştığını gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Varsayılan ayar olan, şirket içinde barındırılan IR üzerinde yapılandırılmış bir proxy yoksa, şirket içinde barındırılan tümleştirme çalışma zamanının yüklendiği makinede aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > Service URL 'SI, Data Factory örneğinizin konumuna bağlı olarak farklılık gösterebilir. Hizmet URL 'sini bulmak için **ADF Kullanıcı arabirimi**  >  **bağlantıları** tümleştirme çalışma zamanları ' nı seçin  >    >  **Şirket içinde barındırılan IR**  >  **düğümlerini** düzenleme  >  **hizmeti URL 'lerini** seçin.
            
    Beklenen yanıt aşağıda verilmiştir:
            
    ![PowerShell komut yanıtının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Beklediğiniz yanıtı almazsanız, uygun şekilde aşağıdaki yöntemlerden birini kullanın:
            
    * "Uzak ad çözümlenemedi" iletisini alırsanız, bir etki alanı adı sistemi (DNS) sorunu vardır. Sorunu çözmesi için ağ ekibinize başvurun.
    * "SSL/TLS sertifikası güvenilir değil" iletisini alırsanız, makinede güvenilir olup olmadığını görmek için [sertifikayı kontrol](https://wu2.frontend.clouddatahub.net/) edin ve ardından Sertifika Yöneticisi 'ni kullanarak ortak sertifikayı yükler. Bu eylem sorunu azaltmalıdır.
    * **Windows**  >  **Olay Görüntüleyicisi (Günlükler)**  >  **uygulama ve hizmet günlükleri**  >  **Integration Runtime** gidin ve DNS, güvenlik duvarı kuralı veya şirket ağı ayarlarından kaynaklanan herhangi bir hata olup olmadığını denetleyin. Böyle bir hata bulursanız bağlantıyı zorla kapatın. Her şirketin kendi özelleştirilmiş ağ ayarları olduğundan, bu sorunları gidermek için ağ ekibinize başvurun.

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
            
![Beklenen PowerShell komut yanıtının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Ara sunucu konuları:
> * Proxy sunucusunun Güvenli Alıcılar listesine alınması gerekip gerekmediğini denetleyin. Bu durumda, [Bu etki alanlarının](./data-movement-security-considerations.md#firewall-requirements-for-on-premisesprivate-network) Güvenli Alıcılar listesinde olduğundan emin olun.
> * "Wu2.frontend.clouddatahub.net/" SSL/TLS sertifikasının proxy sunucuda güvenilir olup olmadığını denetleyin.
> * Proxy üzerinde Active Directory kimlik doğrulaması kullanıyorsanız, hizmet hesabını "Integration Runtime hizmeti" olarak proxy 'ye erişebilen kullanıcı hesabı olarak değiştirin.

### <a name="error-message-self-hosted-integration-runtime-nodelogical-self-hosted-ir-is-in-inactive-running-limited-state"></a>Hata iletisi: şirket içinde barındırılan tümleştirme çalışma zamanı düğümü/mantıksal şirket içinde barındırılan IR, etkin olmayan/"çalışıyor (sınırlı)" durumunda

#### <a name="cause"></a>Nedeni 

Şirket içinde barındırılan tümleşik çalışma zamanı düğümü, aşağıdaki ekran görüntüsünde gösterildiği gibi, **etkin olmayan** bir duruma sahip olabilir:

![Etkin olmayan durumda olan, şirket içinde barındırılan tümleşik çalışma zamanı düğümünün ekran görüntüsü](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Bu davranış, düğümler birbirleriyle iletişim kuramıyorsa oluşur.

#### <a name="resolution"></a>Çözüm

1. Düğüm tarafından barındırılan sanal makinede (VM) oturum açın. **Uygulama ve hizmet günlükleri** altında  >  **Integration Runtime**, Olay Görüntüleyicisi açın ve hata günlüklerini filtreleyin.

1. Bir hata günlüğünde aşağıdaki hatayı içerip içermediğini kontrol edin: 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. Bu hatayı görürseniz, bir komut Istemi penceresinde aşağıdaki komutu çalıştırın: 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. Aşağıdaki ekran görüntüsünde gösterilen "ana bilgisayara bağlantı açılamadı" komut satırı hatası alırsanız, bu sorunu gidermeye yardımcı olması için BT bölümünüze başvurun. Başarılı bir şekilde Telnet 'i daha sonra tümleştirme çalışma zamanı düğümü durumuyla ilgili sorun yaşıyorsanız Microsoft Desteği başvurun.
        
   !["Ana bilgisayara bağlantı açılamadı" komut satırı hatası ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. Hata günlüğünün aşağıdaki girişi içerip içermediğini kontrol edin:

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. Sorunu çözmek için aşağıdaki yöntemlerden birini veya her ikisini deneyin:
    - Tüm düğümleri aynı etki alanına yerleştirin.
    - Barındırılan tüm VM 'nin ana bilgisayar dosyalarındaki ana bilgisayar eşlemesine IP 'yi ekleyin.

### <a name="connectivity-issue-between-the-self-hosted-ir-and-your-data-factory-instance-or-the-self-hosted-ir-and-the-data-source-or-sink"></a>Şirket içinde barındırılan IR ile Data Factory örneğiniz veya şirket içinde barındırılan IR ile veri kaynağı veya havuz arasındaki bağlantı sorunu

Ağ bağlantısı sorununu gidermek için, ağ izlemesini nasıl toplayacağınızı, nasıl kullanacağınızı anlamanız ve [Microsoft Ağ İzleyicisi (Netmon) izlemesini](#analyze-the-netmon-trace) , şirket IÇINDE barındırılan IR 'den gerçek bir şekilde uygulamadan önce nasıl analiz edeceğinizi bilmeniz gerekir.

#### <a name="symptoms"></a>Belirtiler

Aşağıdaki ekran görüntüsünde veya şirket içinde barındırılan IR ile veri kaynağı veya havuz arasında gösterildiği gibi, şirket içinde barındırılan IR ve Data Factory örneğiniz arasındaki bazı bağlantı sorunlarını gidermeniz gerekebilir. 

!["Işlenmiş HTTP isteği başarısız oldu" iletisinin ekran görüntüsü](media/self-hosted-integration-runtime-troubleshoot-guide/http-request-error.png)

Her iki örnekte de aşağıdaki hatalarla karşılaşabilirsiniz:

* "Kopyalama hatayla başarısız oldu: tür = Microsoft. DataTransfer. Common. Shared. HybridDeliveryException, Message = SQL Server bağlanamaz: ' IP adresi '"

* "Bir veya daha fazla hata oluştu. İstek gönderilirken bir hata oluştu. Temel alınan bağlantı kapatıldı: alma sırasında beklenmeyen bir hata oluştu. Aktarım bağlantısından veri okunamıyor: mevcut bir bağlantı uzak ana bilgisayar tarafından zorla kapatıldı. Mevcut bir bağlantı, uzak ana bilgisayar etkinlik KIMLIĞI tarafından zorla kapatıldı. "

#### <a name="resolution"></a>Çözüm

Yukarıdaki hatalarla karşılaştığınızda, bu bölümdeki yönergeleri izleyerek sorun giderin.

- Analiz için bir Netmon izlemesi toplayın: 

    1. Sunucudan istemci tarafına bir sıfırlama görmek için filtreyi ayarlayabilirsiniz. Aşağıdaki örnek ekran görüntüsünde, sunucu tarafının Data Factory sunucusu olduğunu görebilirsiniz.

        ![Data Factory sunucusunun ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/data-factory-server.png)

    1. Sıfırlama paketini aldığınızda Iletim Denetimi Protokolü (TCP) öğesini izleyerek konuşmayı bulabilirsiniz.

        ![TCP görüşmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/find-conversation.png)

    1. Filtreyi kaldırarak istemciyle Data Factory sunucusu arasındaki konuşmayı alın.

        ![Görüşme ayrıntılarının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/get-conversation.png)

- Topladığınız Netmon izlemesinin bir analizi, yaşam süresi (TTL)) toplamının 64 olduğunu gösterir. Aşağıdaki listede ayıklanan [IP yaşam süresi (TTL) ve atlama sınırı temel kavramları](https://packetpushers.net/ip-time-to-live-and-hop-limit-basics/) makalesinde bahsedilen değerlere göre, paketin paketini sıfırlayan ve bağlantısının kesilmesine neden olan Linux sistem olduğunu görebilirsiniz.

    Varsayılan TTL ve atlama sınırı değerleri, burada listelenen farklı işletim sistemleri arasında farklılık gösterir:
    - Linux Kernel 2,4 (Circa 2001): TCP, Kullanıcı Datagram Protokolü (UDP) ve Internet Denetim Iletisi Protokolü (ıCMP) için 255
    - Linux Kernel 4,10 (2015): TCP, UDP ve ıCMP için 64
    - Windows XP (2001): TCP, UDP ve ıCMP için 128
    - Windows 10 (2015): TCP, UDP ve ıCMP için 128
    - Windows Server 2008: TCP, UDP ve ıCMP için 128
    - Windows Server 2019 (2018): TCP, UDP ve ıCMP için 128
    - macOS (2001): TCP, UDP ve ıCMP için 64

    ![61 TTL değerini gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-61.png)
    
    Yukarıdaki örnekte, ağ paketi hedefine ulaştığında, yönlendirici veya ağ cihazları gibi çeşitli sıçramaları yapması gerektiğinden, TTL 64 yerine 61 olarak gösterilir. Son TTL 'nin üretilmesi için yönlendirici veya ağ cihazlarının sayısı kesilmez.
    
    Bu durumda, bir sıfırlama 'nın TTL 64 ile Linux sisteminden gönderilemeyeceğini görebilirsiniz.

-  Sıfırlama cihazının nereden gelebileceğini doğrulamak için, kendinden konak IR 'den dördüncü atlamayı kontrol edin.
 
    *Linux sistem A 'dan TTL 64 ile ağ paketi-> B TTL 64 eksi 1 = 63-> C TTL 63, eksi 1 = 62-> TTL 62 eksi 1 = 61 kendinden konak IR*

- İdeal bir durumda, TTL atlama sayısı 128 olur ve bu da Windows işletim sisteminin Data Factory örneğinizi çalıştırdığı anlamına gelir. Aşağıdaki örnekte gösterildiği gibi, *128 eksi 107 = 21 atlama*, yani paket için 21 ATLAMANıN, TCP 3 el sıkışma sırasında Data Factory örneğinden şirket IÇINDE barındırılan IR 'ye gönderildiği anlamına gelir.
 
    ![107 TTL değerini gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-107.png)

    Bu nedenle, dördüncü atlamanın kendinden konak IR 'den ne olduğunu görmek için ağ ekibine ihtiyacınız vardır. Bu güvenlik duvarıdır, Linux sisteminde olduğu gibi, TCP 3 elsıkışmasının ardından cihazın neden paketi sıfırlamadığına bakmak için günlüklere bakın. 
    
    Araştırırken emin değilseniz, sorunlu süre içinde hem şirket içinde barındırılan IR hem de güvenlik duvarının içinden Netmon izlemesini almayı deneyin. Bu yaklaşım, paketi sıfırlayıp bağlantısının kesilmesine neden olduğunu belirlemenize yardımcı olur. Bu durumda, ileride ilerlemek için ağ ekibinize de ihtiyacınız vardır.

### <a name="analyze-the-netmon-trace"></a>Netmon izlemesini analiz etme

> [!NOTE] 
> Aşağıdaki yönergeler Netmon Trace için geçerlidir. Netmon Trace Şu anda destek dışında olduğundan, bu amaçla Wireshark kullanabilirsiniz.

Toplanan Netmon Trace ile Telnet **8.8.8.8 888** ' i denediğinizde, izlemeyi aşağıdaki ekran görüntülerinde görmeniz gerekir:

!["Bağlantı noktası 888 üzerinde ana bilgisayara bağlantı açılamadı" hata iletisinin gösterildiği ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-1.png)

![Netmon izlemenin açıklamasını gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-2.png)
 

Yukarıdaki resimlerde, bağlantı noktası **888**' de **8.8.8.8** sunucu tarafında TCP bağlantısı yapamadık. bu nedenle, burada iki **synyeniden aktarım** ek paketi görürsünüz. Kaynak **self-konak2** , ilk paketiyle **8.8.8.8** 'ye bağlanamadığı için bağlantıyı kurmaya çalışmaya devam edecektir.

> [!TIP]
> Bu bağlantıyı yapmak için aşağıdaki çözümü deneyin:
> 1. **Yükleme filtresi**  >  **Standart filtre**  >  **adresleri**  >  **IPv4 adresler**' i seçin.
> 1. Filtreyi uygulamak için **IPv4. Address = = 8.8.8.8** girin ve **Uygula**' yı seçin. Bundan sonra yerel makineden hedef **8.8.8.8** iletişimi görmeniz gerekir.

![Filtre adreslerini gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-1.png)
        
![Daha fazla filtre adresi gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-2.png)

Başarılı senaryolar aşağıdaki örneklerde gösterilmiştir: 

- Telnet **8.8.8.8 53** herhangi bir sorun olmadan başarısız olursa, başarılı bir TCP 3 el sıkışması vardır ve oturum bir TCP 4 el sıkışması ile tamamlanır.

    ![Başarılı bir bağlantı senaryosunu gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-1.png)
     
    ![Başarılı bir bağlantı senaryosunun ayrıntılarını gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-2.png)

- Önceki TCP 3 el sıkışması aşağıdaki iş akışını üretir:

    ![TCP 3 el sıkışma iş akışının diyagramı.](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-3-handshake-workflow.png)
 
- Oturumu tamamlaması için TCP 4 el sıkışması aşağıdaki iş akışlarıyla gösterilmektedir:

    ![TCP 4 el sıkışma ayrıntılarının ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake.png)

    ![TCP 4 el sıkışma iş akışının diyagramı.](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake-workflow.png) 

### <a name="microsoft-email-notification-about-updating-your-network-configuration"></a>Ağ yapılandırmanızı güncelleştirme hakkında Microsoft e-posta bildirimi

Aşağıdaki e-posta bildirimini alabilirsiniz; bu durum, ağ yapılandırmanızı, Azure Data Factory için yeni IP adresleriyle iletişime izin verecek şekilde güncelleştirmenizi, 8 Kasım 2020:

   ![Ağ yapılandırması güncelleştirmesi isteyen Microsoft e-posta bildirimi ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/email-notification.png)

#### <a name="determine-whether-this-notification-affects-you"></a>Bu bildirimin sizi etkileyip etkilemediğini belirleme

Bu bildirim aşağıdaki senaryolar için geçerlidir:

##### <a name="scenario-1-outbound-communication-from-a-self-hosted-integration-runtime-thats-running-on-premises-behind-a-corporate-firewall"></a>Senaryo 1: Şirket güvenlik duvarının arkasında şirket içinde çalışan, kendinden konak tümleştirme çalışma zamanından giden iletişim

Etkilenip etkilenmediğini belirleme:

- [Bir güvenlik duvarı yapılandırması ayarlama ve IP adresleri listesine izin verme](data-movement-security-considerations.md#firewall-configurations-and-allow-list-setting-up-for-ip-addresses)bölümünde açıklanan yaklaşımı kullanan tam etki alanı adlarına (FQDN) göre güvenlik duvarı kuralları tanımlıyorsanız *, etkilenmezsiniz* .

- Şirket güvenlik duvarınızdaki giden IP 'Ler için izin verilenler listesini açıkça etkinleştirirseniz *Bu sorundan etkilenmezsiniz.*

   Etkileniyorsanız, şu işlemi gerçekleştirin: 8 Kasım 2020 ' e kadar, ağ altyapınızı en son Data Factory IP adreslerini kullanacak şekilde güncelleştirmek için ağ altyapısı ekibinize bildirin. En son IP adreslerini indirmek için [INDIRILEBILIR JSON dosyalarını kullanarak hizmet etiketlerini bulma](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)bölümüne gidin.

##### <a name="scenario-2-outbound-communication-from-a-self-hosted-integration-runtime-thats-running-on-an-azure-vm-inside-a-customer-managed-azure-virtual-network"></a>Senaryo 2: müşteri tarafından yönetilen Azure sanal ağı içindeki bir Azure VM üzerinde çalışan, şirket içinde barındırılan bir tümleştirme çalışma zamanından giden iletişim

Etkilenip etkilenmediğini belirleme:

- Şirket içinde barındırılan tümleştirme çalışma zamanı içeren özel bir ağda giden ağ güvenlik grubu (NSG) kuralları olup olmadığınızı denetleyin. Giden kısıtlama yoksa etkilenmezsiniz.

- Giden kuralı kısıtlamalarınız varsa, hizmet etiketleri kullanıp kullanmayacağınızı kontrol edin. Hizmet etiketleri kullanıyorsanız, bundan etkilenmezsiniz. Yeni IP aralığı mevcut hizmet etiketlerinizin altında olduğundan herhangi bir şeyi değiştirmeniz veya eklemeniz gerekmez. 

  ![Hedef olarak DataFactory 'yi gösteren bir hedef denetiminin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/destination-check.png)

- Azure sanal ağındaki NSG kurallarınızın izin verilenler listesini açık bir şekilde etkinleştirdiğinizde *Bu sorundan etkilenmezsiniz.*

   Etkileniyorsanız, şu işlemi gerçekleştirin: 8 Kasım 2020 ' e kadar, ağ altyapısı ekibinize, Azure sanal ağ yapılandırmanızda bulunan NSG kurallarını güncelleştirmek üzere en son Data Factory IP adreslerini kullanacak şekilde bildirin. En son IP adreslerini indirmek için [INDIRILEBILIR JSON dosyalarını kullanarak hizmet etiketlerini bulma](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)bölümüne gidin.

##### <a name="scenario-3-outbound-communication-from-ssis-integration-runtime-in-a-customer-managed-azure-virtual-network"></a>Senaryo 3: müşteri tarafından yönetilen bir Azure sanal ağında SSIS Integration Runtime giden iletişim

Etkilenip etkilenmediğini belirleme:

- SQL Server Integration Services (SSIS) Integration Runtime içeren özel bir ağda giden NSG kurallarınızı olup olmadığınızı denetleyin. Giden kısıtlama yoksa etkilenmezsiniz.

- Giden kuralı kısıtlamalarınız varsa, hizmet etiketleri kullanıp kullanmayacağınızı kontrol edin. Hizmet etiketleri kullanıyorsanız, bundan etkilenmezsiniz. Yeni IP aralığı mevcut hizmet etiketlerinizin altında olduğundan herhangi bir değişiklik yapmanız gerekmez.

- Azure sanal ağındaki NSG kurallarınızın izin verilenler listesini açık bir şekilde etkinleştirdiğinizde *Bu sorundan etkilenmezsiniz.*

  Etkileniyorsanız, şu işlemi gerçekleştirin: 8 Kasım 2020 ' e kadar, ağ altyapısı ekibinize, Azure sanal ağ yapılandırmanızda bulunan NSG kurallarını güncelleştirmek üzere en son Data Factory IP adreslerini kullanacak şekilde bildirin. En son IP adreslerini indirmek için [INDIRILEBILIR JSON dosyalarını kullanarak hizmet etiketlerini bulma](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)bölümüne gidin.

### <a name="couldnt-establish-a-trust-relationship-for-the-ssltls-secure-channel"></a>SSL/TLS güvenli kanalı için bir güven ilişkisi kurulamadı 

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan IR Azure Data Factory hizmetine bağlanamadı.

Şirket içinde barındırılan IR olay günlüğünü veya CustomLogEvent tablosundaki istemci bildirim günlüklerini denetlediğinizde, aşağıdaki hata iletisini bulacaksınız:

"Temel alınan bağlantı kapatıldı: SSL/TLS güvenli kanalı için güven ilişkisi kurulamadı. Uzak sertifika, doğrulama yordamına göre geçersiz. "

Data Factory hizmetinin sunucu sertifikasını denetmanın en kolay yolu, tarayıcınızda Data Factory hizmeti URL 'sini açmak. Örneğin, şirket içinde barındırılan IR 'nin yüklü olduğu makinede bulunan [sunucu sertifikasını denetle bağlantısını](https://eu.frontend.clouddatahub.net/) açın ve ardından sunucu sertifikası bilgilerini görüntüleyin.

  ![Azure Data Factory hizmetinin sunucu sertifikasını denetle bölmesinin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/server-certificate.png)

  ![Sunucu sertifika yolunu denetlemek için pencerenin ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/certificate-path.png)

#### <a name="cause"></a>Nedeni

Bu sorunun iki olası nedeni vardır:

- Neden 1: Data Factory hizmeti sunucu sertifikasının kök CA 'sı, şirket içinde barındırılan IR 'nin yüklü olduğu makinede güvenilir değil. 
- Neden 2: ortamınızda bir ara sunucu kullanıyorsunuz, Data Factory hizmetin sunucu sertifikası proxy ile değiştirilmiştir ve değiştirilmiş sunucu sertifikasına, şirket içinde barındırılan IR 'nin yüklendiği makine tarafından güvenilmiyor.

#### <a name="resolution"></a>Çözüm

- 1. neden için: Data Factory sunucu sertifikasına ve sertifika zincirine şirket içinde barındırılan IR 'nin yüklendiği makine tarafından güvenildiğinden emin olun.
- 2. nedenle: şirket içinde barındırılan IR makinesinde bulunan kök CA 'ya güvenin veya proxy 'yi Data Factory sunucu sertifikasının yerini alacak şekilde yapılandırın.

Windows üzerinde sertifika güvenen sertifikalar hakkında daha fazla bilgi için bkz. [Güvenilen kök sertifikayı yükleme](/skype-sdk/sdn/articles/installing-the-trusted-root-certificate).

#### <a name="additional-information"></a>Ek bilgiler
DigiCert tarafından imzalanmış yeni bir SSL sertifikası aldık. DigiCert genel kök G2 'in güvenilir kök CA 'da olup olmadığını denetleyin.

  ![Güvenilen kök sertifika yetkilileri dizininde DigiCert genel kök G2 klasörünü gösteren ekran görüntüsü.](media/self-hosted-integration-runtime-troubleshoot-guide/trusted-root-ca-check.png)

Bu, güvenilen kök CA 'da değilse, [buradan indirin](http://cacerts.digicert.com/DigiCertGlobalRootG2.crt ). 


## <a name="self-hosted-ir-sharing"></a>Şirket içinde barındırılan IR paylaşımı

### <a name="sharing-a-self-hosted-ir-from-a-different-tenant-is-not-supported"></a>Kendi kendine barındırılan bir IR 'yi farklı bir kiracıdan paylaşma desteklenmez 

#### <a name="symptoms"></a>Belirtiler

Şirket içinde barındırılan IR 'yi Azure Data Factory kullanıcı arabiriminden paylaşmaya çalışırken diğer veri fabrikaları (farklı kiracılarda) fark edebilirsiniz, ancak bunu farklı kiracılardaki veri fabrikaları genelinde paylaşamazsınız.

#### <a name="cause"></a>Nedeni

Şirket içinde barındırılan IR kiracılar arasında paylaştırılamaz.

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için aşağıdaki kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Microsoft Q bir sayfa&](/answers/topics/azure-data-factory.html)
*  [Data Factory için yığın taşma Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [Veri akışları eşleme performans Kılavuzu](concepts-data-flow-performance.md)
