---
title: VMware VM’lerini Azure'a geçiş için değerlendirme
description: Azure Geçir Sunucu Değerlendirmesi'ni kullanarak şirket içi VMware VM'lerinin Azure'a geçiş için nasıl değerlendirildiğini açıklar.
ms.topic: tutorial
ms.date: 03/23/2019
ms.openlocfilehash: f33e56f7e0af920363475edfa7e86977c2efd1d9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80336738"
---
# <a name="assess-vmware-vms-by-using-azure-migrate-server-assessment"></a>Azure Geçir Sunucu Değerlendirmesi'ni kullanarak VMware VM'leri değerlendirin

Bu makalede, [Azure Geçiş:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak şirket içi VMware sanal makinelerini (VM'ler) nasıl değerlendirdiğinizi gösterir.


Bu öğretici, VMware VM'lerinin nasıl değerlendirilip Azure'a geçirilen bir serinin ikinci öğreticisidir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir Azure Geçiş projesi ayarlayın.
> * VM'leri değerlendirmek için şirket içinde çalışan bir Azure Geçiş cihazı ayarlayın.
> * Şirket içi VM'lerin sürekli keşfini başlatın. Cihaz, keşfedilen VM'ler için yapılandırma ve performans verilerini Azure'a gönderir.
> * Grup VM'leri keşfetti ve VM grubunu değerlendirdi.
> * Değerlendirmeyi gözden geçirin.

> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için, nasıl yapılacağını makaleleri gözden geçirin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Bu [serinin ilk öğretici tamamlayın.](tutorial-prepare-vmware.md) Bunu yapmazsanız, bu öğreticideki talimatlar çalışmaz.
- İşte ilk öğreticide ne yapman gerekirdi:
    - Azure'u Azure Geçir ile çalışmaya [hazırlayın.](tutorial-prepare-vmware.md#prepare-azure)
    - [Değerlendirme için Değerlendirme için VMware hazırlayın.](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) Buna VMware ayarlarını denetleme, Azure Geçiş'in vCenter Server'a erişmek için kullanabileceği bir hesap ayarlama dahildir.
    - VMware değerlendirmesi için Azure Geçir cihazını dağıtmak için neye ihtiyacınız olduğunu [doğrulayın.](tutorial-prepare-vmware.md#verify-appliance-settings-for-assessment)

## <a name="set-up-an-azure-migrate-project"></a>Azure Geçiş projesi ayarlama

Aşağıdaki gibi yeni bir Azure Geçiş projesi ayarlayın:

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
1. **Hizmetler** altında **Azure Geçişi**’ni seçin.
1. **Genel**Bakış'ta, **Discover, assess ve migrate sunucuları**altında, **sunucuları değerlendir'i seçin ve sunucuları geçirin.**

   ![Sunucuları değerlendirmek ve geçirmek için düğme](./media/tutorial-assess-vmware/assess-migrate.png)

1. **Başlarken**, **araç ekle'yi**seçin.
1. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.     
1. **Proje**Ayrıntıları'nda, proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin. Asya, Avrupa, Birleşik Krallık ve Amerika Birleşik Devletleri desteklenir.

   Proje coğrafyası yalnızca şirket içi sanal makinelerden toplanan meta verileri depolamak için kullanılır. Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

   ![Proje adı ve bölge için kutular](./media/tutorial-assess-vmware/migrate-project.png)

1. **Sonraki'ni**seçin.
1. **Değerlendirme aracını seç'te**Azure **Geçir: Sunucu Değerlendirmesi** > **Sonraki'ni**seçin.

   ![Sunucu Değerlendirme aracı için seçim](./media/tutorial-assess-vmware/assessment-tool.png)

1. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
1. **Gözden Geçir + araç ekle,** ayarları gözden geçir ve araçları **ekle'yi**seçin.
1. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="set-up-the-azure-migrate-appliance"></a>Azure Geçiş cihazını ayarlama

Azure Geçir:Sunucu Değerlendirmesi, hafif bir Azure Geçir cihazı kullanır. Cihaz VM keşfi gerçekleştirir ve VM meta verilerini ve performans verilerini Azure Geçiş'e gönderir.
- Cihaz, indirilen BIR OVA şablonu kullanılarak VMware VM'de ayarlanabilir. Alternatif olarak, cihazı PowerShell yükleyici komut dosyasıyla vm veya fiziksel bir makineye ayarlayabilirsiniz.
- Bu öğretici, OVA şablonu kullanır. Cihazı bir komut dosyası kullanarak kurmak istiyorsanız [bu makaleyi](deploy-appliance-script.md) inceleyin.

Cihazı oluşturduktan sonra, Azure Geçiş:Sunucu Değerlendirmesi'ne bağlanıp bağlanabildiğinizi, ilk kez yapılandırıp yapılandırıp kaydedilemediğini kontrol edin ve Azure Geçiş projesine kaydettirebilirsiniz.



### <a name="download-the-ova-template"></a>OVA şablonuna karşı yükleme

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**seçin.
1. **Discover makinelerde** > **makineleri sanallaştırılmış mı?**, **Evet, VMWare vSphere hypervisor ile**seçin.
1. OVA şablon dosyasını indirmek için **İndir'i** seçin.

   ![OVA dosyasını indirmek için seçmeler](./media/tutorial-assess-vmware/download-ova.png)

### <a name="verify-security"></a>Güvenliği doğrula

Dağıtmadan önce OVA dosyasının güvenli olup olmadığını denetleyin:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
1. OVA dosyası için karma oluşturmak için aşağıdaki komutu çalıştırın:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

Sürüm 2.19.07.30 için oluşturulan karma bu değerlerle eşleşmelidir:

**Algoritma** | **Karma değeri**
--- | ---
MD5 | c06ac2a2c0f870d3b274a0b7a73b78b1
SHA256 | 4ce4faa3a78189a09a26bfa5b817c7afcf5b555eb46999c2fad9d2ebc808540c

### <a name="create-the-appliance-vm"></a>VM cihazını oluşturun

İndirilen dosyayı içe aktarın ve bir VM oluşturun:

1. vSphere Client konsolunda **OvF** > Şablonu Dosya**dağıt'ı'nı**seçin.

   ![OVF şablonunu dağıtmak için menü komutu](./media/tutorial-assess-vmware/deploy-ovf.png)

1. OvF Şablon Sihirbazı **Source**> Kaynak'ta, OVA dosyasının konumunu belirtin.
1. **Ad** ve **Konum**olarak, VM için uygun bir ad belirtin. VM'nin barındırılan stok nesnesini seçin.
1. **Ana Bilgisayar/Küme'de,** VM'nin çalışacağı ana bilgisayarı veya kümeyi belirtin.
1. **Depolama'da,** VM'nin depolama hedefini belirtin.
1. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
1. **Ağ Eşleme'de,** VM'nin bağlanacağı ağı belirtin. Ağ, Azure Geçir Sunucu Değerlendirmesi'ne meta veri göndermek için internet bağlantısına ihtiyaç duyar.
1. Ayarları gözden geçirin ve onaylayın ve ardından **Bitir'i**seçin.

### <a name="verify-appliance-access-to-azure"></a>Azure'a cihaz erişimini doğrulama

VM cihazının [Azure URL'lerine](migrate-appliance.md#url-access)bağlanabileceğinden emin olun.

### <a name="configure-the-appliance"></a>Cihazı yapılandırın

Cihazı ilk kez ayarlayın.

> [!NOTE]
> Cihazı indirilen OVA yerine [PowerShell komut dosyası](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı önemli değildir.

1. vSphere Client konsolunda VM'ye sağ tıklayın ve ardından **Konsolu Aç'ı**seçin.
1. Cihazın dilini, saatini ve parolasını sağlayın.
1. VM'ye bağlanabilen herhangi bir makinede tarayıcı açın ve cihaz web uygulamasının URL'sini açın: **https:// cihaz adı veya IP*adresi*: 44368**.

   Alternatif olarak, uygulama kısayolu seçerek uygulamayı cihaz masaüstünden açabilirsiniz.
1. Web uygulamasında > **ön koşulları ayarlayın,** aşağıdakileri yapın:
   - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
   - **Bağlantı**: Uygulama, VM'nin internet erişimi ne olduğunu denetler. VM proxy kullanıyorsa:
     - **Proxy ayarlarını**seçin ve formda http://ProxyIPAddress proxy adresini ve http://ProxyFQDNdinleme bağlantı noktasını belirtiniz veya .
     - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
     - Yalnızca HTTP proxy'nin desteklendiğini unutmayın.
   - **Zaman senkronizasyonu**: Cihazın üzerindeki süre, keşfin düzgün çalışması için internet saati ile senkronize olmalıdır.
   - **Yükleme güncellemeleri**: Cihaz en son güncellemelerin yüklenmesini sağlar.
   - **Install VDDK**: Cihaz VMWare vSphere Sanal Disk Geliştirme Kiti'nin (VDDK) yüklü olduğunu kontrol eder. Yüklü değilse, VMware'den VDDK 6.7'yi indirin ve indirilen zip içeriğini cihazda belirtilen konuma çıkarın.

     Azure Geçir Sunucusu Geçişi, Azure'a geçiş sırasında makineleri çoğaltmak için VDDK'yı kullanır.       

### <a name="register-the-appliance-with-azure-migrate"></a>Cihazı Azure Geçiş ile kaydedin

1. **Giriş Yap'ı**seçin. Görünmüyorsa, tarayıcıdaki açılır pencere engelleyicisini devre dışı bıraktığınızdan emin olun.
1. Yeni sekmede, Azure kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
1. Başarılı bir şekilde oturum açınca web uygulamasına geri dön.
1. Azure Geçiş projesinin oluşturulduğu aboneliği seçin ve ardından projeyi seçin.
1. Cihaz için bir ad belirtin. Ad 14 karakter veya daha az alfanümerik olmalıdır.
1. **Kaydol**’u seçin.


## <a name="start-continuous-discovery"></a>Sürekli keşfi başlatma

Cihazın VM'lerin yapılandırma ve performans verilerini keşfetmek için vCenter Server'a bağlanması gerekir.

### <a name="specify-vcenter-server-details"></a>vCenter Server ayrıntılarını belirtin
1. **vCenter Server ayrıntılarını belirtin,** vCenter Server örneğinin adını (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server'ın dinlediği özel bir bağlantı noktası belirtebilirsiniz.
2. **Kullanıcı adı** ve **Parola'da,** cihazın vCenter Server örneğinde VM'leri bulmak için kullanacağı vCenter Server hesap kimlik bilgilerini belirtin. 

    - [Önceki öğreticide](/tutorial-prepare-vmware.md#set-up-an-account-for-assessment)gerekli izinleri içeren bir hesap ayarlamanız gerekirdi.
    - Belirli VMware nesnelerine (vCenter Server veri merkezleri, kümeler, kümeler klasörü, ana bilgisayarlar, ana bilgisayarlar klasörü veya tek tek VM'ler) bulma kapsamını genişletmek istiyorsanız, Azure Geçiş tarafından kullanılan hesabı kısıtlamak için [bu makaledeki](set-discovery-scope.md) yönergeleri gözden geçirin.

3. Cihazın vCenter Server'a bağlanabilmesini sağlamak için **Bağlantıyı Doğrula'yı** seçin.
4. **VM'lere yönelik uygulamaları ve bağımlılıkları keşfedin'de**isteğe bağlı olarak kimlik bilgileri ekle'yi tıklatın ve kimlik **bilgilerinin**alakalı olduğu işletim sistemini ve kimlik bilgilerini kullanıcı adı ve parolabelirtin. Sonra **Ekle'yi**tıklatın.

    - [Uygulama bulma özelliği](how-to-discover-applications.md)veya [aracısız bağımlılık çözümleme özelliği](how-to-create-group-machine-dependencies-agentless.md)için kullanılacak bir hesap oluşturduysanız, isteğe bağlı olarak buraya kimlik bilgileri eklersiniz.
    - Bu özellikleri kullanmıyorsanız, bu ayarı atlayabilirsiniz.
    - [Uygulama bulma](migrate-support-matrix-vmware.md#application-discovery)veya aracısız çözümleme için gereken kimlik bilgilerini gözden [geçirin.](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements)

5. **Kaydet ve keşfetme başlatmak**, VM keşif başlatmak için.

Discovery aşağıdaki gibi çalışır:
- Keşfedilen VM meta verilerinin portalda görünmesi yaklaşık 15 dakika sürer.
- Yüklü uygulamaların, rollerin ve özelliklerin keşfi biraz zaman alır. Süre, keşfedilen VM sayısına bağlıdır. 500 VM için uygulama envanterinin Azure Geçiş portalında görünmesi yaklaşık bir saat sürer.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Keşiften sonra, VM'lerin Azure portalında göründüğünü doğrulayabilirsiniz:

1. Azure Geçir panosunu açın.
1. **Azure Geçir - Sunucularda** > Azure**Geçir: Sunucu Değerlendirmesi**, Keşfedilen **sunucuların**sayısını görüntüleyen simgeyi seçin.

## <a name="set-up-an-assessment"></a>Bir değerlendirme ayarlama

Azure Geçir Sunucu Değerlendirmesi'ni kullanarak iki tür değerlendirme oluşturabilirsiniz:

**Değerlendirme** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı değerlendirmeler | **Önerilen VM boyutu**: CPU ve bellek kullanım verilerine dayanır.<br/><br/> **Önerilen disk türü (standart veya premium yönetilen disk)**: IOPS'ye ve şirket içi disklerin üretim emresine dayanır.
**Şirket içinde olduğu gibi** | Şirket içi boyutlandırmaya dayalı değerlendirmeler | **Önerilen VM boyutu**: Şirket içi VM boyutuna göre.<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.

## <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
1. **Sunucular** sekmesinde, **Azure Geçir: Sunucu Değerlendirme** döşemesinde **Değerlendir'i**seçin.

   ![Değerlendir düğmesinin konumu](./media/tutorial-assess-vmware/assess.png)

1. **Değerlendir sunucularında,** değerlendirme için bir ad belirtin.
1. **Tümünü Görüntüle'yi**seçin ve ardından değerlendirme özelliklerini gözden geçirin.

   ![Değerlendirme özellikleri](./media/tutorial-assess-vmware/view-all.png)

1. **Bir grubu seç veya oluştur'da**Yeni **Oluştur'u**seçin ve bir grup adı belirtin. Bir grup değerlendirme için bir veya daha fazla VM'yi bir araya toplar.
1. **Gruba makine ekle'de,** gruba eklemek için VM'leri seçin.
1. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **Değerlendirme Oluştur'u** seçin.

   ![Sunucuları değerlendirin](./media/tutorial-assess-vmware/assessment-create.png)

1. Değerlendirme oluşturulduktan sonra, **Sunucular** > **Azure Geçiş: Sunucu Değerlendirme** > **Değerlendirmeleri'nde**görüntüleyin.
1. Excel dosyası olarak indirmek için **Dışa Aktarma değerlendirmesini** seçin.

## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme açıklar:

- **Azure hazırlığı**: VM'lerin Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'da VM'leri çalıştırmak için tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.

Bir değerlendirmeyi görüntülemek için:

1. **Geçiş hedefleri** > **Sunucularında,** **Azure Geçişinde Değerlendirmeler: Sunucu Değerlendirmesi'ni**seçin. **Assessments**
1. **Değerlendirmeler'de,** açmak için bir değerlendirme seçin.

   ![Değerlendirme özeti](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlık durumunu gözden geçirin

1. **Azure'a hazır durumda,** VM'lerin Azure'a geçişiçin hazır olup olmadığını doğrulayın.
1. VM durumunu gözden geçirin:
    - **Azure'a Hazır**: Azure Geçirim değerlendirmede VM boyutu ve VM'ler için maliyet tahminleri önerdiğinde kullanılır.
    - **Koşullara hazır**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazır değil**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazırlık bilinmiyor**: Azure Geçiş veri kullanılabilirliği sorunları nedeniyle hazır olup olmadığını değerlendiremediğinde kullanılır.

1. Azure **hazırlık** durumunu seçin. VM hazırlık ayrıntılarını görüntüleyebilirsiniz. Bilgi işlem, depolama ve ağ ayarları da dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya inebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Değerlendirme özeti, Azure'da VM'leri çalıştırmanın tahmini işlem ve depolama maliyetini gösterir. Maliyetler, değerlendirilen gruptaki tüm VM'ler için toplanır. Belirli VM'lerin maliyet ayrıntılarını görmek için detaya inebilirsiniz.

> [!NOTE]
> Maliyet tahminleri, bir makinenin boyut önerilerini, disklerini ve özelliklerini temel alar. Tahminler, şirket içi VM'leri IaaS VM olarak çalıştırmak içindir. Azure Geçir Sunucu Değerlendirmesi, PaaS veya SaaS maliyetlerini dikkate almaz.

Değerlendirilen grubun toplam depolama maliyetleri farklı depolama diskleri türlerine bölünür. 

### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Azure Geçir Sunucusu Değerlendirmesi, performansa dayalı bir değerlendirmeye 1 yıldızdan (en düşük) 5 yıldıza (en yüksek) bir güven derecelendirmesi atar.

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-vmware/confidence-rating.png)

Güven derecelendirmesi, değerlendirmenin boyut önerilerinin güvenilirliğini tahmin edebilirsiniz. Derecelendirme, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine dayanır:

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 yıldız
%21-%40 | 2 yıldız
%41-%60 | 3 yıldız
%61-%80 | 4 yıldız
%81-%100 | 5 yıldız

Güven derecelendirmeleri için [en iyi uygulamalar hakkında bilgi edinin.](best-practices-assessment.md#best-practices-for-confidence-ratings)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure Geçir cihazı kurdunuz. Ayrıca bir değerlendirme oluşturdunuz ve gözden geçirdiniz.

Azure Geçiş Sunucusu Geçişi'ni kullanarak VMware VM'leri Azure'a nasıl geçirteceklerini öğrenmek için, serinin üçüncü öğreticisine devam edin:

> [!div class="nextstepaction"]
> [VMware VM’leri geçirme](./tutorial-migrate-vmware.md)
