---
title: Azure Dosyalarını yedekleme hakkında SSS
description: Bu makalede, Azure dosya paylaşımlarınızı Azure Backup hizmetiyle koruma hakkında sık sorulan soruların yanıtlarını bulun.
ms.date: 04/22/2020
ms.topic: conceptual
ms.openlocfilehash: 0db30de655bfc0b98baa81a4ef20532e697fc1f8
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88824738"
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Dosyalarını yedekleme ile ilgili sorular

Bu makale, Azure Dosyalarını yedekleme hakkındaki yaygın sorulara yanıtlar sunar. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. [Tartışma Için Microsoft Q&soru sayfasında](/answers/topics/azure-backup.html)Azure Backup hizmetiyle ilgili sorular da gönderebilirsiniz.

Bu makaledeki bölümleri hızlı taramak için **bu makale altında** sağ taraftaki bağlantıları kullanın.

## <a name="configuring-the-backup-job-for-azure-files"></a>Azure Dosyaları için yedekleme işini yapılandırma

### <a name="why-cant-i-see-some-of-my-storage-accounts-that-i-want-to-protect-which-contain-valid-azure-file-shares"></a>Koruma için geçerli Azure dosya paylaşımları içeren depolama hesaplarımdan bazılarını neden göremiyorum?

Depolama hesabının desteklenen depolama hesabı türlerinden birine ait olduğundan emin olmak için [Azure dosya paylaşımları yedeklemesi Için destek matrisine](azure-file-share-support-matrix.md) bakın. Aradığınız depolama hesabı zaten korunuyor ya da başka bir kasaya kaydedilmiş olabilir. Diğer kasaların koruma için depolama hesabını bulmasıyla [depolama hesabının kasadan kaydını kaldırın](manage-afs-backup.md#unregister-a-storage-account) .

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup"></a>Yedeklemeyi yapılandırmaya çalışırken, Depolama Hesabında Azure dosya paylaşımlarımdan bazılarını neden göremiyorum?

Azure dosya paylaşımının, aynı Kurtarma Hizmetleri kasasında zaten korumalı olup olmadığını veya yakın zamanda silinip silinmediğini denetleyin.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync"></a>Azure Dosya Eşitleme’deki bir Eşitleme Grubuna bağlanan Dosya Paylaşımlarını koruyabilir miyim?

Evet. Eşitleme gruplarına bağlanan Azure dosya paylaşımlarının koruması etkin.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-didnt-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Dosya paylaşımlarını yedeklemeye çalışırken, dosya paylaşımlarını keşfetmek için bir Depolama Hesabına tıkladım. Ancak, bunları korumadı. Bu dosya paylaşımlarını diğer kasalarla korumak Nasıl yaparım? mı?

Yedeklemeye çalışırken, BT içindeki dosya paylaşımlarını bulacak bir depolama hesabı seçmek, depolama hesabını bunun yapıldığı kasaya kaydeder. Dosya paylaşımlarını farklı bir kasala korumayı seçerseniz, seçili depolama hesabının kaydını bu kasadan [silin](manage-afs-backup.md#unregister-a-storage-account) .

### <a name="why-cant-i-change-the-vault-to-configure-backup-for-the-file-share"></a>Dosya paylaşımının yedeklemesini yapılandırmak için kasayı neden değiştiremiyorum?

Depolama hesabı zaten bir kasaya kaydedilmişse veya depolama hesabındaki diğer dosya paylaşımları bir kasa kullanılarak korunuyorsa, bunu değiştirme seçeneği verilmemiş demektir. Bir depolama hesabındaki tüm dosya paylaşımları yalnızca aynı kasa tarafından korunabilir. Kasayı değiştirmek istiyorsanız, bağlı kasadan [depolama hesabındaki tüm dosya paylaşımlarının korumasını durdurmanız](manage-afs-backup.md#stop-protection-on-a-file-share) , depolama hesabının [kaydını](manage-afs-backup.md#unregister-a-storage-account) kaldırmanız ve ardından koruma için farklı bir kasa seçmeniz gerekir.

### <a name="can-i-change-the-vault-to-which-i-back-up-my-file-shares"></a>Dosya Paylaşımlarımı yedeklemediğim kasayı değiştirebilir miyim?

Evet. Bununla birlikte, bağlı kasadan [dosya paylaşımında korumayı durdurmanız](manage-afs-backup.md#stop-protection-on-a-file-share) , bu depolama hesabının [kaydını](manage-afs-backup.md#unregister-a-storage-account) kaldırmanız ve ardından farklı bir kasadan korumanız gerekir.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Aynı Depolama Hesabından, farklı Kasalara iki farklı dosya paylaşımını koruyabilir miyim?

Hayır. Depolama Hesabındaki tüm dosya paylaşımları, aynı Kasa tarafından korunabilir.

## <a name="backup"></a>Backup

### <a name="what-should-i-do-if-my-backups-start-failing-due-to-the-maximum-limit-reached-error"></a>En fazla sınıra ulaşılma nedeniyle yedeklemelerim başarısız olursa ne yapmam gerekir?

Zamanın herhangi bir noktasında dosya paylaşımı için en fazla 200 Anlık Görüntüye sahip olabilirsiniz. Sınır, ilkenizde tanımlandığı şekilde, Azure Backup tarafından alınan anlık görüntüleri içerir. Sınıra ulaştıktan sonra yedeklemeleriniz başarısız olmaya başladıysa, başarılı bir sonraki yedeklemeler için Isteğe bağlı anlık görüntüleri silin.

## <a name="restore"></a>Geri Yükleme

### <a name="can-i-recover-from-a-deleted-azure-file-share"></a>Silinen bir Azure dosya paylaşımından kurtarma gerçekleştirebilir miyim?

Dosya paylaşma geçici olarak silinmiş durumdaysa, geri yükleme işlemini gerçekleştirmek için öncelikle dosya paylaşımının silinmesini yapmanız gerekir. Silmeyi geri alma işlemi, dosya paylaşımının zaman içinde herhangi bir noktaya geri yüklediğiniz etkin duruma gelmesini sağlar. Dosya paylaşımınızın silmeyi geri alma hakkında bilgi edinmek için [Bu bağlantıyı](../storage/files/storage-files-enable-soft-delete.md?tabs=azure-portal#restore-soft-deleted-file-share) ziyaret edin veya [silmeyi geri al dosya paylaşma betiğine](./scripts/backup-powershell-script-undelete-file-share.md)bakın. Dosya paylaşımının kalıcı olarak silinmesi halinde içerikleri ve anlık görüntüleri geri yükleyemezsiniz.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share"></a>Azure dosya paylaşımındaki korumayı durdurursam yedeklemelerden geri yükleme yapabilir miyim?

Evet. Korumayı durdurduğunuzda **Yedekleme Verilerini Koru** seçeneğini belirlediyseniz tüm mevcut geri yükleme noktalarından geri yükleme yapabilirsiniz.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Devam eden bir geri yükleme işini iptal etdiğimde ne olur?

Devam eden bir geri yükleme işi iptal edilirse, geri yükleme işlemi duraklar ve iptalden önce geri yüklenen tüm dosyalar, geri alma işlemleri yapılmadan yapılandırılmış hedefte (orijinal veya alternatif konum) kalır.

## <a name="manage-backup"></a>Yedeklemeyi Yönet

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares"></a>PowerShell 'i Azure dosya paylaşımlarının yedeklerini yapılandırmak/yönetmek/geri yüklemek için kullanabilir miyim?

Evet. Ayrıntılı [belgelere bakın](backup-azure-afs-automation.md).

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-them"></a>Azure yedeklemeleri tarafından alınan anlık görüntülere erişebilir ve bunları bağlayabilir miyim?

Azure Backup tarafından alınan tüm anlık görüntülere Portal, PowerShell veya CLı 'daki anlık görüntüler görüntülenerek erişilebilir. Azure dosyaları paylaşma anlık görüntüleri hakkında daha fazla bilgi edinmek için bkz. [Azure dosyaları için paylaşılan anlık görüntülere genel bakış](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups"></a>Yedeklemeler için yapılandırabildiğim maksimum bekletme nedir?

Maksimum saklama hakkındaki ayrıntılar için [destek matrisine](azure-file-share-support-matrix.md) bakın. Azure Backup, yedekleme ilkesini yapılandırırken bekletme değerlerini girerken anlık görüntü sayısı için gerçek zamanlı bir hesaplama yapar. Tanımlanan saklama değerleriniz için karşılık gelen anlık görüntü sayısı 200 ' i aşarsa, Portal, bekletme değerlerinizi ayarlamanızı isteyen bir uyarı gösterir. Bu sayede herhangi bir zamanda herhangi bir zamanda herhangi bir dosya paylaşımında Azure dosyaları için desteklenen en fazla anlık görüntü sayısını aşamazsınız.

### <a name="what-is-the-impact-on-existing-recovery-points-and-snapshots-when-i-modify-the-backup-policy-for-an-azure-file-share-to-switch-from-daily-policy-to-gfs-policy"></a>Bir Azure dosya paylaşımının yedekleme ilkesini, "günlük Ilke" iken "GFS Ilkesi" olarak değiştirecek şekilde değiştirirken var olan kurtarma noktalarına ve anlık görüntülere etkisi nedir?

Günlük yedekleme ilkesini GFS ilkesine değiştirdiğinizde (haftalık/aylık/yıllık saklama ekleme), davranış aşağıdaki gibidir:

- **Bekletme**: ilkeyi değiştirmenin bir parçası olarak haftalık/aylık/yıllık saklama ekliyorsanız, zamanlanmış yedeklemenin bir parçası olarak oluşturulan tüm gelecek kurtarma noktaları yeni ilkeye göre etiketlenecektir. Var olan tüm kurtarma noktaları hala günlük kurtarma noktaları olarak değerlendirilir ve bu nedenle haftalık/aylık/yıllık olarak etiketlenemez.

- **Anlık görüntüler ve kurtarma noktaları Temizleme**:

  - Günlük bekletme uzatıldıysa, mevcut kurtarma noktalarının sona erme tarihi, Yeni ilkede yapılandırılan günlük saklama değerine göre güncelleştirilir.
  - Günlük tutma azaldıysa, var olan kurtarma noktaları ve anlık görüntüleri, Yeni ilkede yapılandırılan günlük saklama değerine göre sonraki temizleme çalıştırma işinde silinmek üzere işaretlenir ve sonra silinir.

Bunun nasıl çalıştığını gösteren bir örnek aşağıda verilmiştir:

#### <a name="existing-policy-p1"></a>Mevcut Ilke [P1]

|Bekletme türü |Zamanla |Bekletme  |
|---------|---------|---------|
|Günlük olarak    |    Her gün 8 saat    |  100 gün       |

#### <a name="new-policy-modified-p1"></a>Yeni Ilke [değiştirilmiş P1]

| Bekletme türü | Zamanla                       | Bekletme |
| -------------- | ------------------------------ | --------- |
| Günlük olarak          | Her gün 9 saat              | 50 gün   |
| Haftalık         | Pazar günü saat 9 ' da              | 3 hafta   |
| Aylık olarak        | Son Pazartesi günü saat 9 ' da         | 1 ay   |
| Yıllık         | Üçüncü Pazar günü saat 9 ' da | 4 yıl   |

#### <a name="impact"></a>Etki

1. Var olan kurtarma noktalarının sona erme tarihi, yeni ilkenin günlük saklama değerine göre ayarlanır: Yani, 50 gün. Bu nedenle 50 günden eski olan tüm kurtarma noktaları silinmek üzere işaretlenir.

2. Mevcut kurtarma noktaları, yeni ilkeye göre haftalık/aylık/yıllık olarak etiketlenemez.

3. Gelecekteki tüm yedeklemeler yeni zamanlamaya göre tetiklenecektir: Yani 9 PM.

4. Gelecekteki tüm kurtarma noktalarının sona erme tarihi yeni ilkeyle hizalanacaktır.

>[!NOTE]
>İlke değişiklikleri yalnızca zamanlanmış yedekleme işinin bir parçası olarak oluşturulan kurtarma noktalarını etkiler. İsteğe bağlı yedeklemeler için, saklama, yedekleme sırasında belirtilen şekilde **sakla** değerine göre belirlenir.

### <a name="what-is-the-impact-on-existing-recovery-points-when-i-modify-an-existing-gfs-policy"></a>Var olan bir GFS Ilkesini değiştirirken var olan kurtarma noktalarına etkisi nedir?

Dosya paylaşımlarında yeni bir ilke uygulandığında, gelecekteki tüm zamanlanmış yedeklemeler, değiştirilen ilkede yapılandırılan zamanlamaya göre alınacaktır.  Var olan tüm kurtarma noktalarını bekletme, yapılandırılan yeni saklama değerlerine göre hizalanır. Bu nedenle, bekletme genişletildiğinde, var olan kurtarma noktaları yeni ilkeye göre bekletilecek şekilde işaretlenir. Bekletme azaltıldığında, sonraki temizleme işinde Temizleme için işaretlenirler ve sonra silinir.

Bunun nasıl çalıştığını gösteren bir örnek aşağıda verilmiştir:

#### <a name="existing-policy-p2"></a>Mevcut Ilke [P2]

| Bekletme türü | Zamanla           | Bekletme |
| -------------- | ------------------ | --------- |
| Günlük olarak          | Her gün 8 saat | 50 gün   |
| Haftalık         | Pazartesi günü 8 saat  | 3 hafta   |

#### <a name="new-policy-modified-p2"></a>Yeni Ilke [Modified P2]

| Bekletme türü | Zamanla               | Bekletme |
| -------------- | ---------------------- | --------- |
| Günlük olarak          | Her gün 9 saat     | 10 gün   |
| Haftalık         | Pazartesi günü saat 9 ' da      | 2 hafta   |
| Aylık olarak        | Son Pazartesi günü saat 9 ' da | 2 ay  |

#### <a name="impact-of-change"></a>Değişikliğin etkisi

1. Mevcut günlük kurtarma noktalarının sona erme tarihi, 10 gün olan yeni günlük bekletme değerine göre hizalanacaktır. Bu nedenle, 10 günden daha eski olan günlük kurtarma noktaları silinir.

2. Mevcut haftalık kurtarma noktalarının sona erme tarihi, yeni haftalık saklama değerine göre hizalanır ve bu iki haftadan oluşur. Bu nedenle, iki haftadan daha eski olan haftalık kurtarma noktaları silinir.

3. Aylık kurtarma noktaları, yeni ilke yapılandırmasına bağlı olarak, gelecekteki yedeklemelerin bir parçası olarak oluşturulur.

4. Gelecekteki tüm kurtarma noktalarının sona erme tarihi yeni ilkeyle hizalanacaktır.

>[!NOTE]
>İlke değişiklikleri yalnızca zamanlanan yedeklemenin bir parçası olarak oluşturulan kurtarma noktalarını etkiler. İsteğe bağlı yedeklemeler için, bekletme, yedekleme sırasında belirtilen şekilde **sakla** değeri tarafından belirlenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure dosya paylaşımlarını yedeklerken sorunları giderme](troubleshoot-azure-files.md)
