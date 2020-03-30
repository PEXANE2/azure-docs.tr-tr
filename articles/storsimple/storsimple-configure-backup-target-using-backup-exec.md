---
title: Backup Exec ile yedek hedef olarak StorSimple 8000 serisi | Microsoft Dokümanlar
description: Veritas Backup Exec ile StorSimple yedekleme hedef yapılandırmasını açıklar.
services: storsimple
documentationcenter: ''
author: harshakirank
manager: matd
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/05/2016
ms.author: matd
ms.openlocfilehash: 4dcda65384190050e11f1bf9b15c706b0e38c6b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75561652"
---
# <a name="storsimple-as-a-backup-target-with-backup-exec"></a>Backup Exec ile yedek hedef olarak StorSimple

## <a name="overview"></a>Genel Bakış

Azure StorSimple, Microsoft'un karma bulut depolama çözümüdür. StorSimple, şirket içi çözümün bir uzantısı olarak bir Azure depolama hesabı kullanarak ve şirket içi depolama ve bulut depolama da verileri otomatik olarak katmanlayarak üstel veri büyümesinin karmaşıklığını giderir.

Bu makalede, Veritas Backup Exec ile StorSimple tümleştirme ve her iki çözümü entegre etmek için en iyi uygulamaları tartışıyoruz. Ayrıca, StorSimple ile en iyi şekilde entegre olacak yedekleme exec'innasıl ayarlanacağı konusunda da önerilerde bulunuyoruz. Tek tek yedekleme gereksinimlerini ve hizmet düzeyi anlaşmalarını (SLA'lar) karşılamak üzere Yedekleme Exec'i kurmanın en iyi yolu için Veritas'ın en iyi uygulamalarını, yedekleme mimarlarını ve yöneticilerini erteleriz.

Yapılandırma adımlarını ve anahtar kavramları gösterebilsek de, bu makale hiçbir şekilde adım adım yapılandırma veya yükleme kılavuzu değildir. Temel bileşenlerin ve altyapının çalışır durumda olduğunu ve tanımladığımız kavramları desteklemeye hazır olduğunu varsayıyoruz.

### <a name="who-should-read-this"></a>Bunu kim okumalı?

Bu makaledeki bilgiler, depolama, Windows Server 2012 R2, Ethernet, bulut hizmetleri ve Backup Exec hakkında bilgisi olan yedekleme yöneticileri, depolama yöneticileri ve depolama mimarları için en yararlı olacaktır.

## <a name="supported-versions"></a>Desteklenen sürümler

