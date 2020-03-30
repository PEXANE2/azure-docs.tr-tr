---
title: Azure DevTest Labs'da uygulama geçişi ve tümleştirme
description: Bu makalede, uygulama geçişi ve tümleştirme bağlamında Azure DevTest Labs altyapısının yönetimi için kılavuz sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644895"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---application-migration-and-integration"></a>Azure DevTest Labs altyapısının yönetimi - Uygulama geçişi ve tümleştirme
Geliştirme/test laboratuarı ortamınız oluşturulduktan sonra aşağıdaki soruları düşünmeniz gerekir:

- Proje ekibiniz içindeki ortamı nasıl kullanıyorsunuz?
- Gerekli kuruluş ilkelerini izlemenizi ve uygulamanız için değer katmak için çevikliği korumanızı nasıl sağlarsınız?

## <a name="azure-marketplace-images-vs-custom-images"></a>Azure Marketi görüntüleri ve özel görüntüler

### <a name="question"></a>Soru
Azure Marketi görüntüsünü kendi özel kuruluş resmime karşı ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Azure Marketi, belirli endişeleriniz veya kuruluş gereksinimleriniz yoksa varsayılan olarak kullanılmalıdır. Bazı yaygın örnekler şunlardır;

- Temel görüntünün bir parçası olarak bir uygulamanın eklenmesini gerektiren karmaşık yazılım kurulumu.
- Bir uygulamanın yüklenmesi ve kurulumu, Azure Marketi görüntüsüne eklenecek bilgi işlem süresinin verimli bir şekilde kullanılması olmayan saatler sürebilir.
- Geliştiriciler ve sınayıcılar sanal makineye hızlı bir şekilde erişmeye ihtiyaç duyar ve yeni bir sanal makinenin kurulum süresini en aza indirmek ister.
- Tüm makineler için olması gereken uyumluluk veya düzenleyici koşullar (örneğin, güvenlik ilkeleri).

Özel görüntülerin kullanılması hafife alınmamalıdır. Şimdi bu temel görüntüler için VHD dosyaları yönetmek zorunda olarak, ekstra karmaşıklık tanıtmak. Ayrıca rutin yazılım güncellemeleri ile bu temel görüntüleri yama gerekir. Bu güncelleştirmeler, yeni işletim sistemi (OS) güncelleştirmelerini ve yazılım paketinin kendisi için gereken güncelleştirmeleri veya yapılandırma değişikliklerini içerir.

## <a name="formula-vs-custom-image"></a>Formül vs özel görüntü

### <a name="question"></a>Soru
Özel görüntüye karşı bir formül ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Genellikle, bu senaryoda belirleyici faktör maliyet ve yeniden.

Birçok kullanıcı/laboratuvarda temel görüntünün üstünde çok sayıda yazılım içeren bir görüntü gerektiren bir senaryonuz varsa, özel bir görüntü oluşturarak maliyeti azaltabilirsiniz. Bu, görüntünün bir kez oluşturulduğu anlamına gelir. Sanal makinenin kurulum süresini ve kurulum gerçekleştiğinde çalışan sanal makine nedeniyle oluşan maliyeti azaltır.

Ancak, dikkat edilmesi gereken ek bir faktör yazılım paketinizdeki değişikliklerin sıklığıdır. Günlük yapılar çalıştırıyorsanız ve bu yazılımın kullanıcılarınızın sanal makinelerinde olmasını istiyorsanız, özel bir resim yerine bir formül kullanmayı düşünün.

## <a name="use-custom-organizational-images"></a>Özel kuruluş görüntüleri kullanma

### <a name="question"></a>Soru
Özel kuruluş görüntülerimi DevTest Labs ortamına getirmek için kolayca tekrarlanabilir bir işlemi nasıl ayarlayabilirim?

### <a name="answer"></a>Yanıt
[Image Factory deseni bu videoyu](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/)izleyin. Bu senaryo gelişmiş bir senaryodur ve sağlanan komut dosyaları yalnızca örnek komut dosyalarıdır. Herhangi bir değişiklik gerekiyorsa, ortamınızda kullanılan komut dosyalarını yönetmeniz ve korumanız gerekir.

