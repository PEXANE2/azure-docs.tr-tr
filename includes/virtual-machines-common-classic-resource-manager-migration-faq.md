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
ms.openlocfilehash: 57469bef7014010164234638f3d059ac96b125cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78383853"
---
## <a name="what-is-the-time-required-for-migration"></a>Geçiş için gereken süre nedir?

Göçün planlanması ve yürütülmesi büyük ölçüde mimarinin karmaşıklığına bağlıdır ve birkaç ay sürebilir.  

## <a name="what-is-the-definition-of-a-new-customer-on-iaas-vms-classic"></a>IaaS VM'lerde (klasik) yeni bir müşterinin tanımı nedir?

Şubat 2020 ayında (amortisman başlamadan bir ay önce) aboneliklerinde IaaS VM'leri (klasik) olmayan müşteriler yeni müşteri olarak kabul edilir. 

## <a name="what-is-the-definition-of-an-existing-customer-on-iaas-virtual-machines-classic"></a>IaaS Sanal Makineler (klasik) üzerinde mevcut bir müşteri tanımı nedir?

Şubat 2020 ayında aboneliklerinde IaaS VM'leri (Klasik) aktif olarak kullanan veya durduran veya tahsis eden müşteri, varolan bir müşteri olarak kabul edilir. VM'lerini Azure Servis Yöneticisi'nden Azure Kaynak Yöneticisi'ne geçirmek için yalnızca bu müşteriler 1 Mart 2023'e kadar zaman alabilir. 

## <a name="why-am-i-getting-an-error-stating-newclassicvmcreationnotallowedforsubscription"></a>Neden "NewClassicVMCreationNotAllowedForSubscription" belirten bir hata alıyorum?

Emeklilik sürecinin bir parçası olarak, IaaS VM (klasik) artık yeni müşteriler için kullanılabilir. Sizi yeni müşteriler olarak tanımladık ve bu nedenle operasyonunuz alıçlanmadı. [Azure Sanal Makineleri ARM kullanarak](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)kullanmanızı şiddetle öneririz. ARM kullanarak Azure VM'lerini kullanamıyorsanız, abonelik beyaz listesi için lütfen desteğe başvurun.

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Bu geçiş planı Azure sanal makinelerde çalışan mevcut hizmetlerimi ya da uygulamaların herhangi birini etkiliyor mu? 

1 Mart 2023'e kadar IaaS VM'ler için (klasik). IaaS VM'ler (klasik) genel kullanılabilirlik te tam olarak desteklenen hizmetlerdir. Microsoft Azure’da, ayak izinizi genişletmek için bu kaynakları kullanmaya devam edebilirsiniz. 1 Mart 2023'te, bu VM'ler tamamen kullanımdan kaldırılacak ve aktif veya tahsis edilmiş vm'ler & devre dışı bırakılacak. Bulut Hizmetleri (Klasik), Depolama Hesapları (Klasik) gibi diğer klasik kaynaklara hiçbir etkisi olmayacaktır.   

## <a name="what-happens-to-my-vms-if-i-dont-plan-on-migrating-in-the-near-future"></a>Yakın gelecekte geçirmeyi planlamadığım VM'lerime ne olur? 

1 Mart 2023'te, IaaS VM'ler (Klasik) tamamen kullanımdan kaldırılacak ve aktif veya tahsis edilmiş vm'ler devre dışı & durdurulacaktır. İş üzerindeki etkiyi önlemek için, geçişinizi bugün planlamaya başlamak ve 1 Mart 2023'te tamamlamak için son derece yeniden yorum yapıyoruz. Mevcut klasik API'leri, Bulut Hizmetleri'ni ve kaynak modelini küçümsülsülse değiliz. Resource Manager dağıtım modelinde kullanılabilen gelişmiş özellikleri dikkate alarak geçiş kolaylaştırmak istiyoruz. Bu kaynakları Azure Kaynak Yöneticisi'ne geçirmeyi planlamaya başlamanızı öneririz. 

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Bu geçiş planı, mevcut araçlarım için ne anlama geliyor? 

Resource Manager dağıtım modeli için araçlarınızı güncelleştirmek, geçiş planlarınızda hesaba katmanız gereken en önemli değişikliklerden biridir.

## <a name="how-long-will-the-management-plane-downtime-be"></a>Yönetim düzeyi kapalı kalma süresi ne kadar olacak? 

Bu, geçirilmekte olan kaynakların sayısına bağlıdır. Küçük dağıtımlar için (birkaç düzine VM) tüm geçişin bir saatten az sürmesi gerekir. Büyük ölçekli dağıtımlar (yüzlerce VM) için geçiş birkaç saat sürebilir.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Geçiş kaynaklarım Kaynak Yöneticisi'ne işlendikten sonra işlemi geri alabilir miyim? 

