---
title: SAP HANA veritabanlarının yedekleme hatalarını giderme
description: SAP HANA veritabanlarını yedeklemek için Azure Backup kullandığınızda oluşabilecek yaygın hataların nasıl giderileceği açıklanmaktadır.
ms.topic: troubleshooting
ms.date: 11/7/2019
ms.openlocfilehash: 8872cfe87df9b8d0553d777f72fe7102d08dea4d
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78382473"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Azure 'da SAP HANA veritabanlarının yedeklenmesi sorunlarını giderme

Bu makale, Azure sanal makinelerinde SAP HANA veritabanlarının yedeklenmesi için sorun giderme bilgileri sağlar. Şu anda desteklediğimiz SAP HANA yedekleme senaryoları hakkında daha fazla bilgi için bkz. [senaryo desteği](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="prerequisites-and-permissions"></a>Önkoşullar ve Izinler

Yedeklemeleri yapılandırmadan önce [önkoşullara](tutorial-backup-sap-hana-db.md#prerequisites) ve [ön kayıt betiğinin bölümler yaptığını](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) inceleyin.

## <a name="common-user-errors"></a>Ortak Kullanıcı hataları

### <a name="usererrorhanainternalrolenotpresent"></a>UserErrorHANAInternalRoleNotPresent

| **Hata Iletisi**      | <span style="font-weight:normal">Azure Backup 'ın yedeklemeyi yürütmek için gerekli rol ayrıcalıkları yok</span>    |
| ---------------------- | ------------------------------------------------------------ |
| **Olası nedenler**    | Rolün üzerine yazılmış olabilir.                          |
| **Önerilen eylem** | Sorunu çözmek için betiği **bul** bölmesinden çalıştırın veya [buradan](https://aka.ms/scriptforpermsonhana)indirin. Alternatif olarak, ' SAP_INTERNAL_HANA_SUPPORT ' rolünü Iş yükü yedekleme kullanıcısına (AZUREWLBACKUPHANAUSER) ekleyin. |

### <a name="usererrorinopeninghanaodbcconnection"></a>Usererrorınopeninghanaodbcconnection

| Hata İletisi      | <span style="font-weight:normal">HANA sistemine bağlanılamadı</span>                        |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | SAP HANA örneği çalışmıyor olabilir.<br/>Azure Backup 'ın HANA veritabanıyla etkileşim kurması için gerekli izinler ayarlanmamış. |
| **Önerilen eylem** | SAP HANA veritabanının çalışır olup olmadığını denetleyin. Veritabanı çalışır duruma kullanıyorsa, tüm gerekli izinlerin ayarlanmış olup olmadığını denetleyin. İzinlerden herhangi biri eksikse eksik izinleri eklemek için [preregistration betiğini](https://aka.ms/scriptforpermsonhana) çalıştırın. |

### <a name="usererrorhanainstancenameinvalid"></a>Usererrorhanaınstancenamegeçersiz

| Hata İletisi      | <span style="font-weight:normal">Belirtilen SAP HANA örneği geçersiz ya da bulunamıyor</span>  |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Tek bir Azure VM üzerindeki birden çok SAP HANA örneği yedeklenebilir. |
| **Önerilen eylem** | Yedeklemek istediğiniz SAP HANA örneğinde [preregistration betiğini](https://aka.ms/scriptforpermsonhana) çalıştırın. Sorun devam ederse, Microsoft destek 'e başvurun. |

### <a name="usererrorhanaunsupportedoperation"></a>UserErrorHanaUnsupportedOperation

| Hata İletisi      | <span style="font-weight:normal">Belirtilen SAP HANA işlemi desteklenmiyor</span>              |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | SAP HANA için Azure Backup, SAP HANA yerel istemcilerde (Studio/kokpit/DBA kokpiti) gerçekleştirilen artımlı yedeklemeyi ve eylemleri desteklemez |
| **Önerilen eylem** | Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)bakın. |

### <a name="usererrorhanapodoesnotsupportbackuptype"></a>Usererrorhanapoya Notsupportbackuptype

| Hata İletisi      | <span style="font-weight:normal">Bu SAP HANA veritabanı istenen yedekleme türünü desteklemiyor</span>  |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Azure Backup, anlık görüntüleri kullanarak artımlı yedekleme ve yedeklemeyi desteklemez |
| **Önerilen eylem** | Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)bakın. |

### <a name="usererrorhanalsnvalidationfailure"></a>UserErrorHANALSNValidationFailure

| Hata İletisi      | <span style="font-weight:normal">Yedekleme günlük zinciri bozuk</span>                                    |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Günlük yedekleme hedefi, backint 'ten dosya sistemine güncelleştirilmiş olabilir veya backınt yürütülebilir dosyası değiştirilmiş olabilir |
| **Önerilen eylem** | Sorunu çözmek için bir tam yedekleme tetikleyin                   |

### <a name="usererrorincomaptiblesrctargetsystsemsforrestore"></a>Usererrorıncomaptıblesrctargetsystsemsforrestore

| Hata İletisi      | <span style="font-weight:normal">Geri yükleme için kaynak ve hedef sistemler uyumsuz</span>    |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Geri yükleme için hedef sistem kaynakla uyumsuz |
| **Önerilen eylem** | Günümüzde desteklenen geri yükleme türleri hakkında bilgi edinmek için bkz. SAP Note [1642148](https://launchpad.support.sap.com/#/notes/1642148) |

### <a name="usererrorsdctomdcupgradedetected"></a>Usererrorsdctomdcupgradealgılandı

| Hata İletisi      | <span style="font-weight:normal">SDC 'den MDC 'ye yükseltme algılandı</span>                                   |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | SAP HANA örneği SDC 'den MDC 'ye yükseltildi. Güncelleştirme sonrasında yedeklemeler başarısız olur. |
| **Önerilen eylem** | Sorunu çözmek için [SAP HANA 1,0 ' den 2,0 ' ye yükseltme bölümünde](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) listelenen adımları izleyin. |

### <a name="usererrorinvalidbackintconfiguration"></a>Usererrorınvalidbackintconfiguration

| Hata İletisi      | <span style="font-weight:normal">Geçersiz backınt yapılandırması algılandı</span>                       |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Azure Backup için yedekleme parametreleri yanlış belirtildi |
| **Önerilen eylem** | Aşağıdaki (backınt) parametrelerinin ayarlanmış olup olmadığını denetleyin:<br/>\* [catalog_backup_using_backint: true]<br/>\* [enable_accumulated_catalog_backup: false]<br/>\* [parallel_data_backup_backint_channels: 1]<br/>\* [log_backup_timeout_s: 900)]<br/>\* [backint_response_timeout: 7200]<br/>KONAKTA backınt tabanlı parametreler varsa, bunları kaldırın. Parametreler, ana bilgisayar düzeyinde mevcut değilse ancak bir veritabanı düzeyinde el ile değiştirilmişse, bunları daha önce açıklandığı gibi uygun değerlere dönüştürmeniz gerekir. Ya da, [korumayı durdurun ve Azure Portal yedekleme verilerini koruyun](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#stop-protection-for-an-sap-hana-database) ve sonra **yedeklemeyi devam ettirir**' ı seçin. |

## <a name="restore-checks"></a>Geri yükleme denetimleri

### <a name="single-container-database-sdc-restore"></a>Tek kapsayıcı veritabanı (SDC) geri yükleme

HANA için tek bir kapsayıcı veritabanını (SDC) başka bir SDC makinesine geri yüklerken girişlerden yararlanın. Veritabanı adı, köşeli ayraçlar içinde "SDC" eklenmiş şekilde, küçük harfle verilmelidir. HANA örneği büyük harf olarak görüntülenecektir.

"H21" SDC HANA örneğinin yedeklenmekte olduğunu varsayalım. Yedekleme öğeleri sayfasında, yedekleme öğesi adı **"H21 (SDC)"** olarak gösterilir. Bu veritabanını başka bir hedef SDC 'ye geri yüklemeye çalışırsanız, H11 deyin ve aşağıdaki girişlerin sağlanması gerekir.

![Geri yüklenen SDC veritabanı adı](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Aşağıdaki noktalara dikkat edin:

- Varsayılan olarak, geri yüklenen veritabanı adı yedekleme öğesi adı ile doldurulur. Bu durumda, H21 (SDC).
- H11 olarak Target seçildiğinde geri yüklenen veritabanı adı otomatik olarak değişmez. **H11 (SDC) olarak düzenlenmelidir**. SDC ile ilgili olarak, geri yüklenen veritabanı adı, küçük harflerle birlikte hedef örnek KIMLIĞI ve köşeli ayraçlar içine eklenen ' SDC ' olur.
- SDC 'nin yalnızca tek bir veritabanı olduğundan, var olan veritabanı verilerinin kurtarma noktası verileriyle geçersiz kılınmasına izin vermek için onay kutusuna tıklamanız gerekir.
- Linux, büyük/küçük harfe duyarlıdır. Bu nedenle, durumu korumak için dikkatli olun.

### <a name="multiple-container-database-mdc-restore"></a>Birden çok kapsayıcı veritabanı (MDC) geri yükleme

HANA için birden çok kapsayıcı veritabanında standart yapılandırma SISTEM DB + 1 veya daha fazla kiracı DBs 'dir. SAP HANA örneğinin tamamının geri yüklenmesi hem SYSTEMDB hem de kiracı veritabanlarını geri yüklemek anlamına gelir. İlk olarak SYSTEMDB 'yi geri yükler ve ardından kiracı DB 'ye devam eder. Sistem DB temelde, seçili hedefteki sistem bilgilerini geçersiz kılmak anlamına gelir. Bu geri yükleme, hedef örnekteki BackInt ile ilgili bilgileri de geçersiz kılar. Bu nedenle, sistem DB bir hedef örneğe geri yüklendikten sonra, önceden kayıt betiğini yeniden çalıştırın. Yalnızca sonraki kiracı DB geri yüklemeleri başarılı olur.

## <a name="upgrading-from-sap-hana-10-to-20"></a>SAP HANA 1,0 ' den 2,0 ' ye yükseltme

SAP HANA 1,0 veritabanlarını koruyorsanız ve 2,0 ' ye yükseltmek istiyorsanız aşağıdaki adımları gerçekleştirin:

- Eski SDC veritabanı için verileri koruyun ile [korumayı durdurun](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) .
- Yükseltmeyi gerçekleştirin. Tamamlandıktan sonra, HANA artık sistem DB ve kiracı DB 'leri ile MDC 'dir
- [Ön kayıt betiğini](https://aka.ms/scriptforpermsonhana) (SID ve MDC) doğru ayrıntılarla yeniden çalıştırın.
- Azure portal aynı makine için uzantıyı yeniden Kaydet (yedekleme > Görünüm ayrıntıları-> ilgili Azure VM 'yi > yeniden Kaydet ' i seçin).
- Aynı VM için veritabanlarını yeniden keşfet ' e tıklayın. Bu eylem, 2. adımdaki yeni DBs 'Leri doğru ayrıntılarla (SDC değil, SYSTEMDB ve kiracı DB) göstermelidir.
- Bu yeni veritabanları için yedeklemeyi yapılandırın.

## <a name="upgrading-without-an-sid-change"></a>SID değişikliği olmadan yükseltme

Bir SID değişikliğine neden olmayan işletim sistemi veya SAP HANA yükseltmeler aşağıda belirtilen şekilde işlenebilir:

- Veritabanı için verileri tutma ile [Korumayı Durdur](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database)
- Yükseltmeyi gerçekleştirin.
- [Ön kayıt betiğini](https://aka.ms/scriptforpermsonhana)yeniden çalıştırın. Genellikle, yükseltme işlemini görtiğimiz gerekli rolleri kaldırdık. Ön kayıt betiğini çalıştırmak, tüm gerekli rolleri doğrulamaya yardımcı olur.
- Veritabanı için [korumayı](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) yeniden deneyin

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM 'lerinde SAP HANA veritabanlarının yedeklenmesi hakkında [sık sorulan soruları](https://docs.microsoft.com/azure/backup/sap-hana-faq-backup-azure-vm) gözden geçirin]