Azure Ardışık Hatları'nda özel bir görüntü ardışık oluşturmak için DevTest Labs'ı kullanma:

- [Giriş: Azure DevTest Labs'da bir görüntü fabrikası kurarak VM'leri dakikalar içinde hazırlayın](https://blogs.msdn.microsoft.com/devtestlab/2016/09/14/introduction-get-vms-ready-in-minutes-by-setting-up-image-factory-in-azure-devtest-labs/)
- [Görüntü Fabrikası - Bölüm 2! Sanal M'ler Oluşturmak için Azure Ardışık Hatlar Ve Fabrika Laboratuvarı'nı kurulum](https://blogs.msdn.microsoft.com/devtestlab/2017/10/25/image-factory-part-2-setup-vsts-to-create-vms-based-on-devtest-labs/)
- [Görüntü Fabrikası - Bölüm 3: Özel Görüntüleri Kaydet ve Birden Fazla Laboratuvara Dağıt](https://blogs.msdn.microsoft.com/devtestlab/2018/01/10/image-factory-part-3-save-custom-images-and-distribute-to-multiple-labs/)
- [Video: Azure DevTest Labs ile Özel Görüntü Fabrikası](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/)

## <a name="patterns-to-set-up-network-configuration"></a>Ağ yapılandırmasını ayarlamak için desenler

### <a name="question"></a>Soru
Sanal makinelerin genel internete erişememesini nasıl sağlarım? Ağ yapılandırmasını ayarlamak için önerilen desenler var mı?

### <a name="answer"></a>Yanıt
Evet. Göz önünde bulundurulması gereken iki yönü vardır - gelen ve giden trafik.

**Gelen trafik** – Sanal makinenin genel bir IP adresi yoksa, internet üzerinden ulaşılamıyor. Ortak bir yaklaşım, hiçbir kullanıcının ortak bir IP adresi oluşturamayacak şekilde abonelik düzeyi ilkesinin ayarlıolduğundan emin olmaktır.

**Giden trafik** – Sanal makinelerin doğrudan genel internete girmesini önlemek ve trafiği kurumsal bir güvenlik duvarı üzerinden zorlamak istiyorsanız, zorunlu yönlendirme yi kullanarak trafiği ekspres rota veya VPN üzerinden şirket içinde yönlendirebilirsiniz.

> [!NOTE]
> Proxy ayarları olmadan trafiği engelleyen bir proxy sunucunuz varsa, laboratuvarın yapı depolama hesabına özel durumlar eklemeyi unutmayın.

Sanal makineler veya alt ağlar için ağ güvenlik gruplarını da kullanabilirsiniz. Bu adım, / blok trafik izin vermek için ek bir koruma katmanı ekler.

## <a name="new-vs-existing-virtual-network"></a>Yeni ve mevcut sanal ağ

### <a name="question"></a>Soru
DevTest Labs ortamım için varolan bir sanal ağı kullanma m evrilen yeni bir sanal ağ ne zaman oluşturmalıyım?

### <a name="answer"></a>Yanıt
Sanal Bilgilerinizin varolan altyapıyla etkileşimde olması gerekiyorsa, DevTest Labs ortamınızda varolan bir sanal ağ kullanmayı düşünmelisiniz. Buna ek olarak, ExpressRoute kullanıyorsanız, aboneliklerde kullanılmak üzere atanan IP adres alanınızı parçalamayın diye VNet/Subnets miktarını en aza indirmek isteyebilirsiniz. Ayrıca burada VNet eşleme deseni (Hub-Spoke modeli) kullanmayı düşünmelisiniz. Bu yaklaşım, belirli bir bölgedeki abonelikler arasında vnet/subnet iletişimini sağlar, ancak bölgeler arasında bakmak Azure ağında ki güncel bir özelliktir.

Aksi takdirde, her DevTest Labs ortamı kendi sanal ağı olabilir. Ancak, abonelik başına sanal ağ sayısında [sınırlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md) olduğunu unutmayın. Varsayılan tutar 50'dir, ancak bu sınır 100'e yükseltilebilir.

## <a name="shared-public-or-private-ip"></a>Paylaşılan, herkese açık veya özel IP

### <a name="question"></a>Soru
Paylaşılan IP ile ortak IP'yi ne zaman kullanmalıyım?

### <a name="answer"></a>Yanıt
Siteden siteye VPN veya Express Route kullanıyorsanız, makinelerinize dahili ağınızdan erişilebilmek ve herkese açık internet üzerinden erişilememek için özel IP'ler kullanmayı düşünün.

> [!NOTE]
> Laboratuvar sahipleri, kimsenin yanlışlıkla VM'leri için ortak IP adresleri oluşturmadığından emin olmak için bu alt net ilkesini değiştirebilir. Abonelik sahibi, genel IP'lerin oluşturulmasını engelleyen bir abonelik ilkesi oluşturmalıdır.

Paylaşılan ortak IP'leri kullanırken, laboratuardaki sanal makineler ortak bir IP adresini paylaşır. Bu yaklaşım, belirli bir abonelik için ortak IP adreslerinin sınırlarını ihlal etmekten kaçınmanız gerektiğinde yararlı olabilir.

## <a name="limits-of-number-of-virtual-machines-per-user-or-lab"></a>Kullanıcı veya laboratuvar başına sanal makine sayısının sınırları

### <a name="question"></a>Soru
Kullanıcı başına veya laboratuvar başına kaç sanal makine ayarlamam gerektiğikonusunda bir kural var mı?

### <a name="answer"></a>Yanıt
Kullanıcı başına veya laboratuvar başına sanal makine sayısı göz önüne alındığında, üç ana endişe vardır:

- Takımın laboratuvardaki kaynaklara harcayabileceği **toplam maliyet.** Birçok makineyi döndürmek kolaydır. Maliyetleri kontrol etmek için, bir mekanizma kullanıcı ve/veya laboratuvar başına VM sayısını sınırlamaktır
- Laboratuvardaki toplam sanal makine sayısı, kullanılabilir [abonelik düzeyi kotalarından](../azure-resource-manager/management/azure-subscription-service-limits.md) etkilenir. Üst sınırlardan biri abonelik başına 800 kaynak grubudur. DevTest Labs şu anda her VM için yeni bir kaynak grubu oluşturur (paylaşılan ortak IP'ler kullanılmadığı sürece). Bir abonelikte 10 laboratuvar varsa, laboratuvarlar her laboratuvara yaklaşık 79 sanal makine sığdırabilir (10 laboratuvar için 800 üst sınır – 10 kaynak grubu) = laboratuvar başına 79 sanal makine.
- Laboratuvar, Express Route (örneğin) üzerinden şirket içi bağlantıya bağlıysa, VNet/Subnet için **tanımlanmış IP adresi alanları** vardır. Laboratuvardaki VM'lerin oluşturulmamasını sağlamak için (hata: IP adresi alamıyorum), laboratuvar sahipleri kullanılabilir IP adresi alanıyla uyumlu laboratuvar başına maksimum VM'leri belirtebilir.

## <a name="use-resource-manager-templates"></a>Resource Manager şablonlarını kullanma

### <a name="question"></a>Soru
DevTest Labs Ortamımda Kaynak Yöneticisi şablonlarını nasıl kullanabilirim?

### <a name="answer"></a>Yanıt
Kaynak Yöneticisi [şablonlarınızı, DevTest laboratuarları makalesinde Ortamlar özelliğinde](devtest-lab-test-env.md) belirtilen adımları kullanarak DevTest Labs ortamına dağıtırsınız. Temel olarak, Kaynak Yöneticisi şablonlarınızı git deposunda (Azure Repos veya GitHub) denetler ve şablonlarınız için laboratuvara özel bir [depo eklersiniz.](devtest-lab-test-env.md)

Geliştirme makinelerini barındırmak için DevTest Labs kullanıyorsanız bu senaryo yararlı olmayabilir, ancak üretimi temsil eden bir evreleme ortamı oluşturuyorsanız yararlı olabilir.

Ayrıca, laboratuvar başına veya kullanıcı başına sanal makine sayısının, herhangi bir ortam (Kaynak Yöneticisi şablonları) tarafından değil, yalnızca laboratuvarda yerel olarak oluşturulan makine sayısını sınırladığını belirtmekte yarar vardır.

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [DevTest Labs'daki kullanım ortamları.](devtest-lab-test-env.md)