---
title: Azure'a geçiş için fiziksel sunucuları Azure'a geçiş için Azure'a Geçiş Sunucu Değerlendirmesi ile değerlendirin
description: Azure Geçir Sunucu Değerlendirmesi'ni kullanarak Azure'a geçiş için şirket içi fiziksel sunucuların nasıl değerlendirildiğini açıklar.
ms.topic: tutorial
ms.date: 11/18/2019
ms.openlocfilehash: c89c731712a625e5f3b7a1a7e9306f6a7480b96b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76990309"
---
# <a name="assess-physical-servers-with-azure-migrate-server-assessment"></a>Azure Geçiş: Sunucu Değerlendirmesi ile fiziksel sunucuları değerlendirin

Bu makalede, Azure Geçiş: Sunucu Değerlendirmesi aracını kullanarak şirket içi fiziksel sunucuları nasıl değerlendiracağınızı gösterilmektedir.

[Azure Geçiş,](migrate-services-overview.md) uygulamaları, altyapıyı ve iş yüklerini Microsoft Azure'a keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan bir araç merkezi sağlar. Hub, Azure Geçiş araçlarını ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bu öğretici, fiziksel sunucuların nasıl değerlendirilip Azure'a geçirilen bir serinin ikinci sidir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir Azure Geçiş projesi ayarlayın.
> * Fiziksel sunucuları değerlendirmek için şirket içinde çalışan bir Azure Geçiş cihazı ayarlayın.
> * Şirket içi fiziksel sunucuların sürekli keşfini başlatın. Cihaz, keşfedilen sunucular için yapılandırma ve performans verilerini Azure'a gönderir.
> * Grup bulunan sunucular ve sunucu grubunu değerlendirin.
> * Değerlendirmeyi gözden geçirin.

> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için Nasıl Yap'ın olduğu makaleleri inceleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prerequisites"></a>Ön koşullar