-   [Yedek Exec 16 ve sonraki sürümleri](https://www.veritas.com/content/support/en_US/article.100040087)
-   [StorSimple Update 3 ve sonraki sürümler](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Neden StorSimple yedek hedef olarak?

StorSimple, yedek hedef için iyi bir seçimdir, çünkü:

-   Yedekleme uygulamalarının herhangi bir değişiklik olmadan hızlı bir yedekleme hedefi olarak kullanması için standart, yerel depolama sağlar. StorSimple'ı, son yedeklemelerin hızlı bir şekilde geri yüklanması için de kullanabilirsiniz.
-   Bulut katmanlama, uygun maliyetli Azure Depolama'yı kullanmak için bir Azure bulut depolama hesabıyla sorunsuz bir şekilde entegre edilmiştir.
-   Olağanüstü durum kurtarma için otomatik olarak iş yeri dışında depolama sağlar.

## <a name="key-concepts"></a>Önemli kavramlar

Her depolama çözümünde olduğu gibi, çözümün depolama performansının, SLA'ların, değişim oranının ve kapasite büyüme ihtiyaçlarının dikkatli bir şekilde değerlendirilmesi başarı için çok önemlidir. Ana fikir, bir bulut katmanı sunarak, erişim sürelerinizin ve buluta erişim sürelerinizin, StorSimple'ın işini yapma becerisinde temel bir rol oynamasıdır.

StorSimple, iyi tanımlanmış bir veri kümesi (sıcak veri) üzerinde çalışan uygulamalara depolama sağlamak üzere tasarlanmıştır. Bu modelde, çalışma veri kümesi yerel katmanlarda depolanır ve kalan çalışmayan/soğuk/arşivlenmiş veri kümesi buluta katmanlanır. Bu model aşağıdaki şekilde temsil edilir. Neredeyse düz yeşil çizgi, StorSimple aygıtının yerel katmanlarında depolanan verileri temsil eder. Kırmızı çizgi, StorSimple çözümünde tüm katmanlarda depolanan toplam veri miktarını temsil eder. Düz yeşil çizgi ile üstel kırmızı eğri arasındaki boşluk, bulutta depolanan toplam veri miktarını temsil eder.

**StorBasit katmanlama**
![StorBasit katmanlama diyagramı](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

Bu mimari göz önünde bulundurularak, StorSimple'ın yedekleme hedefi olarak çalışmak için ideal olduğunu göreceksiniz. StorSimple'ı şu şekilde kullanabilirsiniz:
-   Yerel çalışma veri kümesinden en sık geri yüklemelerinizi gerçekleştirin.
-   Geri yüklemelerin daha az sıklıkta olduğu iş yeri dışında olağanüstü durum kurtarma ve eski veriler için bulutu kullanın.

## <a name="storsimple-benefits"></a>StorBasit avantajlar

StorSimple, şirket içi ve bulut depolamaya sorunsuz erişimden yararlanarak Microsoft Azure ile sorunsuz bir şekilde entegre edilmiş şirket içi bir çözüm sunar.

StorSimple, katı hal aygıtı (SSD) ve seri bağlı SCSI (SAS) depolama alanına sahip şirket içi aygıt ile Azure Depolama arasında otomatik katmanlama kullanır. Otomatik katmanlama, SSD ve SAS katmanlarında sık sık erişilen verileri yerel tutar. Nadiren erişilen verileri Azure Depolama'ya taşır.

StorSimple bu avantajları sunar:

-   Benzersiz çoğaltma düzeyleri elde etmek için bulut kullanan benzersiz çoğaltma ve sıkıştırma algoritmaları
-   Yüksek kullanılabilirlik
-   Azure coğrafi çoğaltma kullanarak coğrafi çoğaltma
-   Azure tümleştirmesi
-   Bulutta veri şifreleme
-   Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk

StorSimple iki ana dağıtım senaryosu (birincil yedekleme hedefi ve ikincil yedekleme hedefi) sunsa da, temelde düz, blok depolama aygıtıdır. StorSimple tüm sıkıştırma ve deduplication yapar. Bulut ile uygulama ve dosya sistemi arasındaki verileri sorunsuz bir şekilde gönderir ve alır.

StorSimple hakkında daha fazla bilgi için [Bkz. StorSimple 8000 serisi: Karma bulut depolama çözümü.](storsimple-overview.md) Ayrıca, [teknik StorSimple 8000 serisi özellikleri](storsimple-technical-specifications-and-compliance.md)gözden geçirebilirsiniz.

> [!IMPORTANT]
> StorSimple cihazının yedek hedef olarak kullanılması yalnızca StorSimple 8000 Update 3 ve sonraki sürümler için desteklenir.

## <a name="architecture-overview"></a>Mimariye genel bakış

Aşağıdaki tablolar, aygıt modeli-mimari ilk kılavuzunu gösterir.

**StorYerel ve bulut depolama için basit kapasiteler**

| Depolama kapasitesi       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Yerel depolama kapasitesi | &lt;10 TiB\*  | &lt;20 TiB\*  |
| Bulut depolama kapasitesi | &gt;200 TiB\* | &gt;500 TiB\* |

\*Depolama boyutu hiçbir çoğaltma veya sıkıştırma varsayar.

**Birincil ve ikincil yedeklemeler için StorBasit kapasiteler**

| Yedekleme senaryosu  | Yerel depolama kapasitesi  | Bulut depolama kapasitesi  |
|---|---|---|
| Birincil yedekleme  | Kurtarma noktası hedefini (RPO) karşılamak için hızlı kurtarma için yerel depolamada depolanan son yedeklemeler | Yedekleme geçmişi (RPO) bulut kapasitesine sığar |
| İkincil yedekleme | Yedekleme verilerinin ikincil kopyası bulut kapasitesinde depolanabilir  | Yok  |

## <a name="storsimple-as-a-primary-backup-target"></a>Birincil yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimleri yedekleme için tek depo olarak yedekleme uygulamasına sunulur. Aşağıdaki şekilde, tüm yedeklemelerin yedeklemeler ve geri yüklemeler için StorSimple katmanlı birimleri kullandığı bir çözüm mimarisi gösterilmektedir.

![Birincil yedekleme hedef mantıksal diyagramı olarak StorSimple](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Birincil hedef yedekleme mantıksal adımları

1.  Yedek sunucu hedef yedekleme aracısı ile bağlantı kurur ve yedek aracı verileri yedekleme sunucusuna iletir.
2.  Yedekleme sunucusu StorSimple katmanlı birimlerine veri yazar.
3.  Yedek sunucu katalog veritabanını güncelleştirir ve yedekleme işini tamamlar.
4.  Anlık görüntü komut dosyası StorSimple bulut anlık görüntü yöneticisini tetikler (başlat veya sil).
5.  Yedekleme sunucusu, bekletme ilkesine dayalı olarak süresi dolmuş yedeklemeleri siler.


### <a name="primary-target-restore-logical-steps"></a>Birincil hedef geri yükleme mantıksal adımlar

1.  Yedekleme sunucusu depolama deposundan uygun verileri geri başlar.
2.  Yedekleme aracısı verileri yedek sunucudan alır.
3.  Yedekleme sunucusu geri yükleme işini bitirir.

## <a name="storsimple-as-a-secondary-backup-target"></a>İkincil yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimleri öncelikle uzun süreli saklama veya arşivleme için kullanılır.

Aşağıdaki şekilde, ilk yedeklemelerin ve geri yüklemelerin yüksek performanslı bir hacmi hedeflediği bir mimari gösterilmektedir. Bu yedeklemeler kopyalanır ve belirli bir zamanlamada StorSimple katmanlı bir birimde arşivlenir.

Bekletme ilkesi kapasitenizi ve performans gereksinimlerinizi işleyebilir şekilde yüksek performanslı ses düzeyinizi boyutlandırmanız önemlidir.

![İkincil yedekleme hedefi mantıksal diyagramı olarak StorSimple](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>İkincil hedef yedekleme mantıksal adımları

1.  Yedek sunucu hedef yedekleme aracısı ile bağlantı kurur ve yedek aracı verileri yedekleme sunucusuna iletir.
2.  Yedekleme sunucusu yüksek performanslı depolamaya veri yazar.
3.  Yedek sunucu katalog veritabanını güncelleştirir ve yedekleme işini tamamlar.
4.  Yedekleme sunucusu yedeklemeleri bir bekletme ilkesine göre StorSimple'a kopyalar.
5.  Anlık görüntü komut dosyası StorSimple bulut anlık görüntü yöneticisini tetikler (başlat veya sil).
6.  Yedekleme sunucusu, bekletme ilkesine dayalı olarak süresi dolmuş yedeklemeleri siler.

### <a name="secondary-target-restore-logical-steps"></a>İkincil hedef geri yükleme mantıksal adımlar

1.  Yedekleme sunucusu depolama deposundan uygun verileri geri başlar.
2.  Yedekleme aracısı verileri yedek sunucudan alır.
3.  Yedekleme sunucusu geri yükleme işini bitirir.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Çözümü dağıtmak için üç adım gerekiyor:
1. Ağ altyapısını hazırlayın.
2. StorSimple cihazınızı yedek hedef olarak dağıtın.
3. Yedek Exec'i dağıtın.

Her adım aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="set-up-the-network"></a>Ağı ayarlama

StorSimple, Azure bulutuyla entegre edilmiş bir çözüm olduğundan, StorSimple Azure bulutuna etkin ve çalışan bir bağlantı gerektirir. Bu bağlantı, bulut anlık görüntüleri, yönetim ve meta veri aktarımı gibi işlemler ve Azure bulut depolamasına daha eski, daha az erişilen verileri katmanlamak için kullanılır.

Çözümün en iyi şekilde çalışması için, aşağıdaki ağ en iyi uygulamalarını izlemenizi öneririz:

-   StorSimple katmanınızı Azure'a bağlayan bağlantı, bant genişliği gereksinimlerinizi karşılamalıdır. Bunu başarmak için, RPO ve kurtarma süresi hedefi (RTO) SLA'larınıza uyacak şekilde altyapı anahtarlarınıza gerekli Hizmet Kalitesi (QoS) düzeyini uygulayın.
-   Maksimum Azure Blob depolama erişim gecikmeleri yaklaşık 80 ms olmalıdır.

### <a name="deploy-storsimple"></a>StorSimple'ı dağıt

Adım adım StorSimple dağıtım kılavuzu için [bkz.](storsimple-deployment-walkthrough-u2.md)

### <a name="deploy-backup-exec"></a>Yedek Exec'i Dağıt

Yedekleme Exec yükleme en iyi uygulamaları [için, Yedek Exec yükleme için en iyi uygulamalar](https://www.veritas.com/content/support/en_US/doc/72686287-131623464-0/v70444238-131623464)bakın.

## <a name="set-up-the-solution"></a>Çözümü ayarlama

Bu bölümde, bazı yapılandırma örnekleri gösteriyoruz. Aşağıdaki örnekler ve öneriler en temel ve temel uygulamayı göstermektedir. Bu uygulama, doğrudan özel yedekleme gereksinimleriniz için geçerli olmayabilir.

### <a name="set-up-storsimple"></a>StorSimple'ı ayarlama

| StorBasit dağıtım görevleri  | Ek yorumlar |
|---|---|
| Şirket içi StorSimple cihazınızı dağıtın. | Desteklenen sürümler: 3 ve sonraki sürümleri güncelleştirin. |
| Yedek hedefi açın. | Yedekleme hedef modunu açmak veya kapatmak ve durum almak için bu komutları kullanın. Daha fazla bilgi için [storsimple aygıta uzaktan bağlan'a](storsimple-remote-connect.md)bakın.</br> Yedekleme modunu açmak `Set-HCSBackupApplianceMode -enable`için: . </br> Yedekleme modunu kapatmak `Set-HCSBackupApplianceMode -disable`için: . </br> Yedekleme modu ayarlarının geçerli durumunu `Get-HCSBackupApplianceMode`almak için: . |
| Biriminiz için yedekleme verilerini depolayan ortak bir birim kapsayıcı oluşturun. Birim kapsayıcıdaki tüm veriler çoğaltılır. | StorBasit birim kapsayıcıları deduplication etki alanlarını tanımlar.  |
| StorSimple birimleri oluşturun. | Birim boyutu bulut anlık görüntü süresi süresini etkilediğinden, beklenen kullanıma mümkün olduğunca yakın boyutlara sahip birimler oluşturun. Bir birimin nasıl boyutlandırılabildiğini öğrenmek için [bekletme ilkeleri](#retention-policies)hakkında bilgi edinin.</br> </br> StorSimple katmanlı birimleri kullanın ve **daha az sıklıkta erişilen arşiv veri** onay kutusu için bu birimi kullanın'ı seçin. </br> Yalnızca yerel olarak sabitlenmiş birimlerin kullanılması desteklenmez. |
| Tüm yedekleme hedef birimleri için benzersiz bir StorSimple yedekleme ilkesi oluşturun. | StorSimple yedekleme ilkesi birim tutarlılık grubunu tanımlar. |
| Anlık görüntüler sona erdiğinde zamanlamayı devre dışı kAlım. | Anlık görüntüler işlem sonrası işlem olarak tetiklenir. |

### <a name="set-up-the-host-backup-server-storage"></a>Ana bilgisayar yedek sunucu depolamasını ayarlama

Ana bilgisayar yedekleme sunucusu depolamasını aşağıdaki yönergelere göre ayarlayın:  

- Yayılan birimleri (Windows Disk Yönetimi tarafından oluşturulmuş) kullanmayın. Yayılan diskler desteklenmez.
- 64-KB ayırma boyutu ile NTFS kullanarak birimlerinizi biçimlendirin.
- StorSimple birimlerini doğrudan Backup Exec sunucusuna eşleyin.
    - Fiziksel sunucular için iSCSI'yi kullanın.
    - Sanal sunucular için geçiş disklerini kullanın.

## <a name="best-practices-for-storsimple-and-backup-exec"></a>StorSimple ve Backup Exec için en iyi uygulamalar

Çözümünüzü aşağıdaki bölümlerdeki yönergelere göre ayarlayın.

### <a name="operating-system-best-practices"></a>İşletim sistemi en iyi uygulamalar

- NTFS dosya sistemi için Windows Server şifrelemesini ve çoğaltmayı devre dışı kaldırın.
- StorSimple birimlerindeki Windows Server parçalanmasını devre dışı bırakma.
- StorSimple birimlerinde Windows Server dizini sinir'i devre dışı kaldırın.
- Kaynak ana bilgisayarda virüsten koruma taramayı çalıştırın (StorSimple birimlerine karşı değil).
- Görev Yöneticisi'ndeki varsayılan [Windows Server bakımını](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) kapatın. Bunu aşağıdaki yollardan biriyle yapın:
  - Windows Görev Zamanlayıcısı'ndaki Bakım yapılandırmacısını kapatın.
  - Windows Sysinternals'den [PsExec'i](https://technet.microsoft.com/sysinternals/bb897553.aspx) indirin. PsExec'i indirdikten sonra Azure PowerShell'i yönetici olarak çalıştırın ve yazın:
    ```powershell
    psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
    ```

### <a name="storsimple-best-practices"></a>StorBasit en iyi uygulamalar

  -   StorSimple aygıtının Güncelleştirme 3 [veya daha sonra](storsimple-install-update-3.md)güncelleştirildiğinden emin olun.
  -   iSCSI ve bulut trafiğini yalıtın. StorSimple ile yedek sunucu arasındaki trafik için özel iSCSI bağlantılarını kullanın.
  -   StorSimple cihazınızın özel bir yedekleme hedefi olduğundan emin olun. RTO ve RPO'nuzu etkilediği için karışık iş yükleri desteklenmez.

### <a name="backup-exec-best-practices"></a>Yedekleme Exec en iyi uygulamaları

-   Backup Exec, StorSimple biriminde değil, sunucunun yerel bir sürücüsüne yüklenmelidir.
-   Backup Exec depolama eşzamanlı **yazma işlemlerini** izin verilen maksimum aya ayarlayın.
    -   Yedek Exec depolama **bloğunu ve arabellek boyutunu** 512 KB olarak ayarlayın.
    -   Yedekleme Exec depolama **arabelleğe alma ve yazma'yı**açın.
-   StorSimple, Yedekleme Exec tam ve artımlı yedeklemeleri destekler. Sentetik ve diferansiyel yedekleme kullanmamanızı öneririz.
-   Yedekleme veri dosyaları yalnızca belirli bir iş için veri içermelidir. Örneğin, farklı işlerde hiçbir ortam ekine izin verilmez.
-   İş doğrulamasını devre dışı. Gerekirse, doğrulama en son yedekleme işinden sonra zamanlanmalıdır. Bu işin yedekleme pencerenizi etkilediğini anlamak önemlidir.
-   **Depolama** > **diskinizin** > **Ayrıntıları** > **Özelliklerini**seçin. Ön **ayırma disk alanını**kapatın.

En son Yedekleme Exec ayarları ve bu gereksinimleri uygulamak için en iyi uygulamalar için [Veritas web sitesine](https://www.veritas.com)bakın.

## <a name="retention-policies"></a>Elde tutma ilkeleri

En yaygın yedekleme bekletme ilkesi türlerinden biri Büyükbaba, Baba ve Son (GFS) ilkesidir. GFS ilkesinde, her gün artımlı yedekleme gerçekleştirilir ve tam yedeklemeler haftalık ve aylık olarak yapılır. Bu ilke, altı StorSimple katmanlı ciltle sonuçlanır. Bir birim haftalık, aylık ve yıllık tam yedeklemeleri içerir. Diğer beş birim günlük artımlı yedeklemeleri depolar.

Aşağıdaki örnekte, gfs döndürme kullanırız. Örnek aşağıdakileri varsayar:

-   Deduped veya sıkıştırılmış veri kullanılır.
-   Tam yedeklemeler 1 TiB her vardır.
-   Günlük artımlı yedeklemelerin her biri 500 GiB'dir.
-   Dört haftalık yedekleme bir ay boyunca saklanır.
-   12 aylık yedekleme bir yıl boyunca saklanır.
-   Bir yıllık yedekleme 10 yıl saklanır.

Önceki varsayımlara bağlı olarak, aylık ve yıllık tam yedeklemeler için 26-TiB StorBasit katmanlı bir hacim oluşturun. Artımlı günlük yedeklemelerin her biri için 5-TiB StorBasit katmanlı bir hacim oluşturun.

| Yedekleme türü tutma | Boyut (TiB) | GFS çarpanı\* | Toplam kapasite (TiB)  |
|---|---|---|---|
| Haftalık tam | 1 | 4  | 4 |
| Günlük artımlı | 0,5 | 20 (döngüler ayda eşit sayıda hafta) | 12 (Ek kota için 2 adet) |
| Aylık tam | 1 | 12 | 12 |
| Yıllık dolu | 1  | 10 | 10 |
| GFS gereksinimi |   | 38 |   |
| Ek kota  | 4  |   | 42 toplam GFS gereksinimi  |

\*GFS çarpanı, yedekleme ilke gereksinimlerinizi karşılamak için korumanız ve saklamanız gereken kopya sayısıdır.

## <a name="set-up-backup-exec-storage"></a>Yedekleme Exec depolama sını ayarlama

### <a name="to-set-up-backup-exec-storage"></a>Yedekleme Exec depolama sını ayarlamak için

1.  Backup Exec yönetim konsolunda, **Depolama** > **Yapılandırma Depolama** > Disk tabanlı**Depolama** > **Sonraki'ni**seçin.

    ![Yedek Exec yönetim konsolu, depolama sayfasını yapılandırma](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

2.  **Disk Depolama'yı**seçin ve sonra **İleri'yi**seçin.

    ![Yedek Exec yönetim konsolu, depolama sayfasını seçin](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

3.  Temsili bir ad girin, örneğin, **Saturday Full**ve bir açıklama. **Sonraki'ni**seçin.

    ![Yedek Exec yönetim konsolu, ad ve açıklama sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

4.  Disk depolama aygıtını oluşturmak istediğiniz diski seçin ve sonra **İleri'yi**seçin.

    ![Yedek Exec yönetim konsolu, depolama diski seçim sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

5.  Yazma işlemlerinin sayısını **16'ya**artım ve sonra **İleri'yi**seçin.

    ![Yedek Exec yönetim konsolu, eşzamanlı yazma işlemleri ayarları sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

6.  Ayarları gözden geçirin ve **ardından Bitir'i**seçin.

    ![Yedek Exec yönetim konsolu, depolama yapılandırma özet sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

7.  Her toplu atamanın sonunda, depolama aygıtı ayarlarını [StorSimple ve Backup Exec için En İyi uygulamalarda önerilenlerle](#best-practices-for-storsimple-and-backup-exec)eşleşecek şekilde değiştirin.

    ![Yedek Exec yönetim konsolu, depolama aygıtı ayarları sayfası](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

8.  StorSimple birimlerinizi Backup Exec'e atamayı bitirene kadar 1-7 adımları yineleyin.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple'ı birincil yedekleme hedefi olarak ayarlama

> [!NOTE]
> Buluta katmanlı bir yedeklemeden veri geri yüklemesi bulut hızlarında gerçekleşir.

Aşağıdaki şekil, normal bir birimin yedek bir işe eşlenemesini gösterir. Bu durumda, tüm haftalık yedeklemeler Cumartesi tam diskine eşlenir ve artımlı yedeklemeler Pazartesi-Cuma artımlı disklerle eşlenir. Tüm yedeklemeler ve geri yüklemeler StorSimple katmanlı bir birimden dir.

![Birincil yedekleme hedef yapılandırma mantıksal diyagramı](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>Birincil yedekleme hedefi gfs zamanlama örneği olarak StorSimple

Aşağıda, aylık ve yıllık dört haftalık GFS rotasyon çizelgesine bir örnek verilmiştir:

| Frekans/yedekleme türü | Tam | Artımlı (gün 1-5)  |   
|---|---|---|
| Haftalık (hafta 1-4) | Cumartesi | Pazartesi-Cuma |
| Aylık  | Cumartesi  |   |
| Yıllık | Cumartesi  |   |


### <a name="assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple birimlerini Yedek Exec yedekleme işine atama

Aşağıdaki sıra, Backup Exec ve hedef ana bilgisayarın Backup Exec aracısı yönergelerine uygun olarak yapılandırıldığı varsayar.

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-backup-job"></a>StorSimple birimlerini Yedek Exec yedekleme işine atamak için

1.  Backup Exec yönetim konsolunda, Diske **Ana Bilgisayar** > Yedekleme**Yedekleme'yi****Backup** > seçin.

    ![Yedek Exec yönetim konsolu, ana bilgisayar, yedekleme ve diske yedekleme seçin](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

2.  Yedekleme **Tanım Özellikleri** iletişim kutusunda, **Yedekleme**altında, **Edit'i**seçin.

    ![Yedek Exec yönetim konsolu, Yedekleme Tanım Özellikleri iletişim kutusu](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

3.  Tam ve artımlı yedeklemelerinizi, RPO ve RTO gereksinimlerinizi karşılayacak ve Veritas'ın en iyi uygulamalarına uyacak şekilde ayarlayın.

4.  Yedekleme **Seçenekleri** iletişim kutusunda **Depolama'yı**seçin.

    ![Yedek Exec yönetim konsolu, Yedekleme Seçenekleri Depolama iletişim kutusu](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

5.  Karşılık gelen StorSimple birimlerini yedekleme zamanlamanıza atayın.

    > [!NOTE]
    > **Sıkıştırma** ve **Şifreleme türü** **Yok**olarak ayarlanır.

6.  **Doğrula'nın**altında, bu iş onay kutusunun **verilerini doğrulamayı n için** seçin. Bu seçeneğin kullanılması StorSimple katmanlamasını etkileyebilir.

    > [!NOTE]
    > Parçalanma, dizin oluşturma ve arka plan doğrulaması StorSimple katmanını olumsuz etkiler.

    ![Yedek Exec yönetim konsolu, Yedekleme Seçenekleri ayarları doğrulamak](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

7.  Gereksinimlerinizi karşılayacak yedekleme seçeneklerinizin geri kalanını ayarladığınızda, tamamlamak için **Tamam'ı** seçin.

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple'ı ikincil yedekleme hedefi olarak ayarlama

> [!NOTE]
>Buluta katmanlı bir yedeklemeden veri geri yüklemeleri bulut hızlarında gerçekleşir.

Bu modelde, geçici bir önbellek olarak hizmet vermek için bir depolama ortamınız (StorSimple dışında) olmalıdır. Örneğin, alan, giriş/çıkış (I/O) ve bant genişliğini karşılamak için gereksiz bir bağımsız disk ler dizisi (RAID) birimi kullanabilirsiniz. RAID 5, 50 ve 10'u kullanmanızı öneririz.

Aşağıdaki şekil, tipik kısa süreli bekletme yerel (sunucuya) birimleri ve uzun vadeli bekletme arşivleri birimleri gösterir. Bu senaryoda, tüm yedeklemeler yerel (sunucuya) RAID hacminde çalışır. Bu yedeklemeler düzenli olarak çoğaltılır ve arşiv hacmine arşivlenir. Kısa süreli bekletme kapasitenizi ve performans gereksinimlerinizi işleyebilir şekilde yerel (sunucuya) RAID hacmini boyutlandırmak önemlidir.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>İkincil yedekleme hedefi GFS örneği olarak StorSimple

![StorBasit ikincil yedekleme hedef mantıksal diyagramı olarak](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

Aşağıdaki tablo, yerel ve StorSimple disklerde çalışacak yedeklemelerin nasıl ayarlanacaklarını gösterir. Bireysel ve toplam kapasite gereksinimlerini içerir.

### <a name="backup-configuration-and-capacity-requirements"></a>Yedekleme yapılandırması ve kapasite gereksinimleri

| Yedekleme türü ve bekletme | Yapılandırılmış depolama | Boyut (TiB) | GFS çarpanı | Toplam\* kapasite (TiB) |
|---|---|---|---|---|
| 1. Hafta (tam ve artımlı) |Yerel disk (kısa süreli)| 1 | 1 | 1 |
| StorBasit haftalar 2-4 |StorBasit disk (uzun vadeli) | 1 | 4 | 4 |
| Aylık tam |StorBasit disk (uzun vadeli) | 1 | 12 | 12 |
| Yıllık dolu |StorBasit disk (uzun vadeli) | 1 | 1 | 1 |
|GFS hacimboyutu gereksinimi |  |  |  | 18*|

\*Toplam kapasite 17 TiB StorSimple diskve 1 TiB yerel RAID hacmi içerir.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>GFS örnek çizelgesi: GFS rotasyon haftalık, aylık ve yıllık zamanlama

| Hafta | Tam | Artımlı gün 1 | Artımlı gün 2 | Artımlı gün 3 | Artımlı gün 4 | Artımlı gün 5 |
|---|---|---|---|---|---|---|
| 1. Hafta | Yerel RAID hacmi  | Yerel RAID hacmi | Yerel RAID hacmi | Yerel RAID hacmi | Yerel RAID hacmi | Yerel RAID hacmi |
| 2. Hafta | StorBasit haftalar 2-4 |   |   |   |   |   |
| 3. Hafta | StorBasit haftalar 2-4 |   |   |   |   |   |
| 4. Hafta | StorBasit haftalar 2-4 |   |   |   |   |   |
| Aylık | StorBasit aylık |   |   |   |   |   |
| Yıllık | StorBasit yıllık  |   |   |   |   |   |


### <a name="assign-storsimple-volumes-to-a-backup-exec-archive-and-deduplication-job"></a>StorSimple birimlerini Yedekleme Exec arşivine ve çoğaltma işine atama

#### <a name="to-assign-storsimple-volumes-to-a-backup-exec-archive-and-duplication-job"></a>StorSimple birimlerini Yedekleme Exec arşivine ve çoğaltma işine atamak için

1.  Backup Exec yönetim konsolunda, StorSimple birimine arşivlemek istediğiniz işi sağ tıklatın ve ardından **Yedek Tanım Özellikleri** > **Edit'i**seçin.

    ![Yedek Exec yönetim konsolu, Yedekleme Tanım Özellikleri sekmesi](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

2.  Disk > **Düzenleme'ye**Sahne > Yineleme**ekle'yi**seçin. **Add Stage**

    ![Yedek Exec yönetim konsolu, sahne ekle](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

3.  Yinelenen **Seçenekler** iletişim kutusunda, **Kaynak** ve **Zamanlama**için kullanmak istediğiniz değerleri seçin.

    ![Yedek Exec yönetim konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekler](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

4.  **Depolama** açılır listesinde, arşiv işinin verileri depolamasını istediğiniz StorSimple birimini seçin.

    ![Yedek Exec yönetim konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekler](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

5.  Bu iş onay **kutusunun** **verilerini doğrulamayı** seçin ve ardından verileri doğrula'yı seçin.

    ![Yedek Exec yönetim konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekler](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

6.  **Tamam'ı**seçin.

    ![Yedek Exec yönetim konsolu, yedekleme tanımları özellikleri ve yinelenen seçenekler](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

7.  **Yedekleme** sütununa yeni bir aşama ekleyin. Kaynak için **artımlı**kullanın. Hedef için, artımlı yedekleme işinin arşivlendiği StorSimple birimini seçin. Adımları 1-6'yı tekrarlayın.

## <a name="storsimple-cloud-snapshots"></a>StorBasit bulut anlık görüntüleri

StorBasit bulut anlık görüntüleri, StorSimple cihazınızda bulunan verileri korur. Bulut anlık görüntüsü oluşturmak, yerel yedekleme teyplerini iş yeri dışında bir tesise göndermeye eşdeğerdir. Azure coğrafi yedekdepolama alanını kullanıyorsanız, bulut anlık görüntüsü oluşturmak, yedekleme teyplerini birden çok siteye göndermeye eşdeğerdir. Bir felaketten sonra bir aygıtı geri yüklemeniz gerekiyorsa, başka bir StorSimple aygıtını çevrimiçi duruma getirebilir ve bir hata yapabilirsiniz. Başarısız olduktan sonra, en son bulut anlık görüntüsünden verilere (bulut hızlarında) erişebilirsiniz.

Aşağıdaki bölümde, yedekleme sonrası işleme sırasında StorSimple bulut anlık görüntülerini başlatmak ve silmek için kısa bir komut dosyası nın nasıl oluşturulacak olduğu açıklanmaktadır.

> [!NOTE]
> El ile veya programlı olarak oluşturulan anlık görüntüler StorSimple anlık son kullanma ilkesini izlemez. Bu anlık görüntüler el ile veya programlı olarak silinmelidir.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Komut dosyası kullanarak bulut anlık görüntülerini başlatma ve silme

> [!NOTE]
> StorSimple anlık görüntüsünü silmeden önce uyumluluk ve veri saklama yankılarını dikkatle değerlendirin. Yedekleme sonrası komut dosyasının nasıl çalıştırılabildiğini hakkında daha fazla bilgi için [Yedek Exec belgelerine](https://www.veritas.com/support/en_US/article.100032497.html)bakın.

### <a name="backup-lifecycle"></a>Yedekleme yaşam döngüsü

![Yedekleme Yaşam Döngüsü diyagramı](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

### <a name="requirements"></a>Gereksinimler

-   Komut dosyasını çalıştıran sunucunun Azure bulut kaynaklarına erişimi olmalıdır.
-   Kullanıcı hesabı gerekli izinlere sahip olmalıdır.
-   İlişkili StorSimple birimlerine sahip bir StorSimple yedekleme ilkesi ayarlanmalıdır, ancak açık olmamalıdır.
-   StorSimple kaynak adı, kayıt anahtarı, aygıt adı ve yedekleme ilkesi kimliği gerekir.

### <a name="to-start-or-delete-a-cloud-snapshot"></a>Bulut anlık görüntüsünü başlatmak veya silmek için

1. [Azure PowerShell'i yükleyin.](/powershell/azure/overview)
2. İndirin ve kurulum [Yönet-CloudSnapshots.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Manage-CloudSnapshots.ps1) PowerShell komut dosyası.
3. Komut dosyasını çalıştıran sunucuda PowerShell'i yönetici olarak çalıştırın. Komut dosyasının ne `-WhatIf $true` gibi değişiklikler yapacağını görmek için komut dosyasını çalıştırdığınızdan emin olun. Doğrulama tamamlandıktan sonra, `-WhatIf $false`geç. Aşağıdaki komutu çalıştırın:
   ```powershell
   .\Manage-CloudSnapshots.ps1 -SubscriptionId [Subscription Id] -TenantId [Tenant ID] -ResourceGroupName [Resource Group Name] -ManagerName [StorSimple Device Manager Name] -DeviceName [device name] -BackupPolicyName [backup policyname] -RetentionInDays [Retention days] -WhatIf [$true or $false]
   ```
4. Backup Exec iş seçeneklerinizin ön işleme ve işlem sonrası komutlarını düzenleyerek komut dosyasını Yedekleme Exec'deki yedekleme işinize ekleyin.

   ![Yedek Exec konsolu, yedekleme seçenekleri, işlem öncesi ve sonrası komutlar sekmesi](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Günlük yedekleme işinizin sonunda StorSimple bulut anlık görüntü yedekleme ilkenizi işleme sonrası komut dosyası olarak çalıştırmanızı öneririz. RPO ve RTO'nuzla tanışmanıza yardımcı olmak için yedekleme uygulama ortamınızı nasıl yedekleyip geri yükleyin hakkında daha fazla bilgi için lütfen yedek mimarınıza danışın.

## <a name="storsimple-as-a-restore-source"></a>Geri yükleme kaynağı olarak StorSimple

StorSimple aygıtından geri yüklemeler, herhangi bir blok depolama aygıtından geri yükleme gibi çalışır. Buluta katmanlı verilerin geri yüklemeleri bulut hızlarında gerçekleşir. Yerel veriler için geri yüklemeler aygıtın yerel disk hızında gerçekleşir. Geri yükleme nin nasıl gerçekleştirilebildiğini öğrenmek için Yedek Exec belgelerine bakın. Backup Exec'in en iyi uygulamaları geri yüklemesine uymanızı öneririz.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple failover ve olağanüstü durum kurtarma

> [!NOTE]
> Yedek hedef senaryolar için StorSimple Cloud Appliance geri yükleme hedefi olarak desteklenmez.

Bir felaket çeşitli faktörlerden kaynaklanabilir. Aşağıdaki tabloda sık karşılaşılan olağanüstü durum kurtarma senaryoları listelendir.

| Senaryo | Etki | Nasıl kurtarılabilen | Notlar |
|---|---|---|---|
| StorBasit aygıt arızası | Yedekleme ve geri yükleme işlemleri kesilir. | Başarısız aygıtı değiştirin ve [StorSimple failover ve olağanüstü durum kurtarma gerçekleştirin.](storsimple-device-failover-disaster-recovery.md) | Aygıt kurtarma dan sonra geri yükleme gerçekleştirmeniz gerekiyorsa, buluttan yeni aygıta tam veri çalışma kümeleri alınır. Tüm işlemler bulut hızlarındadır. Dizin oluşturma ve kataloglama yeniden tarama işlemi, tüm yedekleme kümelerinin taranıp bulut katmanından yerel aygıt katmanına çekilmesine neden olabilir ve bu da zaman alan bir işlem olabilir. |
| Yedek Exec sunucu hatası | Yedekleme ve geri yükleme işlemleri kesilir. | Yedekleme sunucusunu yeniden oluştur ve veritabanı nı el [ile Yedekleme ve Yedekleme Exec (BEDB) veritabanını geri yükleme konusunda](http://www.veritas.com/docs/000041083)ayrıntılı olarak gerçekleştirin. | Olağanüstü durum kurtarma alanındaki Backup Exec sunucusunu yeniden oluşturmanız veya geri yüklemeniz gerekir. Veritabanını en son noktaya geri yükleyin. Geri yüklenen Backup Exec veritabanı en son yedekleme işlerinizle eşit değilse, dizin oluşturma ve kataloglama gereklidir. Bu dizin ve katalog yeniden tarama işlemi, tüm yedekleme kümelerinin taranıp bulut katmanından yerel aygıt katmanına çekilmesine neden olabilir. Bu daha fazla zaman yoğun hale getirir. |
| Hem yedek sunucunun hem de StorSimple'ın kaybına neden olan site hatası | Yedekleme ve geri yükleme işlemleri kesilir. | Önce StorSimple'ı geri yükleyin, sonra Yedek Exec'i geri yükleyin. | Önce StorSimple'ı geri yükleyin, sonra Yedek Exec'i geri yükleyin. Aygıt kurtarma dan sonra geri yükleme gerçekleştirmeniz gerekiyorsa, tam veri çalışma kümeleri buluttan yeni aygıta alınır. Tüm işlemler bulut hızlarındadır. |

## <a name="references"></a>Başvurular

Bu makale için aşağıdaki belgelere başvuruldu:

- [StorBasit multipath G/Ç kurulumu](storsimple-configure-mpio-windows-server.md)
- [Depolama senaryoları: İnce sağlama](https://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [GPT sürücülerini kullanma](https://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Paylaşılan klasörler için gölge kopyalarını ayarlama](https://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Sonraki adımlar

- [Yedekleme kümesinden](storsimple-restore-from-backup-set-u2.md)nasıl geri yükleyiriz hakkında daha fazla bilgi edinin.
- [Aygıt arızave olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md)işlemleri hakkında daha fazla bilgi edinin.
