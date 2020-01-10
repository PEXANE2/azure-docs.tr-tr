---
title: Azure DevTest Labs 'de uygulama geçişi ve Tümleştirme
description: Bu makalede, uygulama geçişi ve tümleştirme bağlamında Azure DevTest Labs altyapısını idare etmek için rehberlik sunulmaktadır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 14641e9096fa9366334e9f7460ae55cda0e6c2e8
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644895"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---application-migration-and-integration"></a>Azure DevTest Labs altyapısını idare edin-uygulama geçişi ve Tümleştirme
Geliştirme/test laboratuvarı ortamınız kurulduktan sonra, aşağıdaki soruları düşünmeniz gerekir:

- Projeyi Proje takımınızda nasıl kullanacağınızı nasıl kullanıyorsunuz?
- Gerekli tüm kurumsal ilkeleri izlediğinizden ve uygulamanıza değer eklemek için çevikliği korumanıza nasıl emin olabilirsiniz?

## <a name="azure-marketplace-images-vs-custom-images"></a>Azure Market görüntüleri ile özel görüntüler

### <a name="question"></a>Soru
Ne zaman bir Azure Market görüntüsü kullanmalıyım? kendi özel kurumsal görüntümi?

### <a name="answer"></a>Yanıt
Belirli endişeleriniz veya kuruluş gereksinimleriniz yoksa, Azure Marketi varsayılan olarak kullanılmalıdır. Bazı yaygın örnekler şunlardır;

- Temel görüntünün bir parçası olarak bir uygulamanın eklenmesini gerektiren karmaşık yazılım kurulumu.
- Bir uygulamanın yüklenmesi ve kurulumu, bir Azure Marketi görüntüsüne eklenmek üzere işlem zamanının etkili bir şekilde kullanılması için birkaç saat sürebilir.
- Geliştiriciler ve test ediciler, bir sanal makineye hızlı bir şekilde erişim gerektirir ve yeni bir sanal makinenin kurulum süresini en aza indirmek ister.
- Tüm makineler için yerinde olması gereken uyumluluk veya yasal koşullar (örneğin, güvenlik ilkeleri).

Özel görüntülerin kullanılması, hafif olarak düşünülmemelidir. Artık bu temel görüntüler için VHD dosyalarını yönetmeniz gerektiği için daha fazla karmaşıklık sağlar. Ayrıca, bu temel görüntülerin yazılım güncelleştirmeleriyle düzenli olarak yayama yapmanız gerekir. Bu güncelleştirmeler yeni işletim sistemi (OS) güncelleştirmelerini ve yazılım paketinin kendisi için gereken güncelleştirme veya yapılandırma değişikliklerini içerir.

## <a name="formula-vs-custom-image"></a>Formül ve özel görüntü

### <a name="question"></a>Soru
Ne zaman özel görüntüden bir formül kullanmalıyım?

### <a name="answer"></a>Yanıt
Genellikle, Bu senaryodaki karar verme faktörü maliyetlidir ve yeniden kullanılır.

Birçok kullanıcının/laboratuvarın temel görüntünün en üstünde çok fazla yazılım gerektiren bir senaryonuz varsa, özel bir görüntü oluşturarak maliyeti azaltabilirsiniz. Bu, görüntünün bir kez oluşturulduğu anlamına gelir. Bu, sanal makinenin kurulum süresini ve kurulum gerçekleştiğinde çalışan sanal makine nedeniyle oluşan maliyeti azaltır.

Ancak, ek bir faktör yazılım paketinizdeki değişikliklerin sıklığıdır. Günlük derlemeler çalıştırır ve bu yazılımın kullanıcılarınızın sanal makinelerinde olması gerekiyorsa, özel bir görüntü yerine bir formül kullanmayı düşünün.

## <a name="use-custom-organizational-images"></a>Özel kuruluş görüntülerini kullanma

### <a name="question"></a>Soru
Özel kurumsal görüntülerimi DevTest Labs ortamına getirmek için kolayca tekrarlanabilir bir işlem ayarlayabilirim?

### <a name="answer"></a>Yanıt
[Bu videoya görüntü fabrikası düzenine](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/)bakın. Bu senaryo gelişmiş bir senaryodur ve sunulan betikler yalnızca örnek betiklerdir. Herhangi bir değişiklik gerekliyse, ortamınızda kullanılan betikleri yönetmeniz ve korumanız gerekir.

