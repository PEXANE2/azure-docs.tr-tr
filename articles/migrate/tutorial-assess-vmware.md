---
title: Azure geçişi sunucu değerlendirmesini kullanarak Azure 'a geçiş için VMware VM 'lerini değerlendirin
description: Azure geçişi kullanılarak Azure 'a geçiş için şirket içi VMware VM 'lerinin nasıl değerlendirileneceğini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: hamusa
ms.openlocfilehash: 7f161afe13bad8c548806d4b4ceb9372dc511cc3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79239325"
---
# <a name="assess-vmware-vms-by-using-azure-migrate-server-assessment"></a>Azure geçişi sunucu değerlendirmesini kullanarak VMware VM 'lerini değerlendirin

Bu makalede, şirket içi VMware sanal makinelerini (VM 'Ler) Azure geçişi 'nde sunucu değerlendirmesi aracını kullanarak nasıl değerlenbileceğiniz gösterilmektedir.

[Azure geçişi](migrate-services-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Microsoft iş ortaklarından Azure geçiş araçları ve bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bu öğretici, VMware VM 'lerinin Azure 'a nasıl değerlendirileceğini ve geçirileceğini gösteren bir serinin ikinci saniyedir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
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

Bu serideki [ilk öğreticiyi doldurun](tutorial-prepare-vmware.md) . Bunu yapmazsanız, bu öğreticideki yönergeler çalışmaz.

İlk öğreticide yapmanız gerekenler şunlardır:

- Azure geçişi için [Azure Izinleri ayarlayın](tutorial-prepare-vmware.md#prepare-azure) .
- [VMware](tutorial-prepare-vmware.md#prepare-for-vmware-vm-assessment) 'i değerlendirme için hazırla:
   - [Doğrula](migrate-support-matrix-vmware.md#vmware-requirements) VMware ayarları.
   - Bir OVA şablonuyla VMware VM oluşturmak için VMware 'de izinleri ayarlayın.
   - [VM bulma için bir hesap](migrate-support-matrix-vmware.md#vmware-requirements)ayarlayın. 
   - [Gerekli bağlantı noktalarını](migrate-support-matrix-vmware.md#port-access) kullanılabilir hale getirin.
   - Azure 'a erişmek için [gereken URL 'lerden](migrate-replication-appliance.md#url-access) haberdar olun.

## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi ayarlama

Yeni bir Azure geçişi projesini aşağıdaki şekilde ayarlayın:

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
1. **Hizmetler** altında **Azure Geçişi**’ni seçin.
1. **Genel bakış**bölümünde, **sunucuları değerlendir, değerlendir ve geçir**altında, **sunucuları değerlendir ve geçir**' i seçin.

   ![Sunucuları değerlendirmek ve geçirmek için düğme](./media/tutorial-assess-vmware/assess-migrate.png)

1. **Başlarken**' de **Araçlar Ekle**' yi seçin.
1. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.     
1. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. Asya, Avrupa, Birleşik Krallık ve Birleşik Devletler desteklenir.

   Proje coğrafyası yalnızca şirket içi sanal makinelerden toplanan meta verileri depolamak için kullanılır. Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-assess-vmware/migrate-project.png)

1. **İleri**’yi seçin.
1. **Değerlendirme Seç aracında** **Azure geçişi: Sunucu değerlendirmesi** > **İleri**' yi seçin.

   ![Sunucu değerlendirmesi aracı için seçim](./media/tutorial-assess-vmware/assessment-tool.png)

1. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
1. **Gözden geçir + araçlar Ekle**' de ayarları gözden geçirin ve **araç ekle**' yi seçin.
1. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="set-up-the-appliance-vm"></a>Gereç sanal makinesini ayarlama

Azure geçişi sunucu değerlendirmesi, hafif bir VMware VM gereci çalıştırır. Bu gereç VM bulma işlemini gerçekleştirir ve VM meta verilerini ve performans verilerini toplar.

Gereci ayarlamak için şunları yapın:

- Bir OVA şablon dosyasını indirip vCenter Server içe aktarın.
- Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
- Gereci ilk kez yapılandırın ve Azure geçişi projesi ile kaydedin.

Tek bir Azure geçişi projesi için birden çok gereçini ayarlayabilirsiniz. Sunucu değerlendirmesi tüm gereçlerde 35.000 adede kadar VM bulmayı destekler. Her gereç için en fazla 10.000 sunucu bulabilir.

### <a name="download-the-ova-template"></a>OVA şablonunu indirin

1. **Azure geçişi: Sunucu değerlendirmesi** > **sunucuları** > **geçiş hedeflerde** **bul**' u seçin.
1. **Makineleriniz > ** makinelerde **sanallaştırılmış mı?** , **VMware vSphere Hiper Yöneticisi ile Evet '** i seçin.
1. OVA şablon dosyasını indirmek için **İndir** ' i seçin.

   ![OVA dosyasını indirme seçimleri](./media/tutorial-assess-vmware/download-ova.png)

### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtım yapmadan önce OVA dosyasının güvenli olup olmadığını denetleyin:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
1. OVA dosyasının karmasını oluşturmak için aşağıdaki komutu çalıştırın:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

Sürüm 2.19.07.30 için, oluşturulan karma şu değerlerle eşleşmelidir:

**Algoritma** | **Karma değeri**
--- | ---
MD5 | c06ac2a2c0f870d3b274a0b7a73b78b1
SHA256 | 4ce4faa3a78189a09a26bfa5b817c7afcf5b555eb46999c2fad9d2ebc808540c

### <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun:

1. VSphere Istemci konsolunda **dosya** > **ovf şablonu dağıt**' ı seçin.

   ![OVF şablonu dağıtmak için menü komutu](./media/tutorial-assess-vmware/deploy-ovf.png)

1. OVF şablonu Dağıtma Sihirbazı > **kaynak**bölümünde ova dosyasının konumunu belirtin.
1. **Ad** ve **konum**bölümünde VM için bir kolay ad belirtin. VM 'nin barındırıldığı envanter nesnesini seçin.
1. **Konakta/kümede**, sanal makinenin çalıştırılacağı konağı veya kümeyi belirtin.
1. **Depolama**alanında VM için depolama hedefini belirtin.
1. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
1. **Ağ eşlemesinde**, sanal makinenin bağlanacağı ağı belirtin. Ağ, Azure geçişi sunucu değerlendirmesi 'ne meta veri göndermek için internet bağlantısı gerektirir.
1. Ayarları gözden geçirip onaylayın ve ardından **son**' u seçin.

### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç VM 'sinin [Azure URL 'lerine](migrate-appliance.md#url-access)bağlanabildiğinizden emin olun.

### <a name="configure-the-appliance"></a>Gereci yapılandırma

Aşağıdaki adımları kullanarak gereç ayarlayın:

1. VSphere Istemci konsolunda VM 'ye sağ tıklayın ve ardından **Konsolu Aç**' ı seçin.
1. Gereç için dil, saat dilimi ve parola sağlayın.
1. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayolunu seçerek uygulamayı gereç masaüstünden açabilirsiniz.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
   - **Lisans**: lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
   - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
     - **Proxy ayarları**' nı seçin ve proxy adresini ve dinleme bağlantı noktasını http://ProxyIPAddress veya http://ProxyFQDNolarak belirtin.
     - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
     - Yalnızca HTTP proxy 'nin desteklendiğini unutmayın.
   - **Zaman eşitleme**: bulmanın düzgün çalışması için gereç üzerindeki zaman internet ile eşitlenmiş olmalıdır.
   - **Güncelleştirmeleri yükleme**: gereç en son güncelleştirmelerin yüklü olmasını sağlar.
   - **VDDK 'Yi yükleme**: gereç, VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olup olmadığını denetler. Yüklü değilse, VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini gereç üzerindeki belirtilen konuma ayıklayın.

     Azure geçişi sunucu geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır.       

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. **Oturum aç '** ı seçin. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
1. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün.
1. Azure geçişi projesinin oluşturulduğu aboneliği seçin ve ardından projeyi seçin.
1. Gereç için bir ad belirtin. Ad 14 karakter veya daha kısa bir harf olmalıdır.
1. **Kaydol**’u seçin.

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

### <a name="specify-vcenter-server-details"></a>vCenter Server ayrıntılarını belirtin
1. **VCenter Server ayrıntılarını belirtin**bölümünde vCenter Server örneğinin adını (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server dinlediği özel bir bağlantı noktası belirtebilirsiniz.
1. **Kullanıcı adı** ve **parola**' da, gerecin vCenter Server örneğindeki VM 'leri bulması için kullanacağı vCenter Server hesabı kimlik bilgilerini belirtin. 

   Hesabın [bulma için gerekli izinlere](migrate-support-matrix-vmware.md#vmware-requirements)sahip olduğundan emin olun. VCenter hesabına erişimi sınırlayarak [bulma kapsamını](tutorial-assess-vmware.md#set-the-scope-of-discovery) belirleyebilirsiniz.
1. Gerecin vCenter Server bağlanabildiğini sağlamak için **bağlantıyı doğrula** ' yı seçin.

### <a name="specify-vm-credentials"></a>VM kimlik bilgilerini belirtin
Uygulama, rol ve özellik keşfi ve VM 'lerin bağımlılıklarını görselleştirmeye yönelik olarak, VMware VM 'lerine erişimi olan VM kimlik bilgileri sağlayabilirsiniz. Windows VM 'ler için bir kimlik bilgisi ve Linux sanal makineleri için bir kimlik bilgisi ekleyebilirsiniz. Gerekli erişim izinleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware) .

> [!NOTE]
> Bu girdi isteğe bağlıdır, ancak uygulama bulmayı ve aracısız bağımlılık görselleştirmesini etkinleştirmek istiyorsanız buna ihtiyacınız vardır.

1. **VM 'lerde uygulamaları ve bağımlılıkları bul**' da, **kimlik bilgileri ekle**' yi seçin.
1. **Işletim sistemi**için bir seçim yapın.
1. Kimlik bilgisi için kolay bir ad sağlayın.
1. **Kullanıcı adı** ve **parola**' da, VM 'lerde en az konuk erişimi olan bir hesap belirtin.
1. **Add (Ekle)** seçeneğini belirleyin.

VCenter Server örneğini ve sanal makine kimlik bilgilerini (isteğe bağlı) belirledikten sonra, şirket içi ortamı bulmaya başlamak için **Kaydet ve bulmayı Başlat** ' ı seçin.

Keşfedilen VM 'lerin meta verilerinde portalda görünmesi 15 dakika sürer. Yüklenen uygulamaların, rollerin ve özelliklerin bulunması biraz zaman alır. Süre, bulunan VM sayısına bağlıdır. 500 VM 'Ler için, uygulama envanterinin Azure geçişi portalında görünmesi yaklaşık 1 saat sürer.

### <a name="set-the-scope-of-discovery"></a>Bulma kapsamını ayarlama

Bulma için kullanılan vCenter hesabının erişimi sınırlanarak bulma kapsamı oluşturulabilir. Kapsamını veri merkezleri, kümeler, bir küme klasörü, konaklar, konaklar klasörü veya ayrı VM 'Ler vCenter Server olarak ayarlayabilirsiniz.

Kapsamı ayarlamak için aşağıdaki yordamları gerçekleştirin.

#### <a name="1-create-a-vcenter-user-account"></a>1. bir vCenter Kullanıcı hesabı oluşturun
1.  VCenter Server Yöneticisi olarak vSphere Web Istemcisinde oturum açın.
1.  **Yönetim** > **SSO kullanıcıları ve grupları**' nı seçin ve ardından **Kullanıcılar** sekmesini seçin.
1.  **Yeni Kullanıcı** simgesini seçin.
1.  Yeni bir kullanıcı oluşturmak için gerekli bilgileri girin ve ardından **Tamam**' ı seçin.

#### <a name="2-define-a-new-role-with-required-permission"></a>2. gerekli izne sahip yeni bir rol tanımlayın
Bu yordam aracısız sunucu geçişi için gereklidir.
1.  VCenter Server Yöneticisi olarak vSphere Web Istemcisinde oturum açın.
1.  **Yönetim** > **Rol Yöneticisi**' ne gidin.
1.  Açılır menüden vCenter Server örneğinizi seçin.
1.  **Rol oluştur**' u seçin.
1.  Yeni rol için bir ad girin (örneğin, <em>Azure_Migrate</em>).
1.  Yeni tanımlanan role [izin](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware) atayın.
1.  **Tamam**’ı seçin.

#### <a name="3-assign-permissions-on-vcenter-objects"></a>3. vCenter nesnelerinde izinleri atama

VCenter 'daki envanter nesnelerine, atanmış bir role sahip vCenter Kullanıcı hesabına izin atamak için iki yaklaşım vardır.

Sunucu değerlendirmesi için, keşfedilen VM 'Lerin barındırıldığı tüm üst nesneler için vCenter Kullanıcı hesabına **salt okuma** rolünü uygulamanız gerekir. Tüm üst nesneler dahil edilecek: Ana bilgisayar, konak klasörü, küme ve hiyerarşide veri merkezine kadar olan kümelerin klasörü. Bu izinler hiyerarşideki alt nesnelere yayılacaktır.

Benzer şekilde sunucu geçişi için, geçirilecek VM 'Lerin barındırıldığı tüm üst nesneler için vCenter Kullanıcı hesabı [izinlerine](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware) sahip kullanıcı tanımlı bir rol uygulamanız gerekir. Bu rol, <em>Azure _Migrate</em>olarak adlandırılabilir.

![İzin atama](./media/tutorial-assess-vmware/assign-perms.png)

Alternatif yaklaşım, Kullanıcı hesabını ve rolü veri merkezi düzeyinde atamak ve bunları alt nesnelere yaymalıdır. Ardından hesaba, bulma/geçiş yapmak istemediğiniz her nesne için **erişim** rolü (örneğin, bir VM) verin. 

Bu alternatif yapılandırma, kısabersome. Her yeni alt nesneye Ayrıca otomatik olarak üst öğeden devralınan erişim verildiğinden, bu, yanlışlıkla erişim denetimleri sunar. Bu nedenle, ilk yaklaşımı kullanmanızı öneririz.

> [!NOTE]
> Şu anda, vCenter hesabının vCenter VM klasör düzeyinde erişimi varsa sunucu değerlendirmesi VM 'Leri bulamaz. Bulma işlemini VM klasörlerine göre kapsam yapmak istiyorsanız, aşağıdaki yordamı kullanarak bunu yapabilirsiniz. VCenter hesabının VM düzeyinde salt okuma erişimine sahip olmasını sağlar.
>
> 1. Keşfi kapsamında olmasını istediğiniz VM klasörlerindeki tüm VM 'lerde salt okunurdur izinleri atayın.
> 1. VM 'Lerin barındırıldığı tüm üst nesnelere salt okuma erişimi verin. Veri merkezine kadar olan hiyerarşideki tüm üst nesneler (ana bilgisayar, konak klasörü, küme, küme klasörü) dahil edilir. İzinleri tüm alt nesnelere yaymaya gerek yoktur.
> 1. Veri merkezini **koleksiyon kapsamı**olarak seçerek bulma için kimlik bilgilerini kullanın. Ayarlanan rol tabanlı erişim denetimi, karşılık gelen vCenter kullanıcısının yalnızca kiracıya özgü VM 'lere erişmesini sağlar.
>
> Konaklar ve kümelerin klasörlerinin desteklendiğini unutmayın.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulmadan sonra, VM 'Lerin Azure portal göründüğünü doğrulayabilirsiniz:

1. Azure geçişi panosunu açın.
1. Azure **geçişi-sunucuları** > **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucuların**sayısını gösteren simgeyi seçin.

## <a name="set-up-an-assessment"></a>Değerlendirme ayarlama

Azure geçişi sunucu değerlendirmesini kullanarak iki tür değerlendirme oluşturabilirsiniz:

**Değerlendirme** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı değerlendirmeler | **ÖNERILEN VM boyutu**: CPU ve bellek kullanım verilerine göre.<br/><br/> **Önerilen disk türü (Standart veya Premium yönetilen disk)** : Şirket ıçı disklerin IOPS ve aktarım hızına göre.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler | **ÖNERILEN VM boyutu**: ŞIRKET içi VM boyutuna göre.<br/><br> **Önerilen disk türü**: değerlendirme için seçtiğiniz depolama türü ayarına göre.

## <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
1. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğunda **değerlendir**' i seçin.

   ![Değerlendirme düğmesinin konumu](./media/tutorial-assess-vmware/assess.png)

1. **Sunucuları değerlendir**bölümünde, değerlendirme için bir ad belirtin.
1. **Tümünü görüntüle**' yi seçin ve ardından değerlendirme özelliklerini gözden geçirin.

   ![Değerlendirme özellikleri](./media/tutorial-assess-vmware/view-all.png)

1. **Grup Seç veya oluştur**' da, **Yeni oluştur**' u seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla VM 'yi toplar.
1. **Gruba makine ekleme**' de gruba eklenecek VM 'ler ' i seçin.
1. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **değerlendirme oluştur** ' u seçin.

   ![Sunucuları değerlendir](./media/tutorial-assess-vmware/assessment-create.png)

1. Değerlendirme oluşturulduktan sonra, **Azure geçişi: Sunucu değerlendirmesi** > **değerlendirmeleri** > **sunucularda** görüntüleyin.
1. Bir Excel dosyası olarak indirmek için **dışarı aktarma değerlendirmesi** ' ni seçin.

## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme şunları açıklar:

- **Azure hazırlığı**: VM 'lerin Azure 'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: VM 'leri Azure 'da çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: geçişten sonra disk depolaması için tahmini maliyetler.

Bir değerlendirmeyi görüntülemek için:

1. **Sunucular** > **geçiş hedefleri** ' nde **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendirmeler** ' ı seçin.
1. **Değerlendirmede**, açmak için bir değerlendirme seçin.

   ![Değerlendirme özeti](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlığını gözden geçirme

1. **Azure 'a hazırlık**bölümünde, VM 'lerin Azure 'a geçiş için hazır olup olmadığını doğrulayın.
1. VM durumunu gözden geçirin:
    - **Azure Için hazırlanma**: Azure geçişi, değerlendirmede VM 'ler IÇIN bir VM boyutu ve maliyet tahminleri önermesinde kullanılır.
    - **Koşullara hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazır olma durumu bilinmiyor**: Azure geçişi, veri kullanılabilirliği sorunları nedeniyle hazırlığı değerlendiremediği zaman kullanılır.

1. Bir **Azure hazırlık** durumu seçin. VM hazırlığı ayrıntıları ' nı görüntüleyebilirsiniz. Ayrıca, işlem, depolama ve ağ ayarları dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya gidebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Değerlendirme özeti, Azure 'da çalışan VM 'lerin tahmini işlem ve depolama maliyetini gösterir. Ücretler, değerlendirilen gruptaki tüm VM 'Ler için toplanır. Belirli VM 'Ler için maliyet ayrıntılarını görmek için ayrıntıya gidebilirsiniz.

> [!NOTE]
> Maliyet tahminleri, bir makine, diskleri ve özellikleri için boyut önerilerini temel alır. Tahminler, şirket içi VM 'Leri IaaS VM 'Leri olarak çalıştırmaya yöneliktir. Azure geçişi sunucu değerlendirmesi PaaS veya SaaS maliyetlerini göz önünde bulundurmaz.

Değerlendirilen grup için toplanan depolama maliyetleri, farklı türlerde depolama disklerinin üzerine bölünür. 

### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Azure geçişi sunucu değerlendirmesi, 1 yıldız (en düşük) ile 5 yıldız (en yüksek) arasında performans tabanlı bir değerlendirmeye bir güvenilirlik derecelendirmesi atar.

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-vmware/confidence-rating.png)

Güvenilirlik derecelendirmesi, değerlendirmenin boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur. Derecelendirme, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine göre belirlenir:

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 yıldız
%21-%40 | 2 yıldız
%41-%60 | 3 yıldız
%61-%80 | 4 yıldız
%81-%100 | 5 yıldız

Güvenilirlik derecelendirmeleri için [en iyi uygulamalar hakkında bilgi edinin](best-practices-assessment.md#best-practices-for-confidence-ratings) .

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure geçişi gereci ayarlarsınız. Ayrıca bir değerlendirme oluşturmuş ve gözden geçirdiniz.

Azure geçiş sunucusu geçişini kullanarak VMware VM 'lerini Azure 'a geçirmeyi öğrenmek için, serideki üçüncü öğreticiye geçin:

> [!div class="nextstepaction"]
> [VMware VM 'lerini geçirme](./tutorial-migrate-vmware.md)
