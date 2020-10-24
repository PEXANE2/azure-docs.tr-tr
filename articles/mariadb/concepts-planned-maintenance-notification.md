---
title: Planlı bakım bildirimi-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda planlı bakım bildirimi özelliği açıklanmaktadır
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 10/21/2020
ms.openlocfilehash: 2ce6959cf025d2e29f0ee3dfb3b29b3eaf5d4221
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92490841"
---
# <a name="planned-maintenance-notification-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda planlı bakım bildirimi

MariaDB için Azure veritabanı 'nda planlı bakım olaylarına nasıl hazırlanacağınızı öğrenin.

## <a name="what-is-a-planned-maintenance"></a>Planlı bakım nedir?

MariaDB hizmeti için Azure veritabanı, temel alınan donanım, işletim sistemi ve veritabanı altyapısının otomatik düzeltme eki uygular. Düzeltme Eki, yeni hizmet özellikleri, güvenlik ve yazılım güncelleştirmeleri içerir. MariaDB altyapısı için, ikincil sürüm yükseltmeleri otomatik ve düzeltme eki uygulama döngüsünün bir parçası olarak dahil edilmiştir. Düzeltme için gereken kullanıcı eylemi veya yapılandırma ayarları yoktur. Düzeltme Eki, kapsamlı olarak test edilir ve güvenli dağıtım uygulamaları kullanılarak alınır.

Planlı bakım, bu hizmet güncelleştirmeleri belirli bir Azure bölgesindeki sunuculara dağıtıldığında bakım penceresidir. Planlı bakım sırasında, hizmet güncelleştirmesi sunucularını barındıran Azure bölgesinde dağıtıldığında müşterileri bilgilendirmek için bir bildirim olayı oluşturulur. İki planlı bakım arasındaki en az süre 30 gündür. Sonraki bakım penceresi 72 saat için önceden bir bildirim alırsınız.

## <a name="planned-maintenance---duration-and-customer-impact"></a>Planlı bakım-süre ve müşteri etkisi

Belirli bir Azure bölgesinin planlı bir bakımının genellikle 15 saat çalıştırması beklenir. Bu pencere, gerekirse bir geri alma planının yürütülmesi için arabellek süresini de içerir. Planlı bakım sırasında, veritabanı sunucusu yeniden başlatmaları veya yük devretme işlemleri, son kullanıcılar için veritabanı sunucularının kısa bir kullanım dışı kalmasına yol açabilir. MariaDB sunucuları için Azure veritabanı, kapsayıcılarda çalışmaktadır, böylece veritabanı sunucusu yeniden başlatmaları genellikle hızlı, genellikle 60-120 saniye içinde tamamlanır. Her sunucu yeniden başlatmalarının dahil olduğu tüm planlı bakım olayı, mühendislik ekibi tarafından dikkatle izlenir. Sunucu yük devretmesi süresi, veritabanı kurtarma zamanına bağlıdır. Bu, sunucuda yük devretme sırasında ağır işlem etkinliğinizi varsa veritabanının daha uzun sürmesine neden olabilir. Yeniden başlatma zamanından daha uzun bir süre önlemek için planlı bakım olayları sırasında çalışan uzun işlemleri (toplu yükleme) önlemek önerilir.

Özet olarak, planlanan bakım olayı 15 saat boyunca çalışırken, tek tek sunucu etkisi, sunucudaki hareketsel etkinliğe bağlı olarak genellikle 60 saniye sürer. Planlı bakım başlamadan önce bir bildirim 72 takvim saati, belirli bir bölge için bakım devam ederken ise başka bir uyarı gönderilir.

## <a name="how-can-i-get-notified-of-planned-maintenance"></a>Planlı bakım hakkında nasıl bildirim alabilirim?

Planlı bakım bildirimleri özelliğini, yaklaşan bir planlı bakım olayına yönelik uyarılar almak için kullanabilirsiniz. Gelecekteki bakım 72 takvim saatleriyle ilgili bildirim, belirli bir bölge için bakım devam ederken olay ve başka bir durum hakkında bildirim alacaksınız.

### <a name="planned-maintenance-notification"></a>Planlı bakım bildirimi

> [!IMPORTANT]
> Planlı bakım bildirimleri şu anda tüm bölgelerde önizleme aşamasında mevcuttur Orta Batı ABD **hariç**

**Planlı bakım bildirimleri** , MariaDB Için Azure veritabanınıza yaklaşan planlı bakım olayı için uyarı almanızı sağlar. Bu bildirimler [hizmet durumunun](../service-health/overview.md) planlanmış bakımıyla tümleşiktir ve abonelikleriniz için tüm zamanlanmış bakımı tek bir yerden görüntülemenize olanak sağlar. Ayrıca, farklı kaynaklardan sorumlu farklı kişileriniz olabileceğinden farklı kaynak grupları için bildirimin doğru kitlelere ölçeklendirilmesine de yardımcı olur. Olaydan önce yaklaşan bakım 72 takvim saatleriyle ilgili bildirimi alacaksınız.

