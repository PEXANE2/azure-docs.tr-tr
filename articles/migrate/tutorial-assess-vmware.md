---
title: Azure geçişi sunucu değerlendirmesi ile VMware VM 'lerini değerlendirin
description: Azure geçişi sunucu değerlendirmesi kullanılarak Azure 'a geçiş için şirket içi VMware VM 'lerinin nasıl değerlendirileneceğini açıklar.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: 6c395d7e2be151e97808fa9601ff6001801d243b
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86110360"
---
# <a name="assess-vmware-vms-with-server-assessment"></a>Sunucu Değerlendirmesiyle VMware VM'lerini Değerlendirme

Bu makalede, şirket içi VMware sanal makinelerini (VM 'Ler) [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak nasıl değerlenbileceğiniz gösterilmektedir.


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

- Bu serideki [ilk öğreticiyi doldurun](tutorial-prepare-vmware.md) . Bunu yapmazsanız, bu öğreticideki yönergeler çalışmaz.
- İlk öğreticide yapmanız gerekenler şunlardır:
    - Azure 'u Azure geçişi ile çalışacak şekilde [hazırlayın](tutorial-prepare-vmware.md#prepare-azure) .
    - [VMware için değerlendirme değerlendirmesi hazırlayın](tutorial-prepare-vmware.md#prepare-for-assessment) . Bu, VMware ayarlarının denetlenmesini ve Azure geçişi 'nin vCenter Server erişmek için kullanabileceği bir hesap ayarlamayı içerir.
    - VMware değerlendirmesi için Azure geçişi gereci dağıtmak üzere gerekenleri [doğrulayın](tutorial-prepare-vmware.md#verify-appliance-settings-for-assessment) .

## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi ayarlama

Yeni bir Azure geçişi projesini aşağıdaki şekilde ayarlayın:

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış**bölümünde, **sunucuları değerlendir, değerlendir ve geçir**altında, **sunucuları değerlendir ve geçir**' i seçin.

   ![Sunucuları değerlendirmek ve geçirmek için düğme](./media/tutorial-assess-vmware/assess-migrate.png)

4. **Başlarken**' de **Araçlar Ekle**' yi seçin.
5. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.     
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-assess-vmware/migrate-project.png)

7. **İleri**’yi seçin.
8. **Değerlendirme Seç aracında** **Azure geçişi: Sunucu değerlendirmesi**  >  **İleri**' yi seçin.

   ![Sunucu değerlendirmesi aracı için seçim](./media/tutorial-assess-vmware/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden geçir + araçlar Ekle**' de ayarları gözden geçirin ve **araç ekle**' yi seçin.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="set-up-the-azure-migrate-appliance"></a>Azure geçişi gereç 'yi ayarlama

Azure geçişi: Sunucu değerlendirmesi basit bir Azure geçişi gereci kullanır. Gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi 'ne VM meta verilerini ve performans verilerini gönderir. Gereç çeşitli yollarla ayarlanabilir.

- İndirilen bir OVA şablonu kullanarak bir VMware VM 'de ayarlayın. Bu, bu öğreticide kullanılan yöntemdir.
- Bir VMware VM 'de veya bir PowerShell yükleyici betiği ile fiziksel makinede ayarlama yapın. [Bu yöntem](deploy-appliance-script.md) , BIR ova şablonu kullanarak bir VM ayarlayamıyorum veya Azure Kamu kullanıyorsanız kullanılmalıdır.

Gereci oluşturduktan sonra Azure geçişi 'ne bağlanıp bağlanamadıktan sonra sunucu değerlendirmesi yapın, ilk kez yapılandırın ve Azure geçişi projesine kaydedin.


### <a name="download-the-ova-template"></a>OVA şablonunu indirin

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **VMware vSphere Hiper Yöneticisi ile Evet '** i seçin.
3. OVA şablon dosyasını indirmek için **İndir** ' i seçin.

   ![OVA dosyasını indirme seçimleri](./media/tutorial-assess-vmware/download-ova.png)

### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtım yapmadan önce OVA dosyasının güvenli olup olmadığını denetleyin:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA dosyasının karmasını oluşturmak için aşağıdaki komutu çalıştırın:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. En son gereç sürümlerini ve karma değerlerini doğrulayın:

    - Azure genel bulutu için:
    
        **Algoritma** | **İndir** | **SHA256**
        --- | --- | ---
        VMware (10,9 GB) | [En son sürüm](https://aka.ms/migrate/appliance/vmware) | cacbdaef927fe5477fa4e1f494fcb7203cbd6b6ce7402b79f234bc0fe69663dd

    - Azure Kamu için:
    
        **Algoritma** | **İndir** | **SHA256**
        --- | --- | ---
        VMware (63,1 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2120300&clcid=0x409 ) | 3d5822038646b81f458d89d706832c0a2c0e827bfa9b0a55cc478eaf2757a4de


### <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun:

1. VSphere istemci konsolunda, **File**  >  **ovf şablonu**' nu Dağıt ' ı seçin.

   ![OVF şablonu dağıtmak için menü komutu](./media/tutorial-assess-vmware/deploy-ovf.png)

2. OVF şablonu Dağıtma Sihirbazı > **kaynak**bölümünde ova dosyasının konumunu belirtin.
3. **Ad** ve **konum**bölümünde VM için bir kolay ad belirtin. VM 'nin barındırıldığı envanter nesnesini seçin.
4. **Konakta/kümede**, sanal makinenin çalıştırılacağı konağı veya kümeyi belirtin.
5. **Depolama**alanında VM için depolama hedefini belirtin.
6. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
7. **Ağ eşlemesinde**, sanal makinenin bağlanacağı ağı belirtin. Ağ, Azure geçişi sunucu değerlendirmesi 'ne meta veri göndermek için internet bağlantısı gerektirir.
8. Ayarları gözden geçirip onaylayın ve ardından **son**' u seçin.

## <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç sanal makinesinin, [kamu](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.

### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

> [!NOTE]
> Gereci, indirilen OVA yerine bir [PowerShell betiği](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı ilgili değildir.

1. VSphere Istemci konsolunda VM 'ye sağ tıklayın ve ardından **Konsolu Aç**' ı seçin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayolunu seçerek uygulamayı gereç masaüstünden açabilirsiniz.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
   - **Lisans**: lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
   - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
     - **Proxy ayarları**' nı seçin ve proxy adresini ve dinleme bağlantı noktasını veya biçiminde belirtin http://ProxyIPAddress http://ProxyFQDN .
     - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
     - Yalnızca HTTP proxy’si desteklenir.
   - **Zaman eşitleme**: bulmanın düzgün çalışması için gereç üzerindeki zaman internet ile eşitlenmiş olmalıdır.
   - **Güncelleştirmeleri yükleme**: gereç en son güncelleştirmelerin yüklü olmasını sağlar.
   - **VDDK 'Yi yükleme**: gereç, VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olup olmadığını denetler. Yüklü değilse, VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini gereç üzerindeki belirtilen konuma ayıklayın.

     Azure geçişi sunucu geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır.       

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. **Oturum aç '** ı seçin. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
2. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün.
4. Azure geçişi projesinin oluşturulduğu aboneliği seçin ve ardından projeyi seçin.
5. Gereç için bir ad belirtin. Ad 14 karakter veya daha kısa bir harf olmalıdır.
6. **Kaydol**’u seçin.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

### <a name="specify-vcenter-server-details"></a>vCenter Server ayrıntılarını belirtin
1. **VCenter Server ayrıntılarını belirtin**bölümünde vCenter Server örneğinin adını (FQDN) veya IP adresini belirtin. Varsayılan bağlantı noktasını bırakabilir veya vCenter Server dinlediği özel bir bağlantı noktası belirtebilirsiniz.
2. **Kullanıcı adı** ve **parola**' da, gerecin vCenter Server örneğindeki VM 'leri bulması için kullanacağı vCenter Server hesabı kimlik bilgilerini belirtin. 

    - [Önceki öğreticide](tutorial-prepare-vmware.md#set-up-permissions-for-assessment)gerekli izinlere sahip bir hesap ayarlamış olmanız gerekir.
    - Bulma işlemini belirli VMware nesneleri (vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, bir konaklar klasörü veya ayrı VM 'Ler) olarak atamak istiyorsanız, Azure geçişi tarafından kullanılan hesabı kısıtlamak için [Bu makaledeki](set-discovery-scope.md) yönergeleri gözden geçirin.

3. Gerecin vCenter Server bağlanabildiğini sağlamak için **bağlantıyı doğrula** ' yı seçin.
4. **VM 'lerde uygulamaları ve bağımlılıkları bul**' da, isteğe bağlı olarak **kimlik bilgileri ekle**' ye tıklayın ve kimlik bilgilerinin ilgili olduğu işletim sistemini ve kimlik bilgilerinin kullanıcı adını ve parolasını belirtin. Daha sonra **Ekle**'ye tıklayın.

    - [Uygulama bulma özelliği](how-to-discover-applications.md)veya [aracısız bağımlılık Analizi özelliği](how-to-create-group-machine-dependencies-agentless.md)için kullanmak üzere bir hesap oluşturduysanız, isteğe bağlı olarak burada kimlik bilgileri eklersiniz.
    - Bu özellikleri kullanmıyorsanız, bu ayarı atlayabilirsiniz.
    - [Uygulama bulma](migrate-support-matrix-vmware.md#application-discovery-requirements)veya [aracısız analiz](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)için gereken kimlik bilgilerini gözden geçirin.

5. VM bulmayı **başlatmak için bulmayı kaydedin ve başlatın**.

Bulma işlemi aşağıdaki gibi çalışmaktadır:
- Keşfedilen VM meta verilerinin portalda görünmesi 15 dakika içinde sürer.
- Yüklenen uygulamaların, rollerin ve özelliklerin bulunması biraz zaman alır. Süre, bulunan VM sayısına bağlıdır. 500 VM 'Ler için, uygulama envanterinin Azure geçişi portalında görünmesi yaklaşık bir saat sürer.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulmadan sonra, VM 'Lerin Azure portal göründüğünü doğrulayabilirsiniz:

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucuların**sayısını gösteren simgeyi seçin.

## <a name="set-up-an-assessment"></a>Değerlendirme ayarlama

Azure geçişi sunucu değerlendirmesini kullanarak iki tür değerlendirme oluşturabilirsiniz:

**Değerlendirme türü** | **Ayrıntılar**
--- | --- 
**Azure VM** | Şirket içi sunucularınızı Azure sanal makinelerine geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure 'a geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md)'lerinizi, [Hyper-V sanal](how-to-set-up-appliance-hyper-v.md)makinelerinizi ve [fiziksel sunucuları](how-to-set-up-appliance-physical.md) değerlendirebilirsiniz. [Daha fazla bilgi edinin](concepts-assessment-calculation.md)
**Azure VMware Çözümü (AVS)** | Şirket içi sunucularınızı [Azure VMware çözümüne (AVS)](../azure-vmware/introduction.md)geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure VMware çözümüne (AVS) geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md) 'lerinizi değerlendirebilirsiniz. [Daha fazla bilgi edinin](concepts-azure-vmware-solution-assessment-calculation.md)

Sunucu değerlendirmesi iki boyutlandırma ölçütü seçeneği sağlar:

**Boyutlandırma ölçütü** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı öneriler oluşturan değerlendirmeler | **Azure VM değerlendirmesi**: VM boyutu ÖNERISI, CPU ve bellek kullanımı verilerini temel alır.<br/><br/> Disk türü önerisi (Standart HDD/SSD veya Premium yönetilen diskler), şirket içi disklerin ıOPS ve aktarım hızını temel alır.<br/><br/> **Azure VMware çözümü (AVS) değerlendirmesi**: AVS düğümleri ÖNERISI, CPU ve bellek kullanımı verilerini temel alır.
**Şirket içi olarak** | Öneriler oluşturmak için performans verilerini kullanmayan değerlendirmeler. | **Azure VM değerlendirmesi**: VM boyutu önerisi, ŞIRKET içi VM boyutunu temel alır<br/><br> Önerilen disk türü, değerlendirme için depolama türü ayarında neleri seçdiklerinize bağlıdır.<br/><br/> **Azure VMware çözümü (AVS) değerlendirmesi**: AVS düğümleri önerisi, ŞIRKET içi VM boyutunu temel alır.

## <a name="run-an-assessment"></a>Değerlendirme çalıştırma

*Azure VM değerlendirmesi* 'ni aşağıdaki şekilde çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğunda **değerlendir**' i seçin.

   ![Değerlendirme düğmesinin konumu](./media/tutorial-assess-vmware/assess.png)

3. **Sunucuları değerlendir**bölümünde, değerlendirme türünü "Azure VM" olarak seçin, bulma kaynağını seçin ve değerlendirme adını belirtin.

    ![Değerlendirme temelleri](./media/tutorial-assess-vmware/assess-servers-azurevm.png)
 
4. **Tümünü görüntüle**' yi seçin ve ardından değerlendirme özelliklerini gözden geçirin.

   ![Değerlendirme özellikleri](./media/tutorial-assess-vmware/view-all.png)

5. **Değerlendirmek için makineleri seçmek**üzere **İleri** ' ye tıklayın. **Grup Seç veya oluştur**' da, **Yeni oluştur**' u seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla VM 'yi toplar.
6. **Gruba makine ekleme**' de gruba eklenecek VM 'ler ' i seçin.
7. Değerlendirme ayrıntılarını gözden geçirmek için **İleri** ' ye ve **değerlendirme oluştur** ' a tıklayın.
8. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **değerlendirme oluştur** ' u seçin.

   ![Sunucuları değerlendir](./media/tutorial-assess-vmware/assessment-create.png)

8. Değerlendirme oluşturulduktan sonra **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**  >  **değerlendirmeleri**' nde görüntüleyin.
9. Bir Excel dosyası olarak indirmek için **dışarı aktarma değerlendirmesi** ' ni seçin.

Bir **Azure VMware çözümü (AVS) değerlendirmesi**çalıştırmak istiyorsanız, [burada](how-to-create-azure-vmware-solution-assessment.md)bahsedilen adımları izleyin.


## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme şunları açıklar:

- **Azure hazırlığı**: VM 'lerin Azure 'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: VM 'leri Azure 'da çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: geçişten sonra disk depolaması için tahmini maliyetler.

Bir değerlendirmeyi görüntülemek için:

1. **Geçiş hedefleri**  >  **sunucularında** **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendirmeler** ' ı seçin.
2. **Değerlendirmede**, açmak için bir değerlendirme seçin.

   ![Değerlendirme özeti](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlığını gözden geçirme

1. **Azure 'a hazırlık**bölümünde, VM 'lerin Azure 'a geçiş için hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure Için hazırlanma**: Azure geçişi, değerlendirmede VM 'ler IÇIN bir VM boyutu ve maliyet tahminleri önermesinde kullanılır.
    - **Koşullara hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazır olma durumu bilinmiyor**: Azure geçişi, veri kullanılabilirliği sorunları nedeniyle hazırlığı değerlendiremediği zaman kullanılır.

3. Bir **Azure hazırlık** durumu seçin. VM hazırlığı ayrıntıları ' nı görüntüleyebilirsiniz. Ayrıca, işlem, depolama ve ağ ayarları dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya gidebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Değerlendirme özeti, Azure 'da çalışan VM 'lerin tahmini işlem ve depolama maliyetini gösterir. Ücretler, değerlendirilen gruptaki tüm VM 'Ler için toplanır. Belirli VM 'Ler için maliyet ayrıntılarını görmek için ayrıntıya gidebilirsiniz.

> [!NOTE]
> Maliyet tahminleri, bir makine, diskleri ve özellikleri için boyut önerilerini temel alır. Tahminler, şirket içi VM 'Leri IaaS VM 'Leri olarak çalıştırmaya yöneliktir. Azure geçişi sunucu değerlendirmesi PaaS veya SaaS maliyetlerini göz önünde bulundurmaz.

Değerlendirilen grup için toplanan depolama maliyetleri, farklı türlerde depolama disklerinin üzerine bölünür. 

### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Azure geçişi sunucu değerlendirmesi, bir yıldız (en düşük) ile beş yıldız (en yüksek) arasında performans tabanlı bir değerlendirmeye bir güvenilirlik derecelendirmesi atar.

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
> [VMware VM’leri geçirme](./tutorial-migrate-vmware.md)
