---
title: Yedekleme merkezi-SSS
description: Bu makalede yedekleme merkezi hakkında sık sorulan sorular yanıtlanmaktadır
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: 7e227eb6a14d06791e52ec33e090afdcb01bab61
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094050"
---
# <a name="backup-center---frequently-asked-questions"></a>Yedekleme merkezi-sık sorulan sorular

## <a name="management"></a>Yönetim

### <a name="can-backup-center-be-used-across-tenants"></a>Yedekleme merkezi kiracılar genelinde kullanılabilir mi?

Evet, [Azure açık Thouse](https://docs.microsoft.com/azure/lighthouse/overview) kullanıyorsanız ve farklı kiracılar genelinde abonelikler için erişim temsilcisi seçtiyseniz, kiracılar genelinde yedeklemeleri yönetmek Için Backup Center 'ı tek bir cam bölmesi olarak kullanabilirsiniz.

### <a name="can-backup-center-be-used-to-manage-both-recovery-services-vaults-and-backup-vaults"></a>Yedekleme merkezi, hem kurtarma hizmetleri kasalarını hem de yedekleme kasalarını yönetmek için kullanılabilir mi?

Evet, yedekleme merkezi hem [Kurtarma Hizmetleri kasalarını](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) hem de [yedekleme](backup-vault-overview.md)kasalarını yönetebilir.

### <a name="is-there-a-delay-before-data-surfaces-in-backup-center"></a>Yedekleme merkezindeki veri yüzeylerinden önce bir gecikme var mı?

Yedekleme merkezi, gerçek zamanlı bilgiler sağlamaya yönelik olarak tasarlanmıştır. Bir varlığın tek bir kasa ekranında gösterdiği zaman ve aynı varlığın yedekleme merkezi 'nde gösterdiği zaman arasında birkaç saniye gecikme olabilir.

## <a name="configuration"></a>Yapılandırma

### <a name="do-i-need-to-configure-anything-to-see-data-in-backup-center"></a>Verileri yedekleme merkezi 'nde görmek için bir şey yapılandırmam gerekiyor mu?

Hayır. Yedekleme merkezi kullanıma hazır hale gelir. Bununla birlikte, yedekleme merkezi altındaki [yedekleme raporlarını](https://docs.microsoft.com/azure/backup/configure-reports) görüntülemek için kasalarınız için raporlamayı yapılandırmanız gerekir.

### <a name="do-i-need-to-have-any-special-permissions-to-use-backup-center"></a>Yedekleme merkezi 'ni kullanmak için özel izinlere sahip olmam gerekiyor mu?

Yedekleme merkezi 'nin bu şekilde herhangi bir yeni izin gerekmez. Yönettiğiniz kaynaklara yönelik doğru Azure RBAC erişiminizin düzeyine sahip olduğunuz sürece bu kaynaklar için yedekleme merkezi 'ni kullanabilirsiniz. Örneğin, yedeklemeleriniz hakkındaki bilgileri görüntülemek için kasalarınıza **okuyucu** erişimine sahip olmanız gerekir. Yedeklemeyi yapılandırmak ve yedeklemeyle ilgili diğer eylemleri gerçekleştirmek için **yedek katkıda bulunan** veya **yedekleme işletmeni** rollerinin olması gerekir. [Azure Backup Için Azure rolleri](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault)hakkında daha fazla bilgi edinin.

## <a name="pricing"></a>Fiyatlandırma

### <a name="do-i-need-to-pay-anything-extra-to-use-backup-explorer"></a>Yedekleme Gezginini kullanmak için ek ücret ödemem gerekiyor mu?

Şu anda yedekleme merkezini kullanmak için ek maliyet (yedekleme maliyetlerinizden ayrı) yoktur. Ancak yedekleme merkezi altında [yedekleme raporları](https://docs.microsoft.com/azure/backup/configure-reports) kullanıyorsanız, yedekleme raporları Için Azure izleyici günlüklerini kullanmanın bir [maliyeti](https://azure.microsoft.com/pricing/details/monitor/) vardır.

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

* [Kurtarma Hizmetleri kasaları hakkında sık sorulan sorular](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq)
* [Azure VM yedeklemeleri hakkında sık sorulan sorular](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq)
