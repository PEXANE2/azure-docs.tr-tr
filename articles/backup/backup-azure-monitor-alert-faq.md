---
title: Uyarı ve rapor izleme hakkında SSS
description: Bu makalede, Azure Backup Izleme uyarısı ve rapor Azure Backup hakkında sık sorulan soruların yanıtlarını bulun.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: 670314d8703889016431838a9f5b0a6dc56b84b8
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88892567"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Azure Backup Izleme uyarısı-SSS

Bu makalede Azure Backup izleme ve raporlama hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="configure-azure-backup-reports"></a>Azure Backup raporlarını yapılandırma

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-log-analytics-la-workspace"></a>Nasıl yaparım? rapor verilerinin bir Log Analytics (LA) çalışma alanına aktarılmaya başlayıp başlamadığına bakın.

Yapılandırdığınız LA çalışma alanına gidin, **Günlükler** menü öğesine gidin ve sorgu CoreAzureBackup ' ı çalıştırın | 1 yapın. Döndürülmekte olan bir kayıt görürseniz, verilerin çalışma alanına akışını başlatan anlamına gelir. İlk veri gönderme, 24 saate kadar sürebilir.

### <a name="what-is-the-frequency-of-data-push-to-an-la-workspace"></a>Bir LA çalışma alanına veri gönderme sıklığı nedir?

Kasadaki Tanılama verileri, bazı gecikmeye sahip Log Analytics çalışma alanına potılmış olur. Her olay, kurtarma hizmetleri kasasından gönderdikten sonra, Log Analytics çalışma alanına 20 ila 30 dakika sonra ulaşır. Gecikme hakkında daha fazla ayrıntı aşağıda verilmiştir:

* Tüm çözümlerde, yedekleme hizmetinin yerleşik uyarıları, oluşturulduktan hemen sonra gönderilir. Bu nedenle, genellikle 20 ila 30 dakika sonra Log Analytics çalışma alanında görünürler.
* Tüm çözümlerde, isteğe bağlı yedekleme işleri ve geri yükleme işleri, tamamlanır almaz gönderilir.
* SQL yedekleme dışındaki tüm çözümler için, zamanlanmış yedekleme işleri, tamamlanır almaz gönderilir.
* SQL yedekleme için, günlük yedeklemeleri 15 dakikada bir gerçekleşebildiğinden, günlükler de dahil olmak üzere tamamlanan tüm zamanlanmış yedekleme işlerinin bilgileri toplu olarak oluşturulur ve her 6 saatte bir gönderilir.
* Tüm çözümlerde yedekleme öğesi, ilke, kurtarma noktaları, depolama vb. gibi diğer bilgiler günde en az bir kez gönderilir.
* Yedekleme yapılandırmasındaki (ilkeyi değiştirme veya ilkeyi değiştirme gibi) bir değişiklik, ilgili tüm yedekleme bilgilerinin bir anında gönderimini tetikler.

### <a name="how-long-can-i-retain-reporting-data"></a>Raporlama verilerini ne kadar süreyle saklayabilir?

Bir LA çalışma alanı oluşturduktan sonra, verileri en fazla 2 yıl tutmayı seçebilirsiniz. Varsayılan olarak, bir LA çalışma alanı verileri 31 gün korur.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-la-workspace"></a>LA çalışma alanını yapılandırdıktan sonra raporlardaki tüm verilerimi görüyorum mi?

 Tanılama ayarlarını yapılandırdıktan sonra oluşturulan tüm veriler, LA çalışma alanına gönderilir ve raporlarda kullanılabilir. Devam eden işler raporlama için itilmedi. İş bittikten veya başarısız olduktan sonra raporlara gönderilir.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Raporları kasaların ve aboneliklerde görüntüleyebilir miyim?

Evet, raporları kasaların ve aboneliklerde ve bölgeler arasında görüntüleyebilirsiniz. Verileriniz, tek bir LA çalışma alanında ya da bir LA çalışma alanı grubu içinde bulunabilir.

### <a name="can-i-view-reports-across-tenants"></a>Raporları kiracılar arasında görüntüleyebilir miyim?

Müşterilerinizin abonelikleri veya LA çalışma alanlarına temsilci erişimi olan bir [Azure](https://azure.microsoft.com/services/azure-lighthouse/) açık Kullanıcı kullanıyorsanız, tüm kiracılarınızdaki verileri görüntülemek Için Yedekleme raporları ' nı kullanabilirsiniz.

## <a name="recovery-services-vault"></a>Kurtarma Hizmetleri kasası

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Azure Backup Aracısı iş durumunun portalda yansıtılması ne kadar sürer?

Azure portal, Azure Backup Aracısı iş durumunun yansıtılması 15 dakika kadar sürebilir.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Yedekleme işi başarısız olduğunda uyarı verilmesi ne kadar sürer?

Azure Backup hatasının 20 dakika içinde bir uyarı oluşturulur.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Bildirimler yapılandırılırsa bir e-postanın gönderilmeyeceği bir durum var mı?

Evet. Aşağıdaki durumlarda bildirimler gönderilmez:

* Bildirimler saatlik olarak yapılandırılırsa ve saat içinde bir uyarı oluşturulur ve çözülür
* Bir iş iptal edildiğinde
* İlk yedekleme işi devam ettiğinden ikinci bir yedekleme işi başarısız olursa

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

* Azure VM yedeklemeleriyle ilgili [yaygın sorular](backup-azure-vm-backup-faq.md).
* Azure Backup aracısıyla ilgili [yaygın sorular](backup-azure-file-folder-backup-faq.md)
