---
title: Azure Dosyalarını yedekleme hakkında SSS
description: Bu makalede, Azure dosya paylaşımlarınızı Azure Yedekleme hizmetiyle nasıl koruyacağınıza ilişkin sık sorulan soruların yanıtlarını keşfedin.
ms.date: 07/29/2019
ms.topic: conceptual
ms.openlocfilehash: c69d4642aefbd599d3783dcdfa059a0cd9d129d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302551"
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Dosyalarını yedekleme ile ilgili sorular

Bu makale, Azure Dosyalarını yedekleme hakkındaki yaygın sorulara yanıtlar sunar. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

Bu makaledeki bölümleri hızlı taramak için **bu makale altında** sağ taraftaki bağlantıları kullanın.

## <a name="configuring-the-backup-job-for-azure-files"></a>Azure Dosyaları için yedekleme işini yapılandırma

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares"></a>Korumak istediğim ve geçerli Azure dosya paylaşımları içeren Depolama Hesaplarımın bazılarını neden göremiyorum?

Azure Dosya Paylaşımları için Yedekleme, önizleme sırasında her Depolama Hesabı türünü desteklemez. Desteklenen Depolama Hesaplarının listesini görüntülemek için [buraya](troubleshoot-azure-files.md#limitations-for-azure-file-share-backup-during-preview) başvurun. Aradığınız Depolama Hesabının zaten korumalı veya başka bir Kasa ile kayıtlı olması da mümkündür. Depolama Hesabını koruma için diğer Kasalarda bulmak için kasadan [kaldırın](troubleshoot-azure-files.md#configuring-backup).

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup"></a>Yedeklemeyi yapılandırmaya çalışırken, Depolama Hesabında Azure dosya paylaşımlarımdan bazılarını neden göremiyorum?

Azure dosya paylaşımının, aynı Kurtarma Hizmetleri kasasında zaten korumalı olup olmadığını veya yakın zamanda silinip silinmediğini denetleyin.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync"></a>Azure Dosya Eşitleme’deki bir Eşitleme Grubuna bağlanan Dosya Paylaşımlarını koruyabilir miyim?

Evet. Eşitleme Gruplarına bağlanan Azure Dosya Paylaşımlarının koruması etkindir ve Genel önizlemenin parçasıdır.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-did-not-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Dosya paylaşımlarını yedeklemeye çalışırken, dosya paylaşımlarını keşfetmek için bir Depolama Hesabına tıkladım. Ancak koruyamadım. Bu dosya paylaşımlarını başka bir Kasa ile nasıl koruyabilirim?

Yedeklemeye çalışırken, dosya paylaşımlarını keşfetmek için bir Depolama Hesabı seçildiğinde Depolama Hesabı, bu işlemin yapıldığı Kasaya kaydolur. Dosya paylaşımları farklı bir Kasa ile korumayı seçerseniz, bu Kasadan seçilen Depolama Hesabının [Kaydını Sil](troubleshoot-azure-files.md#configuring-backup)’in.

### <a name="can-i-change-the-vault-to-which-i-back-up-my-file-shares"></a>Dosya paylaşımlarımı yedeklediğim Kasa'yı değiştirebilir miyim?

Evet. Ancak, bağlı Kasa'dan [bir dosya paylaşımında korumayı durdurmanız,](manage-afs-backup.md#stop-protection-on-a-file-share) bu Depolama Hesabının [Kaydını Açmanız](troubleshoot-azure-files.md#configuring-backup) ve ardından farklı bir Vault'tan korumanız gerekir.

### <a name="in-which-geos-can-i-back-up-azure-file-shares"></a>Azure Dosyası paylaşımlarını hangi coğrafyalarda yedekleyebilirim?

Azure Dosya paylaşımları için yedekleme şu anda Önizleme aşamasındadır ve yalnızca aşağıdaki bölgelerde kullanılabilir:

- Doğu Avustralya (AE)
- Avustralya Güneydoğu (ASE)
- Güney Brezilya (BRS)
- Orta Kanada (CNC)
- Doğu Kanada (CE)
- Orta ABD (CUS)
- Doğu Asya (EA)
- Doğu ABD (EUS)
- Doğu ABD 2 (EUS2)
- Doğu Japonya (JPE)
- Batı Japonya (JPW)
- Hindistan Orta (INC)
- Hindistan Güney (INS)
- Güney Kore - Orta (KRC)
- Güney Kore - Güney (KRS)
- Orta Kuzey ABD (NCUS)
- Kuzey Avrupa (NE)
- Orta Güney ABD (SCUS)
- Güneydoğu Asya (SEA)
- Güney Birleşik Krallık (UKS)
- Batı Birleşik Krallık (UKW)
- Batı Avrupa (WE)
- Batı ABD (WUS)
- Orta Batı ABD (WCUS)
- Batı ABD 2 (WUS 2)
- ABD Gov Arizona (UGA)
- ABD Gov Texas (UGT)
- ABD Gov Virginia (UGV)
- Avustralya Merkez (ACL)
- Hindistan Batı (INW)
- Güney Afrika Kuzey(SAN)
- BAE Kuzey(UAN)
- Fransa Merkez (FRC)
- Almanya Kuzey (GN)                       
- Almanya Batı Orta (GWC)
- Güney Afrika Batı (SAW)
- BAE Merkez (UAC)
- NWE (Norveç Doğu)     
- NWW (Norveç Batı)
- SZN (İsviçre Kuzey)

[AskAzureBackupTeam@microsoft.com](mailto:askazurebackupteam@microsoft.com) Yukarıda listelenmemiş belirli bir coğrafi içinde kullanmanız gerekiyorsa yazın.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vault"></a>Kasada kaç tane Azure dosya paylaşımını koruyabilirim?

Önizleme sırasında, Kasa başına en fazla 50 Depolama Hesabından Azure dosya paylaşımını koruyabilirsiniz. Tek bir kasada en fazla 200 Azure dosya paylaşımını koruyabilirsiniz.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Aynı Depolama Hesabından, farklı Kasalara iki farklı dosya paylaşımını koruyabilir miyim?

Hayır. Depolama Hesabındaki tüm dosya paylaşımları, aynı Kasa tarafından korunabilir.

## <a name="backup"></a>Backup

### <a name="how-many-scheduled-backups-can-i-configure-per-file-share"></a>Dosya paylaşımı başına kaç tane zamanlanmış yedekleme yapılandırabilirim?

Azure Yedekleme şu anda Azure Dosya Paylaşımlarının zamanlanmış bir kez yedeklemesini yapılandırmayı destekler.

### <a name="how-many-on-demand-backups-can-i-take-per-file-share"></a>Dosya paylaşımı başına kaç tane İsteğe Bağlı yedekleme alabilirim?

Zamanın herhangi bir noktasında dosya paylaşımı için en fazla 200 Anlık Görüntüye sahip olabilirsiniz. Sınır, ilkenizde tanımlandığı şekilde, Azure Backup tarafından alınan anlık görüntüleri içerir. Sınıra ulaştıktan sonra yedekleriniz başarısız olmaya başlarsa, daha sonraki yedeklemelerin başarılı olması için İsteğe Bağlı geri yükleme noktalarını silin.

## <a name="restore"></a>Geri Yükleme

### <a name="can-i-recover-from-a-deleted-azure-file-share"></a>Silinen bir Azure dosya paylaşımından kurtarma gerçekleştirebilir miyim?

Bir Azure dosya paylaşımı silindiğinde, size silinecek yedeklemelerin listesi gösterilir ve onay beklenir. Silinen Azure dosya paylaşımı geri yüklenemez.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share"></a>Azure dosya paylaşımındaki korumayı durdurursam yedeklemelerden geri yükleme yapabilir miyim?

Evet. Korumayı durdurduğunuzda **Yedekleme Verilerini Koru** seçeneğini belirlediyseniz tüm mevcut geri yükleme noktalarından geri yükleme yapabilirsiniz.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Devam eden bir geri yükleme işini iptal edersem ne olur?

Devam eden bir geri yükleme işi iptal edilirse, geri yükleme işlemi durur ve iptalden önce geri yüklenen tüm dosyalar, herhangi bir geri alma olmadan yapılandırılmış hedefte (orijinal veya alternatif konum) kalır.

## <a name="manage-backup"></a>Yedeklemeyi yönet

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares"></a>Azure Dosyası paylaşımlarının yedeklemelerini yapılandırmak/yönetmek/geri yüklemek için PowerShell'i kullanabilir miyim?

Evet. Lütfen detaylı [belgelere](backup-azure-afs-automation.md) buradan bakınız.

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it"></a>Azure Backup tarafından alınan anlık görüntülere erişebilir ve bu görüntüleri bağlayabilir miyim?

Azure Backup tarafından alınan tüm Anlık Görüntülere, portaldaki, PowerShell veya CLI’daki Anlık Görüntüler Görüntülenerek erişilebilir. Azure Dosyaları paylaşım anlık görüntüleri hakkında daha fazla bilgi edinmek için bkz. [Azure Dosyaları için paylaşım anlık görüntülerine genel bakış (önizleme)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups"></a>Yedeklemeler için yapılandırabileceğim maksimum bekletme nedir?

Azure dosya paylaşımları için yedekleme, ilkeleri 180 güne kadar bekletme yle yapılandırma olanağı sunar. Ancak, [PowerShell'deki "İsteğe bağlı yedekleme" seçeneğini](backup-azure-afs-automation.md#trigger-an-on-demand-backup)kullanarak, 10 yıl boyunca bile bir kurtarma noktasını koruyabilirsiniz.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share"></a>Bir Azure dosya paylaşımı için Yedekleme ilkesini değiştirdiğimde ne olur?

Dosya paylaşımlarında yeni bir ilke uygulandığında yeni ilkenin zamanlama ve bekletmesi geçerli olur. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Yedekleme'nin diğer alanları hakkında daha fazla bilgi edinmek için şu diğer Yedekleme SSS'lerinden bazılarına bakın:

- [Kurtarma Hizmetleri kasası hakkında SSS](backup-azure-backup-faq.md)
- [Azure VM yedeklemesi hakkında SSS](backup-azure-vm-backup-faq.md)
- [Azure Backup aracısı hakkında SSS](backup-azure-file-folder-backup-faq.md)
