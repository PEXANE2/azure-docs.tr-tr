---
title: include dosyası
description: include dosyası
services: virtual-machines
author: tanmaygore
ms.service: virtual-machines
ms.topic: include
ms.date: 02/06/2020
ms.author: tagore
ms.custom: include file
ms.openlocfilehash: a9c045162e650b4468fb53676e367d2c658bf7cc
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77912588"
---
## <a name="what-is-the-time-required-for-migration"></a>Geçiş için gereken süre nedir?

Geçişin planlanması ve yürütülmesi büyük ölçüde mimarinin karmaşıklığına bağlıdır ve birkaç ay sürebilir.  

## <a name="what-is-the-definition-of-a-new-customer-on-iaas-vms-classic"></a>IaaS VM 'lerinde (klasik) yeni bir müşterinin tanımı nedir?

Febrauary 2020 (kullanım dışı bırakılmadan önce bir ay) için aboneliklerinde IaaS VM 'Leri (klasik) olmayan müşteriler yeni müşteri olarak kabul edilir. 

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Bu geçiş planı Azure sanal makinelerde çalışan mevcut hizmetlerimi ya da uygulamaların herhangi birini etkiliyor mu? 

IaaS VM 'Leri (klasik) için 1 Mart 2023 ' ye kadar değil. IaaS VM 'Leri (klasik) genel kullanıma yönelik olarak tam olarak desteklenen hizmetlerdir. Microsoft Azure’da, ayak izinizi genişletmek için bu kaynakları kullanmaya devam edebilirsiniz. 1 Mart 2023 ' de, bu VM 'Ler tamamen kullanımdan kaldırılacak ve etkin veya ayrılmış VM 'Ler serbest bırakılacak & durdurulur. Cloud Services (klasik), depolama hesapları (klasik) vb. gibi klasik diğer kaynaklara hiçbir etkisi olmaz.   

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>Yakın gelecekte geçirmeyi planlamadığım VM'lerime ne olur? 

1 Mart 2023 ' de, IaaS VM 'Leri (klasik) tamamen kullanımdan kaldırılacak ve etkin veya ayrılmış VM 'Ler serbest bırakılacak & durdurulur. İş etkisini engellemek için, şimdi geçişinizi planlamaya başlamak ve 1 Mart 2023 ' den önce bu öğeyi doldurmanız çok daha fazla yorum yaptık. Mevcut klasik API 'Leri, Cloud Services ve kaynak modelini kullanımdan kaldırmıyoruz. Resource Manager dağıtım modelinde kullanılabilen gelişmiş özellikleri dikkate alarak geçiş kolaylaştırmak istiyoruz. Bu kaynakları Azure Resource Manager geçirmeye yönelik planlamayı başlatmanız önerilir. 

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Bu geçiş planı, mevcut araçlarım için ne anlama geliyor? 

Resource Manager dağıtım modeli için araçlarınızı güncelleştirmek, geçiş planlarınızda hesaba katmanız gereken en önemli değişikliklerden biridir.

## <a name="how-long-will-the-management-plane-downtime-be"></a>Yönetim düzeyi kapalı kalma süresi ne kadar olacak? 

Bu, geçirilmekte olan kaynakların sayısına bağlıdır. Küçük dağıtımlar için (birkaç düzine VM) tüm geçişin bir saatten az sürmesi gerekir. Büyük ölçekli dağıtımlar (yüzlerce VM) için geçiş birkaç saat sürebilir.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Geçiş kaynaklarım Kaynak Yöneticisi'ne işlendikten sonra işlemi geri alabilir miyim? 

Kaynaklar hazır durumda olduğu sürece geçişinizi iptal edebilirsiniz. Kaynakların işleme süreci başarıyla tamamlandıktan sonra geri alma desteklenmez.

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>İşleme süreci başarısız olursa geçişi geri alabilir miyim? 

İşleme süreci başarısız olursa geçişi geri alamazsınız. Kaydetme işlemi de dahil olmak üzere tüm geçiş işlemleri birden fazla kez denenebilir. Bu nedenle kısa bir süre sonra işlemi yeniden denemenizi öneririz. Hala bir hata durumunda bir destek bileti oluşturun.

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>Kaynak Yöneticisi altında IaaS kullanmam gerekiyorsa başka bir hızlı yol devresi satın almam gerekir mi? 

Hayır. Yakın zamanda [ExpressRoute devrelerini klasikten Resource Manager dağıtım modeline taşıma](../articles/expressroute/expressroute-move.md) özelliğini etkinleştirdik. Bir ExpressRoute devreniz varsa yeni bir tane satın almak zorunda değilsiniz.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Klasik Iaas Kaynaklarım için Rol Tabanlı Erişim Denetimi yapılandırdıysam ne olur? 

Geçiş sırasında kaynaklar klasikten Resource Manager’a dönüşür. Bu nedenle geçişten sonra gerçekleşmesi gereken RBAC İlkesi güncelleştirmelerini planlamanızı öneririz.

## <a name="i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Klasik VM 'lerimi bir kasada yedekledim. VM’lerimi klasik moddan Resource Manager moduna geçirip bunları bir Kurtarma Hizmetleri kasasında koruyabilir miyim?