Tüm olaylar için **Planlı bakım bildirimi** 72 saat bildirimi sağlamak için her girişimde bulunacağız. Bununla birlikte, kritik veya güvenlik düzeltme ekleri durumlarında, bildirim olaya yakın bir şekilde gönderilebilir veya atlanamaz.

Azure portal planlı bakım bildirimini denetleyebilir ya da uyarıları bildirim alacak şekilde yapılandırabilirsiniz. 

### <a name="check-planned-maintenance-notification-from-azure-portal"></a>Azure portal planlı bakım bildirimini denetle

1. [Azure Portal](https://portal.azure.com) **hizmet durumu**' nu seçin.
2. **Planlı bakım** sekmesini seçin
3. Planlı bakım bildirimini denetlemek istediğiniz **aboneliği**, * * bölgeyi ve **hizmeti** seçin. 
   
### <a name="to-receive-planned-maintenance-notification"></a>Planlı bakım bildirimi almak için

1. [Portalda](https://portal.azure.com) **hizmet durumu**' nu seçin.
2. **Uyarılar** bölümünde **sistem durumu uyarıları**' nı seçin.
3. **+ Hizmet sistem durumu uyarısı Ekle** ' yi seçin ve alanları girin.
4. Gerekli alanları doldurun. 
5. **Olay türünü**seçin, **Planlı bakım** ' ı seçin veya **tümünü seçin**
6. **Eylem gruplarında** , uyarıyı nasıl almak istediğinizi tanımlar (bir e-posta alın, mantıksal uygulama tetiklemesi vb.)  
7. Oluşturma sonrasında kuralın etkinleştir ' in Evet olarak ayarlandığından emin olun.
8. Uyarınızı gerçekleştirmek için **Uyarı kuralı oluştur** ' u seçin

**Hizmet durumu uyarıları**oluşturma hakkında ayrıntılı adımlar için, [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../service-health/alerts-activity-log-service-notifications.md)konusuna bakın.

## <a name="can-i-cancel-or-postpone-planned-maintenance"></a>Planlı Bakımı iptal edebilir veya erteleyebilir miyim?

Sunucunuzu güvenli, kararlı ve güncel tutmak için bakım yapmanız gerekir. Planlı bakım olayı iptal edilemez veya ertelenemez. Bildirim, belirli bir Azure bölgesine gönderildikten sonra, bu bölgedeki herhangi bir sunucu için düzeltme eki uygulama değişiklikleri yapılamaz. Düzeltme Eki, tüm bölge için aynı anda kullanıma alınır. MariaDB için Azure veritabanı hizmeti, hizmet için ayrıntılı denetim veya özelleştirme gerektirmeyen bulut Yerel uygulaması için tasarlanmıştır.

## <a name="are-all-the-azure-regions-patched-at-the-same-time"></a>Tüm Azure bölgeleri aynı zamanda düzeltme eki uygulandı mı?

Hayır, dağıtım Wise pencere zamanlamaları sırasında tüm Azure bölgelerinin düzeltme eki uygulanır. Dağıtım Wise penceresi genellikle belirli bir Azure bölgesinde 5 PM-8 ile yerel saat olan bir sonraki günden uzatılır. Coğrafi eşlenmiş Azure bölgeleri, farklı günlerde düzeltme eki uygulanmış. Veritabanı sunucularının yüksek kullanılabilirlik ve iş sürekliliği için, [çapraz bölge okuma Çoğaltmalarından](./concepts-read-replicas.md#cross-region-replication) yararlanmak önerilir.

## <a name="retry-logic"></a>Yeniden deneme mantığı

Geçici arıza olarak da bilinen geçici bir hata, kendisini çözecek bir hatadır. Bakım sırasında [geçici hatalar](./concepts-connectivity.md#transient-errors) meydana gelebilir. Bu olayların çoğu, sistem tarafından 60 saniyeden daha az bir süre içinde otomatik olarak azalır. Geçici hatalar, [yeniden deneme mantığı](./concepts-connectivity.md#handling-transient-errors)kullanılarak işlenmelidir.


## <a name="next-steps"></a>Sonraki adımlar

- MariaDB için Azure veritabanı ile çalışmaya yönelik herhangi bir soru veya öneri için, adresindeki MariaDB ekibi için Azure veritabanı ekibine bir e-posta gönderin *AskAzureDBforMySQL@service.microsoft.com* .
- Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](howto-alert-metric.md) bölümüne bakın.
- [MariaDB için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
- [Geçici hataları işleyin ve MariaDB için Azure veritabanı 'na verimli bir şekilde bağlanın](concepts-connectivity.md)
