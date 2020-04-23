---
title: Azure Dosyalarını yedekleme hakkında SSS
description: Bu makalede, Azure dosya paylaşımlarınızı Azure Backup hizmetiyle koruma hakkında sık sorulan soruların yanıtlarını bulun.
ms.date: 04/22/2020
ms.topic: conceptual
ms.openlocfilehash: d7b19fd11e6784a188a18f6a613eef5ff4f77764
ms.sourcegitcommit: 354a302d67a499c36c11cca99cce79a257fe44b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82105650"
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Dosyalarını yedekleme ile ilgili sorular

Bu makale, Azure Dosyalarını yedekleme hakkındaki yaygın sorulara yanıtlar sunar. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

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

### <a name="can-i-change-the-vault-to-which-i-back-up-my-file-shares"></a>Dosya Paylaşımlarımı yedeklemediğim kasayı değiştirebilir miyim?

Evet. Bununla birlikte, bağlı kasadan [dosya paylaşımında korumayı durdurmanız](manage-afs-backup.md#stop-protection-on-a-file-share) , bu depolama hesabının [kaydını](manage-afs-backup.md#unregister-a-storage-account) kaldırmanız ve ardından farklı bir kasadan korumanız gerekir.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vault"></a>Kasada kaç tane Azure dosya paylaşımını koruyabilirim?

Azure dosya paylaşımlarını her kasa için 50 depolama hesabından koruyabilirsiniz. Tek bir kasada en fazla 200 Azure dosya paylaşımını koruyabilirsiniz.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Aynı Depolama Hesabından, farklı Kasalara iki farklı dosya paylaşımını koruyabilir miyim?

Hayır. Depolama Hesabındaki tüm dosya paylaşımları, aynı Kasa tarafından korunabilir.

## <a name="backup"></a>Backup

### <a name="what-should-i-do-if-my-backups-start-failing-due-to-the-maximum-limit-reached-error"></a>En fazla sınıra ulaşılma nedeniyle yedeklemelerim başarısız olursa ne yapmam gerekir?

Zamanın herhangi bir noktasında dosya paylaşımı için en fazla 200 Anlık Görüntüye sahip olabilirsiniz. Sınır, ilkenizde tanımlandığı şekilde, Azure Backup tarafından alınan anlık görüntüleri içerir. Sınıra ulaştıktan sonra yedeklemeleriniz başarısız olmaya başladıysa, başarılı bir sonraki yedeklemeler için Isteğe bağlı anlık görüntüleri silin.

## <a name="restore"></a>Geri Yükleme

### <a name="can-i-recover-from-a-deleted-azure-file-share"></a>Silinen bir Azure dosya paylaşımından kurtarma gerçekleştirebilir miyim?

Bir Azure dosya paylaşımının silindiği zaman, silinecek yedeklemelerin listesi gösterilir ve bir onay istenir. Şu anda, silinen bir Azure dosya paylaşma geri yüklenemez.

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

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share"></a>Bir Azure dosya paylaşımı için Yedekleme ilkesini değiştirdiğimde ne olur?

Dosya paylaşımlarında yeni bir ilke uygulandığında yeni ilkenin zamanlama ve bekletmesi geçerli olur. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Backup diğer alanlarıyla ilgili daha fazla bilgi edinmek için, bu diğer yedekleme hakkında SSS bölümüne bakın:

- [Kurtarma Hizmetleri kasası hakkında SSS](backup-azure-backup-faq.md)
- [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md)
- [Azure Backup aracısı hakkında SSS](backup-azure-file-folder-backup-faq.md)
