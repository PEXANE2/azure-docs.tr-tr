---
title: Azure geçişi ile Azure 'a geçiş için VMware VM 'lerini değerlendirin
description: Azure geçişi kullanılarak Azure 'a geçiş için şirket içi VMware VM 'lerinin nasıl değerlendirileneceğini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: 7b27637ca63ec69d7f4c33f05e7c037d67676b2d
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828290"
---
# <a name="assess-vmware-vms-with-azure-migrate-server-assessment"></a>Azure geçişi ile VMware VM 'lerini değerlendirin: Sunucu Değerlendirmesi

Bu makalede, Azure geçişi kullanılarak şirket içi VMware VM 'lerinin nasıl değerlendirireceğiniz gösterilmektedir: Sunucu değerlendirmesi aracı.

[Azure geçişi](migrate-services-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.



Bu öğretici, VMware VM 'lerinin Azure 'a nasıl değerlendirileceğini ve geçirileceğini gösteren bir serinin ikinci saniyedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir Azure geçişi projesi ayarlayın.
> * VM 'Leri değerlendirmek için şirket içinde çalışan bir Azure geçiş gereci ayarlayın.
> * Şirket içi VM 'lerin sürekli olarak bulunmasını başlatın. Gereç, bulunan VM 'Ler için yapılandırma ve performans verilerini Azure 'a gönderir.
> * Bulunan VM 'Leri gruplandırın ve VM grubunu değerlendirin.
> * Değerlendirmeyi gözden geçirin.



> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için, nasıl yapılır makalelerini gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

- Bu serideki ilk öğreticiyi [doldurun](tutorial-prepare-vmware.md) . Bunu yapmazsanız, bu öğreticideki yönergeler çalışmaz.
- İlk öğreticide yapmanız gerekenler şunlardır:
    - Azure geçişi için [Azure Izinleri ayarlayın](tutorial-prepare-vmware.md#prepare-azure) .
    - [VMware](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) 'i değerlendirme için hazırlayın. VMware ayarlarının doğrulanması gerekir ve bir OVA şablonuyla bir VMware VM oluşturma izinlerine sahip olmanız gerekir. Ayrıca, VM keşfi için ayarlanmış bir hesabınız olmalıdır. Gerekli bağlantı noktaları kullanılabilir olmalıdır ve Azure 'a erişim için gereken URL 'Lerden haberdar olmanız gerekir.


## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi ayarlama

Yeni bir Azure geçişi projesini aşağıdaki şekilde ayarlayın.

1. **Tüm hizmetlere**> Azure Portal **Azure geçişi**' ni arayın.
2. **Hizmetler**altında **Azure geçişi**' ni seçin.
3. Genel olarak, **sunucuları keşfet, değerlendir ve geçir**altında, **sunucuları değerlendir ve geçir**' **e**tıklayın.

    ![Sunucuları bulma ve değerlendirme](./media/tutorial-assess-vmware/assess-migrate.png)

4. **Başlarken**' de **Araçlar Ekle**' ye tıklayın.
5. **Projeyi geçir**' de Azure aboneliğinizi seçin ve yoksa bir kaynak grubu oluşturun.     
6. **Proje ayrıntıları**' nda proje adını ve projeyi oluşturmak istediğiniz coğrafi konumu belirtin. Asya, Avrupa, UK ve Birleşik Devletler desteklenir.

    - Proje Coğrafya yalnızca şirket içi VM 'lerden toplanan meta verileri depolamak için kullanılır.
    - Bir geçiş çalıştırdığınızda herhangi bir hedef bölge seçebilirsiniz.

    ![Azure geçişi projesi oluşturma](./media/tutorial-assess-vmware/migrate-project.png)


7.           **İleri**'ye tıklayın.
8. **Değerlendirme Seç aracında**Azure geçişi ' **ni seçin: Sunucu değerlendirmesi** > **İleri**.

    ![Azure geçişi projesi oluşturma](./media/tutorial-assess-vmware/assessment-tool.png)

9. **Geçiş aracı Seç**bölümünde şimdi > **İleri** **için bir geçiş aracı eklemeyi atla**' yı seçin.
10. **Gözden geçir + araçlar Ekle**' de ayarları gözden geçirin ve **araç ekle**' ye tıklayın.
11. Azure geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız, Azure geçişi panosundaki **sunuculardan** erişebilirsiniz.


## <a name="set-up-the-appliance-vm"></a>Gereç sanal makinesini ayarlama

Azure Geçişi: Sunucu değerlendirmesi bir basit VMware VM gereci çalıştırır.

- Bu gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi sunucu değerlendirmesi ' ne VM meta verilerini ve performans verilerini gönderir.
- Gereci kurmak için şunları yapın:
    - Bir OVA şablon dosyasını indirip vCenter Server içe aktarın.
    - Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
    - Gereci ilk kez yapılandırın ve Azure geçişi projesi ile kaydedin.
- Tek bir Azure geçişi projesi için birden çok gereçini ayarlayabilirsiniz. Tüm gereçlerde 35.000 VM 'ye kadar bulma desteklenir. Her gereç için en fazla 10.000 sunucu bulunabilir.

### <a name="download-the-ova-template"></a>OVA şablonunu indirin

1. **Geçiş hedefleri** > sunucularındaAzuregeçişi > : **Sunucu değerlendirmesi**için **bul**' a tıklayın.
2. Makinelerde bulunan makinelerde**makineler sanallaştırılmış mı?** , **VMware vSphere Hiper Yöneticisi ile Evet 'e**tıklayın. > 
3. İndirmek için **İndir** ' e tıklayın. OVA şablon dosyası.

    ![.ova dosyasını indir](./media/tutorial-assess-vmware/download-ova.png)


### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce OVA dosyasının güvenli olduğundan emin olun.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Sürüm 2.19.07.30 için, oluşturulan karma bu değerlerle eşleşmelidir. 

  **Algoritma** | **Karma değeri**
  --- | ---
  MD5 | 27230f3b012187860281b912ee661709
  SHA256 | c0a5b5998b7f38ac6e57ea9a808ecc4295795e18f9ca99c367585068883f06e7


### <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.

    ![OVF dağıtma](./media/tutorial-assess-vmware/deploy-ovf.png)

2. OVF şablonu Dağıtma Sihirbazı > **kaynak**bölümünde ova dosyasının konumunu belirtin.
3. **Ad** ve **konum**bölümünde VM için bir kolay ad belirtin. VM 'nin barındırıldığı envanter nesnesini seçin.
5. **Konakta/kümede**, sanal makinenin çalıştırılacağı konağı veya kümeyi belirtin.
6. **Depolama**alanında VM için depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. **Ağ eşlemesinde**, sanal makinenin bağlanacağı ağı belirtin. Ağ, Azure geçişi sunucu değerlendirmesi 'ne meta veri göndermek için internet bağlantısı gerektirir.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç VM 'sinin [Azure URL 'lerine](migrate-support-matrix-vmware.md#assessment-url-access-requirements)bağlanabildiğinizden emin olun.


### <a name="configure-the-appliance"></a>Gereci yapılandırma

Aşağıdaki adımları kullanarak gereç 'yi ayarlayın.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı gereç masaüstünden açabilirsiniz.
4. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
        - **Proxy ayarları**' na tıklayın ve proxy adresini ve dinleme bağlantı noktasını, veya http://ProxyIPAddress http://ProxyFQDN biçiminde belirtin.
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: bulmanın düzgün çalışması için gereç üzerindeki zaman internet ile eşitlenmiş olmalıdır.
    - **Güncelleştirmeleri yükler**: Gereç, en son güncelleştirmelerin yüklenmesini sağlar.
    - **VDDK 'Yi yükler**: Gereç, VMWare vSphere sanal disk geliştirme seti 'nin (VDDK) yüklü olduğunu denetler.
        - Azure Geçişi: Sunucu geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır.
        - VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini gereç üzerindeki belirtilen konuma ayıklayın.

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. **Oturum aç**' a tıklayın. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
2. Yeni sekmede, Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adınızla ve parolanızla oturum açın.
    - PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün.
2. Azure geçişi projesinin oluşturulduğu aboneliği seçin, ardından projeyi seçin.
3. Gereç için bir ad belirtin. Ad 14 karakter veya daha az olmalıdır.
4. **Kaydol**' a tıklayın.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Şimdi, gerecden vCenter Server 'e bağlanın ve VM bulmayı başlatın.

1. **VCenter Server ayrıntılarını belirtin**bölümünde vCenter Server adı (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server dinlediği özel bir bağlantı noktası belirtebilirsiniz.
2. **Kullanıcı adı** ve **parola**' da, gerecin vCenter sunucusundaki VM 'leri bulması için kullanacağı salt okuma hesabı kimlik bilgilerini belirtin. Hesabın [bulma için gerekli izinlere](migrate-support-matrix-vmware.md#assessment-vcenter-server-permissions)sahip olduğundan emin olun. VCenter hesabına erişimi uygun şekilde sınırlayarak bulma kapsamını belirleyebilirsiniz; kapsam bulma hakkında daha fazla [](tutorial-assess-vmware.md#scoping-discovery)bilgi edinin.
3. Gerecin vCenter Server bağlanabildiğini sağlamak için **bağlantıyı doğrula** ' ya tıklayın.
4. Bağlantı kurulduktan sonra Kaydet ' e tıklayın **ve bulmayı başlatın**.

Bu, bulmayı başlatır. Keşfedilen VM 'lerin meta verilerinde portalda görünmesi 15 dakika sürer.

### <a name="scoping-discovery"></a>Kapsam bulma

Bulma için kullanılan vCenter hesabının erişimi sınırlanarak bulma kapsamı oluşturulabilir. Kapsamı, veri merkezleri, kümeler, kümeler klasörü, konaklar, konaklar klasörü veya ayrı VM 'Ler vCenter Server şekilde ayarlayabilirsiniz. 

> [!NOTE]
> Bugün, vCenter hesabının vCenter VM klasör düzeyinde erişimi varsa sunucu değerlendirmesi VM 'Leri bulamaz. Bulma işlemini VM klasörlerine göre yapmak istiyorsanız, vCenter hesabının bir VM düzeyinde salt okuma erişimi olduğundan emin olarak bunu yapabilirsiniz.  Bu, bunu nasıl yapacağınız hakkında yönergeler aşağıda verilmiştir:
>
> 1. Keşfi kapsamında olmasını istediğiniz VM klasörlerindeki tüm VM 'lerde salt okunurdur izinleri atayın. 
> 2. VM 'Lerin barındırıldığı tüm üst nesnelere salt okuma erişimi verin. Tüm üst nesneler-konak, konaklar klasörü, küme, küme klasörü, veri merkezine kadar olan hiyerarşide yer alır. İzinleri tüm alt nesnelere yaymaya gerek yoktur.
> 3. Veri merkezini *koleksiyon kapsamı*olarak seçme bulma için kimlik bilgilerini kullanın. RBAC kümesi, karşılık gelen vCenter kullanıcısının yalnızca kiracıya özgü VM 'lere erişmesini sağlar.
>
> Konaklar ve kümeler klasörünün desteklendiğini unutmayın.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulmadan sonra, VM 'Lerin Azure portal göründüğünü doğrulayabilirsiniz.

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular** > **Azure geçişi: Sunucu değerlendirmesi** sayfasında, **bulunan sunucuların**sayısını görüntüleyen simgeye tıklayın.

## <a name="set-up-an-assessment"></a>Değerlendirme ayarlama

Azure geçişi 'ni kullanarak oluşturabileceğiniz iki tür değerlendirme vardır: etmenize yardımcı olur.

**Değerlendirmesini** | **Ayrıntılar** | **Veri**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı değerlendirmeler | **ÖNERILEN VM boyutu**: CPU ve bellek kullanımı verilerine göre.<br/><br/> **Önerilen disk türü (Standart veya Premium yönetilen disk)** : Şirket içi disklerin ıOPS ve aktarım hızına göre.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler. | **ÖNERILEN VM boyutu**: Şirket içi VM boyutuna bağlı olarak<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.


### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğu, **değerlendir**' e tıklayın.

    ![Değerlendir](./media/tutorial-assess-vmware/assess.png)

2. **Sunucuları değerlendir**bölümünde, değerlendirme için bir ad belirtin.
3. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-vmware/view-all.png)

3. **Grup Seç veya oluştur**' da, **Yeni oluştur**' u seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla VM 'yi toplar.
4. **Gruba makine ekleme**' de gruba eklenecek VM 'ler ' i seçin.
5. Grubu oluşturmak için **değerlendirme oluştur** ' a tıklayın ve değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-vmware/assessment-create.png)

6. Değerlendirme oluşturulduktan sonra **sunucuları** > **Azure geçişi ' nde görüntüleyin: Sunucu değerlendirmesi** > **değerlendirmeleri**.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.



## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme şunları açıklar:

- **Azure hazırlığı**: VM 'Lerin Azure 'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure 'da VM 'Leri çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçiş sonrasında disk depolaması için tahmini maliyetler.

### <a name="view-an-assessment"></a>Değerlendirme görüntüleme

1. **Geçiş hedefleri** >  **sunucularında**Azure geçişi 'nde **değerlendirmeler ' a tıklayın: Sunucu değerlendirmesi**.
2. **Değerlendirmede**, bir değerlendirmeye tıklayarak açın.

    ![Değerlendirme özeti](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlığını gözden geçirme

1. **Azure 'a hazırlık**bölümünde, VM 'lerin Azure 'a geçiş için hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure Için hazırlanın**: Azure geçişi, değerlendirmede VM 'Lerin bir VM boyutunu ve maliyet tahminlerini önerir.
    - **Koşullara hazırlanın**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazırlanma**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazır olma durumu bilinmiyor**: Azure geçişi, veri kullanılabilirliği sorunları nedeniyle hazırlığı değerlendiremez.

2. **Azure hazırlık** durumuna tıklayın. VM hazırlığı ayrıntılarını görüntüleyebilir ve işlem, depolama ve ağ ayarları dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya gidebilirsiniz.



### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm Azure 'da çalışan VM 'lerin tahmini işlem ve depolama maliyetini gösterir.

1. Aylık işlem ve depolama maliyetlerini gözden geçirin. Ücretler, değerlendirilen gruptaki tüm VM 'Ler için toplanır.

    - Maliyet tahminleri, bir makine için boyut önerilerini ve bunların disklerini ve özelliklerini temel alır.
    - İşlem ve depolama için tahmini aylık maliyetler gösterilir.
    - Maliyet tahmini, şirket içi VM 'Leri IaaS VM 'Leri olarak çalıştırmaya yöneliktir. Azure geçişi sunucu değerlendirmesi PaaS veya SaaS maliyetlerini göz önünde bulundurmaz.

2. Aylık depolama maliyeti tahminlerini gözden geçirebilirsiniz. Bu görünüm, farklı türlerdeki depolama disklerinin üzerine bölünen, değerlendirilen grup için toplanan depolama maliyetlerini gösterir.
3. Belirli VM 'Lerin ayrıntılarını görmek için ayrıntıya gidebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performans tabanlı değerlendirmeler çalıştırdığınızda, değerlendirmeye bir güvenilirlik derecelendirmesi atanır.

![Güvenilirlik derecesi](./media/tutorial-assess-vmware/confidence-rating.png)

- 1-yıldız (en düşük) ile 5 yıldız (en yüksek) arasında bir derecelendirme verilir.
- Güvenilirlik derecelendirmesi, değerlendirme tarafından belirtilen boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlıdır.

Bir değerlendirme için güven derecelendirmeleri aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız

Güvenilirlik derecelendirmeleri için en iyi uygulamalar hakkında [daha fazla bilgi edinin](best-practices-assessment.md#best-practices-for-confidence-ratings) .


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure geçişi gereci ayarlama
> * Bir değerlendirme oluşturulup gözden geçirildi

VMware VM 'lerini Azure 'a geçirme sunucu geçişi ile Azure 'a geçirmeyi öğrenmek için serideki üçüncü öğreticiye geçin.

> [!div class="nextstepaction"]
> [VMware VM 'lerini geçirme](./tutorial-migrate-vmware.md)
