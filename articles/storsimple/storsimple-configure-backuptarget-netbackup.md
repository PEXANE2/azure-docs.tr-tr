---
title: NetBackup ile yedek hedef olarak StorSimple 8000 serisi | Microsoft Dokümanlar
description: Veritas NetBackup ile StorSimple yedekleme hedef yapılandırmasını açıklar.
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
ms.date: 06/15/2017
ms.author: matd
ms.openlocfilehash: 957fff73f2406e0e057a7c978dd76a6bd9c156b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67876200"
---
# <a name="storsimple-as-a-backup-target-with-netbackup"></a>NetBackup ile yedek hedef olarak StorSimple

## <a name="overview"></a>Genel Bakış

Azure StorSimple, Microsoft'un karma bulut depolama çözümüdür. StorSimple, şirket içi çözümün bir uzantısı olarak bir Azure depolama hesabı kullanarak ve şirket içi depolama ve bulut depolama da verileri otomatik olarak katmanlayarak üstel veri büyümesinin karmaşıklığını giderir.

Bu makalede, Veritas NetBackup ile StorSimple tümleştirme ve her iki çözümü entegre etmek için en iyi uygulamaları tartışıyoruz. Ayrıca, Veritas NetBackup'ın StorSimple ile en iyi şekilde entegre edilebilmek için nasıl kurulacağı konusunda önerilerde bulunuyoruz. Veritas'ın en iyi uygulamalarını, yedekleme mimarlarını ve yöneticilerini, veritas NetBackup'ı tek tek yedekleme gereksinimlerini ve hizmet düzeyi anlaşmalarını (SLA'lar) karşılamak üzere ayarlamanın en iyi yolu için erteleriz.

Yapılandırma adımlarını ve anahtar kavramları gösterebilsek de, bu makale hiçbir şekilde adım adım yapılandırma veya yükleme kılavuzu değildir. Temel bileşenlerin ve altyapının çalışır durumda olduğunu ve tanımladığımız kavramları desteklemeye hazır olduğunu varsayıyoruz.

### <a name="who-should-read-this"></a>Bunu kim okumalı?

Bu makaledeki bilgiler, depolama, Windows Server 2012 R2, Ethernet, bulut hizmetleri ve Veritas NetBackup hakkında bilgisi olan yedekleme yöneticileri, depolama yöneticileri ve depolama mimarları için en yararlı olacaktır.

### <a name="supported-versions"></a>Desteklenen sürümler

-   NetBackup 7.7.x ve sonraki sürümleri
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
![StorBasit katmanlama diyagramı](./media/storsimple-configure-backup-target-using-netbackup/image1.jpg)

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

