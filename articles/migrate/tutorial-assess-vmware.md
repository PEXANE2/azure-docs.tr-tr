---
title: Azure geçişi sunucu değerlendirmesi ile VMware VM 'lerini değerlendirin
description: Azure geçişi sunucu değerlendirmesi kullanılarak Azure 'a geçiş için şirket içi VMware VM 'lerinin nasıl değerlendirileneceğini açıklar.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: 7616ff48c03c0de61d9179724fd8e351c6440319
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936257"
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

- İndirilen bir OVA şablonu kullanarak bir VMware VM 'de ayarlayın. **Bu, bu öğreticide kullanılan yöntemdir.**
- Bir VMware VM 'de veya bir PowerShell yükleyici betiği ile fiziksel makinede ayarlama yapın. [Bu yöntem](deploy-appliance-script.md) , BIR ova şablonu kullanarak bir VM ayarlayamıyorum veya Azure Kamu kullanıyorsanız kullanılmalıdır.

Gereci oluşturduktan sonra Azure geçişi 'ne bağlanıp bağlanamadıktan sonra sunucu değerlendirmesi yapın, ilk kez yapılandırın ve Azure geçişi projesine kaydedin.


### <a name="generate-the-azure-migrate-project-key"></a>Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makinelerde**  >  **makineler sanallaştırılmış mı?**, **VMware vSphere hiper yöneticiyle Evet '** i seçin.
3. **1: Azure geçişi proje anahtarı oluştur**' da, Azure geçiş gereci VMware VM 'leri bulmak için ayarlayacaksınız. ad, 14 karakter veya daha kısa olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="download-the-ova-template"></a>OVA şablonunu indirin
**2: Azure geçişi yükleme gereci indirin**, öğesini seçin. OVA dosyası ve **İndir**' e tıklayın. 


   ![Bulma makineleri için seçimler](./media/tutorial-assess-vmware/servers-discover.png)


   ![Anahtar oluştur seçimleri](./media/tutorial-assess-vmware/generate-key-vmware.png)


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
        VMware (11,6 GB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140333) | 9a06a316199330481f95b381a4d1d558f3869614a0ded68f1cc4f2584aa4f353

    - Azure Kamu için:
    
        **Algoritma** | **İndir** | **SHA256**
        --- | --- | ---
        VMware (85 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140337) | 31b1bfdd4fc29b3eb923c7c6e7a898af79b7cac0404426,18809def2284188


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
1. **Lisans koşullarını**kabul edin ve üçüncü taraf bilgilerini okuyun.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
   - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
     - Proxy adresini belirtmek için **proxy ayarla** ' ya tıklayın (formdaki http://ProxyIPAddress veya http://ProxyFQDN) dinleme bağlantı noktasında).
     - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
     - Yalnızca HTTP proxy’si desteklenir.
     - Proxy ayrıntıları eklediyseniz veya proxy ve/veya kimlik doğrulamasını devre dışı bırakırsanız, bağlantıyı tetiklemek için **Kaydet** 'e tıklayarak bağlantı denetimini yeniden başlatın.
   - **Zaman eşitleme**: bulmanın düzgün çalışması için gereç üzerindeki zaman internet ile eşitlenmiş olmalıdır.
   - **Güncelleştirmeleri yükleme**: gereç en son güncelleştirmelerin yüklü olmasını sağlar. Denetim tamamlandıktan sonra gereç **hizmetlerini görüntüle** ' ye tıklayarak gereç üzerinde çalışan bileşenlerin durumunu ve sürümlerini görebilirsiniz.
   - **VDDK 'Yi yükleme**: gereç, VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olduğunu denetler. Yüklü değilse, VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini **yükleme yönergelerindeki**belirtilen konuma ayıklayın.

     Azure geçişi sunucu geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır. 