- Bu serinin ilk öğretici [tamamlayın.](tutorial-prepare-physical.md) Bunu yapmazsanız, bu öğreticideki talimatlar çalışmaz.
- İşte ilk öğreticide ne yapman gerekirdi:
    - Azure Geçiş için [Azure izinleri ayarlayın.](tutorial-prepare-physical.md#prepare-azure)
    - Değerlendirme için [fiziksel sunucular hazırlayın.](tutorial-prepare-physical.md#prepare-for-physical-server-assessment) Cihaz gereksinimleri doğrulanmalıdır. Ayrıca fiziksel sunucu bulma için ayarlanmış bir hesap olmalıdır. Gerekli bağlantı noktaları kullanılabilir olmalıdır ve Azure'a erişmek için gereken URL'lerden haberdar olmalısınız.


## <a name="set-up-an-azure-migrate-project"></a>Azure Geçiş projesi ayarlama

Aşağıdaki şekilde yeni bir Azure Geçişi projesi oluşturun.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel Bakış** bölümünde **Sunucuları bul, değerlendir ve geçiş** altında **Sunucuları değerlendir ve geçir** üzerine tıklayın.

    ![Sunucuları keşfedin ve değerlendirin](./media/tutorial-assess-physical/assess-migrate.png)

4. **Başlarken** bölümünde **Araç ekle**’ye tıklayın.
5. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.     
6. **Proje**Ayrıntıları'nda, proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin. Asya, Avrupa, İngiltere ve AMERIKA Birleşik Devletleri desteklenir.

    - Proje coğrafyası yalnızca şirket içi sunuculardan toplanan meta verileri depolamak için kullanılır.
    - Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

    ![Azure Geçiş projesi oluşturma](./media/tutorial-assess-physical/migrate-project.png)


7. **İleri**'ye tıklayın.
8. **Değerlendirme aracını seç'te**Azure **Geçir: Sunucu Değerlendirmesi** > **Sonraki'ni**seçin.

    ![Azure Geçiş projesi oluşturma](./media/tutorial-assess-physical/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden Geçir + araç ekle,** ayarları gözden geçir ve araçları **ekle'yi**tıklatın.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.


## <a name="set-up-the-appliance"></a>Cihazı ayarlama

Azure Geçiş: Sunucu Değerlendirmesi hafif bir cihaz çalıştırın.

- Bu cihaz fiziksel sunucu bulma gerçekleştirir ve Sunucu meta verilerini ve performans verilerini Azure Geçiş Sunucu Değerlendirmesi'ne gönderir.
- Cihazı kurmak için:
    - Azure portalından Azure Geçir yükleyici komut dosyası içeren sıkıştırılmış bir dosya indirin.
    - Sıkıştırılmış dosyadan içeriğini ayıklayın. PowerShell konsoluna yönetim ayrıcalıklarıyla başlatın.
    - Cihaz web uygulamasını başlatmak için PowerShell komut dosyasını çalıştırın.
    - Cihazı ilk kez yapılandırın ve Azure Geçiş projesine kaydedin.
- Tek bir Azure Geçiş projesi için birden çok cihaz ayarlayabilirsiniz. Tüm cihazlarda, istediğiniz sayıda fiziksel sunucu keşfedebilirsiniz. Cihaz başına en fazla 250 sunucu bulunabilir.

### <a name="download-the-installer-script"></a>Yükleyici komut dosyasını indirin

Cihaz için sıkıştırılmış dosyayı indirin.

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2. **Discover makinelerinde** >  **Not virtualized/Other****makineleriniz sanallaştırıldı mı?**
3. Sıkıştırılmış dosyayı indirmek için **İndir'i** tıklatın.

    ![Yükleyiciyi indirin](./media/tutorial-assess-physical/download-appliance.png)


### <a name="verify-security"></a>Güvenliği doğrula

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Sıkıştırılanması gereken dosya için karma oluşturmak için aşağıdaki komutu çalıştırın
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```

3.  En son cihaz sürümü için, oluşturulan karma bu ayarlarla eşleşmelidir.

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 1e92ede3e87c03bd148e56a708cdd33f
  SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e6de7b9f475b6542beef114b20bfdac3c

### <a name="run-the-azure-migrate-installer-script"></a>Azure Geçir yükleyici komut dosyasını çalıştırma

Yükleyici komut dosyası aşağıdakileri yapar:

- Fiziksel sunucu bulma ve değerlendirme için aracılar ve bir web uygulaması yükler.
- Windows Etkinleştirme Hizmeti, IIS ve PowerShell ISE dahil olmak üzere Windows rollerini yükleyin.
- IIS yeniden yazılabilir bir modül indirin ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Geçiş için kalıcı ayar ayrıntılarıyla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Yol altında aşağıdaki dosyaları oluşturur:
    - **Config Files**: %ProgramData%\Microsoft Azure\Config
    - **Günlük Dosyaları**: %ProgramData%\Microsoft Azure\Günlükler

Komut dosyasını aşağıdaki gibi çalıştırın:

1. Sıkıştırılmış dosyayı sunucuda cihazı barındıracak bir klasöre ayıklayın.
2. PowerShell'i yönetim (yüksek) ayrıcalığıyla yukarıdaki sunucuda başlatın.
3. PowerShell dizinini indirilen sıkıştırılmış dosyadan içeriğin ayıklandığı klasörle değiştirin.
4. **AzureMigrateInstaller.ps1** adlı komut dosyasını aşağıdaki komutu çalıştırarak çalıştırın:
    ```
    PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1
    ```
Komut dosyası, başarılı bir şekilde bittiğinde cihaz web uygulamasını başlatacaktır.

Herhangi bir sorun durumunda, sorun giderme için C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerine erişebilirsiniz.

> [!NOTE]
> Lütfen mevcut bir Azure Geçir ekibe Azure Geçir yükleyici komut dosyası nda yürütmeyin.

### <a name="verify-appliance-access-to-azure"></a>Azure'a cihaz erişimini doğrulama

Cihazın [Azure URL'lerine](migrate-appliance.md#url-access)bağlanabileceğinden emin olun.


### <a name="configure-the-appliance"></a>Cihazı yapılandırın

Cihazı ilk kez ayarlayın.

1. Cihaza bağlanabilen herhangi bir makinede bir tarayıcı açın ve cihaz web uygulamasının URL'sini açın: **https:// cihaz adı veya IP*adresi:* 44368**.

   Alternatif olarak, uygulama kısayolu tıklayarak masaüstünden uygulamayı açabilirsiniz.
2. Web uygulamasında > **ön koşulları ayarlayın,** aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, sunucunun internet erişimi ne olduğunu denetler. Sunucu proxy kullanıyorsa:
        - **Proxy ayarlarını**tıklatın ve proxy adresini ve dinleme http://ProxyIPAddress http://ProxyFQDNbağlantı noktasını formda veya .
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. Sunucu bulmanın düzgün çalışması için cihazdaki süre internet süresiyle senkronize olmalıdır.
    - **Yükleme güncelleştirmeleri**: Azure Geçir Sunucusu Değerlendirmesi, cihazın en son güncelleştirmeleri yüklü olduğunu denetler.

### <a name="register-the-appliance-with-azure-migrate"></a>Cihazı Azure Geçiş ile kaydedin

1. **Giriş Yap'ı**tıklatın. Görünmüyorsa, tarayıcıdaki açılır pencere engelleyicisini devre dışı bıraktığınızdan emin olun.
2. Yeni sekmede, Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adınız ve şifrenizle oturum açın.
    - PIN ile oturum açma desteklenmez.
3. Başarılı bir şekilde oturum imzaladıktan sonra web uygulamasına geri dön.
4. Azure Geçiş projesinin oluşturulduğu aboneliği seçin. Sonra projeyi seçin.
5. Cihaz için bir ad belirtin. Ad 14 karakter veya daha az alfanümerik olmalıdır.
6. **Kaydol'u**tıklatın.


## <a name="start-continuous-discovery"></a>Sürekli keşfi başlatma

Şimdi, cihazdan keşfedilecek fiziksel sunuculara bağlanın ve keşfi başlatın.

1. Cihazın sunucuları bulmak için kullanacağı hesap kimlik bilgilerini belirtmek için **Kimlik Bilgileri Ekle'yi** tıklatın.  
2. **İşletim Sistemi'ni**belirtin, kimlik bilgileri, **Kullanıcı Adı** ve **Şifre** için uygun adı belirtin ve **Ekle'yi**tıklatın.
Windows ve Linux sunucuları için her biri bir kimlik bilgileri kümesi ekleyebilirsiniz.
4. **Sunucu ekle'yi**tıklatın ve sunucu ayrıntılarını belirtin- FQDN/IP adresi ve sunucuya bağlanmak için kimlik bilgilerinin dostu adı (satır başına bir giriş).
3. **Doğrula**'ya tıklayın. Doğrulamadan sonra, bulunabilecek sunucuların listesi gösterilir.
    - Bir sunucu için doğrulama başarısız olursa, **Durum** sütunundaki simgenin üzerine geçerek hatayı gözden geçirin. Sorunları düzeltin ve yeniden doğrulayın.
    - Bir sunucukaldırmak için **Sil'>** seçin.
4. Doğrulamadan sonra **Kaydet'i** tıklatın ve bulma işlemini başlatmak için keşfi başlatın.

Bu keşif başlar. Keşfedilen sunucunun meta verilerinin Azure portalında görünmesi sunucu başına yaklaşık 1,5 dakika sürer.

### <a name="verify-servers-in-the-portal"></a>Portaldaki sunucuları doğrulama

Keşiften sonra, sunucuların Azure portalında görününcülettiğini doğrulayabilirsiniz.

1. Azure Geçir panosunu açın.
2. **Azure Geçir - Sunucular** > Azure**Geçir: Sunucu Değerlendirme** **sayfasında, Keşfedilen sunucuların**sayısını görüntüleyen simgeyi tıklatın.

## <a name="set-up-an-assessment"></a>Bir değerlendirme ayarlama

Azure Geçiş'i kullanarak oluşturabileceğiniz iki tür değerlendirme vardır: Sunucu Değerlendirmesi.

**Değerlendirme** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı değerlendirmeler | **Önerilen VM boyutu**: CPU ve bellek kullanım verilerine dayanır.<br/><br/> **Önerilen disk türü (standart veya premium yönetilen disk)**: IOPS'ye ve şirket içi disklerin üretim emresine dayanır.
**Şirket içinde olduğu gibi** | Şirket içi boyutlandırmaya dayalı değerlendirmeler. | **Önerilen VM boyutu**: Şirket içi sunucu boyutuna göre<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.


### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure Geçir: Sunucu Değerlendirme** döşemesinde **Değerlendir'i**tıklatın.

    ![Değerlendirme](./media/tutorial-assess-physical/assess.png)

2. **Değerlendir sunucularında,** değerlendirme için bir ad belirtin.
3. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-physical/view-all.png)

3. **Bir grubu seç veya oluştur'da**Yeni **Oluştur'u**seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla sunucuyu bir araya toplar.
4. **Gruba makine ekle'de,** gruba eklemek için sunucuları seçin.
5. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **Değerlendirme Oluştur'u** tıklatın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-physical/assessment-create.png)

6. Değerlendirme oluşturulduktan sonra, **Sunucular** > **Azure Geçiş: Sunucu Değerlendirme** > **Değerlendirmeleri'nde**görüntüleyin.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.



## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme açıklar:

- **Azure hazırlığı**: Sunucuların Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'daki sunucuları çalıştırmak için tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.

### <a name="view-an-assessment"></a>Bir değerlendirmeyi görüntüleme

1. **Geçiş hedefleri** >  **Sunucularında,** **Azure Geçişinde Değerlendirmeler: Sunucu Değerlendirmesi'ni**tıklatın. **Assessments**
2. **Değerlendirmeler'de,** açmak için bir değerlendirmeye tıklayın.

    ![Değerlendirme özeti](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlık durumunu gözden geçirin

1. **Azure'a hazır durumda,** sunucuların Azure'a geçişe hazır olup olmadığını doğrulayın.
2. Durumu gözden geçirin:
    - **Azure için Hazır**: Azure Geçirim, değerlendirmede VM boyutu ve VM'ler için maliyet tahminleri önerir.
    - **Koşullara hazır**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazır değil**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazırlık bilinmiyor**: Veri kullanılabilirliği sorunları nedeniyle Azure Geçiş hazır durumunu değerlendiremediğinde kullanılır.

2. **Azure hazırlık** durumunu tıklatın. Sunucuhazırlık ayrıntılarını görüntüleyebilir ve bilgi işlem, depolama ve ağ ayarları da dahil olmak üzere sunucu ayrıntılarını görmek için ayrıntıya inebilirsiniz.



### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm, Azure'da VM'leri çalıştırmanın tahmini işlem ve depolama maliyetini gösterir.

1. Aylık işlem ve depolama maliyetlerini gözden geçirin. Maliyetler, değerlendirilen gruptaki tüm sunucular için toplanır.

    - Maliyet tahminleri, bir makinenin boyut önerilerini ve disklerini ve özelliklerini temel alınr.
    - Hesaplama ve depolama için tahmini aylık maliyetler gösterilir.
    - Maliyet tahmini, şirket içi sunucuları IaaS VM olarak çalıştırmak içindir. Azure Geçir Sunucu Değerlendirmesi, PaaS veya SaaS maliyetlerini dikkate almaz.

2. Aylık depolama maliyeti tahminlerini gözden geçirebilirsiniz. Bu görünüm, değerlendirilen grup için farklı depolama diskleri türlerine bölünmüş toplu depolama maliyetlerini gösterir.
3. Belirli sunucuların ayrıntılarını görmek için ayrıntıya inebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performansa dayalı değerlendirmeler çalıştırdığınızda, değerlendirmeye bir güven derecelendirmesi atanır.

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-physical/confidence-rating.png)

- 1-star (en düşük) ile 5 yıldızlı (en yüksek) arasında bir derecelendirme verilir.
- Güven derecelendirmesi, değerlendirme tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin edinmesine yardımcı olur.
- Güven derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine dayanır.

Bir değerlendirme için güven derecelendirmeleri aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız

Güven derecelendirmeleri için en iyi uygulamalar hakkında [daha fazla bilgi edinin.](best-practices-assessment.md#best-practices-for-confidence-ratings)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure Geçiş cihazı ayarlama
> * Bir değerlendirme oluşturuldu ve gözden geçirildi

Azure Geçişi: Sunucu Geçişi ile fiziksel sunucuları Azure'a nasıl geçiracağınızı öğrenmek için serinin üçüncü öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Fiziksel sunucuları geçirme](./tutorial-migrate-physical-virtual-machines.md)
