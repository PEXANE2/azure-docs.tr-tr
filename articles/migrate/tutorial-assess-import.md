---
title: Azure Geçişi Sunucu Değerlendirmesi ile içeri aktarılan sunucu verilerini kullanarak sunucuları değerlendirme
description: Azure Geçişi Sunucu Değerlendirmesi ile içeri aktarılan verileri kullanarak şirket içi sunucuları Azure geçişi için değerlendirme sürecini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: ff7e423063859a6cdc1a4362fb030c0deb75eb32
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89658697"
---
# <a name="assess-servers-by-using-imported-data"></a>İçeri aktarılan verileri kullanarak sunucuları değerlendirme

Bu makalede [Azure Geçişi: Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracıyla sunucu meta verilerini virgülle ayrılmış değerler (CSV) biçiminde içeri aktararak şirket içi sunucuları değerlendirme süreci anlatılmıştır. Bu değerlendirme yöntemini kullanmak için Azure Geçişi aletini ayarlayarak bir değerlendirme oluşturmanıza gerek yoktur. Bu yöntem şu durumlarda yararlıdır:

- Aleti dağıtmadan önce hızlı bir başlangıç değerlendirmesi oluşturmak istiyorsanız.
- Azure Geçişi aletini kuruluşunuzda dağıtma imkanınız yoksa.
- Şirket içi sunuculara erişim sağlayan kimlik bilgilerini paylaşamıyorsanız.
- Güvenlik kısıtlamaları nedeniyle alet tarafından toplanan verilerin Azure'a gönderilmesini sağlayamıyorsanız. İçeri aktarılan dosya ile paylaşılan veriler sizin kontrolünüzde olur. Ayrıca verilerin büyük çoğunluğu (örneğin IP adresleri) isteğe bağlıdır.

## <a name="before-you-start"></a>Başlamadan önce

Şu noktalara dikkat edin:

- Tek bir CSV dosyasına en fazla 20.000 sunucu ekleyebilirsiniz.
- Azure Geçişi projesinde CSV dosyası kullanarak 20.000 adede kadar sunucu ekleyebilirsiniz.
- CSV dosyası kullanarak Sunucu Değerlendirmesi aracına sunucu bilgilerini birden fazla oturumda yükleyebilirsiniz.
- Uygulama bilgilerini toplamak, şirket içi ortamınızı geçiş açısından değerlendirmek için yararlıdır. Ancak Sunucu Değerlendirmesi şu an için uygulama düzeyinde değerlendirme yapmamakta ve değerlendirme oluştururken uygulamaları dikkate almamaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Geçişi projesi oluşturma.
> * Sunucu bilgilerini kullanarak bir CSV dosyası oluşturma.
> * Dosyayı Sunucu Değerlendirmesi aracına aktararak sunucu bilgilerini ekleme.
> * Değerlendirme oluşturma ve gözden geçirme.

> [!NOTE]
> Öğreticiler, hızlıca bir kavram kanıtı oluşturabilmeniz için senaryoyla ilgili en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için nasıl yapılır kılavuzlarını inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="set-azure-permissions-for-azure-migrate"></a>Azure Geçişi için gerekli Azure izinlerini ayarlama

Azure Geçişi projesi oluşturmak için Azure hesabınızda belirli izinler vermeniz gerekir.

1. Azure portalında ilgili aboneliği açın ve **Erişim denetimi (IAM)** öğesini seçin.
2. **Erişimi denetle** bölümünde ilgili hesabı bulun ve seçerek izinleri görüntüleyin.
3. **Katkıda bulunan** veya **Sahip** izinlerine sahip olduğunuzdan emin olun.
    - Ücretsiz Azure hesabı oluşturduysanız aboneliğinizin sahibi siz olursunuz.
    - Aboneliğin sahibi siz değilseniz sahibiyle iletişime geçerek gerekli rolün atanmasını sağlayın.

## <a name="set-up-an-azure-migrate-project"></a>Azure Geçişi projesi oluşturma

Yeni bir Azure Geçişi projesi oluşturmak için:

1. Azure portalının **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel Bakış** bölümünde **Sunucuları bul, değerlendir ve geçiş** altında **Sunucuları değerlendir ve geçir**'i seçin.

    ![Sunucuları bulma ve değerlendirme](./media/tutorial-assess-import/assess-migrate.png)

4. **Başlarken** bölümünde **Araç ekle**'yi seçin.
5. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
6. **Proje ayrıntıları** bölümünde proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin. Daha fazla bilgi için:

    - [Genel](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government) için desteklenen coğrafyaları inceleyin.
    - Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

    ![Azure Geçişi projesi oluşturma](./media/tutorial-assess-import/migrate-project.png)

7. **İleri**’yi seçin.
8. **Değerlendirme aracı seç** bölümünde **Azure Geçişi: Sunucu Değerlendirmesi** > **İleri** seçeneklerini belirleyin.

    ![Azure Geçişi değerlendirmesi oluşturma](./media/tutorial-assess-import/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden geçirme + araç ekleme** bölümünde ayarları gözden geçirip **Araç ekle**’yi seçin.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Daha sonra proje sayfasına yönlendirilirsiniz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="prepare-the-csv"></a>CSV dosyasını hazırlama

CSV şablonunu indirin ve içine sunucu bilgilerini ekleyin.

### <a name="download-the-template"></a>Şablonu indirme

1. **Geçiş hedefleri** > **Sunucular** > **Azure Geçişi: Sunucu Değerlendirmesi** bölümünde **Bul**'u seçin.
2. **Makineleri bul** bölümünde **CSV kullanarak içeri aktar**'ı seçin.
3. CSV şablonunu indirmek için **İndir**'i seçin. Alternatif olarak [buradan doğrudan indirebilirsiniz](https://go.microsoft.com/fwlink/?linkid=2109031).

    ![CSV şablonunu indirin](./media/tutorial-assess-import/download-template.png)

### <a name="add-server-information"></a>Sunucu bilgilerini ekleyin

Sunucu verilerini toplayın ve CSV dosyasına ekleyin.

- Verileri toplamak için şirket içi sunucu yönetiminde kullandığınız VMware vSphere veya yapılandırma yönetimi veritabanınız (CMDB) gibi araçlarda bulunan verileri dışarı aktarabilirsiniz.
- Örnek verileri gözden geçirmek için [örnek dosyayı](https://go.microsoft.com/fwlink/?linkid=2108405) indirin.

Aşağıdaki tabloda doldurulacak dosya alanları özetlenmiştir:

**Alan adı** | **Zorunlu** | **Ayrıntılar**
--- | --- | ---
**Sunucu adı** | Yes | Tam etki alanı adını (FQDN) belirtmeniz önerilir.
**IP adresi** | Hayır | Sunucunun adresi.
**Çekirdekler** | Yes | Sunucu için ayrılmış olan işlemci çekirdeği sayısı.
**Bellek** | Yes | Sunucu için ayrılmış olan toplam RAM miktarı (MB).
**İşletim sistemi adı** | Yes | Sunucunun işletim sistemi. <br/> Değerlendirme aracı, [bu listede](#supported-operating-system-names) yer alan adlarla eşleşen veya onları içeren işletim sistemi adlarını tanır.
**İşletim sistemi sürümü** | Hayır | Sunucunun işletim sistemi sürümü.
**İşletim sistemi mimarisi** | Hayır | Sunucunun işletim sistemi mimarisi <br/> Geçerli değerler: x64, x86, amd64, 32-bit veya 64-bit
**Disk sayısı** | Hayır | Her diskle ilgili ayrıntılı bilgi sağlanmışsa gerekli değildir.
**Disk 1 boyutu**  | Hayır | Maksimum disk boyutu (GB).<br/>Şablona [sütun ekleyerek](#add-multiple-disks) daha fazla diskin ayrıntılarını ekleyebilirsiniz. En fazla sekiz disk ekleyebilirsiniz.
**Disk 1 okuma işlemi/saniye** | Hayır | Saniyede gerçekleştirilen disk okuma işlemi sayısı.
**Disk 1 yazma işlemi/saniye** | Hayır | Saniyede gerçekleştirilen disk yazma işlemi sayısı.
**Disk 1 okuma aktarım hızı** | Hayır | Diskten bir saniyede okunan veri miktarı (MB/sn).
**Disk 1 yazma aktarım hızı** | Hayır | Diske bir saniyede yazılan veri miktarı (MB/sn).
**CPU kullanım yüzdesi** | Hayır | Kullanılan CPU yüzdesi.
**Bellek kullanım yüzdesi** | Hayır | Kullanılan RAM yüzdesi.
**Toplam disk okuma işlemi/saniye** | Hayır | Tüm diskler için saniye başına okuma işlemi sayısı. <br/> Disk düzeyinde veri sağlayamıyorsanız bu alanı kullanabilirsiniz. 
**Toplam disk yazma işlemi/saniye** | Hayır | Tüm diskler için saniye başına yazma işlemi sayısı. <br/> Disk düzeyinde veri sağlayamıyorsanız bu alanı kullanabilirsiniz.
**Toplam disk okuma aktarım hızı** | Hayır | Tüm disklerden okunan veri miktarı (MB/sn). <br/> Disk düzeyinde veri sağlayamıyorsanız bu alanı kullanabilirsiniz. 
**Toplam disk yazma aktarım hızı** | Hayır | Tüm disklere yazılan veri miktarı (MB/sn). <br/> Disk düzeyinde veri sağlayamıyorsanız bu alanı kullanabilirsiniz.
**Ağa gelen aktarım hızı** | Hayır | Sunucu tarafından alınan veri miktarı (MB/sn).
**Ağdan giden aktarım hızı** | Hayır | Sunucu tarafından gönderilen veri miktarı (MB/sn).
**Üretici yazılımı türü** | Hayır | Sunucunun üretici yazılımı. Değerler "BIOS" veya "UEFI" olabilir.
**MAC adresi**| Hayır | Sunucunun MAC adresi.


### <a name="add-operating-systems"></a>İşletim sistemi ekleme

Değerlendirme, belirli işletim sistemi adlarını tanır. Belirttiğiniz adların [desteklenen adlar listesinde](#supported-operating-system-names) yer alan girişlerle tam olarak eşleşmesi gerekir.

### <a name="add-multiple-disks"></a>Birden çok disk ekleme

Şablonda ilk disk için varsayılan alanlar verilmiştir. Benzer sütunlar ekleyerek en fazla sekiz disk ekleyebilirsiniz.

Örneğin ikinci disk için tüm alanları belirtmek isterseniz şu sütunları ekleyin:

- Disk 2 boyutu
- Disk 2 okuma işlemi/saniye
- Disk 2 yazma işlemi/saniye
- Disk 2 okuma aktarım hızı
- Disk 2 yazma aktarım hızı

Disk düzeyinde veri sağlayamıyorsanız aşağıdaki alanları kullanarak sunucuya göre disk performans verilerini belirtebilirsiniz. Alanlarla ilgili ayrıntılı bilgi için [bu bölüme](#add-server-information) bakın.
- Toplam disk okuma işlemi/saniye
- Toplam disk yazma işlemi/saniye
- Toplam disk okuma aktarım hızı
- Toplam disk yazma aktarım hızı

## <a name="import-the-server-information"></a>Sunucu bilgilerini içeri aktarma

Bilgileri CSV şablonuna ekledikten sonra Sunucu Değerlendirmesi aracına aktarabilirsiniz.

1. Azure Geçişi'nin **Makineleri bul** bölümünde doldurduğunuz şablonu seçin.
2. **İçeri aktar**'ı seçin.
3. İçeri aktarma durumu gösterilir.
    - Durum alanında uyarılar görünürse bunları düzeltebilir veya düzeltmeden devam edebilirsiniz.
    - Değerlendirmenin doğruluğunu artırmak için uyarıları dikkate alarak sunucu bilgilerini düzeltmeniz önerilir.
    - Uyarıları görüntülemek ve düzeltmek için **Uyarı ayrıntılarını indir (CSV)** öğesini seçin. Bunu yaptığınızda CSV dosyası uyarılarla birlikte indirilir. Uyarıları inceleyin ve sorunları gerektiği gibi giderin.
    - Durumda hata görünürse ve içeri aktarma işlemi **Başarısız** olursa içeri aktarma işlemini gerçekleştirmek için ilgili hataları mutlaka gidermeniz gerekir:
        1. Hata bilgilerinin yer aldığı CSV dosyasını indirin.
        1. Hataları gözden geçirin ve gerektiği gibi düzeltin. 
        1. Değiştirdiğiniz dosyayı yeniden karşıya yükleyin.
4. İçeri aktarma durumu **Tamamlandı** olduğunda sunucu bilgileri içeri aktarılmış demektir.

## <a name="update-server-information"></a>Sunucu bilgilerini güncelleştirme

Aynı **Sunucu adı** değerini kullanarak yeniden veri içeri aktardığınızda sunucu bilgilerini güncelleştirmiş olursunuz. **Sunucu adı** alanını değiştiremezsiniz. Şu an için sunucuları silme işlemi desteklenmez.

## <a name="verify-servers-in-the-portal"></a>Portalda sunucuları doğrulama

Bulma işlemi sonrasında sunucuların Azure portalında göründüğünü doğrulamak için:

1. Azure Geçişi panosunu açın.
2. **Azure Geçişi - Sunucular** > **Azure Geçişi: Sunucu Değerlendirmesi** sayfasında **Bulunan sunucuların** sayısını gösteren simgeyi seçin.
3. **İçeri aktarma tabanlı** sekmesini seçin.

## <a name="set-up-and-run-an-assessment"></a>Değerlendirme ayarlama ve çalıştırma

Sunucu Değerlendirmesi'ni kullanarak iki tür değerlendirme oluşturabilirsiniz.


**Değerlendirme Türü** | **Ayrıntılar**
--- | --- 
**Azure VM** | Şirket içi sunucularınızı Azure sanal makinelerine geçirmeye yönelik değerlendirmeler. <br/><br/> Bu değerlendirme türünü kullanarak şirket içi ortamınızdaki [VMware VM'lerini](how-to-set-up-appliance-vmware.md), [Hyper-V VM'lerini](how-to-set-up-appliance-hyper-v.md) ve [fiziksel sunucuları](how-to-set-up-appliance-physical.md) Azure geçişi için değerlendirebilirsiniz.
**Azure VMware Çözümü (AVS)** | Şirket içi sunucularınızı [Azure VMware Çözümü'ne (AVS)](../azure-vmware/introduction.md) geçirmeye yönelik değerlendirmeler. <br/><br/> Bu değerlendirme türünü kullanarak şirket içi ortamınızdaki [VMware VM'lerinizi](how-to-set-up-appliance-vmware.md) Azure VMware Çözümü (AVS) geçişi için değerlendirebilirsiniz. [Daha fazla bilgi edinin](concepts-azure-vmware-solution-assessment-calculation.md)

### <a name="sizing-criteria"></a>Boyutlandırma ölçütü

Sunucu Değerlendirmesi iki boyutlandırma ölçütü seçeneği sunar:

**Boyutlandırma ölçütü** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine göre öneriler sunan değerlendirmeler | **Azure VM değerlendirmesi**: VM boyutu önerisi, CPU ve bellek kullanımı verilerini dikkate alır.<br/><br/> Disk türü önerisi (standart HDD/SSD veya premium yönetilen diskler), şirket içi ortamda bulunan disklerin IOPS ve aktarım hızı verilerini dikkate alır.<br/><br/> **Azure VMware Çözümü (AVS) değerlendirmesi**: AVS düğümü önerisi, CPU ve bellek kullanımı verilerini dikkate alır.
**Şirket içinde olduğu gibi** | Bu değerlendirmeler, öneride bulunmak için performans verilerini kullanmaz. | **Azure VM değerlendirmesi**: VM boyutu önerisi, şirket içi VM boyutunu temel alır<br/><br> Önerilen disk türü, değerlendirme için belirlediğiniz depolama türü ayarına bağlıdır.<br/><br/> **Azure VMware Çözümü (AVS) değerlendirmesi**: AVS düğümü önerisi, şirket içi VM boyutunu temel alır.


Değerlendirme çalıştırmak için:

1. Değerlendirme oluşturmaya yönelik [en iyi yöntemleri](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesindeki **Azure Geçişi: Sunucu Değerlendirmesi** kutucuğunda **Değerlendir**'i seçin.

    ![Değerlendirme](./media/tutorial-assess-physical/assess.png)

3. **Sunucuları değerlendirme** bölümünde değerlendirme adını belirtin ve **değerlendirme** türünü Azure VM değerlendirmesi gerçekleştirmek için *Azure VM*, AVS değerlendirmesi gerçekleştirmek için ise *Azure VMware Çözümü (AVS)* olarak belirleyin.

    ![Değerlendirmeyle ilgili temel bilgiler](./media/how-to-create-assessment/assess-servers-azurevm.png)

4. **Bulma kaynağı** alanında **Azure Geçişi'ne içeri aktarma yoluyla eklenen makineler**'i seçin.

5. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’yi seçin.

    ![Değerlendirme özellikleri](./media/tutorial-assess-physical/view-all.png)

6. **Değerlendirilecek makineleri seçin**'in yanındaki **İleri**'yi seçin. **Grup oluşturun veya seçin** alanında **Yeni oluştur**'u seçip bir grup adı belirtin. Grup, değerlendirme için bir veya daha fazla VM'yi bir araya getirir.
7. **Makineleri gruba ekleyin** bölümünde gruba eklemek istediğiniz sunucuları seçin.
8. Değerlendirme ayrıntılarını gözden geçirmek için **Gözden geçir ve değerlendirmeyi oluştur**'un yanındaki **İleri**'yi seçin.
9. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **Değerlendirme oluştur**'u seçin.

    ![Değerlendirme oluşturma](./media/tutorial-assess-physical/assessment-create.png)

9. Değerlendirme oluşturulduktan sonra **Sunucular** > **Azure Geçişi: Sunucu Değerlendirmesi** > **Değerlendirmeler** sayfasından görüntüleyin.
10. Microsoft Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’ı seçin.

**Azure VMware Çözümü (AVS)** değerlendirmesi hakkında daha fazla bilgi edinmek için [buraya](how-to-create-azure-vmware-solution-assessment.md) bakın. 

## <a name="review-an-azure-vm-assessment"></a>Azure VM değerlendirmesini gözden geçirme

Azure VM değerlendirmesi şunları ifade eder:

- **Azure için hazır olma**: Sunucuların Azure geçişi için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Sunucuları Azure'da çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyeti.
- **Aylık depolama maliyeti tahmini**: Geçiş sonrasında disk depolaması için tahmini maliyetler.

### <a name="view-an-assessment"></a>Değerlendirmeyi görüntüleme

1. **Azure Geçişi: Sunucu Değerlendirmesi**'nin **Geçiş hedefleri** > **Sunucular** bölümünde **Değerlendirmeler**'i seçin.
2. **Değerlendirmeler** sayfasında açmak istediğiniz değerlendirmeye tıklayın.

    ![Değerlendirme özeti](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure için hazır olmayı gözden geçirme

1. **Azure için hazır olma** bölümünde sunucuların Azure geçişi için hazır olup olmadığını belirleyebilirsiniz.
2. Durumu gözden geçirin:
    - **Azure için hazır**: Azure Geçişi, değerlendirme kapsamındaki VM'ler için VM boyutu önerisi ve maliyet tahmini sunar.
    - **Koşullarla hazır**: Sorunlar ve önerilen çözümler gösterilir.
    - **Azure için hazır değil**: Sorunlar ve önerilen çözümler gösterilir.
    - **Hazır olma durumu bilinmiyor**: Yeterli veri bulunmadığından Azure Geçişi hazır olma durumunu değerlendiremiyor.

3. **Azure için hazır olma** durumlarından birini seçin. Sunucunun hazır olma durumuyla ilgili ayrıntıları görüntüleyebilir ve detaya giderek işlem, depolama ve ağ ayarları gibi sunucu ayrıntılarını inceleyebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirme

Bu görünümde, Azure’da çalışan sanal makinelerin tahmini işlem ve depolama maliyeti görüntülenir. Seçenekleriniz şunlardır:

- Aylık işlem ve depolama maliyetlerini gözden geçirebilirsiniz. Değerlendirilen gruptaki tüm sunucular için toplu maliyetler gösterilir.

    - Maliyet tahminleri, tek bir makine için boyut önerileri ile disklerini ve özelliklerini temel alır.
    - İşlem ve depolama için aylık tahmini maliyetler gösterilir.
    - Tahmini maliyetler, hizmet olarak altyapı (IaaS) VM’leri şeklinde çalıştırılan şirket içi sunuculara yöneliktir. Sunucu Değerlendirmesi, hizmet olarak platform (PaaS) veya hizmet olarak yazılım (SaaS) maliyetlerini hesaba katmaz.

- Aylık depolama maliyeti tahminlerini gözden geçirin. Bu görünüm değerlendirilen grup için farklı depolama diski türlerine göre ayrılmış toplam maliyetleri gösterir.
- Belirli VM'lerin ayrıntılarını görmek için detaya gidebilirsiniz.

> [!NOTE]
> CSV kullanılarak Sunucu Değerlendirmesi aracına aktarılan sunucuların değerlendirmesine güven derecelendirmeleri atanmaz.

## <a name="supported-operating-system-names"></a>Desteklenen işletim sistemi adları

CSV'de belirtilen işletim sistemi adlarının bu listedeki adlarla eşleşmesi veya bunları içermesi gerekir. Bu, değerlendirmenin belirtilen adları geçerli olarak kabul etmesi için şarttır.

<!-- BEGIN A - H -->

:::row:::
   :::column span="2":::
      **A - H**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Apple Mac OS X 10
   :::column-end:::
   :::column span="":::
      Asianux 3<br/>
      Asianux 4<br/>
      Asianux 5
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
      Debian GNU/Linux 4<br/>
      Debian GNU/Linux 5<br/>
      Debian GNU/Linux 6<br/>
      Debian GNU/Linux 7<br/>
      Debian GNU/Linux 8
   :::column-end:::
   :::column span="":::
      FreeBSD
   :::column-end:::
:::row-end:::

<!-- BEGIN I - R -->

:::row:::
   :::column span="2":::
      **I - R**
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
      **S - T**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      SCO OpenServer 5<br/>
      SCO OpenServer 6<br/>
      SCO UnixWare 7
   :::column-end:::
   :::column span="":::
      Serenity Systems eComStation 1<br/>
      Serenity Systems eComStation 2
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
      **U - Z**
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
      Windows Server Threshold<br/>
      Windows Vista<br/>
      Windows Web Server 2008 R2<br/>
      Windows XP Professional
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * CSV dosyası kullanarak Azure Geçişi: Sunucu Değerlendirmesi aracına sunucu aktarma.
> * Değerlendirme oluşturma ve gözden geçirme.

Şimdi daha yüksek doğruluk oranına sahip değerlendirmeler oluşturmak için [bir alet dağıtabilir](./migrate-appliance.md) ve [bağımlılık analizi](./concepts-dependency-visualization.md) ile sunucuları daha ayrıntılı bir değerlendirme gerçekleştirmek üzere bir araya getirebilirsiniz.