1. İsterseniz, gerecin tüm önkoşulları karşılayıp karşılamadığını denetlemek için gereç yapılandırması sırasında dilediğiniz zaman **Önkoşulları yeniden çalıştırabilirsiniz** .

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. Portaldan kopyalanmış **Azure geçişi proje anahtarını** yapıştırın. Anahtarınız yoksa, sunucu değerlendirmesi ' ne gidin **> var olan gereçlerini keşfet> yönetin**, anahtar oluşturma sırasında verdiğiniz gereç adını seçin ve ilgili anahtarı kopyalayın.
1. **Oturum**aç ' a tıklayın. Yeni bir tarayıcı sekmesinde bir Azure oturum açma istemi açar. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün. 
4. Günlüğe kaydetme için kullanılan Azure Kullanıcı hesabının, anahtar üretimi sırasında oluşturulan Azure kaynakları üzerinde doğru [izinleri](tutorial-prepare-vmware.md#prepare-azure) varsa, Gereç kaydı başlatılır.
1. Gereç başarıyla kaydedildikten sonra, **Ayrıntıları görüntüle**' ye tıklayarak kayıt ayrıntılarına bakabilirsiniz.


## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

1. **1. Adım: vCenter Server kimlik**bilgilerini belirtin bölümünde kimlik bilgileri için kolay bir ad belirtmek üzere **kimlik bilgileri ekle** ' ye tıklayın, gerecin vCenter Server örneğindeki VM 'leri bulması için kullanacağı vCenter Server hesabı Için **Kullanıcı adı** ve **parola** ekleyin.
    - [Önceki öğreticide](tutorial-prepare-vmware.md#set-up-permissions-for-assessment)gerekli izinlere sahip bir hesap ayarlamış olmanız gerekir.
    - Bulma işlemini belirli VMware nesneleri (vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, bir konaklar klasörü veya ayrı VM 'Ler) olarak atamak istiyorsanız, Azure geçişi tarafından kullanılan hesabı kısıtlamak için [Bu makaledeki](set-discovery-scope.md) yönergeleri gözden geçirin.
1. **2. Adım: vCenter Server ayrıntıları belirtin**bölümünde, açılan listeden kimlik bilgileri için kolay ad seçmek üzere **bulma kaynağı Ekle** ' ye tıklayın, vCenter Server ÖRNEĞININ **IP adresini/FQDN** 'sini belirtin. **Bağlantı noktasını** varsayılan (443) olarak bırakabilir veya vCenter Server dinlediği ve **Kaydet**' e tıklayarak özel bir bağlantı noktası belirtebilirsiniz.
1. Kaydet 'e tıklanınca, Gereç vCenter Server bağlantısını doğrulamayı dener ve tablodaki **doğrulama durumunu** vCenter Server IP ADRESINE/FQDN 'ye göre gösterir.
1. Bulmayı başlatmadan önce vCenter Server bağlantıyı **yeniden doğrulayabilirsiniz** .
1. **3. Adım: yüklü uygulamaları bulmaya ve aracısız bağımlılık eşlemesi gerçekleştirmeye YÖNELIK VM kimlik bilgilerini sağlayın**, kimlik bilgileri **Ekle**' ye tıklayın ve kimlik bilgilerinin sağlandığı işletim sistemini, kimlik bilgileri Için kolay adı ve **Kullanıcı** adını ve **parolayı**belirtin. Ardından **Kaydet**' e tıklayın.

    - [Uygulama bulma özelliği](how-to-discover-applications.md)veya [aracısız bağımlılık Analizi özelliği](how-to-create-group-machine-dependencies-agentless.md)için kullanmak üzere bir hesap oluşturduysanız, isteğe bağlı olarak burada kimlik bilgileri eklersiniz.
    - Bu özellikleri kullanmak istemiyorsanız, adımı atlamak için kaydırıcıya tıklayabilirsiniz. Amacınızı daha sonra dilediğiniz zaman tersine çevirebilirsiniz.
    - [Uygulama bulma](migrate-support-matrix-vmware.md#application-discovery-requirements)veya [aracısız bağımlılık Analizi](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)için gereken kimlik bilgilerini gözden geçirin.

5. VM bulmayı **başlatmak için bulmayı Başlat**' a tıklayın. Bulma işlemi başarılı bir şekilde başlatıldıktan sonra, tablodaki vCenter Server IP adresine/FQDN 'ye karşı bulma durumunu kontrol edebilirsiniz.

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
