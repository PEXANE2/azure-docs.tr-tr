---
title: Azure Geçir Sunucu Değerlendirmesi ile içe aktarılan sunucu verilerini kullanarak sunucuları değerlendirin
description: İçe aktarılan verileri kullanarak Azure'a geçiş için şirket içi sunucuların Azure'a geçiş için nasıl değerlendirildiğini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 2a30222902fd8797908202562a04018209842af2
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115054"
---
# <a name="assess-servers-by-using-imported-data"></a>Aktarılıverileri kullanarak sunucuları değerlendirin

Bu makalede, sunucu meta verilerini virgülle ayrılmış değerler (CSV) biçiminde içe [aktararak, Azure Geçiş: Sunucu Değerlendirme](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracı yla şirket içi sunucuların nasıl değerlendirilecekleri açıklanmaktadır. Bu değerlendirme yöntemi, bir değerlendirme oluşturmak için Azure Geçiş cihazını kurmanızı gerektirmez. Aşağıdakiler için yararlıdır:

- Cihazı dağıtmadan önce hızlı, ilk bir değerlendirme oluşturmak istiyorsunuz.
- Azure Geçir cihazını kuruluşunuzda dağıtamazsınız.
- Şirket içi sunuculara erişime izin veren kimlik bilgilerini paylaşamazsınız.
- Güvenlik kısıtlamaları, cihaz tarafından toplanan verileri toplamanızı ve Azure'a göndermenizi engeller. İçe aktarılan bir dosyada paylaştığınız verileri denetleyebilirsiniz. Ayrıca, verilerin çoğu (örneğin, IP adresleri sağlama) isteğe bağlıdır.

## <a name="before-you-start"></a>Başlamadan önce

Bu noktalara dikkat edin:

- Tek bir CSV dosyasında en fazla 20.000 sunucu ekleyebilirsiniz.
- Bir Azure Geçiş projesinde CSV kullanarak en fazla 20.000 sunucu ekleyebilirsiniz.
- Sunucu bilgilerini CSV kullanarak sunucu değerlendirmesine birden çok kez yükleyebilirsiniz.
- Uygulama bilgilerini toplamak, şirket içi çevrenizi geçiş için değerlendirmede yararlıdır. Ancak, Sunucu Değerlendirmesi şu anda uygulama düzeyinde değerlendirme yapmaz veya bir değerlendirme oluştururken uygulamaları dikkate almaz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir Azure Geçiş projesi ayarlayın.
> * Sunucu bilgileriyle bir CSV dosyanı doldurun.
> * Sunucu bilgilerini Sunucu Değerlendirmesi'ne eklemek için dosyayı içeri aktarın.
> * Bir değerlendirme oluşturun ve gözden geçirin.

> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilirsiniz, böylece bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için Nasıl Yap'ın hazır olduğu kılavuzlarını inceleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.

## <a name="set-azure-permissions-for-azure-migrate"></a>Azure Geçiş için Azure izinlerini ayarlama

Azure hesabınız, bir Azure Geçiş projesi oluşturmak için izinlere ihtiyaç duyar.

1. Azure portalında aboneliği açın ve **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve ardından izinleri görüntülemek için hesabı seçin.
3. **Katkıda Bulunan** veya **Sahip** İzni aldığınızdan emin olun.
    - Ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi sizsiniz.
    - Abonelik sahibi değilseniz, rolü atamak için sahibiyle birlikte çalışın.

## <a name="set-up-an-azure-migrate-project"></a>Azure Geçiş projesi ayarlama

Yeni bir Azure Geçiş projesi ayarlamak için:

1. Azure portalında, **Tüm hizmetlerde** **Azure Geçiş'i**arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel**Bakış'ta, **Discover, assess ve migrate sunucuları**altında, **sunucuları değerlendir'i seçin ve sunucuları geçirin.**

    ![Sunucuları keşfedin ve değerlendirin](./media/tutorial-assess-import/assess-migrate.png)

4. **Başlarken**, **ekle aracı(lar)** seçeneğini belirleyin.
5. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
6. **PROJE AYRINTILARInDA,** proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin. Daha fazla bilgi için:

    - Desteklenen coğrafyaları gözden [geçirin.](migrate-support-matrix.md#supported-geographies) Proje coğrafyası yalnızca şirket içi sanal makinelerden toplanan meta verileri depolamak için kullanılır.
    - Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

    ![Azure Geçiş projesi oluşturma](./media/tutorial-assess-import/migrate-project.png)

7. **Sonraki'ni**seçin.
8. **Değerlendirme aracını seç'te**Azure **Geçir: Sunucu Değerlendirmesi** > **Sonraki'ni**seçin.

    ![Azure Geçiş değerlendirmesi oluşturma](./media/tutorial-assess-import/assessment-tool.png)

9. **Geçiş aracını seç** bölümünde **Şimdilik geçiş aracı eklemeyi atla** > **İleri** seçeneğini belirleyin.
10. **Gözden Geçir + araç ekle,** ayarları gözden geçir ve ardından araç **ekle'yi**seçin.
11. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin. Daha sonra proje sayfasına götürülürsünüz. Projeyi görmüyorsanız Azure Geçişi panosundaki **Sunuculardan** erişebilirsiniz.

## <a name="prepare-the-csv"></a>CSV'yi hazırlayın

CSV şablonu indirin ve sunucu bilgilerini ekleyin.

### <a name="download-the-template"></a>Şablonu indirme

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**seçin.
2. **Discover makinelerinde,** **CSV kullanarak Içe Aktar'ı**seçin.
3. CSV şablonu indirmek için **İndir'i** seçin. Alternatif olarak, [doğrudan indirebilirsiniz.](https://go.microsoft.com/fwlink/?linkid=2109031)

    ![CSV şablonu indirin](./media/tutorial-assess-import/download-template.png)

### <a name="add-server-information"></a>Sunucu bilgilerini ekleme

Sunucu verilerini toplayın ve CSV dosyasına ekleyin.

- Veri toplamak için, VMware vSphere veya yapılandırma yönetimi veritabanınız (CMDB) gibi şirket içi sunucu yönetimi için kullandığınız araçlardan dışa aktarabilirsiniz.
- Örnek verileri incelemek için [örnek dosyamızı](https://go.microsoft.com/fwlink/?linkid=2108405)indirin.

Aşağıdaki tabloda doldurulacak dosya alanları özetlenebilir:

**Alan adı** | **Zorunlu** | **Şey**
--- | --- | ---
**Sunucu adı** | Evet | Tam nitelikli alan adı (FQDN) belirtmenizi öneririz.
**IP adresi** | Hayır | Sunucu adresi.
**Çekirdekler** | Evet | Sunucuya ayrılan işlemci çekirdeği sayısı.
**Bellek** | Evet | Toplam RAM, MB, sunucuya tahsis.
**İşletim sistemi adı** | Evet | Sunucu işletim sistemi. <br/> [Bu](#supported-operating-system-names) listedeki adlarla eşleşen veya bunları içeren işletim sistemi adları değerlendirme tarafından tanınır.
**İşletim sistemi sürümü** | Hayır | Sunucu işletim sistemi sürümü.
**Disk sayısı** | Hayır | Tek tek disk ayrıntıları sağlanıyorsa gerekli değildir.
**Disk 1 boyutu**  | Hayır | Maksimum disk boyutu, GB olarak.<br/>Şablona [sütunlar ekleyerek](#add-multiple-disks) daha fazla disk için ayrıntı ekleyebilirsiniz. En fazla sekiz disk ekleyebilirsiniz.
**Disk 1 ops okuyun** | Hayır | Disk işlemleri saniyede okuyun.
**Disk 1 yazma ops** | Hayır | Saniyede disk yazma işlemleri.
**Disk 1 okuma iş** | Hayır | Veriler saniyede diskten, mb/saniyede okunur.
**Disk 1 yazma iş** | Hayır | Saniyede diske yazılan veriler, saniyede MB olarak.
**CPU kullanım yüzdesi** | Hayır | Kullanılan CPU yüzdesi.
**Bellek kullanım yüzdesi** | Hayır | Kullanılan RAM yüzdesi.
**Toplam diskler ops okuyun** | Hayır | Saniyede disk okuma işlemleri.
**Toplam diskler ops yazmak** | Hayır | Saniyede disk yazma işlemleri.
**Toplam diskler iş bilgilerini okuyun** | Hayır | Veriler diskten, saniyede MB olarak okunur.
**Toplam diskler iş yoluyla yazmak** | Hayır | Saniyede MB olarak diske yazılan veriler.
**Ağ Giriş** | Hayır | Sunucu tarafından alınan veriler, saniyede MB olarak.
**Ağ Çıkış girişi** | Hayır | Sunucu tarafından saniyede MB olarak aktarılan veriler.
**Firmware türü** | Hayır | Sunucu firmware. Değerler "BIOS" veya "UEFI" olabilir.
**MAC adresi**| Hayır | Sunucu MAC adresi.


### <a name="add-operating-systems"></a>İşletim sistemleri ekleme

Değerlendirme belirli işletim sistemi adlarını tanır. Belirttiğiniz herhangi bir [ad, desteklenen adlar listesindeki](#supported-operating-system-names)dizelerden biriyle tam olarak eşleşmelidir.

### <a name="add-multiple-disks"></a>Birden çok disk ekleme

Şablon, ilk disk için varsayılan alanlar sağlar. Sekiz diske kadar benzer sütunlar ekleyebilirsiniz.

Örneğin, ikinci bir disk için tüm alanları belirtmek için aşağıdaki sütunları ekleyin:

- Disk 2 boyutu
- Disk 2 ops okuyun
- Disk 2 yazma ops
- Disk 2 okuma iş
- Disk 2 yazma iş


## <a name="import-the-server-information"></a>Sunucu bilgilerini alma

CSV şablonuna bilgi ekledikten sonra sunucuları Sunucu Değerlendirmesi'ne aktarın.

1. Azure Geçir'de, **Discover makinelerinde**tamamlanan şablona gidin.
2. **İçeri aktar**'ı seçin.
3. Alma durumu gösterilir.
    - Durum durumunda uyarılar görünüyorsa, bunları düzeltebilir veya bunları ele almadan devam edebilirsiniz.
    - Değerlendirme doğruluğunu artırmak için, uyarılarda önerildiği gibi sunucu bilgilerini geliştirin.
    - Uyarıları görüntülemek ve düzeltmek için **Uyarı Ayrıntılarını İndir'i seçin. CSV**. Bu işlem, csv uyarıları dahil indirir. Uyarıları gözden geçirin ve gerektiğinde sorunları düzeltin.
    - İçe aktarma durumunun **Başarısız**olması için durum daki hatalar görünüyorsa, içe aktarmaya devam etmeden önce bu hataları düzeltmeniz gerekir:
        1. Şimdi hata ayrıntıları içeren CSV'yi indirin.
        1. Hataları gerektiği gibi gözden geçirin ve ele alın. 
        1. Değiştirilen dosyayı yeniden yükleyin.
4. Alma durumu **tamamlandığında,** sunucu bilgileri içe aktarıldı.

## <a name="update-server-information"></a>Sunucu bilgilerini güncelleştir

Aynı **Sunucu adı**ile sunucu için veri aktararak bir sunucu için bilgileri güncelleştirebilirsiniz. **Sunucu adı** alanını değiştiremezsiniz. Sunucuları silme şu anda desteklenmez.

## <a name="verify-servers-in-the-portal"></a>Portaldaki sunucuları doğrulama

Sunucuların keşiften sonra Azure portalında görünerek görüntülenindiğini doğrulamak için:

1. Azure Geçir panosunu açın.
2. Azure **Geçiş - Sunucular** > **Azure Geçir: Sunucu Değerlendirmesi** **sayfasında, Keşfedilen sunucuların**sayısını görüntüleyen simgeyi seçin.
3. **İçe Aktarma sekmesini** seçin.

## <a name="set-up-and-run-an-assessment"></a>Bir değerlendirme ayarlama ve çalıştırma

Sunucu Değerlendirmesi'ni kullanarak iki tür değerlendirme oluşturabilirsiniz.

**Değerlendirme türü** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Belirtilen performans-veri değerlerine dayalı değerlendirmeler. | **Önerilen VM boyutu**: CPU ve bellek kullanım verilerine dayanır.<br/><br/> **Önerilen disk türü (standart veya premium yönetilen disk)**: Saniyede giriş/çıkış (IOPS) ve şirket içi disklerin çıktısı temel alınarak.
**Şirket içinde olduğu gibi** | Şirket içi boyutlandırmaya dayalı değerlendirmeler. | **Önerilen VM boyutu**: Belirtilen sunucu boyutuna göre.<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.

Bir değerlendirme çalıştırmak için:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure Geçir: Sunucu Değerlendirme** döşemesinde **Değerlendir'i**seçin.

    ![Değerlendirme](./media/tutorial-assess-physical/assess.png)

3. **Değerlendir sunucularında,** değerlendirme için bir ad belirtin.
4. **Bulma kaynağında,** **Azure Geçiş'e alma yoluyla eklenen Makineleri**seçin.
5. Değerlendirme özelliklerini gözden geçirmek için **Tümünü Görüntüle'yi** seçin.

    ![Değerlendirme özellikleri](./media/tutorial-assess-physical/view-all.png)

6. **Bir grubu seç veya oluştur'da**Yeni **Oluştur'u**seçin ve bir grup adı belirtin. Bir grup değerlendirme için bir veya daha fazla VM'yi bir araya toplar.
7. **Gruba makine ekle'de,** gruba eklemek için sunucuları seçin.
8. Grubu oluşturmak için **değerlendirme** oluştur'u seçin ve ardından değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/tutorial-assess-physical/assessment-create.png)

9. Değerlendirme oluşturulduktan sonra, **Sunucular** > **Azure Geçiş: Sunucu Değerlendirme** > **Değerlendirmeleri'nde**görüntüleyin.
10. Microsoft Excel dosyası olarak indirmek için **Dışa Aktarma değerlendirmesini** seçin.

## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme açıklar:

- **Azure hazırlığı**: Sunucuların Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'daki sunucuları çalıştırmak için tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.

### <a name="view-an-assessment"></a>Bir değerlendirmeyi görüntüleme

1. **Geçiş hedefleri** > **Sunucularında,** **Azure Geçişinde Değerlendirmeler: Sunucu Değerlendirmesi'ni**seçin. **Assessments**
2. **Değerlendirmeler'de,** açmak için bir değerlendirme seçin.

    ![Değerlendirme özeti](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlık durumunu gözden geçirin

1. **Azure hazır durumunda,** sunucuların Azure'a geçişe hazır olup olmadığını belirleyin.
2. Durumu gözden geçirin:
    - **Azure için Hazır**: Azure Geçirim, değerlendirmede VM boyutu ve VM'ler için maliyet tahminleri önerir.
    - **Koşullara hazır**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazır değil**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazırlık bilinmiyor**: Azure Geçir, veri kullanılabilirliği sorunları nedeniyle hazır olup olmadığını değerlendiremez.

3. Azure **hazırlık** durumunu seçin. Sunucuya hazırlık ayrıntılarını görüntüleyebilir ve bilgi işlem, depolama ve ağ ayarları da dahil olmak üzere sunucu ayrıntılarını görmek için ayrıntıya inebilirsiniz.

### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm, Azure'da VM'leri çalıştırmanın tahmini işlem ve depolama maliyetini gösterir. Şunları yapabilirsiniz:

- Aylık işlem ve depolama maliyetlerini gözden geçirin. Maliyetler, değerlendirilen gruptaki tüm sunucular için toplanır.

    - Maliyet tahminleri, bir makinenin boyut önerilerini ve disklerini ve özelliklerini temel alınr.
    - Hesaplama ve depolama için tahmini aylık maliyetler gösterilir.
    - Maliyet tahmini, şirket içi sunucuları hizmet olarak altyapı (IaaS) VM'leri olarak çalıştırmak içindir. Sunucu Değerlendirmesi, hizmet olarak platform (PaaS) veya hizmet olarak yazılım (SaaS) maliyetlerini dikkate almaz.

- Aylık depolama maliyeti tahminlerini gözden geçirin. Bu görünüm, değerlendirilen grup için farklı depolama diskleri türleri arasında bölünmüş toplu depolama maliyetlerini gösterir.
- Belirli VM'ler için ayrıntıları görmek için ayrıntıya inin.

> [!NOTE]
> CSV kullanılarak Sunucu Değerlendirmesi'ne aktarılan sunucuların değerlendirmelerine güven derecelendirmeleri atanmez.

## <a name="supported-operating-system-names"></a>Desteklenen işletim sistemi adları

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
      IBM İşLET/2
   :::column-end:::
   :::column span="":::
      Ms-dos
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
      Kırmızı Şapka Kurumsal Linux 2<br/>
      Kırmızı Şapka Kurumsal Linux 3<br/>
      Kırmızı Şapka Kurumsal Linux 4<br/>
      Kırmızı Şapka Kurumsal Linux 5<br/>
      Kırmızı Şapka Kurumsal Linux 6<br/>
      Kırmızı Şapka Kurumsal Linux 7<br/>
      Kırmızı Şapka Fedora
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
      Serenity Sistemleri eComStation 1<br/>
      Serenity Sistemleri eComStation 2
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Güneş Mikrosistemleri Solaris 8<br/>
      Güneş Mikrosistemleri Solaris 9
   :::column-end:::
   :::column span="":::
      SUSE Linux Kurumsal 10<br/>
      SUSE Linux Kurumsal 11<br/>
      SUSE Linux Kurumsal 12<br/>
      SUSE Linux Enterprise 8/9<br/>
      SUSE Linux Kurumsal 11<br/>
      SUSE açıkSUSE
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
      Windows Sunucu Eşiği<br/>
      Windows Vista<br/>
      Windows Web Server 2008 R2<br/>
      Windows XP Professional
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure'a aktarılan sunucular Aktadi: CSV kullanarak Sunucu Değerlendirmesi.
> * Bir değerlendirme oluşturuldu ve gözden geçirildi.

Şimdi, daha doğru değerlendirmeler için [bir cihaz dağıtın](./migrate-appliance.md) ve bağımlılık [çözümlemesi](./concepts-dependency-visualization.md)kullanarak daha derin bir değerlendirme için sunucuları gruplar halinde bir araya toplayın.
