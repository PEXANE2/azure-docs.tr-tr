---
title: İzleme Uyarısı ve Raporlar SSS
description: Bu makalede, Azure Yedekleme İzleme Uyarısı ve Azure Yedekleme raporlarıyla ilgili sık sorulan soruların yanıtlarını keşfedin.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: f5be97458ba658f315c31ae34e540842b64e3ec4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76989578"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Azure Yedekleme İzleme Uyarısı - SSS

Bu makalede, Azure Yedekleme izleme ve raporlama ile ilgili sık sorulan soruları yanıtlar.

## <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-log-analytics-la-workspace"></a>Raporlama verilerinin Bir Günlük Analizi (LA) Çalışma Alanına akmaya başları olup olmadığını nasıl kontrol edebilirim?

Yapılandırdığınız LA Çalışma Alanına gidin, **Günlükler** menü öğesine gidin ve CoreAzureBackup sorgusunu çalıştırın | almak 1. Döndürülen bir kaydı görürseniz, bu verilerin çalışma alanına akmaya başladığı anlamına gelir. İlk veri itme 24 saat kadar sürebilir.

### <a name="what-is-the-frequency-of-data-push-to-an-la-workspace"></a>LA Çalışma alanına yapılan veri itme sıklığı nedir?

Kasadaki tanılama verileri Log Analytics çalışma alanına biraz gecikmeyle pompalanır. Her etkinlik, Recovery Services kasasından itildikten 20 ila 30 dakika sonra Log Analytics çalışma alanına gelir. Burada gecikme hakkında daha fazla bilgi vardır:

* Tüm çözümlerde, yedekleme hizmetinin yerleşik uyarıları oluşturuldukları anda itilir. Bu nedenle genellikle Log Analytics çalışma alanında 20 ila 30 dakika sonra görünürler.
* Tüm çözümlerde, isteğe bağlı yedekleme işleri ve geri yükleme işleri, işleri bitirir bitirmez itilir.
* SQL yedeklemesi dışındaki tüm çözümler için, zamanlanmış yedekleme işleri biter bitmez itilir.
* SQL yedeklemesi için, günlük yedeklemeleri her 15 dakikada bir gerçekleşebildiği için, günlükler de dahil olmak üzere tamamlanan tüm zamanlanmış yedekleme işlerinin bilgileri toplu olarak doldurularak her 6 saatte bir itilir.
* Tüm çözümlerde, yedekleme öğesi, ilke, kurtarma noktaları, depolama ve benzeri diğer bilgiler günde en az bir kez itilir.
* Yedekleme yapılandırmasındaki bir değişiklik (ilke değiştirme veya düzenleme ilkesi gibi) ilgili tüm yedekleme bilgilerinin itme sini tetikler.

### <a name="how-long-can-i-retain-reporting-data"></a>Raporlama verilerini ne kadar tutabilirim?

Bir LA Çalışma Alanı oluşturduktan sonra, verileri en fazla 2 yıl tutmayı seçebilirsiniz. Varsayılan olarak, BIR LA Çalışma Alanı verileri 31 gün boyunca saklar.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-la-workspace"></a>LA Çalışma Alanını yapılandırdıktan sonra tüm verilerimi raporlarda görecek miyim?

 Tanılama ayarlarını yapılandırmadan sonra oluşturulan tüm veriler LA Çalışma Alanına itilir ve raporlarda kullanılabilir. Devam etmekte olan işler raporlama için itilmiş değildir. İş bittikten veya başarısız olduktan sonra raporlara gönderilir.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Raporları kasalar ve abonelikler arasında görüntüleyebilir miyim?

Evet, raporları kasalar ve aboneliklerin yanı sıra bölgeler arasında da görüntüleyebilirsiniz. Verileriniz tek bir LA Çalışma alanında veya bir grup LA Çalışma Alanında bulunabilir.

### <a name="can-i-view-reports-across-tenants"></a>Raporları kiracılar arasında görüntüleyebilir miyim?

Müşterilerinizin aboneliklerine veya LA Çalışma Alanlarına yetkili erişimi olan bir [Azure Deniz Feneri](https://azure.microsoft.com/services/azure-lighthouse/) kullanıcısıysanız, tüm kiracılarınızdaki verileri görüntülemek için Yedek Raporlar'ı kullanabilirsiniz.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Azure yedekleme aracısı iş durumunun portala yansıması ne kadar sürer?

Azure portalının Azure yedekleme aracısı iş durumunu yansıtması 15 dakika kadar sürebilir.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Yedekleme işi başarısız olduğunda uyarı verilmesi ne kadar sürer?

Azure yedekleme hatasına 20 dakika mesafede bir uyarı yükseltilir.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Bildirimler yapılandırılırsa e-postanın gönderilmeyeceğinin bir durumu var mı?

Evet. Aşağıdaki durumlarda bildirimler gönderilmez.

* Bildirimler saatlik olarak yapılandırılırsa ve bir uyarı yükseltilir ve bir saat içinde çözülürse
* Bir iş iptal edildiğinde
* Orijinal yedekleme işi devam ettiği için ikinci bir yedekleme işi başarısız olursa

## <a name="recovery-services-vault"></a>Kurtarma Hizmetleri Kasası


### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Azure yedekleme aracısı iş durumunun portala yansıması ne kadar sürer?

Azure portalının Azure yedekleme aracısı iş durumunu yansıtması 15 dakika kadar sürebilir.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Yedekleme işi başarısız olduğunda uyarı verilmesi ne kadar sürer?

Azure yedekleme hatasına 20 dakika mesafede bir uyarı yükseltilir.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Bildirimler yapılandırılırsa e-postanın gönderilmeyeceğinin bir durumu var mı?

Evet. Aşağıdaki durumlarda bildirimler gönderilmez:

* Bildirimler saatlik olarak yapılandırılırsa ve bir uyarı yükseltilir ve bir saat içinde çözülürse
* Bir iş iptal edildiğinde
* Orijinal yedekleme işi devam ettiği için ikinci bir yedekleme işi başarısız olursa

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

* Azure VM yedeklemeleriyle ilgili [yaygın sorular](backup-azure-vm-backup-faq.md).
* Azure Backup aracısıyla ilgili [yaygın sorular](backup-azure-file-folder-backup-faq.md)
