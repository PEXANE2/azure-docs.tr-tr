---
title: Azure geçişi ile Azure 'a geçiş için Hyper-V VM 'lerini değerlendirin | Microsoft Docs
description: Azure geçişi sunucu değerlendirmesi kullanılarak Azure 'a geçiş için şirket içi Hyper-V VM 'lerinin nasıl değerlendirileneceğini açıklar.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: eae7e2d371ed8156debe9ae24cf0744bd6273943
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88950281"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Azure geçişi sunucu değerlendirmesi ile Hyper-V VM 'lerini değerlendirin

Bu makalede, şirket içi Hyper-V VM 'lerini [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak nasıl değerlenbileceğiniz gösterilmektedir.


Bu öğretici, Hyper-V VM 'lerinin Azure 'a nasıl değerlendirileceğini ve geçirileceğini gösteren bir serinin ikinci saniyedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure geçişi projesi ayarlayın.
> * Bir Azure geçiş gereci ayarlayın ve kaydedin.
> * Şirket içi VM 'lerin sürekli olarak bulunmasını başlatın.
> * Bulunan VM 'Leri gruplandırın ve grubu değerlendirin.
> * Değerlendirmeyi gözden geçirin.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için, nasıl yapılır makalelerini gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

- Bu serideki ilk öğreticiyi [doldurun](tutorial-prepare-hyper-v.md) . Bunu yapmazsanız, bu öğreticideki yönergeler çalışmaz.
- İlk öğreticide yapmanız gerekenler şunlardır:
    - Azure 'u Azure geçişi ile çalışacak şekilde [hazırlayın](tutorial-prepare-hyper-v.md#prepare-azure) .
    - [Hyper-V](tutorial-prepare-hyper-v.md#prepare-for-assessment) konakları ve VM değerlendirmesi hazırlayın.
    - Hyper-V değerlendirmesi için Azure geçişi gereci dağıtmak üzere gerekenleri [doğrulayın](tutorial-prepare-hyper-v.md#prepare-for-appliance-deployment) .

## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi ayarlama

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. Arama sonuçlarında **Azure geçişi**' ni seçin.
3. **Genel Bakış** bölümünde **Sunucuları bul, değerlendir ve geçiş** altında **Sunucuları değerlendir ve geçir** üzerine tıklayın.

    ![Sunucuları bulma ve değerlendirme](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. **Başlarken** bölümünde **Araç ekle**’ye tıklayın.
5. **Projeyi geçir** sekmesinde Azure aboneliğinizi seçin ve yoksa bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda proje adını ve projeyi oluşturmak istediğiniz bölgeyi belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

    - Proje bölgesi yalnızca şirket içi VM 'lerden toplanan meta verileri depolamak için kullanılır.
    - VM 'Leri geçirirken farklı bir Azure hedef bölgesi seçebilirsiniz. Tüm Azure bölgeleri geçiş hedefi için desteklenir.

    ![Azure geçişi projesi oluşturma](./media/tutorial-assess-hyper-v/migrate-project.png)

7. **İleri**’ye tıklayın.
8. **Değerlendirme Seç aracında** **Azure geçişi: Sunucu değerlendirmesi**  >  **İleri**' yi seçin.

    ![Azure geçişi projesi oluşturma](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden geçir + araçlar Ekle**' de ayarları gözden geçirin ve **araç ekle**' ye tıklayın.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="set-up-the-azure-migrate-appliance"></a>Azure geçişi gereç 'yi ayarlama


Azure geçişi: Sunucu değerlendirmesi basit bir Azure geçişi gereci kullanır. Gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi 'ne VM meta verilerini ve performans verilerini gönderir. Gereç çeşitli yollarla ayarlanabilir.

- İndirilen bir Hyper-V VHD 'yi kullanarak bir Hyper-V sanal makinesine ayarlayın. Bu, bu öğreticide kullanılan yöntemdir.
- Bir PowerShell yükleyici betiği ile bir Hyper-V VM 'sinde veya fiziksel makinede ayarlama yapın. [Bu yöntem](deploy-appliance-script.md) , VHD 'yi kullanarak bir VM ayarlayamıyorum veya Azure Kamu kullanıyorsanız kullanılmalıdır.

Gereci oluşturduktan sonra Azure geçişi 'ne bağlanıp bağlanamadıktan sonra sunucu değerlendirmesi yapın, ilk kez yapılandırın ve Azure geçişi projesine kaydedin.

### <a name="generate-the-azure-migrate-project-key"></a>Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **Hyper-V ile Evet '** i seçin.
3. **1: Azure geçişi proje anahtarı oluşturma**' da, Hyper-V VM 'lerini bulmak için ayarlayabileceğiniz Azure geçiş gereci için bir ad sağlayın. ad 14 karakter veya daha kısa olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="download-the-vhd"></a>VHD 'YI indirin

**2: Azure geçişi yükleme gereci indirin**, öğesini seçin. VHD dosyası ve **İndir**' e tıklayın. 

   ![Bulma makineleri için seçimler](./media/tutorial-assess-hyper-v/servers-discover.png)


   ![Anahtar oluştur seçimleri](./media/tutorial-assess-hyper-v/generate-key-hyperv.png)


### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtmadan önce daraltılmış dosyanın güvenli olduğunu denetleyin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.

2. ZIP dosyasının karmasını oluşturmak için aşağıdaki PowerShell komutunu çalıştırın
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v1.19.06.27.zip -Algorithm SHA256```

3.  En son gereç sürümlerini ve karma değerlerini doğrulayın:

    - Azure genel bulutu için:

        **Senaryo** | **İndir** | **SHA256**
        --- | --- | ---
        Hyper-V (10,4 GB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140422) |  79c151588de049cc102f61b910d61360402324dc8d8a14f47772da351b46d9127

    - Azure Kamu için:

        **Senaryon*** | **İndir** | **SHA256**
        --- | --- | ---
        Hyper-V (85 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140424) |  8025f315e41c01ebdb4ffb1de87982ae6cc4ea7c4cce612612c7e90a44e79b44


### <a name="create-the-appliance-vm"></a>Gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve VM 'yi oluşturun.

1. Sıkıştırılmış VHD dosyasını gereç VM 'sinin yerleştirileceği Hyper-V konağına indirdikten sonra, daraltılmış dosyayı ayıklayın.
    - Ayıklanan konumda, dosya **AzureMigrateAppliance_VersionNumber**adlı bir klasöre UNIP 'leri yok.
    - Bu klasör **AzureMigrateAppliance_VersionNumber**olarak da adlandırılan bir alt klasör içerir.
    - Bu alt klasör, daha fazla üç alt **görüntü**, **sanal sabit disk**ve **sanal makine**içerir.

2. Hyper-V Yöneticisi'ni açın. **Eylemler**' de **sanal makineyi içeri aktar**' a tıklayın.

    ![VHD dağıtma](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. Sanal makineyi Içeri aktarma sihirbazında > **başlamadan önce** **İleri**' ye tıklayın.
3. **Klasörü bul**' da, **sanal makineler** klasörünü seçin. Ardından **İleri**'ye tıklayın.
1. **Sanal makine seç**bölümünde **İleri**' ye tıklayın.
2. **Içeri aktarma türünü seçin**bölümünde **sanal makineyi Kopyala (yenı bir benzersiz kimlik oluştur)** seçeneğine tıklayın. Ardından **İleri**'ye tıklayın.
3. **Hedef Seç**bölümünde varsayılan ayarı bırakın. **İleri**’ye tıklayın.
4. **Depolama klasörlerinde**varsayılan ayarı bırakın. **İleri**’ye tıklayın.
5. **Ağ seçin**bölümünde, VM 'nin kullanacağı sanal anahtarı belirtin. Anahtar, verileri Azure 'a göndermek için internet bağlantısı gerektirir. Sanal anahtar oluşturma hakkında [bilgi edinin](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) .
6. **Özet**bölümünde ayarları gözden geçirin. Ardından, **Son**'a tıklayın.
7. Hyper-V Yöneticisi 'nde **sanal makineler**>, VM 'yi başlatın.


## <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç sanal makinesinin, [kamu](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.

### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

> [!NOTE]
> Gereci indirilen VHD yerine bir [PowerShell betiği](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı ilgili değildir.

1. Hyper-V Yöneticisi > **sanal makineler**' de, **bağlan**' a > VM ' ye sağ tıklayın.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayoluna tıklayarak uygulamayı gereç masaüstünden açabilirsiniz.
1. **Lisans koşullarını**kabul edin ve üçüncü taraf bilgilerini okuyun.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
    - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
      - Proxy 'yi **Ayarla** ' ya tıklayın ve proxy adresini (form http://ProxyIPAddress veya http://ProxyFQDN) dinleme bağlantı noktasında) belirtin.
      - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
      - Yalnızca HTTP proxy’si desteklenir.
      - Proxy ayrıntıları eklediyseniz veya proxy ve/veya kimlik doğrulamasını devre dışı bırakırsanız, bağlantıyı tetiklemek için **Kaydet** 'e tıklayarak bağlantı denetimini yeniden başlatın.
    - **Zaman eşitleme**: Saat doğrulandı. VM bulmanın düzgün çalışması için gereç süresi internet saatine eşit olmalıdır.
    - **Güncelleştirmeleri yükleme**: Azure geçişi sunucu değerlendirmesi, gerecin en son güncelleştirmelerin yüklü olduğunu denetler. Denetim tamamlandıktan sonra gereç **hizmetlerini görüntüle** ' ye tıklayarak gereç üzerinde çalışan bileşenlerin durumunu ve sürümlerini görebilirsiniz.

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. Portaldan kopyalanmış **Azure geçişi proje anahtarını** yapıştırın. Anahtarınız yoksa, sunucu değerlendirmesi ' ne gidin **> var olan gereçlerini keşfet> yönetin**, anahtar oluşturma sırasında verdiğiniz gereç adını seçin ve ilgili anahtarı kopyalayın.
1. **Oturum**aç ' a tıklayın. Yeni bir tarayıcı sekmesinde bir Azure oturum açma istemi açar. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmesinde, Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra Web uygulamasına geri dönün. 
4. Günlüğe kaydetme için kullanılan Azure Kullanıcı hesabının, anahtar üretimi sırasında oluşturulan Azure kaynakları üzerinde doğru [izinleri](tutorial-prepare-hyper-v.md#prepare-azure) varsa, Gereç kaydı başlatılır.
1. Gereç başarıyla kaydedildikten sonra, **Ayrıntıları görüntüle**' ye tıklayarak kayıt ayrıntılarına bakabilirsiniz.



### <a name="delegate-credentials-for-smb-vhds"></a>SMB VHD 'leri için temsilci kimlik bilgileri

SMB 'lerde VHD 'ler çalıştırıyorsanız, Gereç üzerinden Hyper-V konaklarına kimlik bilgileri temsilcisini etkinleştirmeniz gerekir. Bunu yapmak için, her bir konağın gereç için bir temsilci görevi görmesini sağlayabilirsiniz. Öğreticileri sırayla izlediyseniz, değerlendirme ve geçiş için Hyper-V ' yi hazırladığınız zaman önceki öğreticide bunu yaptınız. Konaklar için CredSSP 'yi [el ile](tutorial-prepare-hyper-v.md#enable-credssp-to-delegate-credentials)veya bunu yapan [bir komut dosyası çalıştırarak](tutorial-prepare-hyper-v.md#run-the-script) ayarlamanız gerekir.

Gereçte aşağıdaki şekilde etkinleştirin:

#### <a name="option-1"></a>1\. Seçenek

Gereç sanal makinesinde bu komutu çalıştırın. HyperVHost1/HyperVHost2, örnek ana bilgisayar adlarıdır.

```
Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
```

Örnek: ` Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force `

#### <a name="option-2"></a>2\. Seçenek

Alternatif olarak, bunu gereç üzerindeki Yerel Grup İlkesi Düzenleyicisi yapın:

1. **Yerel bilgisayar ilkesi**  >  **bilgisayar yapılandırması**' nda, **Yönetim Şablonları**  >  **sistem**  >  **kimlik bilgileri temsili**' ne tıklayın.
2. **Yeni kimlik bilgileri aktarımına Izin ver**' e çift tıklayın ve **etkin**' i seçin.
3. **Seçenekler**' de **göster**' e tıklayın ve bulmayı Istediğiniz her Hyper-V konağını, **WSMan/** as öneki ile listeye ekleyin.
4. Ardından, **kimlik bilgileri temsilcisi**' nde, **yalnızca NTLM sunucu kimlik doğrulamasıyla yeni kimlik bilgileri aktarmaya izin ver**' e çift tıklayın. Daha sonra, bulmayı istediğiniz her Hyper-V konağını, **WSMan/** as öneki ile listeye ekleyin.

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecden Hyper-V konaklarına veya kümelerine bağlanın ve VM bulmayı başlatın.

1. **1. Adım: Hyper-V ana bilgisayar kimlik bilgilerini sağlayın**bölümünde kimlik bilgileri için kolay bir ad belirtmek üzere **kimlik bilgileri ekle** ' ye tıklayın, gerecin VM 'leri bulması için kullanacağı bir Hyper-v Konağı/kümesi Için **Kullanıcı adı** ve **parola** ekleyin. **Kaydet**'e tıklayın.
1. Aynı anda birden çok kimlik bilgisi eklemek istiyorsanız, kaydetmek için **daha fazla Ekle** ' ye tıklayın ve daha fazla kimlik bilgisi ekleyin. Hyper-V VM 'Leri bulmak için birden çok kimlik bilgisi desteklenir.
1. **2. Adım: Hyper-v Konağı/kümesi ayrıntılarını sağlama**bölümünde, Hyper-v Konağı/kümesi **IP adresini/FQDN** 'yi ve konağa/kümeye bağlanacak kimlik bilgileri için kolay adı belirtmek üzere **bulma kaynağı Ekle** ' ye tıklayın.
1. **Tek seferde tek bir öğe ekleyebilir** veya tek bir go içinde **birden fazla öğe ekleyebilirsiniz** . Ayrıca Hyper-V konağı/kümesi ayrıntılarını **Içeri aktarma CSV**aracılığıyla sağlama seçeneği de vardır.

    ![Keşif kaynağı ekleme seçimleri](./media/tutorial-assess-hyper-v/add-discovery-source-hyperv.png)

    - **Tek öğe Ekle**' yi seçerseniz, kimlik bilgileri ve Hyper-V konağı/kümesi **IP adresi/FQDN** için kolay ad belirtmeniz ve **Kaydet**' e tıklamanız gerekir.
    - **Birden çok öğe Ekle** _(varsayılan olarak seçilidir)_ seçeneğini belirlerseniz, metin kutusunda kimlik bilgileri Için kolay ad ile Hyper-V konağı/KÜMESI **IP adresi/FQDN** belirterek birden çok kayıt ekleyebilirsiniz. Eklenen kayıtları **doğrulayın** ve **Kaydet**' e tıklayın.
    - **CSV 'Yi Içeri aktar**' ı SEÇERSENIZ bir CSV şablon dosyası indirebilir, dosyayı Hyper-V konağı/kümesi **IP adresi/FQDN** ve kimlik bilgileri için kolay ad ile doldurabilirsiniz. Sonra dosyayı gereç içine aktarır, dosyadaki kayıtları **doğrulayın** ve **Kaydet**' e tıklayın.

1. Kaydet 'e tıkladığınızda gereç, eklenen Hyper-V konaklarına/kümelerine bağlantıyı doğrulamaya çalışır ve her konak/küme için tablodaki **doğrulama durumunu** gösterir.
    - Başarıyla doğrulanan konaklar/kümeler için IP adreslerine/FQDN 'ye tıklayarak daha fazla ayrıntı görüntüleyebilirsiniz.
    - Bir konakta doğrulama başarısız olursa, tablonun durum sütununda **doğrulama** ' ya tıklayarak hatayı gözden geçirin. Sorunu düzeltin ve tekrar doğrulayın.
    - Konakları veya kümeleri kaldırmak için **Sil**'e tıklayın.
    - Belirli bir konağı kümeden kaldıramazsınız. Tüm kümeyi yalnızca çıkarabilirsiniz.
    - Kümedeki belirli konaklarla ilgili sorunlar olsa bile bir küme ekleyebilirsiniz.
1. Keşfi başlatmadan önce herhangi bir zamanda konaklara/kümelere bağlantıyı **yeniden doğrulayabilirsiniz** .
1. Başarıyla doğrulanan konaklardan/kümelerdeki VM bulmayı **başlatmak için bulmayı Başlat**' a tıklayın. Bulma işlemi başarılı bir şekilde başlatıldıktan sonra, tablodaki her bir konağa/kümeye yönelik bulma durumunu denetleyebilirsiniz.

Bu, bulmayı başlatır. Bulunan sunucuların meta verilerinin Azure portal görünmesi için ana bilgisayar başına yaklaşık 2 dakika sürer.

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma işlemi tamamlandıktan sonra, VM 'Lerin portalda göründüğünü doğrulayabilirsiniz.

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi** sayfasında, **bulunan sunucuların**sayısını görüntüleyen simgeye tıklayın.

## <a name="set-up-an-assessment"></a>Değerlendirme ayarlama

Azure geçişi sunucu değerlendirmesi kullanarak çalıştırabileceğiniz iki tür değerlendirme vardır.

**Değerlendirme** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı değerlendirmeler | **ÖNERILEN VM boyutu**: CPU ve bellek kullanım verilerine göre.<br/><br/> **Önerilen disk türü (Standart veya Premium yönetilen disk)**: Şirket ıçı disklerin IOPS ve aktarım hızına göre.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler. | **ÖNERILEN VM boyutu**: ŞIRKET içi VM boyutuna göre<br/><br> **Önerilen disk türü**: değerlendirme için seçtiğiniz depolama türü ayarına göre.



### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendir**' e tıklayın.

    ![Değerlendirme](./media/tutorial-assess-hyper-v/assess.png)

3. **Sunucuları değerlendir**bölümünde, değerlendirme için bir ad belirtin.
4. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. **Grup Seç veya oluştur**' da, **Yeni oluştur**' u seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla VM 'yi toplar.
4. **Gruba makine ekleme**' de gruba eklenecek VM 'ler ' i seçin.
5. Grubu oluşturmak için **değerlendirme oluştur** ' a tıklayın ve değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Değerlendirme oluşturulduktan sonra **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde görüntüleyin.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.


## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme şunları açıklar:

- **Azure hazırlığı**: VM 'lerin Azure 'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: VM 'leri Azure 'da çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: geçişten sonra disk depolaması için tahmini maliyetler.


### <a name="view-an-assessment"></a>Değerlendirme görüntüleme

1. **Geçiş hedefleri**  >   **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **değerlendirmeler**' ı tıklatın.
2. **Değerlendirmede**, bir değerlendirmeye tıklayarak açın.

    ![Değerlendirme özeti](./media/tutorial-assess-hyper-v/assessment-summary.png)


### <a name="review-azure-readiness"></a>Azure hazırlığını gözden geçirme

1. **Azure 'a hazırlık**bölümünde, VM 'lerin Azure 'a geçiş için hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure Için hazırlanma**: Azure geçişi, değerlendirmede VM 'ler IÇIN bir VM boyutu ve maliyet tahminleri önerir.
    - **Koşullara hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazır olma durumu bilinmiyor**: veri kullanılabilirliği sorunları nedeniyle Azure geçişi hazırlığı değerlendiremez kullanılır.

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

![Güvenilirlik derecelendirmesi](./media/tutorial-assess-hyper-v/confidence-rating.png)

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

Hyper-V VM 'lerini Azure geçişi sunucu geçişi ile Azure 'a geçirmeyi öğrenmek için serideki üçüncü öğreticiye geçin.

> [!div class="nextstepaction"]
> [Hyper-V VM’leri Azure’a Geçirme](./tutorial-migrate-hyper-v.md)