Kaynaklar hazır durumda olduğu sürece geçişinizi iptal edebilirsiniz. Kaynakların işleme süreci başarıyla tamamlandıktan sonra geri alma desteklenmez.

## <a name="can-i-roll-back-my-migration-if-the-commit-operation-fails"></a>İşleme süreci başarısız olursa geçişi geri alabilir miyim? 

İşleme süreci başarısız olursa geçişi geri alamazsınız. Kaydetme işlemi de dahil olmak üzere tüm geçiş işlemleri birden fazla kez denenebilir. Bu nedenle kısa bir süre sonra işlemi yeniden denemenizi öneririz. Hala bir hatayla karşı laşıyorsanız, bir destek bileti oluşturun.

## <a name="do-i-have-to-buy-another-express-route-circuit-if-i-have-to-use-iaas-under-resource-manager"></a>Kaynak Yöneticisi altında IaaS kullanmam gerekiyorsa başka bir hızlı yol devresi satın almam gerekir mi? 

Hayır. Yakın zamanda [ExpressRoute devrelerini klasikten Resource Manager dağıtım modeline taşıma](../articles/expressroute/expressroute-move.md) özelliğini etkinleştirdik. Bir ExpressRoute devreniz varsa yeni bir tane satın almak zorunda değilsiniz.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Klasik Iaas Kaynaklarım için Rol Tabanlı Erişim Denetimi yapılandırdıysam ne olur? 

Geçiş sırasında kaynaklar klasikten Resource Manager’a dönüşür. Bu nedenle geçişten sonra gerçekleşmesi gereken RBAC İlkesi güncelleştirmelerini planlamanızı öneririz.

## <a name="i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Klasik vm'lerimi bir kasada yedekledim. VM’lerimi klasik moddan Resource Manager moduna geçirip bunları bir Kurtarma Hizmetleri kasasında koruyabilir miyim?

Bir VM'yi klasikten Kaynak Yöneticisi moduna taşıdığınızda, geçişten önce alınan yedeklemeler yeni geçirilen Kaynak Yöneticisi VM'ye geçirilmeyecektir. Ancak, klasik VM yedeklemelerinizi tutmak istiyorsanız, geçişten önce aşağıdaki adımları izleyin. 

1. Kurtarma Hizmetleri kasasında, **Korumalı Öğeler** sekmesine gidin ve VM'yi seçin. 
2. Korumayı Durdur’a tıklayın. *İlişkili yedekleme verilerini sil* seçeneğini **işaretlenmemiş** olarak bırakın.

> [!NOTE]
> Verileri saklarsanız yedek örnek maliyeti tahsil edilir. Yedekleme kopyaları bekletme aralığına göre budanacaktır. Ancak, yedekleme verilerini açıkça silene kadar son yedekleme kopyası her zaman tutulur. Sanal makinenin saklama aralığınızı kontrol etmeniz ve bekletme aralığı sona erdiğinde kasadaki korumalı kalemde "Yedekleme Verilerini Sil"i tetiklemeniz önerilir. 
>
>

Sanal makineyi Kaynak Yöneticisi moduna geçirmek için, 

1. Yedekleme/anlık görüntü uzantısını VM'den silin.
2. Sanal makineyi, klasik moddan Resource Manager moduna geçirin. Sanal makineye karşılık gelen depolama ve ağ bilgilerinin de Resource Manager moduna geçirildiğinden emin olun.

Ayrıca, geçirilen VM yedeklemek istiyorsanız, [yedekleme sağlamak](../articles/backup/quick-backup-vm-portal.md#enable-backup-on-a-vm)için Sanal Makine yönetim bıçak gidin.

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

## <a name="i-got-the-message-vm-is-reporting-the-overall-agent-status-as-not-ready-hence-the-vm-cannot-be-migrated-ensure-that-the-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-the-vm-hence-this-vm-cannot-be-migrated"></a>Ben mesajı aldım *"VM Hazır değil olarak genel ajan durumunu bildiriyor. Bu nedenle, VM geçirilemez. VM Aracısı'nın genel aracı durumunu Hazır olarak bildirdiğinden emin olun"* veya *"VM, Durumu VM'den bildirilmeyen Uzantı içerir. Bu nedenle, bu VM geçirilemez."*

Bu ileti, VM’nin giden İnternet bağlantısı olmadığında alınır. VM aracısı, aracı durumunu güncelleştirmek için, giden bağlantı üzerinden her beş dakikada bir Azure depolama hesabına erişir.