Azure Pipelines içinde özel bir görüntü işlem hattı oluşturmak için DevTest Labs kullanma:

- [Giriş: Azure DevTest Labs bir görüntü fabrikası ayarlayarak dakikalar içinde kullanıma hazırlamış VM 'Leri alın](https://blogs.msdn.microsoft.com/devtestlab/2016/09/14/introduction-get-vms-ready-in-minutes-by-setting-up-image-factory-in-azure-devtest-labs/)
- [Görüntü fabrikası – Bölüm 2! VM oluşturmak için Azure Pipelines ve fabrika Laboratuvarı ayarlama](https://blogs.msdn.microsoft.com/devtestlab/2017/10/25/image-factory-part-2-setup-vsts-to-create-vms-based-on-devtest-labs/)
- [Görüntü fabrikası – Bölüm 3: özel görüntüleri kaydetme ve birden çok laboratuvara dağıtma](https://blogs.msdn.microsoft.com/devtestlab/2018/01/10/image-factory-part-3-save-custom-images-and-distribute-to-multiple-labs/)
- [Video: Azure DevTest Labs ile özel görüntü fabrikası](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/)

## <a name="patterns-to-set-up-network-configuration"></a>Ağ yapılandırmasını ayarlama desenleri

### <a name="question"></a>Soru
Geliştirme ve test sanal makinelerinin herkese açık internet 'e ulaşamıyor Nasıl yaparım? emin misiniz? Ağ yapılandırmasını kurmak için önerilen desenler var mı?

### <a name="answer"></a>Yanıt
Evet. Dikkate alınması gereken iki nokta vardır: gelen ve giden trafik.

**Gelen trafik** : sanal makınenın genel IP adresi yoksa, İnternet tarafından erişilemez. Yaygın bir yaklaşım, bir kullanıcının genel IP adresi oluşturamayacak şekilde bir abonelik düzeyi ilkesinin ayarlanmış olmasını sağlamaktır.

**Giden trafik** : sanal makinelerin doğrudan ortak internet 'e gitmesini ve trafiği bir kurumsal güvenlik duvarı üzerinden zorunlu kılmak istiyorsanız, zorlamalı yönlendirme kullanarak trafiği Express Route veya VPN aracılığıyla yönlendirme sağlayabilirsiniz.

> [!NOTE]
> Proxy ayarları olmayan trafiği engelleyen bir ara sunucunuz varsa, laboratuvarın yapıt depolama hesabına özel durum eklemeyi unutmayın.

Sanal makineler veya alt ağlar için ağ güvenlik grupları da kullanabilirsiniz. Bu adım trafiğe izin vermek/engellemek için ek bir koruma katmanı ekler.

## <a name="new-vs-existing-virtual-network"></a>Yeni ve var olan sanal ağ karşılaştırması

### <a name="question"></a>Soru
DevTest Labs ortamım için yeni bir sanal ağ oluşturmalı ve var olan bir sanal ağı kullanıyor mıyım?

### <a name="answer"></a>Yanıt
Sanal makinelerinizin mevcut altyapıyla etkileşime ihtiyacı varsa, DevTest Labs ortamınızda var olan bir sanal ağı kullanmayı göz önünde bulundurmanız gerekir. Ayrıca, ExpressRoute kullanırsanız, aboneliklerde kullanılmak üzere atanan IP adresi alanınızı parçalara atabilmeniz için sanal ağlar/alt ağların miktarını en aza indirmek isteyebilirsiniz. Ayrıca, burada VNet eşleme modelini (hub-ışınsal-uç modeli) kullanmayı göz önünde bulundurmanız gerekir. Bu yaklaşım, belirli bir bölgedeki abonelikler arasında VNET/alt ağ iletişimini mümkün olmakla birlikte bölgeler arasında eşleme, Azure ağ iletişimi için bir güncel özelliktir.

Aksi halde, her DevTest Labs ortamının kendi sanal ağı olabilir. Ancak, abonelik başına sanal ağ sayısında [sınırlar](../azure-resource-manager/management/azure-subscription-service-limits.md) olduğunu unutmayın. Varsayılan miktar 50 ' dir, ancak bu sınır 100 olarak yükseltilebilir.

## <a name="shared-public-or-private-ip"></a>Paylaşılan, genel veya özel IP

### <a name="question"></a>Soru
Paylaşılan IP ile genel IP ve özel IP 'yi ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Siteden siteye VPN veya Express Route kullanıyorsanız, makinelerinizin iç ağınız aracılığıyla erişilebilir olması ve genel İnternet üzerinden erişilemez olması için özel IP 'Ler kullanmayı düşünün.

> [!NOTE]
> Laboratuvar sahipleri bu alt ağ ilkesini, yanlışlıkla VM 'Ler için genel IP adresleri oluşturmadığından emin olmak için değiştirebilir. Abonelik sahibinin ortak IP 'Lerin oluşturulmasını önleyecek bir abonelik ilkesi oluşturması gerekir.

Paylaşılan genel IP 'Leri kullanırken bir laboratuvardaki sanal makineler ortak bir IP adresi paylaşır. Bu yaklaşım, belirli bir abonelik için genel IP adresleri sınırlarına ulaşmaktan kaçınmak gerektiğinde yararlı olabilir.

## <a name="limits-of-number-of-virtual-machines-per-user-or-lab"></a>Kullanıcı veya laboratuvar başına sanal makine sayısı sınırları

### <a name="question"></a>Soru
Kullanıcı başına veya laboratuvar başına kaç sanal makine ayarlayabilirim?

### <a name="answer"></a>Yanıt
Kullanıcı başına veya laboratuvar başına sanal makine sayısını göz önünde bulundurarak, başlıca üç sorun vardır:

- Ekibin laboratuvardaki kaynaklar üzerinde harcayacağı **genel maliyet** . Birçok makine çalıştırmak kolaydır. Maliyetleri denetlemek için, bir mekanizma Kullanıcı başına ve/veya laboratuvar başına sanal makine sayısını sınırlandırmanız
- Bir laboratuvardaki sanal makinelerin toplam sayısı, kullanılabilir [abonelik düzeyi kotalarıyla](../azure-resource-manager/management/azure-subscription-service-limits.md) etkilendi. Üst limitlerden biri, abonelik başına 800 kaynak gruplarıdır. DevTest Labs Şu anda her VM için yeni bir kaynak grubu oluşturuyor (paylaşılan genel IP 'Ler kullanılmamışsa). Bir abonelikte 10 laboratuvar varsa, laboratuvarlar her laboratuvarda yaklaşık 79 sanal makineye (10 laboratuvarın kendisi için 800 üst sınırı – 10 kaynak grubu 79) uygun olabilir.
- Laboratuvar, Express Route aracılığıyla şirket içi ağa bağlıysa (örneğin), VNet/alt ağ için **TANıMLANMıŞ IP adresi alanları** mevcuttur. Laboratuvardaki VM 'Lerin oluşturulmasının başarısız olmamasını sağlamak için (hata: IP adresi alınamıyor), laboratuvar sahipleri, kullanılabilir IP adresi alanı ile birlikte Laboratuvar başına en fazla VM 'Leri belirtebilir.

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma

### <a name="question"></a>Soru
DevTest Labs Ortamumdaki Kaynak Yöneticisi şablonlarını nasıl kullanabilirim?

### <a name="answer"></a>Yanıt
Kaynak Yöneticisi şablonlarınızı [DevTest Labs makalesindeki ortamlar özelliğinde](devtest-lab-test-env.md) bahsedilen adımları kullanarak bir DevTest Labs ortamına dağıtırsınız. Temel olarak, Kaynak Yöneticisi şablonlarınızı bir git deposuna (Azure Repos veya GitHub) kontrol edersiniz ve [şablonlarınıza yönelik özel bir depoyu](devtest-lab-test-env.md) laboratuvara eklersiniz.

Geliştirme makinelerini barındırmak için DevTest Labs kullanıyorsanız bu senaryo yararlı olmayabilir, ancak üretim temsilcisi olan bir hazırlama ortamı oluşturuyorsanız faydalı olabilir.

Ayrıca, laboratuvar başına veya Kullanıcı başına sanal makine sayısının yalnızca laboratuvarda yerel olarak oluşturulan ve herhangi bir ortamda (Kaynak Yöneticisi şablonları) değil, yalnızca laboratuvar üzerinde oluşturulmuş makine sayısını sınırlayan bir değer de vardır.

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [DevTest Labs 'de ortamları kullanma](devtest-lab-test-env.md).