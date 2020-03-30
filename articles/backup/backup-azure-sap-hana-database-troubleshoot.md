---
title: Sorun giderme SAP HANA veritabanları yedekleme hataları
description: SAP HANA veritabanlarını yedeklemek için Azure Yedekleme'yi kullandığınızda oluşabilecek sık karşılaşılan hataları nasıl gidereceklerini açıklar.
ms.topic: troubleshooting
ms.date: 11/7/2019
ms.openlocfilehash: 6520f106011b632da2725f456aeb278c7748ddc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79459319"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Azure'daki SAP HANA veritabanlarının yedekleme sorunu

Bu makalede, Azure sanal makinelerde SAP HANA veritabanlarını yedeklemek için sorun giderme bilgileri sağlanmaktadır. Şu anda desteklediğimiz SAP HANA yedekleme senaryoları hakkında daha fazla bilgi için [Senaryo desteğine](sap-hana-backup-support-matrix.md#scenario-support)bakın.

## <a name="prerequisites-and-permissions"></a>Ön koşullar ve İzinler

Ön [koşullara](tutorial-backup-sap-hana-db.md#prerequisites) ve [ön kayıt komut dosyasının](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) yedeklemeleri yapılandırmadan önce bölümleri ne yaptığına bakın.

## <a name="common-user-errors"></a>Sık karşılaşılan kullanıcı hataları

### <a name="usererrorhanainternalrolenotpresent"></a>UserErrorHANAInternalRoleNotPresent

| **Hata İletisi**      | <span style="font-weight:normal">Azure yedeklemesi yedekleme gerçekleştirmek için gerekli rol ayrıcalıklarına sahip değildir</span>    |
| ---------------------- | ------------------------------------------------------------ |
| **Olası nedenler**    | Rol üzerine yazılmış olabilir.                          |
| **Önerilen eylem** | Sorunu gidermek için komut dosyasını **DB'yi Keşfedin** bölmesinden çalıştırın veya [buradan](https://aka.ms/scriptforpermsonhana)indirin. Alternatif olarak, İş Yükü Yedekleme Kullanıcısına (AZUREWLBACKUPHANAUSER) 'SAP_INTERNAL_HANA_SUPPORT' rolünü ekleyin. |

### <a name="usererrorinopeninghanaodbcconnection"></a>KullanıcıHatasıAçılışHanaOdbcBağlantı

| Hata İletisi      | <span style="font-weight:normal">HANA sistemine bağlanılamamış</span>                        |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | SAP HANA örneği aşağı olabilir.<br/>Azure yedeklemesinin HANA veritabanıyla etkileşimde olması için gerekli izinler ayarlı değildir. |
| **Önerilen eylem** | SAP HANA veritabanının açIlip dolmadığını kontrol edin. Veritabanı çalışır durumdaysa, gerekli tüm izinlerin ayarlanıp ayarlanıp ayarlolmadığını denetleyin. İzinlerden herhangi biri eksikse, eksik izinleri eklemek için [ön kayıt komut dosyasını](https://aka.ms/scriptforpermsonhana) çalıştırın. |

### <a name="usererrorhanainstancenameinvalid"></a>KullanıcıHatasıHanaInstanceNameGeçersiz

| Hata İletisi      | <span style="font-weight:normal">Belirtilen SAP HANA örneği geçersiz dir veya bulunamaz</span>  |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Tek bir Azure VM'deki birden çok SAP HANA örneği yedeklenemez. |
| **Önerilen eylem** | Yedeklemek istediğiniz SAP HANA örneğinde [ön kayıt komut dosyasını](https://aka.ms/scriptforpermsonhana) çalıştırın. Sorun devam ediyorsa, Microsoft desteğine başvurun. |

### <a name="usererrorhanaunsupportedoperation"></a>UserErrorHanaUnsupportedOperation

| Hata İletisi      | <span style="font-weight:normal">Belirtilen SAP HANA işlemi desteklenmiyor</span>              |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | SAP HANA için Azure yedeklemesi, SAP HANA yerel istemcilerinde (Studio/ Cockpit/ DBA Cockpit) gerçekleştirilen artımlı yedeklemeyi ve eylemleri desteklemez |
| **Önerilen eylem** | Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)bakın. |

### <a name="usererrorhanapodoesnotsupportbackuptype"></a>UserErrorHANAPODoesNotSupportBackupType

| Hata İletisi      | <span style="font-weight:normal">Bu SAP HANA veritabanı istenen yedekleme türünü desteklemiyor</span>  |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Azure yedekleme, anlık görüntüleri kullanarak artımlı yedekleme ve yedeklemeyi desteklemez |
| **Önerilen eylem** | Daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)bakın. |

### <a name="usererrorhanalsnvalidationfailure"></a>KullanıcıHatasıHANALSNValidationBaşarısızlık

| Hata İletisi      | <span style="font-weight:normal">Yedek günlük zinciri kırıldı</span>                                    |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Günlük yedekleme hedefi yedekten dosya sistemine güncelleştirilmiş veya geri uygulanabilir çalıştırılabilir değiştirilmiş olabilir |
| **Önerilen eylem** | Sorunu gidermek için tam bir yedekleme tetikleme                   |

### <a name="usererrorincomaptiblesrctargetsystsemsforrestore"></a>KullanıcıHatasıComaptibleSrcTargetSystsemsForRestore

| Hata İletisi      | <span style="font-weight:normal">Geri yükleme için kaynak ve hedef sistemleri uyumsuz</span>    |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Geri yükleme için hedef sistem kaynakla uyumsuzdur |
| **Önerilen eylem** | Bugün desteklenen geri yükleme türleri hakkında bilgi edinmek için SAP Note [1642148'e](https://launchpad.support.sap.com/#/notes/1642148) bakın |

### <a name="usererrorsdctomdcupgradedetected"></a>KullanıcıHatasıSDCtoMDCUpgradeDetected

| Hata İletisi      | <span style="font-weight:normal">SDC-MDC yükseltmesi algılandı</span>                                   |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | SAP HANA örneği SDC'den MDC'ye yükseltildi. Yedeklemeler güncelleştirmeden sonra başarısız olur. |
| **Önerilen eylem** | Sorunu çözmek için [SAP HANA 1.0'dan 2.0'a yükseltme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) bölümünde listelenen adımları izleyin |

### <a name="usererrorinvalidbackintconfiguration"></a>KullanıcıHatasıInvalidBackintConfiguration

| Hata İletisi      | <span style="font-weight:normal">Algılanan geçersiz backint yapılandırması</span>                       |
| ------------------ | ------------------------------------------------------------ |
| **Olası nedenler**    | Azure yedeklemesi için destek parametreleri yanlış belirtilir |
| **Önerilen eylem** | Aşağıdaki (backint) parametrelerin inip ayarlılamadığını kontrol edin:<br/>\*[catalog_backup_using_backint:true]<br/>\*[enable_accumulated_catalog_backup:yanlış]<br/>\*[parallel_data_backup_backint_channels:1]<br/>\*[log_backup_timeout_s:900)]<br/>\*[backint_response_timeout:7200]<br/>HOST'ta backint tabanlı parametreler varsa, bunları kaldırın. Parametreler HOST düzeyinde mevcut değilse ancak veritabanı düzeyinde el ile değiştirildiyse, bunları daha önce açıklandığı gibi uygun değerlere geri getirin. Veya, [durdurma koruması](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#stop-protection-for-an-sap-hana-database) çalıştırın ve Azure portalındaki yedekleme verilerini koruyun ve ardından **Yedeklemeye Devam'ı**seçin. |

## <a name="restore-checks"></a>Denetimleri geri yükleme

### <a name="single-container-database-sdc-restore"></a>Tek Kapsayıcı Veritabanı (SDC) geri yükleme

HANA için tek bir kapsayıcı veritabanını (SDC) başka bir SDC makinesine geri getirirken girişlerin icabına bak. Veritabanı adı küçük harfli ve parantez içinde "sdc" eklenen verilmelidir. HANA örneği büyük harflerle görüntülenir.

Bir SDC HANA örneği "H21" yedeklenir varsayalım. Yedek öğeler sayfası yedek madde adını **"h21(sdc)"** olarak gösterir. Bu veritabanını başka bir hedef SDC'ye geri yüklemeye çalışırsanız, H11 deyin, ardından aşağıdaki girişlerin sağlanması gerekir.

![Geri yüklenen SDC veritabanı adı](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Aşağıdaki noktalara dikkat edin:

- Varsayılan olarak, geri yüklenen db adı yedek madde adı ile doldurulur. Bu durumda, h21(sdc).
- Hedefi H11 olarak seçmek, geri yüklenen db adını otomatik olarak değiştirmez. **H11(sdc) olarak düzenlenmelidir.** SDC ile ilgili olarak, geri yüklenen db adı küçük harflerve 'sdc' parantez içinde eklenen hedef örnek kimliği olacaktır.
- SDC yalnızca tek bir veritabanına sahip olabileceğinden, kurtarma noktası verileriyle varolan veritabanı verilerinin geçersiz kılınmasını sağlamak için onay kutusunu da tıklatmanız gerekir.
- Linux büyük/küçük harf duyarlıdır. Bu yüzden davayı korumaya dikkat et.

### <a name="multiple-container-database-mdc-restore"></a>Çoklu Kapsayıcı Veritabanı (MDC) geri yükleme

HANA için birden çok kapsayıcı veritabanlarında, standart yapılandırma SYSTEMDB + 1 veya daha fazla Kiracı DBs olduğunu. Tüm SAP HANA örneğini geri yüklemek, hem SYSTEMDB'yi hem de Kiracı DB'lerini geri yüklemek anlamına gelir. Bir SYSTEMDB ilk geri yüklenir ve daha sonra Kiracı DB için devam eder. Sistem DB aslında seçilen hedef üzerinde sistem bilgilerini geçersiz kılmak anlamına gelir. Bu geri yükleme, hedef örnekteki BackInt ilgili bilgileri de geçersiz kılar. Bu nedenle, sistem DB hedef örneğe geri yüklendikten sonra ön kayıt komut dosyasını yeniden çalıştırın. Ancak o zaman sonraki kiracı DB geri yüklemeleri başarılı olacaktır.

## <a name="upgrading-from-sap-hana-10-to-20"></a>SAP HANA 1.0'dan 2.0'a yükseltme

SAP HANA 1.0 veritabanlarını koruyorsanız ve 2.0'a yükseltmek istiyorsanız aşağıdaki adımları gerçekleştirin:

- Eski SDC veritabanı için korumayı [durdurun.](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database)
- Yükseltmegerçekleştirin. Tamamlandıktan sonra, HANA şimdi MDC bir sistem DB ve kiracı DB(ler) ile
- (sid ve mdc) doğru ayrıntıları ile [ön kayıt komut yeniden](https://aka.ms/scriptforpermsonhana) çalıştırın.
- Azure portalında aynı makine için uzantıyı yeniden kaydedin (Yedekleme -> görünüm ayrıntıları -> İlgili Azure VM 'yi seçin -> yeniden kaydolun).
- Aynı VM için DB'leri Yeniden Keşfet'i tıklatın. Bu eylem, yeni DB'leri adım 2'de doğru ayrıntılarla göstermelidir (SYSTEMDB ve Kiracı DB, SDC değil).
- Bu yeni veritabanları için yedekleme yapılandırma.

## <a name="upgrading-without-an-sid-change"></a>SID değişikliği olmadan yükseltme

SID değişikliğine neden olmayan OS veya SAP HANA yükseltmeleri aşağıda belirtildiği gibi işlenebilir:

- Veritabanı için verileri tutma ile [korumayı durdurma](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database)
- Yükseltmegerçekleştirin.
- [Ön kayıt komut dosyasını](https://aka.ms/scriptforpermsonhana)yeniden çalıştırın. Genellikle, yükseltme işleminin gerekli rolleri kaldıran gördük. Ön kayıt komut dosyasının çalıştırılması, gerekli tüm rolleri doğrulamaya yardımcı olur.
- Veritabanı için [yeniden koruma](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) yı devam ettirme

## <a name="re-registration-failures"></a>Yeniden kayıt hataları

Yeniden kaydetme işlemini tetiklemeden önce aşağıdaki belirtilerden birini veya birkaçını denetleyin:

- Tüm işlemler (yedekleme, geri yükleme ve yedekleme yapılandırma gibi) aşağıdaki hata kodlarından biriyle VM'de başarısız oluyor: **WorkloadExtensionNotReachable, UserErrorWorkloadExtensionNotInstalled, WorkloadExtensionNotPresent, WorkloadExtensionNotPresent, WorkloadExtensionDidntDequeueMsg**.
- Yedekleme öğesinin **Yedekleme Durumu** alanı **erişilemeyeceğini**gösteriyorsa, aynı durumla sonuçlanabilecek diğer tüm nedenleri ekarte edin:

  - VM'de yedekleme yle ilgili işlemleri gerçekleştirmek için izin eksikliği
  - VM kapatılır, bu nedenle yedeklemeler gerçekleşemez
  - Ağ sorunları

Bu belirtiler aşağıdaki nedenlerden biri veya daha fazlası için ortaya çıkabilir:

- Bir uzantı silindi veya portaldan kaldırıldı.
- VM, yerinde disk geri yüklemeyoluyla zamanda geri yüklendi.
- VM uzun bir süre için kapatıldı, bu nedenle üzerindeki uzantı yapılandırması sona erdi.
- VM silindi ve silinen VM ile aynı ada ve aynı kaynak grubunda başka bir VM oluşturuldu.

Önceki senaryolarda, VM'de yeniden kaydetme işlemini tetiklemenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM'lerde SAP HANA veritabanlarını yedekleme yle ilgili [sık sorulan soruları](https://docs.microsoft.com/azure/backup/sap-hana-faq-backup-azure-vm) gözden geçirin.