Bir VM 'yi klasik moddan Kaynak Yöneticisi moduna taşıdığınızda, geçişten önce alınan yedeklemeler, yeni geçirilen Kaynak Yöneticisi sanal makinesine geçirilmez. Ancak, klasik sanal makinelerinize ait yedeklemelerinizi korumak istiyorsanız geçişten önce bu adımları izleyin. 

1. Kurtarma Hizmetleri kasasında **korunan öğeler** sekmesine gidin ve VM 'yi seçin. 
2. Korumayı Durdur'ı tıklatın. *İlişkili yedekleme verilerini sil* seçeneğini **işaretlenmemiş** olarak bırakın.

> [!NOTE]
> Veri tutana kadar yedek örnek maliyeti ücretlendirilecektir. Yedekleme kopyaları, bekletme aralığına göre ayıklanacaktır. Ancak, yedekleme verilerini açıkça silinceye kadar son yedekleme kopyası her zaman tutulur. Bekletme aralığı bittikten sonra, sanal makinenin bekletme aralığınızı denetlemeniz ve kasadaki Korunan öğede "yedekleme verilerini silme" tetiklenmesi önerilir. 
>
>

Sanal makineyi Kaynak Yöneticisi moduna geçirmek için 

1. Yedekleme/anlık görüntü uzantısını VM'den silin.
2. Sanal makineyi, klasik moddan Resource Manager moduna geçirin. Sanal makineye karşılık gelen depolama ve ağ bilgilerinin de Resource Manager moduna geçirildiğinden emin olun.

Ayrıca, geçirilen VM 'yi yedeklemek istiyorsanız, [yedeklemeyi etkinleştirmek](../articles/backup/quick-backup-vm-portal.md#enable-backup-on-a-vm)Için sanal makine yönetimi dikey penceresine gidin.

## <a name="can-i-validate-my-subscription-or-resources-to-see-if-theyre-capable-of-migration"></a>Geçişe uygun olup olmadıklarını görmek için aboneliğimi ya da kaynaklarımı doğrulayabilir miyim? 

Evet. Platform destekli geçiş seçeneğinde, geçiş için hazırlanmanın ilk adımı kaynakları geçişe uygun olup olmadığını doğrulamaktır. Doğrulama işlemi başarısız olursa, geçiş işleminin neden tamamlanamadığına dair tüm nedenlerle ilgili iletiler alırsınız.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-the-iaas-resources-for-migration"></a>Geçiş için IaaS kaynaklarını hazırlarken bir kota hatasıyla karşılaşırsam ne olur? 

Geçiş işlemini durdurmanızı ve VM’leri geçirdiğiniz bölgede kotaları artırmak için bir destek isteği göndermenizi öneririz. Kota isteği onaylandıktan sonra geçiş adımlarını yeniden başlatabilirsiniz.

## <a name="how-do-i-report-an-issue"></a>Bir sorunu nasıl bildirebilirim? 

Geçiş hakkında sorularınızı ve sorunlarınızı ClassicIaaSMigration anahtar sözcüğü ile [VM forumumuza](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) gönderin. Tüm sorularınızı bu foruma göndermenizi öneririz. Destek anlaşmanız varsa, destek bileti de açabilirsiniz.

## <a name="what-if-i-dont-like-the-names-of-the-resources-that-the-platform-chose-during-migration"></a>Geçiş sırasında platformun seçtiği kaynak adlarını beğenmezsem ne yapabilirim? 

Klasik dağıtım modelinde adlarını özellikle belirttiğiniz tüm kaynaklar geçiş sırasında korunur. Bazı durumlarda yeni kaynaklar oluşturulur. Örneğin tüm VM’ler için ağ arabirimleri oluşturulur. Geçiş sırasında oluşturulan bu yeni kaynakların adlarını denetleme olanağını şu anda desteklemiyoruz. Bu özellik için oylarınızı [Azure geri bildirim forumuna](https://feedback.azure.com) girebilirsiniz.

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Yetkilendirme bağlantıları olan aboneliklerde kullanılan ExpressRoute devrelerini geçirebilir miyim? 

Çapraz abonelik yetkilendirme bağlantılar kullanan ExpressRoute devreleri kapalı kalma süresi olmadan otomatik olarak geçirilemez. Bunları elle nasıl geçirebileceğiniz hakkında yönergelerimiz vardır. Adımlar ve daha fazla bilgi için bkz. [ExpressRoute devrelerini ve ilgili sanal ağları klasikten Resource Manager dağıtım modeline geçirme](../articles/expressroute/expressroute-migration-classic-resource-manager.md).

## <a name="i-got-the-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated"></a>*"VM, genel aracı durumunu kullanılamıyor olarak raporluyor" iletisini aldım. Bu nedenle, VM geçirilemez. VM aracısının genel aracı durumunu Ready olarak bildirdiğinden emin olun "* veya *" VM, durumu VM 'den bildirilmeyen bir uzantı içeriyor. Bu nedenle, bu VM geçirilemez. "*

Bu ileti, VM’nin giden İnternet bağlantısı olmadığında alınır. VM aracısı, aracı durumunu güncelleştirmek için, giden bağlantı üzerinden her beş dakikada bir Azure depolama hesabına erişir.
