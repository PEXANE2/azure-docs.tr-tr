---
title: Azure geçişi sunucu değerlendirmesi ile içeri aktarılan sunucu verilerini kullanarak sunucuları değerlendirme
description: İçeri aktarılan verileri kullanarak Azure geçişi sunucu değerlendirmesi ile Azure 'a geçiş için şirket içi sunucuların nasıl değerlendirileneceğini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 519520538c16b1bde18f0810344864d37090accf
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84342655"
---
# <a name="assess-servers-by-using-imported-data"></a>İçeri aktarılan verileri kullanarak sunucuları değerlendirme

Bu makalede, sunucu meta verilerini virgülle ayrılmış değerler (CSV) biçiminde içe aktararak [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracı ile şirket içi sunucuların nasıl değerlendirileneceği açıklanmaktadır. Bu değerlendirme yöntemi, değerlendirme oluşturmak için Azure geçişi gerecini ayarlamanıza gerek yoktur. Şu durumlarda yararlı olur:

- Gereci dağıtmadan önce hızlı, ilk değerlendirme oluşturmak istersiniz.
- Azure geçişi gerecini kuruluşunuza dağıtamazsınız.
- Şirket içi sunuculara erişime izin veren kimlik bilgilerini paylaşamazsınız.
- Güvenlik kısıtlamaları, Gereç tarafından toplanan verileri Azure 'a toplamayı ve göndermeyi önler. İçeri aktarılan bir dosyada paylaştığınız verileri kontrol edebilirsiniz. Ayrıca, verilerin büyük bölümü (örneğin, IP adresleri sağlama) isteğe bağlıdır.

## <a name="before-you-start"></a>Başlamadan önce

Şu noktalara dikkat edin:

- Tek bir CSV dosyasında en fazla 20.000 sunucu ekleyebilirsiniz.
- CSV kullanarak bir Azure geçişi projesine en fazla 20.000 sunucusu ekleyebilirsiniz.
- CSV kullanarak sunucu bilgilerini sunucu değerlendirmesine birden çok kez yükleyebilirsiniz.
- Uygulama bilgilerinin toplanması, şirket içi ortamınızı geçiş için değerlendirmek için yararlıdır. Ancak, sunucu değerlendirmesi Şu anda uygulama düzeyinde değerlendirme gerçekleştirmez veya bir değerlendirme oluştururken uygulamaları hesaba alabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir Azure geçişi projesi ayarlayın.
> * Sunucu bilgileriyle bir CSV dosyası girin.
> * Sunucu değerlendirmesi ' ne sunucu bilgilerini eklemek için dosyayı içeri aktarın.
> * Bir değerlendirme oluşturun ve gözden geçirin.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde oluşturabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için nasıl yapılır kılavuzlarını gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="set-azure-permissions-for-azure-migrate"></a>Azure geçişi için Azure izinleri ayarlama

Azure hesabınız, Azure geçişi projesi oluşturmak için izinlere ihtiyaç duyuyor.

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve ardından izinleri görüntülemek için seçin.
3. **Katkıda bulunan** veya **sahip** izninizin olduğundan emin olun.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

## <a name="set-up-an-azure-migrate-project"></a>Azure geçişi projesi ayarlama

Yeni bir Azure geçişi projesi ayarlamak için:

1. Azure portal, **tüm hizmetler**' de **Azure geçişi**' ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış**bölümünde, **sunucuları değerlendir, değerlendir ve geçir**altında, **sunucuları değerlendir ve geçir**' i seçin.

    ![Sunucuları bulma ve değerlendirme](./media/tutorial-assess-import/assess-migrate.png)

4. **Başlarken**' de **araç ekle**' yi seçin.
5. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. Daha fazla bilgi için:

    - [Genel](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government) için desteklenen coğrafyaları inceleyin.
    - Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

    ![Azure geçişi projesi oluşturma](./media/tutorial-assess-import/migrate-project.png)

7. **İleri**’yi seçin.
8. **Değerlendirme Seç aracında** **Azure geçişi: Sunucu değerlendirmesi**  >  **İleri**' yi seçin.

    ![Azure geçişi değerlendirmesi oluşturma](./media/tutorial-assess-import/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden geçir + araçlar Ekle**' de ayarları gözden geçirin ve ardından **araç ekle**' yi seçin.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Bundan sonra proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="prepare-the-csv"></a>CSV 'yi hazırlama

CSV şablonunu indirin ve buna sunucu bilgilerini ekleyin.

### <a name="download-the-template"></a>Şablonu indirme

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. **Makine bul**' da **CSV kullanarak içeri aktar**' ı seçin.
3. CSV şablonunu indirmek için **İndir** ' i seçin. Alternatif olarak, [doğrudan indirebilirsiniz](https://go.microsoft.com/fwlink/?linkid=2109031).

    ![CSV şablonunu indir](./media/tutorial-assess-import/download-template.png)

### <a name="add-server-information"></a>Sunucu bilgileri ekleme

Sunucu verilerini toplayın ve CSV dosyasına ekleyin.

- Verileri toplamak için, VMware vSphere veya yapılandırma yönetimi veritabanınız (CMDB) gibi şirket içi sunucu yönetimi için kullandığınız araçlardan dışarı aktarabilirsiniz.
- Örnek verileri gözden geçirmek için [örnek dosyayı](https://go.microsoft.com/fwlink/?linkid=2108405)indirin.

Aşağıdaki tabloda, doldurulacak dosya alanları özetlenmektedir:

**Alan adı** | **Zorunlu** | **Ayrıntılar**
--- | --- | ---
**Sunucu adı** | Yes | Tam etki alanı adını (FQDN) belirtmeyi öneririz.
**IP adresi** | No | Sunucu adresi.
**Çekirdekler** | Yes | Sunucuya ayrılan işlemci çekirdekleri sayısı.
**Bellek** | Yes | Sunucuya ayrılan toplam RAM (MB).
**İşletim sistemi adı** | Yes | Sunucu işletim sistemi. <br/> [Bu](#supported-operating-system-names) listedeki adları eşleşen veya içeren işletim sistemi adları değerlendirme tarafından tanınır.
**İşletim sistemi sürümü** | No | Sunucu işletim sistemi sürümü.
**İşletim sistemi mimarisi** | No | Sunucu işletim sistemi mimarisi <br/> Geçerli değerler şunlardır: x64, x86, AMD64, 32-bit veya 64 bit
**Disk sayısı** | No | Ayrı disk ayrıntıları sağlanmışsa gerekli değildir.
**Disk 1 boyutu**  | No | Maksimum disk boyutu (GB cinsinden).<br/>Şablona [sütun ekleyerek](#add-multiple-disks) daha fazla disk için ayrıntılar ekleyebilirsiniz. En fazla sekiz disk ekleyebilirsiniz.
**Disk 1 okuma Ops** | No | Saniye başına disk okuma işlemi.
**Disk 1 yazma Ops** | No | Saniye başına disk yazma işlemi.
**Disk 1 okuma aktarım hızı** | No | Diskten saniyede okunan veriler, MB/saniye cinsinden.
**Disk 1 yazma aktarım hızı** | No | Saniyede diske yazılan veriler (MB/saniye).
**CPU kullanım yüzdesi** | No | Kullanılan CPU yüzdesi.
**Bellek kullanım yüzdesi** | No | Kullanılan RAM yüzdesi.
**Toplam disk okuma Ops** | No | Saniye başına disk okuma işlemi.
**Toplam disk yazma Ops** | No | Saniye başına disk yazma işlemi.
**Toplam disk okuma performansı** | No | Diskten MB/saniye cinsinden okunan veriler.
**Toplam disk yazma performansı** | No | Diske yazılan veriler MB/saniye cinsinden.
**Aktarım sırasında ağ** | No | Sunucu tarafından saniyede MB cinsinden alınan veriler.
**Ağ çıkış performansı** | No | Sunucu tarafından saniye başına MB cinsinden aktarılan veriler.
**Bellenim türü** | No | Sunucu üretici yazılımı. Değerler "BIOS" veya "UEFı" olabilir.
**MAC adresi**| No | Sunucu MAC adresi.


### <a name="add-operating-systems"></a>İşletim sistemleri ekleme

Değerlendirme, belirli işletim sistemi adlarını tanır. Belirttiğiniz ad, [desteklenen adlar listesindeki](#supported-operating-system-names)dizelerden biriyle tam olarak eşleşmelidir.

### <a name="add-multiple-disks"></a>Birden çok disk Ekle

Şablon, ilk disk için varsayılan alanlar sağlar. En fazla sekiz diske benzer sütunlar ekleyebilirsiniz.

Örneğin, ikinci bir disk için tüm alanları belirtmek için şu sütunları ekleyin:

- Disk 2 boyutu
- Disk 2 okuma Ops
- Disk 2 yazma Ops
- Disk 2 okuma performansı
- Disk 2 yazma işleme


## <a name="import-the-server-information"></a>Sunucu bilgilerini içeri aktarma

CSV şablonuna bilgi ekledikten sonra sunucuları sunucu değerlendirmesi içine aktarın.

1. Azure geçişi 'nde, **bulma makineler**bölümünde, tamamlanan şablona gidin.
2. **İçeri aktar**'ı seçin.
3. İçeri aktarma durumu gösterilir.
    - Durum durumunda uyarı görünürse, bunları çözebilir ya da bunları bilmeden devam edebilirsiniz.
    - Değerlendirme doğruluğunu artırmak için, Uyarılar bölümünde önerildiği gibi sunucu bilgilerini geliştirebilirsiniz.
    - Uyarıları görüntülemek ve onarmak için **uyarı ayrıntılarını İndir ' i seçin. CSV**. Bu işlem CSV 'yi dahil edilen uyarılarla indirir. Uyarıları gözden geçirin ve sorunları gerektiği şekilde giderin.
    - Durum, içeri aktarma durumunun **başarısız**olması için durumda görünüyorsa, içeri aktarmaya devam edebilmeniz için önce bu hataları çözmeniz gerekir:
        1. Artık hata ayrıntılarını içeren CSV 'yi indirin.
        1. Hataları gözden geçirin ve gerektiği şekilde çözün. 
        1. Değiştirilen dosyayı yeniden karşıya yükleyin.
4. İçeri aktarma durumu **tamamlandığında**, sunucu bilgileri içeri aktarılır.

## <a name="update-server-information"></a>Sunucu bilgilerini güncelleştir

Sunucu için verileri aynı **sunucu adıyla**yeniden içeri aktararak sunucu bilgilerini güncelleştirebilirsiniz. **Sunucu adı** alanını değiştiremezsiniz. Sunucuları silme işlemi şu anda desteklenmiyor.

## <a name="verify-servers-in-the-portal"></a>Portalda sunucuları doğrulama

Azure portal, bulma sonrasında sunucuların göründüğünü doğrulamak için:

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi** sayfasında, **bulunan sunucuların**sayısını görüntüleyen simgeyi seçin.
3. **Içeri aktarma tabanlı** sekmesini seçin.

## <a name="set-up-and-run-an-assessment"></a>Değerlendirme ayarlama ve çalıştırma

Sunucu değerlendirmesi kullanarak iki tür değerlendirme oluşturabilirsiniz.

**Değerlendirme türü** | **Ayrıntılar** | **Veri**
--- | --- | ---
**Performans tabanlı** | Belirtilen performans verileri değerlerini temel alan değerlendirmeler. | **ÖNERILEN VM boyutu**: CPU ve bellek kullanım verilerine göre.<br/><br/> **Önerilen disk türü (Standart veya Premium yönetilen disk)**: giriş/çıkış/sanıye (IOPS) ve şirket içi disklerin verimlilik temelinde.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler. | **ÖNERILEN VM boyutu**: belirtilen sunucu boyutunu temel alır.<br/><br> **Önerilen disk türü**: değerlendirme için seçtiğiniz depolama türü ayarına göre.

Bir değerlendirme çalıştırmak için:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğunda **değerlendir**' i seçin.

    ![Değerlendirme](./media/tutorial-assess-physical/assess.png)

3. **Sunucuları değerlendir**bölümünde, değerlendirme için bir ad belirtin.
4. **Bulma kaynağı**' nda **Azure geçişi ' ne içeri aktarma yoluyla eklenen makineler**' i seçin.
5. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’yi seçin.

    ![Değerlendirme özellikleri](./media/tutorial-assess-physical/view-all.png)

6. **Grup Seç veya oluştur**' da, **Yeni oluştur**' u seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla VM 'yi toplar.
7. **Gruba makine ekleme**' de gruba eklenecek sunucular ' ı seçin.
8. Grubu oluşturmak için **değerlendirme oluştur** ' u seçin ve ardından değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-physical/assessment-create.png)

9. Değerlendirme oluşturulduktan sonra **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**  >  **değerlendirmeleri**' nde görüntüleyin.
10. Bir Microsoft Excel dosyası olarak indirmek için **dışarı aktarma değerlendirmesi** ' ni seçin.

## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme şunları açıklar:

- **Azure hazırlığı**: sunucuların Azure 'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure 'da sunucuları çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: geçişten sonra disk depolaması için tahmini maliyetler.

### <a name="view-an-assessment"></a>Değerlendirme görüntüleme

1. **Geçiş hedefleri**  >  **sunucularında** **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendirmeler** ' ı seçin.
2. **Değerlendirmede**, açmak için bir değerlendirme seçin.

    ![Değerlendirme özeti](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlığını gözden geçirme

1. **Azure 'a hazırlık**bölümünde, sunucuların Azure 'a geçiş için hazır olup olmadığını saptayın.
2. Durumu gözden geçirin:
    - **Azure Için hazırlanma**: Azure geçişi, değerlendirmede VM 'ler IÇIN bir VM boyutu ve maliyet tahminleri önerir.
    - **Koşullara hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazır olma durumu bilinmiyor**: Azure geçişi, veri kullanılabilirliği sorunları nedeniyle hazırlığı değerlendiremez.

3. Bir **Azure hazırlık** durumu seçin. İşlem, depolama ve ağ ayarları dahil sunucu ayrıntılarını görmek için sunucu hazırlığı ayrıntılarını görüntüleyebilir ve ayrıntıya gidebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm Azure 'da çalışan VM 'lerin tahmini işlem ve depolama maliyetini gösterir. Seçenekleriniz şunlardır:

- Aylık işlem ve depolama maliyetlerini gözden geçirin. Ücretler, değerlendirilen gruptaki tüm sunucular için toplanır.

    - Maliyet tahminleri, bir makine için boyut önerilerini ve bunların disklerini ve özelliklerini temel alır.
    - İşlem ve depolama için tahmini aylık maliyetler gösterilir.
    - Maliyet tahmini, şirket içi sunucuları hizmet olarak altyapı (IaaS) VM 'Leri olarak çalıştırmak içindir. Sunucu değerlendirmesi hizmet olarak platform (PaaS) veya hizmet olarak yazılım (SaaS) maliyetlerini kabul etmez.

- Aylık depolama maliyeti tahminlerini gözden geçirin. Bu görünüm, değerlendirilen grup için toplanan depolama maliyetlerini gösterir ve farklı türlerde depolama diskleri arasında bölünür.
- Belirli VM 'Lerin ayrıntılarını görmek için detaya gidin.

> [!NOTE]
> Güven derecelendirmeleri, CSV kullanılarak sunucu değerlendirmesi içine aktarılan sunucu değerlendirmelerine atanmaz.

## <a name="supported-operating-system-names"></a>Desteklenen işletim sistemi adları

CSV 'de belirtilen işletim sistemi adları, bu listedeki adları eşleşmeli veya içermelidir. Bu, değerlendirme tarafından geçerli olarak tanınmak üzere belirtilen adlar için gereklidir.

<!-- BEGIN A - H -->

:::row:::
   :::column span="2":::
      **A-H**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Apple Mac OS X 10
   :::column-end:::
   :::column span="":::
      Asıanux 3<br/>
      Asıanux 4<br/>
      Asıanux 5
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      CentOS<br/>
      CentOS 4/5
   :::column-end:::
   :::column span="":::
      CoreOS Linux
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Borçlu GNU/Linux 4<br/>
      Borçlu GNU/Linux 5<br/>
      Borçlu GNU/Linux 6<br/>
      Borçlu GNU/Linux 7<br/>
      De, GNU/Linux 8
   :::column-end:::
   :::column span="":::
      FreeBSD
   :::column-end:::
:::row-end:::

<!-- BEGIN I - R -->

:::row:::
   :::column span="2":::
      **I-R**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      IBM OS/2
   :::column-end:::
   :::column span="":::
      MS-DOS
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Novell NetWare 5<br/>
      Novell NetWare 6
   :::column-end:::
   :::column span="":::
      Oracle Linux<br/>
       Oracle Linux 4/5<br/>
      Oracle Solaris 10<br/>
       Oracle Solaris 11
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Red Hat Enterprise Linux 2<br/>
      Red Hat Enterprise Linux 3<br/>
      Red Hat Enterprise Linux 4<br/>
      Red Hat Enterprise Linux 5<br/>
      Red Hat Enterprise Linux 6<br/>
      Red Hat Enterprise Linux 7<br/>
      Red Hat Fedora
   :::column-end:::
:::row-end:::

<!-- BEGIN S - T -->

:::row:::
   :::column span="2":::
      **S-T**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      SCO OpenServer 5<br/>
      SCO OpenServer 6<br/>
      SCO UnixWare 7
   :::column-end:::
   :::column span="":::
      Serenlik sistemleri eComStation 1<br/>
      Çevre sistemleri, eComStation 2
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Sun Microsystems Solaris 8<br/>
      Sun Microsystems Solaris 9
   :::column-end:::
   :::column span="":::
      SUSE Linux Enterprise 10<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE Linux Enterprise 12<br/>
      SUSE Linux Enterprise 8/9<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE openSUSE
   :::column-end:::
:::row-end:::

<!-- BEGIN U - Z -->
:::row:::
   :::column span="2":::
      **U-Z**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Ubuntu Linux
   :::column-end:::
   :::column span="":::
      VMware ESXi 4<br/>
      VMware ESXi 5<br/>
      VMware ESXi 6
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Windows 10<br/>
      Windows 2000<br/>
      Windows 3<br/>
      Windows 7<br/>
      Windows 8<br/>
      Windows 95<br/>
      Windows 98<br/>
      Windows NT<br/>
      Windows Server (R) 2008<br/>
      Windows Server 2003
   :::column-end:::
   :::column span="":::
      Windows Server 2008<br/>
      Windows Server 2008 R2<br/>
      Windows Server 2012<br/>
      Windows Server 2012 R2<br/>
      Windows Server 2016<br/>
      Windows Server 2019<br/>
      Windows Server eşiği<br/>
      Windows Vista<br/>
      Windows Web Server 2008 R2<br/>
      Windows XP Professional
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * İçeri aktarılan sunucular Azure geçişi: CSV kullanarak sunucu değerlendirmesi.
> * Bir değerlendirme oluşturulup gözden geçirildi.

Şimdi daha doğru değerlendirmeler için [bir gereç dağıtın](./migrate-appliance.md) ve [bağımlılık Analizi](./concepts-dependency-visualization.md)kullanarak daha derin değerlendirme için sunucuları birlikte gruplar halinde toplayın.