![Birincil yedekleme hedef mantıksal diyagramı olarak StorSimple](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Birincil hedef yedekleme mantıksal adımları

1.  Yedek sunucu hedef yedekleme aracısı ile bağlantı kurur ve yedek aracı verileri yedekleme sunucusuna iletir.
2.  Yedekleme sunucusu StorSimple katmanlı birimlerine veri yazar.
3.  Yedek sunucu katalog veritabanını güncelleştirir ve yedekleme işini tamamlar.
4.  Anlık görüntü komut dosyası StorSimple anlık görüntü yöneticisini tetikler (başlat veya sil).
5.  Yedekleme sunucusu, bekletme ilkesine dayalı olarak süresi dolmuş yedeklemeleri siler.

### <a name="primary-target-restore-logical-steps"></a>Birincil hedef geri yükleme mantıksal adımlar

1.  Yedekleme sunucusu depolama deposundan uygun verileri geri başlar.
2.  Yedekleme aracısı verileri yedek sunucudan alır.
3.  Yedekleme sunucusu geri yükleme işini bitirir.

## <a name="storsimple-as-a-secondary-backup-target"></a>İkincil yedekleme hedefi olarak StorSimple

Bu senaryoda, StorSimple birimleri öncelikle uzun süreli saklama veya arşivleme için kullanılır.

Aşağıdaki şekilde, ilk yedeklemelerin ve geri yüklemelerin yüksek performanslı bir hacmi hedeflediği bir mimari gösterilmektedir. Bu yedeklemeler kopyalanır ve belirli bir zamanlamada StorSimple katmanlı bir birimde arşivlenir.

Bekletme ilkesi kapasitenizi ve performans gereksinimlerinizi işleyebilir şekilde yüksek performanslı ses düzeyinizi boyutlandırmanız önemlidir.

![İkincil yedekleme hedefi mantıksal diyagramı olarak StorSimple](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>İkincil hedef yedekleme mantıksal adımları

1.  Yedek sunucu hedef yedekleme aracısı ile bağlantı kurur ve yedek aracı verileri yedekleme sunucusuna iletir.
2.  Yedekleme sunucusu yüksek performanslı depolamaya veri yazar.
3.  Yedek sunucu katalog veritabanını güncelleştirir ve yedekleme işini tamamlar.
4.  Yedekleme sunucusu yedeklemeleri bir bekletme ilkesine göre StorSimple'a kopyalar.
5.  Anlık görüntü komut dosyası StorSimple anlık görüntü yöneticisini tetikler (başlat veya sil).
6.  Yedekleme sunucusu, bekletme ilkesini temel alan süresi dolmuş yedeklemeleri siler.

### <a name="secondary-target-restore-logical-steps"></a>İkincil hedef geri yükleme mantıksal adımlar

1.  Yedekleme sunucusu depolama deposundan uygun verileri geri başlar.
2.  Yedekleme aracısı verileri yedek sunucudan alır.
3.  Yedekleme sunucusu geri yükleme işini bitirir.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu çözümü dağıtmak için üç adım gerekiyor:
1. Ağ altyapısını hazırlayın.
2. StorSimple cihazınızı yedek hedef olarak dağıtın.
3. Veritas NetBackup'ı dağıtın.

Her adım aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="set-up-the-network"></a>Ağı ayarlama

StorSimple, Azure bulutuyla entegre edilmiş bir çözüm olduğundan, StorSimple Azure bulutuna etkin ve çalışan bir bağlantı gerektirir. Bu bağlantı, bulut anlık görüntüleri, veri yönetimi ve meta veri aktarımı gibi işlemler ve Azure bulut depolamasına daha eski, daha az erişilen verileri katmanlamak için kullanılır.

Çözümün en iyi şekilde çalışması için, aşağıdaki ağ en iyi uygulamalarını izlemenizi öneririz:

-   StorSimple katmanlamasını Azure'a bağlayan bağlantı, bant genişliği gereksinimlerinizi karşılamalıdır. Bunu başarmak için, RPO ve kurtarma süresi hedefi (RTO) SLA'larınıza uyacak şekilde altyapı anahtarlarınıza uygun Hizmet Kalitesi (QoS) düzeyini uygulayın.

-   Maksimum Azure Blob depolama erişim gecikmeleri yaklaşık 80 ms olmalıdır.

### <a name="deploy-storsimple"></a>StorSimple'ı dağıt

Adım adım StorSimple dağıtım kılavuzu için, [şirket içi StorSimple aygıtınızı dağıtın'](storsimple-deployment-walkthrough-u2.md)a bakın.

### <a name="deploy-netbackup"></a>NetBackup Dağıt

Adım adım NetBackup 7.7.x dağıtım kılavuzu için [NetBackup 7.7.x belgelerine](http://www.veritas.com/docs/000094423)bakın.

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

- Yayılan birimleri kullanmayın (Windows Disk Yönetimi tarafından oluşturulmuş); yayılan birimler desteklenmez.
- 64-KB ayırma boyutu ile NTFS kullanarak birimlerinizi biçimlendirin.
- StorSimple birimlerini doğrudan NetBackup sunucusuna eşleyin.
    - Fiziksel sunucular için iSCSI'yi kullanın.
    - Sanal sunucular için geçiş disklerini kullanın.


## <a name="best-practices-for-storsimple-and-netbackup"></a>StorSimple ve NetBackup için en iyi uygulamalar

Çözümünüzü aşağıdaki birkaç bölümdeki yönergelere göre ayarlayın.

### <a name="operating-system-best-practices"></a>İşletim sistemi en iyi uygulamalar

- NTFS dosya sistemi için Windows Server şifrelemesini ve çoğaltmayı devre dışı kaldırın.
- StorSimple birimlerindeki Windows Server parçalanmasını devre dışı bırakma.
- StorSimple birimlerinde Windows Server dizini sinir'i devre dışı kaldırın.
- Kaynak ana bilgisayarda virüsten koruma taramayı çalıştırın (StorSimple birimlerine karşı değil).
- Görev Yöneticisi'ndeki varsayılan [Windows Server bakımını](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) kapatın. Bunu aşağıdaki yollardan biriyle yapın:
  - Windows Görev Zamanlayıcısı'ndaki Bakım yapılandırmacısını kapatın.
  - Windows Sysinternals'den [PsExec'i](https://technet.microsoft.com/sysinternals/bb897553.aspx) indirin. PsExec'i indirdikten sonra Windows PowerShell'i yönetici olarak çalıştırın ve yazın:
    ```powershell
    psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
    ```

### <a name="storsimple-best-practices"></a>StorBasit en iyi uygulamalar

-   StorSimple aygıtının Güncelleştirme 3 [veya daha sonra](storsimple-install-update-3.md)güncelleştirildiğinden emin olun.
-   iSCSI ve bulut trafiğini yalıtın. StorSimple ile yedek sunucu arasındaki trafik için özel iSCSI bağlantılarını kullanın.
-   StorSimple cihazınızın özel bir yedekleme hedefi olduğundan emin olun. RTO ve RPO'nuzu etkilediği için karışık iş yükleri desteklenmez.

### <a name="netbackup-best-practices"></a>NetBackup en iyi uygulamaları

-   NetBackup veritabanı sunucuya yerel olmalı ve StorSimple biriminde yer almamalıdır.
-   Olağanüstü durum kurtarma için, Bir StorSimple biriminde NetBackup veritabanını yedekleyin.
-   Bu çözüm için NetBackup'ın tam ve artımlı yedeklemelerini (NetBackup'da diferansiyel artımlı yedeklemeolarak da adlandırılır) destekliyoruz. Sentetik ve kümülatif artımlı yedeklemeler kullanmamanızı öneririz.
-   Yedekleme veri dosyaları yalnızca belirli bir iş için veri içermelidir. Örneğin, farklı işlerde hiçbir ortam ekine izin verilmez.

En son NetBackup ayarları ve bu gereksinimleri uygulamak için en iyi uygulamalar için [www.veritas.com'daki](https://www.veritas.com)NetBackup belgelerine bakın.


## <a name="retention-policies"></a>Elde tutma ilkeleri

En yaygın yedekleme bekletme ilkesi türlerinden biri Büyükbaba, Baba ve Son (GFS) ilkesidir. GFS ilkesinde, her gün artımlı yedekleme gerçekleştirilir ve tam yedeklemeler haftalık ve aylık olarak yapılır. Bu ilke, storSimple katmanlı altı birimle sonuçlanır: bir birim haftalık, aylık ve yıllık tam yedeklemeleri içerir; diğer beş birim günlük artımlı yedeklemeleri depolar.

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

## <a name="set-up-netbackup-storage"></a>NetBackup depolamasını ayarlama

### <a name="to-set-up-netbackup-storage"></a>NetBackup depolamasını ayarlamak için

1.  NetBackup Yönetim Konsolunda, **Medya ve Aygıt Yönetim** > **Aygıtları** > **Disk Havuzları'nı**seçin. Disk Havuzu Yapılandırma Sihirbazı'nda, depolama sunucusu türü **AdvancedDisk'i**seçin ve ardından **İleri'yi**seçin.

    ![NetBackup Yönetim Konsolu, Disk Havuzu Yapılandırma Sihirbazı](./media/storsimple-configure-backup-target-using-netbackup/nbimage1.png)

2.  Sunucunuzu seçin ve sonra **İleri'yi**seçin.

    ![NetBackup Yönetim Konsolu, sunucu seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage2.png)

3.  StorSimple ses inizi seçin.

    ![NetBackup Yönetim Konsolu, StorSimple birim diskini seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage3.png)

4.  Yedekleme hedefi için bir ad girin ve sihirbazı bitirmek için > **İleri'yi** seçin. **Next**

5.  Ayarları gözden geçirin ve **ardından Bitir'i**seçin.

6.  Her toplu atamanın sonunda, depolama aygıtı ayarlarını [StorSimple ve NetBackup için en iyi uygulamalarda önerilenlerle](#best-practices-for-storsimple-and-netbackup)eşleşecek şekilde değiştirin.

7. StorSimple birimlerinizi atamanız bitene kadar 1-6 adımlarını yineleyin.

    ![NetBackup Yönetim Konsolu, disk yapılandırması](./media/storsimple-configure-backup-target-using-netbackup/nbimage5.png)

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>StorSimple'ı birincil yedekleme hedefi olarak ayarlama

> [!NOTE]
> Buluta katmanlı bir yedeklemeden veri geri yüklemeleri bulut hızlarında gerçekleşir.

Aşağıdaki şekil, normal bir birimin yedek bir işe eşlenemesini gösterir. Bu durumda, tüm haftalık yedeklemeler Cumartesi tam diskine eşlenir ve artımlı yedeklemeler Pazartesi-Cuma artımlı disklerle eşlenir. Tüm yedeklemeler ve geri yüklemeler StorSimple katmanlı bir birimden dir.

![Birincil yedekleme hedef yapılandırma mantıksal diyagramı](./media/storsimple-configure-backup-target-using-netbackup/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>Birincil yedekleme hedefi gfs zamanlama örneği olarak StorSimple

Aşağıda, aylık ve yıllık dört haftalık GFS rotasyon çizelgesine bir örnek verilmiştir:

| Frekans/yedekleme türü | Tam | Artımlı (gün 1-5)  |   
|---|---|---|
| Haftalık (hafta 1-4) | Cumartesi | Pazartesi-Cuma |
| Aylık  | Cumartesi  |   |
| Yıllık | Cumartesi  |   |

## <a name="assigning-storsimple-volumes-to-a-netbackup-backup-job"></a>Bir NetBackup yedekleme işine StorSimple birimlerini atama

Aşağıdaki sıra, NetBackup ve hedef ana bilgisayarın NetBackup aracı yönergelerine uygun olarak yapılandırıldığı varsayar.

### <a name="to-assign-storsimple-volumes-to-a-netbackup-backup-job"></a>Bir NetBackup yedekleme işine StorSimple birimleri atamak için

1. NetBackup Yönetim Konsolunda **NetBackup Management'ı**, **Sağ**tıklatıla Politikalar'ı ve ardından **Yeni İlke'yi**seçin.

   ![NetBackup Yönetim Konsolu, yeni bir ilke oluşturmak](./media/storsimple-configure-backup-target-using-netbackup/nbimage6.png)

2. Yeni **İlke Ekle** iletişim kutusunda, ilke için bir ad girin ve ardından **İlke Yapılandırma Sihirbazı'nı kullan** onay kutusunu seçin. **Tamam'ı**seçin.

   ![NetBackup Yönetim Konsolu, Yeni İlke iletişim kutusu ekle](./media/storsimple-configure-backup-target-using-netbackup/nbimage7.png)

3. Yedekleme İlkesi Yapılandırma Sihirbazı'nda, istediğiniz yedekleme türünü seçin ve sonra **İleri'yi**seçin.

   ![NetBackup Yönetim Konsolu, yedek türünü seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage8.png)

4. İlke türünü ayarlamak için **Standart'ı**seçin ve sonra **İleri'yi**seçin.

   ![NetBackup Yönetim Konsolu, politika türünü seçin](./media/storsimple-configure-backup-target-using-netbackup/nbimage9.png)

5. Ana bilgisayarını seçin, **istemci işletim sistemi onay** kutusunu algıla'yı seçin ve sonra **Ekle'yi**seçin. **Sonraki'ni**seçin.

   ![NetBackup Yönetim Konsolu, yeni bir ilke istemcileri listeleyin](./media/storsimple-configure-backup-target-using-netbackup/nbimage10.png)

6. Yedeklemek istediğiniz sürücüleri seçin.

   ![NetBackup Yönetim Konsolu, yeni bir ilke için yedekleme seçimleri](./media/storsimple-configure-backup-target-using-netbackup/nbimage11.png)

7. Yedekleme döndürme gereksinimlerinizi karşılayan sıklık ve bekletme değerlerini seçin.

   ![NetBackup Yönetim Konsolu, yeni bir ilke için yedekleme frekansı ve döndürme](./media/storsimple-configure-backup-target-using-netbackup/nbimage12.png)

8. **Sonraki** > **Sonraki** > **Bitiş'i**seçin.  İlke oluşturulduktan sonra zamanlamayı değiştirebilirsiniz.

9. Oluşturduğunuz ilkeyi genişletmek için seçin ve ardından **Zamanlamalar'ı**seçin.

   ![NetBackup Yönetim Konsolu, yeni bir ilke için zamanlamaları](./media/storsimple-configure-backup-target-using-netbackup/nbimage13.png)

10. **Differential-Inc'e**sağ tıklayın, **yeniye Kopyala'yı**seçin ve ardından **Tamam'ı**seçin.

    ![NetBackup Yönetim Konsolu, yeni bir ilke kopyalama programı](./media/storsimple-configure-backup-target-using-netbackup/nbimage14.png)

11. Yeni oluşturulan zamanlamayı sağ tıklatın ve sonra **Değiştir'i**seçin.

12. **Öznitelikler** sekmesinde, **Geçersiz Kılma ilkesi depolama birimi seçim** onay kutusunu seçin ve ardından Pazartesi artımlı yedeklemelerin gittiği birimi seçin.

    ![NetBackup Yönetim Konsolu, değişiklik programı](./media/storsimple-configure-backup-target-using-netbackup/nbimage15.png)

13. **Pencereyi Başlat** sekmesinde, yedeklemeleriniz için zaman penceresini seçin.

    ![NetBackup Yönetim Konsolu, başlangıç pencereni değiştir](./media/storsimple-configure-backup-target-using-netbackup/nbimage16.png)

14. **Tamam'ı**seçin.

15. Her artımlı yedekleme için 10-14 adımlarını yineleyin. Oluşturduğunuz her yedekleme için uygun birim ve zamanlamayı seçin.

16. **Diferansiyel-Inc** çizelgesine sağ tıklayın ve silin.

17. Yedekleme ihtiyaçlarınızı karşılamak için Tam zamanlamanızı değiştirin.

    ![NetBackup Yönetim Konsolu, tam tarifesi değiştir](./media/storsimple-configure-backup-target-using-netbackup/nbimage17.png)

18. Başlangıç penceresini değiştirin.

    ![NetBackup Yönetim Konsolu, başlangıç penceresini değiştirin](./media/storsimple-configure-backup-target-using-netbackup/nbimage18.png)

19. Son zamanlama şu şekilde görünür:

    ![NetBackup Yönetim Konsolu, son program](./media/storsimple-configure-backup-target-using-netbackup/nbimage19.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>StorSimple'ı ikincil yedekleme hedefi olarak ayarlama

> [!NOTE]
>Buluta katmanlı bir yedeklemeden veri geri yüklemeleri bulut hızlarında gerçekleşir.

Bu modelde, geçici bir önbellek olarak hizmet vermek için bir depolama ortamınız (StorSimple dışında) olmalıdır. Örneğin, alan, giriş/çıkış (I/O) ve bant genişliğini karşılamak için gereksiz bir bağımsız disk ler dizisi (RAID) birimi kullanabilirsiniz. RAID 5, 50 ve 10'u kullanmanızı öneririz.

Aşağıdaki şekil, tipik kısa süreli bekletme yerel (sunucuya) birimleri ve uzun vadeli bekletme arşivleri birimleri gösterir. Bu senaryoda, tüm yedeklemeler yerel (sunucuya) RAID hacminde çalışır. Bu yedeklemeler düzenli olarak çoğaltılır ve arşiv hacmine arşivlenir. Kısa süreli bekletme kapasitenizi ve performans gereksinimlerinizi işleyebilir şekilde yerel (sunucuya) RAID hacmini boyutlandırmak önemlidir.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>İkincil yedekleme hedefi GFS örneği olarak StorSimple

![İkincil yedekleme hedefi mantıksal diyagramı olarak StorSimple](./media/storsimple-configure-backup-target-using-netbackup/secondarybackuptargetdiagram.png)

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


## <a name="assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorBasit birimleri NetBackup arşivine ve çoğaltma işine atama

NetBackup depolama ve medya yönetimi için çok çeşitli seçenekler sunduğundan, depolama yaşam döngüsü ilkesi (SLP) gereksinimlerinizi doğru bir şekilde değerlendirmek için Veritas'a veya NetBackup mimarınıza danışmanızı öneririz.

İlk disk havuzlarını tanımladıktan sonra, toplam dört ilke için üç ek depolama yaşam döngüsü ilkeleri tanımlamanız gerekir:
* LocalRAIDVolume
* StorSimpleWeek2-4
* StorSimpleMonthlyFulls
* StorSimpleYearlyFulls

### <a name="to-assign-storsimple-volumes-to-a-netbackup-archive-and-duplication-job"></a>StorSimple birimlerini NetBackup arşivine ve çoğaltma işine atamak için

1. NetBackup Yönetim Konsolunda, **Depolama** > **Depolama Yaşam Döngüsü İlkeleri** > **Yeni Depolama Yaşam Döngüsü İlkesi'ni**seçin.

   ![NetBackup Yönetim Konsolu, yeni depolama yaşam döngüsü ilkesi](./media/storsimple-configure-backup-target-using-netbackup/nbimage20.png)

2. Anlık görüntü için bir ad girin ve sonra **Ekle'yi**seçin.

3. Yeni **İşlem** iletişim kutusunda, **Özellikler** sekmesinde, **Operasyon**için **Yedek'i**seçin. **Hedef depolama,** **bekletme türü**ve **Bekletme dönemi**için istediğiniz değerleri seçin. **Tamam'ı**seçin.

   ![NetBackup Yönetim Konsolu, Yeni İşlem iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage22.png)

   Bu, ilk yedekleme işlemini ve deposunu tanımlar.

4. Önceki işlemi vurgulamak için seçin ve sonra **Ekle'yi**seçin. Depolama **İşlemini Değiştir** iletişim kutusunda, Hedef **depolama,** **Bekletme türü**ve **Bekletme süresi**için istediğiniz değerleri seçin.

   ![NetBackup Yönetim Konsolu,Depolama İşlemini Değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage23.png)

5. Önceki işlemi vurgulamak için seçin ve sonra **Ekle'yi**seçin. Yeni **Depolama Yaşam Döngüsü İlkesi** iletişim kutusunda, bir yıl boyunca aylık yedeklemeler ekleyin.

   ![NetBackup Yönetim Konsolu, Yeni Depolama Yaşam Döngüsü İlkesi iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage24.png)

6. İhtiyacınız olan kapsamlı SLP bekletme ilkesini oluşturana kadar adımları 4-5'i yineleyin.

   ![NetBackup Yönetim Konsolu, Yeni Depolama Yaşam Döngüsü İlkesi iletişim kutusunda ilkeler ekleme](./media/storsimple-configure-backup-target-using-netbackup/nbimage25.png)

7. **SLP**bekletme ilkenizi tanımlamayı bitirdiğinizde, İlke altında, [StorSimple birimlerini NetBackup yedekleme işine atamada](#assigning-storsimple-volumes-to-a-netbackup-backup-job)ayrıntılı adımları izleyerek bir yedekleme ilkesi tanımlayın.

8. **Zamanlamalar**altında, **Zamanlamayı Değiştir** iletişim kutusunda, **Tam'ı**sağ tıklatın ve sonra **Değiştir'i**seçin.

   ![NetBackup Yönetim Konsolu, Zamanlama değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage26.png)

9. Geçersiz **kılma ilkesi depolama alanı seçim** kutusunu seçin ve ardından 1-6 adımlarında oluşturduğunuz SLP bekletme ilkesini seçin.

   ![NetBackup Yönetim Konsolu, ilke depolama seçimini geçersiz kıl](./media/storsimple-configure-backup-target-using-netbackup/nbimage27.png)

10. **Tamam'ı**seçin ve artımlı yedekleme zamanlaması için yineleyin.

    ![NetBackup Yönetim Konsolu, Artımlı yedeklemeler için Zamanlama değiştir iletişim kutusu](./media/storsimple-configure-backup-target-using-netbackup/nbimage28.png)


| Yedekleme türü tutma | Boyut (TiB) | GFS çarpanı\* | Toplam kapasite (TiB)  |
|---|---|---|---|
| Haftalık tam |  1  |  4 | 4  |
| Günlük artımlı  | 0,5  | 20 (döngüler aylık hafta sayısına eşittir) | 12 (Ek kota için 2 adet) |
| Aylık tam  | 1 | 12 | 12 |
| Yıllık dolu | 1  | 10 | 10 |
| GFS gereksinimi  |     |     | 38 |
| Ek kota  | 4  |    | 42 toplam GFS gereksinimi |

\*GFS çarpanı, yedekleme ilke gereksinimlerinizi karşılamak için korumanız ve saklamanız gereken kopya sayısıdır.

## <a name="storsimple-cloud-snapshots"></a>StorBasit bulut anlık görüntüleri

StorBasit bulut anlık görüntüleri, StorSimple cihazınızda bulunan verileri korur. Bulut anlık görüntüsü oluşturmak, yerel yedekleme teyplerini iş yeri dışında bir tesise göndermeye eşdeğerdir. Azure coğrafi yedekdepolama alanını kullanıyorsanız, bulut anlık görüntüsü oluşturmak, yedekleme teyplerini birden çok siteye göndermeye eşdeğerdir. Bir felaketten sonra bir aygıtı geri yüklemeniz gerekiyorsa, başka bir StorSimple aygıtını çevrimiçi duruma getirebilir ve bir hata yapabilirsiniz. Başarısız olduktan sonra, en son bulut anlık görüntüsünden verilere (bulut hızlarında) erişebilirsiniz.

Aşağıdaki bölümde, yedekleme sonrası işleme sırasında StorSimple bulut anlık görüntülerini başlatmak ve silmek için kısa bir komut dosyası nın nasıl oluşturulacak olduğu açıklanmaktadır.

> [!NOTE]
> El ile veya programlı olarak oluşturulan anlık görüntüler StorSimple anlık son kullanma ilkesini izlemez. Bu anlık görüntüler el ile veya programlı olarak silinmelidir.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Komut dosyası kullanarak bulut anlık görüntülerini başlatma ve silme

> [!NOTE]
> StorSimple anlık görüntüsünü silmeden önce uyumluluk ve veri saklama yankılarını dikkatle değerlendirin. Yedekleme sonrası komut dosyasının nasıl çalıştırılabildiğini hakkında daha fazla bilgi için [NetBackup belgelerine](http://www.veritas.com/docs/000094423)bakın.

### <a name="backup-lifecycle"></a>Yedekleme yaşam döngüsü

![Yedekleme Yaşam Döngüsü diyagramı](./media/storsimple-configure-backup-target-using-netbackup/backuplifecycle.png)

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
4. NetBackup'daki yedekleme işinize komut dosyasını ekleyin. Bunu yapmak için NetBackup iş seçeneklerinizin ön işleme ve işlem sonrası komutlarını edin.

> [!NOTE]
> Günlük yedekleme işinizin sonunda StorSimple bulut anlık görüntü yedekleme ilkenizi işleme sonrası komut dosyası olarak çalıştırmanızı öneririz. RPO ve RTO'nuzla tanışmanıza yardımcı olmak için yedekleme uygulama ortamınızı nasıl yedekleyip geri yükleyin hakkında daha fazla bilgi için lütfen yedek mimarınıza danışın.

## <a name="storsimple-as-a-restore-source"></a>Geri yükleme kaynağı olarak StorSimple

StorSimple aygıtından geri yüklemeler, herhangi bir blok depolama aygıtından geri yükleme gibi çalışır. Buluta katmanlı verilerin geri yüklemeleri bulut hızlarında gerçekleşir. Yerel veriler için geri yüklemeler aygıtın yerel disk hızında gerçekleşir. Geri yükleme nin nasıl gerçekleştirilebildiğini öğrenmek için [NetBackup belgelerine](http://www.veritas.com/docs/000094423)bakın. NetBackup geri yükleme en iyi uygulamalara uymanızı öneririz.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple failover ve olağanüstü durum kurtarma

> [!NOTE]
> Yedek hedef senaryolar için StorSimple Cloud Appliance geri yükleme hedefi olarak desteklenmez.

Bir felaket çeşitli faktörlerden kaynaklanabilir. Aşağıdaki tabloda sık karşılaşılan olağanüstü durum kurtarma senaryoları listelendir.

| Senaryo | Etki | Nasıl kurtarılabilen | Notlar |
|---|---|---|---|
| StorBasit aygıt arızası | Yedekleme ve geri yükleme işlemleri kesilir. | Başarısız aygıtı değiştirin ve [StorSimple failover ve olağanüstü durum kurtarma gerçekleştirin.](storsimple-device-failover-disaster-recovery.md) | Aygıt kurtarma dan sonra geri yükleme gerçekleştirmeniz gerekiyorsa, buluttan yeni aygıta tam veri çalışma kümeleri alınır. Tüm işlemler bulut hızlarındadır. Dizin ve katalog yeniden tarama işlemi, tüm yedekleme kümelerinin taranıp bulut katmanından yerel aygıt katmanına çekilmesine neden olabilir ve bu da zaman alan bir işlem olabilir. |
| NetBackup sunucu hatası | Yedekleme ve geri yükleme işlemleri kesilir. | Yedekleme sunucusunu yeniden oluştur ve veritabanı geri yüklemegerçekleştirin. | Olağanüstü durum kurtarma alanındaki NetBackup sunucusunu yeniden oluşturmanız veya geri yüklemeniz gerekir. Veritabanını en son noktaya geri yükleyin. Geri yüklenen NetBackup veritabanı en son yedekleme işlerinizle eşit değilse, dizin oluşturma ve kataloglama gereklidir. Bu dizin ve katalog yeniden tarama işlemi, tüm yedekleme kümelerinin taranıp bulut katmanından yerel aygıt katmanına çekilmesine neden olabilir. Bu daha fazla zaman yoğun hale getirir. |
| Hem yedek sunucunun hem de StorSimple'ın kaybına neden olan site hatası | Yedekleme ve geri yükleme işlemleri kesilir. | Önce StorSimple'ı geri yükleyin, sonra NetBackup'ı geri yükleyin. | Önce StorSimple'ı geri yükleyin, sonra NetBackup'ı geri yükleyin. Aygıt kurtarma dan sonra geri yükleme gerçekleştirmeniz gerekiyorsa, tam veri çalışma kümeleri buluttan yeni aygıta alınır. Tüm işlemler bulut hızlarındadır. |

## <a name="references"></a>Başvurular

Bu makale için aşağıdaki belgelere başvuruldu:

- [StorBasit multipath G/Ç kurulumu](storsimple-configure-mpio-windows-server.md)
- [Depolama senaryoları: İnce sağlama](https://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [GPT sürücülerini kullanma](https://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Paylaşılan klasörler için gölge kopyalarını ayarlama](https://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Sonraki adımlar

- [Yedekleme kümesinden](storsimple-restore-from-backup-set-u2.md)nasıl geri yükleyiriz hakkında daha fazla bilgi edinin.
- [Aygıt arızave olağanüstü durum kurtarma](storsimple-device-failover-disaster-recovery.md)işlemleri hakkında daha fazla bilgi edinin.
