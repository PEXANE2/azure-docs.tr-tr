---
title: Azure Geçişi ile Azure'a geçiş için Hyper-V V'leri değerlendirin | Microsoft Dokümanlar
description: Azure Geçir Sunucu Değerlendirmesi'ni kullanarak Azure'a geçiş için şirket içi Hyper-V V'M'lerin nasıl değerlendirildiğini açıklar.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: c627902268af3a91e172223c1741dd24ea21fa92
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535460"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Azure Geçiş Sunucusu Değerlendirmesi ile Hyper-V V'leri Değerlendirin

Bu makalede, [Azure Geçiş:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak şirket içi Hyper-V V VM'leri nasıl değerlendirdiğinizi gösterilmektedir.


Bu öğretici, Hyper-V V MM'lerin azure'a nasıl değerlendirilip geçirilenbir dizideki ikinci öğreticidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Geçiş projesi ayarlayın.
> * Bir Azure Geçiş cihazı ayarlayın ve kaydedin.
> * Şirket içi VM'lerin sürekli keşfini başlatın.
> * Grup VM'leri keşfetti ve grubu değerlendirdi.
> * Değerlendirmeyi gözden geçirin.

> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için Nasıl Yap'ın olduğu makaleleri inceleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prerequisites"></a>Ön koşullar

- Bu serinin ilk öğretici [tamamlayın.](tutorial-prepare-hyper-v.md) Bunu yapmazsanız, bu öğreticideki talimatlar çalışmaz.
- İşte ilk öğreticide ne yapman gerekirdi:
    - Azure'u Azure Geçir ile çalışmaya [hazırlayın.](tutorial-prepare-hyper-v.md#prepare-azure)
    - [Hyper-V](tutorial-prepare-hyper-v.md#prepare-hyper-v-for-assessment) ana bilgisayarları ve VM değerlendirmesini hazırlayın.
    - Hyper-V değerlendirmesi için Azure Geçir cihazını dağıtmak için neye ihtiyacınız olduğunu [doğrulayın.](tutorial-prepare-hyper-v.md#prepare-for-appliance-deployment)

## <a name="set-up-an-azure-migrate-project"></a>Azure Geçiş projesi ayarlama

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. Arama sonuçlarında **Azure Geçiş'i**seçin.
3. **Genel Bakış** bölümünde **Sunucuları bul, değerlendir ve geçiş** altında **Sunucuları değerlendir ve geçir** üzerine tıklayın.

    ![Sunucuları keşfedin ve değerlendirin](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. **Başlarken** bölümünde **Araç ekle**’ye tıklayın.
5. **Projeyi Geçir** sekmesinde, Azure aboneliğinizi seçin ve yoksa bir kaynak grubu oluşturun.
6. **Proje**Ayrıntıları'nda, proje adını ve projeyi oluşturmak istediğiniz bölgeyi belirtin. Kamu ve [hükümet bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafyaları gözden [geçirin.](migrate-support-matrix.md#supported-geographies-public-cloud)

    - Proje bölgesi yalnızca şirket içi VM'lerden toplanan meta verileri depolamak için kullanılır.
    - VM'leri geçirdiğinizde farklı bir Azure hedef bölgesi seçebilirsiniz. Tüm Azure bölgeleri geçiş hedefi için desteklenir.

    ![Azure Geçiş projesi oluşturma](./media/tutorial-assess-hyper-v/migrate-project.png)

7. **İleri**’ye tıklayın.
8. **Değerlendirme aracını seç'te**Azure **Geçir: Sunucu Değerlendirmesi** > **Sonraki'ni**seçin.

    ![Azure Geçiş projesi oluşturma](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden Geçir + araç ekle,** ayarları gözden geçir ve araçları **ekle'yi**tıklatın.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="set-up-the-azure-migrate-appliance"></a>Azure Geçiş cihazını ayarlama


Azure Geçir:Sunucu Değerlendirmesi, hafif bir Azure Geçir cihazı kullanır. Cihaz VM keşfi gerçekleştirir ve VM meta verilerini ve performans verilerini Azure Geçiş'e gönderir. Cihaz çeşitli şekillerde ayarlanabilir.

- İndirilen Hyper-V VHD kullanarak Hyper-V VM'de ayarlayın. Bu, bu öğreticide kullanılan yöntemdir.
- PowerShell yükleyici komut dosyasına sahip bir Hyper-V VM veya fiziksel makineüzerinde ayarlayın. [Bu yöntem,](deploy-appliance-script.md) VHD kullanarak bir VM ayarlayamıyorsanız veya Azure Devlet'teyseniz kullanılmalıdır.

Cihazı oluşturduktan sonra, Azure Geçiş:Sunucu Değerlendirmesi'ne bağlanıp bağlanabildiğinizi, ilk kez yapılandırıp yapılandırıp kaydedilemediğini kontrol edin ve Azure Geçiş projesine kaydettirebilirsiniz.

### <a name="download-the-vhd"></a>VHD'yi indirin

Cihaz için sıkıştırılmış VHD şablonuna indirin.

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2. **Discover makinelerinde** >  **Yes, with Hyper-V****makineleriniz sanallaştırıldı mı?**
3. VHD dosyasını indirmek için **İndir'i** tıklatın.

    ![VM'yi İndir](./media/tutorial-assess-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Güvenliği doğrula

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.

2. ZIP dosyası için karma oluşturmak için aşağıdaki PowerShell komutunu çalıştırın
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v1.19.06.27.zip -Algorithm SHA256```

3.  Cihaz sürümü 2.19.07.30 için oluşturulan karma bu ayarlarla eşleşmelidir.

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 29a7531f32bcf69f32d964fa5ae950bc
  SHA256 | 37b3f27bc4f475872e355f04fcb8f38606c84534c17d1609f2d124444569b31

### <a name="create-the-appliance-vm"></a>VM cihazını oluşturun

İndirilen dosyayı içe aktarın ve VM'yi oluşturun.

1. Sıkıştırılmış VHD dosyasını, vm cihazının yerleştirileceği Hyper-V ana bilgisayara indirdikten sonra, sıkıştırılmış dosyayı ayıklayın.
    - Çıkarılan konumda, dosya **AzureMigrateAppliance_VersionNumber**adlı bir klasöre unzips .
    - Bu klasör, **AzureMigrateAppliance_VersionNumber**olarak da adlandırılan bir alt klasör içerir.
    - Bu alt klasör üç alt klasör daha içerir - **Anlık Görüntüler,** **Sanal Sabit Diskler**ve **Sanal Makineler.**

2. Hyper-V Yöneticisi'ni açın. **Eylemlerde,** **Sanal MakineYi İçe Aktar'ı**tıklatın.

    ![VHD'yi dağıt](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. Sanal Makine > Alma Sihirbazı'nda **başlamadan önce** **İleri'yi**tıklatın.
3. **Klasörü Bul'da** **Sanal Makineler** klasörünü seçin. Ardından **İleri**’ye tıklayın.
1. **Sanal Makine Seç'te** **İleri'yi**tıklatın.
2. **Alma Türü Seç'te,** sanal **makineyi kopyala'yı tıklatın (yeni bir benzersiz kimlik oluşturun)**. Ardından **İleri**’ye tıklayın.
3. **Hedef Seç'te**varsayılan ayarbırakın. **İleri**’ye tıklayın.
4. **Depolama Klasörleri'nde**varsayılan ayarı bırakın. **İleri**’ye tıklayın.
5. **Ağ**Seç'te, VM'nin kullanacağı sanal anahtarı belirtin. Anahtar, Azure'a veri göndermek için internet bağlantısına ihtiyaç duyar. Sanal anahtar oluşturma hakkında [bilgi edinin.](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines)
6. **Özetle,** ayarları gözden geçirin. Ardından, **Son**'a tıklayın.
7. Sanal **Makineler>** Hyper-V Manager'da VM'yi başlatın.


## <a name="verify-appliance-access-to-azure"></a>Azure'a cihaz erişimini doğrulama

VM cihazının [genel](migrate-appliance.md#public-cloud-urls) ve [resmi](migrate-appliance.md#government-cloud-urls) bulutlar için Azure URL'lerine bağlanabileceğinden emin olun.

### <a name="configure-the-appliance"></a>Cihazı yapılandırın

Cihazı ilk kez ayarlayın.

> [!NOTE]
> Cihazı indirilen VHD yerine [PowerShell komut dosyası](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı önemli değildir.

1. Sanal **Makineler>** Hyper-V Manager'da VM > **Connect'e**sağ tıklayın.
2. Cihazın dilini, saatini ve parolasını sağlayın.
3. VM'ye bağlanabilen herhangi bir makinede tarayıcı açın ve cihaz web uygulamasının URL'sini açın: **https:// cihaz adı veya IP*adresi*: 44368**.

   Alternatif olarak, uygulama kısayolu tıklayarak cihaz masaüstünden uygulamayı açabilirsiniz.
1. Web uygulamasında > **ön koşulları ayarlayın,** aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM'nin internet erişimi ne olduğunu denetler. VM proxy kullanıyorsa:
      - **Proxy ayarlarını**tıklatın ve proxy adresini ve dinleme http://ProxyIPAddress http://ProxyFQDNbağlantı noktasını formda veya .
      - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
      - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. VM keşfinin düzgün çalışması için cihazdaki süre internet süresiyle uyumlu olmalıdır.
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


### <a name="delegate-credentials-for-smb-vhds"></a>Kobİ VHD'ler için temsilci kimlik bilgileri

SMB'lerde VHD çalıştırıyorsanız, cihazdan Hyper-V ana bilgisayarlarına kimlik bilgilerini delegasyonuna olanak sağlamalısınız. Bu aşağıdakileri gerektirir:

- Her ev sahibinin cihaz için bir temsilci olarak hareket etmesini sağlarsınız. Öğreticileri sırayla izlediyseniz, bunu önceki eğitimde, hyper-v'yi değerlendirme ve geçiş için hazırladığınızda yaptınız. Ev sahipleri için CredSSP'yi [el ile](tutorial-prepare-hyper-v.md#enable-credssp-on-hosts)veya bunu yapan [bir komut dosyası çalıştırarak](tutorial-prepare-hyper-v.md#prepare-with-a-script) ayarlamanız gerekir.
- Azure Geçiş cihazının istemci olarak hareket edebilmesi için CredSSP delegasyonuna olanak tanıyan temsilcilik, kimlik bilgilerini bir ana bilgisayara devretebilir.

Cihazı aşağıdaki gibi etkinleştirin:

#### <a name="option-1"></a>Seçenek 1

VM cihazında bu komutu çalıştırın. HyperVHost1/HyperVHost2 örnek ana bilgisayar adlarıdır.

```
Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
```

Örnek: ` Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force `

#### <a name="option-2"></a>2. Seçenek

Alternatif olarak, cihaz la ilgili Yerel Grup İlkesi Düzenleyicisi'nde bunu yapın:

1. **Yerel Bilgisayar İlkesi** > **Bilgisayar Yapılandırmasında,** **Yönetim Şablonları** > **Sistem** > **Kimlik Bilgileri Delegasyonu'na**tıklayın.
2. **Yeni kimlik bilgilerini atamasını İzin ver'e**çift tıklatın ve **Etkin'i**seçin.
3. **Seçenekler'de,** **Wsman/** önek olarak keşfetmek istediğiniz her Hyper-V ana bilgisayarını **göster'i**tıklatın ve ekleyin.
4. Daha sonra, **Kimlik Bilgileri Delegasyonu'nda,** **ntlm yalnızca sunucu kimlik doğrulaması ile yeni kimlik bilgilerini ataya izin**ver'i çift tıklatın. Yine, **wsman/** önek olarak, listeye keşfetmek istediğiniz her Hyper-V ana bilgisayar ekleyin.

## <a name="start-continuous-discovery"></a>Sürekli keşfi başlatma

Cihazdan Hyper-V ana bilgisayarlarına veya kümelerine bağlanın ve VM keşfini başlatın.

1. **Kullanıcı adı** ve **Parola'da,** cihazın VM'leri keşfetmek için kullanacağı hesap kimlik bilgilerini belirtin. Kimlik bilgileri için uygun bir ad belirtin ve **ayrıntıları kaydet'i**tıklatın.
2. **Ana bilgisayar ekle'yi**tıklatın ve Hyper-V ana bilgisayar/küme ayrıntılarını belirtin.
3. **Doğrula**'ya tıklayın. Doğrulamadan sonra, her ana bilgisayar/kümede bulunabilecek VM sayısı gösterilir.
    - Doğrulama bir ana bilgisayar için başarısız olursa, **Durum** sütunundaki simgenin üzerine geçerek hatayı gözden geçirin. Sorunları düzeltin ve yeniden doğrulayın.
    - Ana bilgisayarları veya kümeleri kaldırmak için **Sil'>** seçin.
    - Belirli bir ana bilgisayarı kümeden kaldıramazsınız. Yalnızca tüm kümeyi kaldırabilirsiniz.
    - Kümedeki belirli ana bilgisayarlarla ilgili sorunlar olsa bile, bir küme ekleyebilirsiniz.
4. Doğrulamadan sonra **Kaydet'i** tıklatın ve bulma işlemini başlatmak için keşfi başlatın.

Bu keşif başlar. Keşfedilen sunucuların meta verilerinin Azure portalında görünmesi ana bilgisayar başına yaklaşık 1,5 dakika sürer.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Keşif bittikten sonra, VM'lerin portalda göründüğünü doğrulayabilirsiniz.

1. Azure Geçir panosunu açın.
2. **Azure Geçir - Sunucular** > Azure**Geçir: Sunucu Değerlendirme** **sayfasında, Keşfedilen sunucuların**sayısını görüntüleyen simgeyi tıklatın.

## <a name="set-up-an-assessment"></a>Bir değerlendirme ayarlama

Azure Geçir Sunucu Değerlendirmesi'ni kullanarak çalıştırabileceğiniz iki tür değerlendirme vardır.

**Değerlendirme** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı değerlendirmeler | **Önerilen VM boyutu**: CPU ve bellek kullanım verilerine dayanır.<br/><br/> **Önerilen disk türü (standart veya premium yönetilen disk)**: IOPS'ye ve şirket içi disklerin üretim emresine dayanır.
**Şirket içinde olduğu gibi** | Şirket içi boyutlandırmaya dayalı değerlendirmeler. | **Önerilen VM boyutu**: Şirket içi VM boyutuna göre<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.



### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucularda** > **Azure Geçir: Sunucu Değerlendirmesi**, **Değerlendir'i**tıklatın.

    ![Değerlendirme](./media/tutorial-assess-hyper-v/assess.png)

3. **Sunucuları**Değerlendir'de, değerlendirme için bir ad belirtin.
4. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. **Bir grubu seç veya oluştur'da**Yeni **Oluştur'u**seçin ve bir grup adı belirtin. Bir grup değerlendirme için bir veya daha fazla VM'yi bir araya toplar.
4. **Gruba makine ekle'de,** gruba eklemek için VM'leri seçin.
5. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **Değerlendirme Oluştur'u** tıklatın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Değerlendirme oluşturulduktan sonra, **Sunucular** > **Azure Geçiş: Sunucu Değerlendirmesi'nde**görüntüleyin.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.


## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme açıklar:

- **Azure hazırlığı**: VM'lerin Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'da VM'leri çalıştırmak için tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.


### <a name="view-an-assessment"></a>Bir değerlendirmeyi görüntüleme

1. **Geçiş hedef** >  **sunucularında Azure** > **Geçiş: Sunucu Değerlendirmesi**, **Değerlendirmeler'i**tıklatın.
2. **Değerlendirmeler'de,** açmak için bir değerlendirmeye tıklayın.

    ![Değerlendirme özeti](./media/tutorial-assess-hyper-v/assessment-summary.png)


### <a name="review-azure-readiness"></a>Azure hazırlık durumunu gözden geçirin

1. **Azure'a hazır durumda,** VM'lerin Azure'a geçişiçin hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure için Hazır**: Azure Geçirim, değerlendirmede VM boyutu ve VM'ler için maliyet tahminleri önerir.
    - **Koşullara hazır**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazır değil**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazırlık bilinmiyor**: Veri kullanılabilirliği sorunları nedeniyle Azure Geçiş hazır durumunu değerlendiremediğinde kullanılır.

2. **Azure hazırlık** durumunu tıklatın. VM hazırlık ayrıntılarını görüntüleyebilir ve bilgi işlem, depolama ve ağ ayarları da dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya inebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm, Azure'da VM'leri çalıştırmanın tahmini işlem ve depolama maliyetini gösterir.

1. Aylık işlem ve depolama maliyetlerini gözden geçirin. Maliyetler, değerlendirilen gruptaki tüm VM'ler için toplanır.

    - Maliyet tahminleri, bir makinenin boyut önerilerini ve disklerini ve özelliklerini temel alınr.
    - Hesaplama ve depolama için tahmini aylık maliyetler gösterilir.
    - Maliyet tahmini, şirket içi VM'leri IaaS VM olarak çalıştırmak içindir. Azure Geçir Sunucu Değerlendirmesi, PaaS veya SaaS maliyetlerini dikkate almaz.

2. Aylık depolama maliyeti tahminlerini gözden geçirebilirsiniz. Bu görünüm, değerlendirilen grup için farklı depolama diskleri türlerine bölünmüş toplu depolama maliyetlerini gösterir.
3. Belirli VM'ler için ayrıntıları görmek için ayrıntıya inebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performansa dayalı değerlendirmeler çalıştırdığınızda, değerlendirmeye bir güven derecelendirmesi atanır.

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-hyper-v/confidence-rating.png)

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

Hyper-V VM'leri Azure'a Azure'a nasıl geçirteceklerini öğrenmek için serinin üçüncü öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Hyper-V VM’leri Azure’a Geçirme](./tutorial-migrate-hyper-v.md)
