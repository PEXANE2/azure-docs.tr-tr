---
title: Azure DevTest Labs hakkında | Microsoft Docs
description: DevTest Labs 'in Azure sanal makinelerini oluşturmayı, yönetmeyi ve izlemeyi nasıl kolaylaştırdığını öğrenin
ms.topic: article
ms.date: 06/20/2020
ms.openlocfilehash: b7e6493e5fb1c99dfa7962ed464dca3ebc6b0c7e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85480210"
---
# <a name="about-azure-devtest-labs"></a>Azure DevTest Labs hakkında
Azure DevTest Labs, ekiplerde bulunan geliştiricilerin, onayları beklemeden sanal makineleri (VM 'Ler) ve PaaS kaynaklarını verimli bir şekilde kendi kendine yönetmesine olanak sağlar.

DevTest Labs, önceden yapılandırılmış tabanlardan veya Azure Resource Manager şablonlarından oluşan laboratuvarlar oluşturur. Bunlar, ortamları oluşturmak için kullanabileceğiniz tüm gerekli araçlara ve yazılımlara sahiptir. Birkaç dakika içinde, saatler veya günler yerine ortamları oluşturabilirsiniz.

DevTest Labs 'i kullanarak aşağıdaki görevleri gerçekleştirerek uygulamalarınızın en son sürümlerini test edebilirsiniz:

- Yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızlı bir şekilde sağlayın.
- İsteğe bağlı ortamlar sağlamak için, dağıtım işlem hattınızı DevTest Labs ile kolayca tümleştirin.
- Çoklu test aracıları sağlayarak ve eğitim ve tanıtımlar için önceden sağlanmış ortamlar oluşturarak yük testinizi ölçeklendirin.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]

## <a name="capabilities"></a>Özellikler
DevTest Labs, VM 'lerle çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Beş basit adımdan daha azını izleyerek VM 'Leri hızlı bir şekilde oluşturun.
- Ekip lideri veya merkezi BT tarafından yapılandırılan, onaylanan ve yetkilendirilmiş VM temellerine ait seçkin bir listeden seçim yapın.
- Önceden oluşturulmuş özel görüntülerden, tüm yazılım ve araçların zaten yüklü olduğu VM 'Ler oluşturun. 
- VM 'Ler oluşturulduğunda yüklenen yazılımın en son Derlemeleriyle birlikte özel görüntüler içeren formüllerden VM 'Ler oluşturun. 
- VM 'Ler sağlandıktan sonra dağıtılan uzantılar olan yapıtları yükler.
- VM 'lerde otomatik olarak kapatmaları ve otomatik başlatma zamanlamalarını ayarlayın.
- Oluşturma işlemine geçmeden önceden oluşturulmuş bir VM talep edin.

DevTest Labs, PaaS ortamları ile çalışan geliştiriciler için aşağıdaki özellikleri sağlar:

- Üç basit adımdan daha azını izleyerek PaaS ortamlarını hızlıca oluşturmak için Kaynak Yöneticisi kullanın.
- Ekip lideri veya merkezi BT tarafından yapılandırılan ve yetkilendirilmiş Kaynak Yöneticisi şablonlarının seçkin bir listesinden seçim yapın.
- Laboratuvar bağlamı dahilinde Azure 'ı araştırmak için bir Kaynak Yöneticisi şablonu kullanarak boş bir kaynak grubu (Sandbox) çalıştırın.

DevTest Labs Ayrıca, bu BT 'nin wastes 'yi denetlemesine, kaynakların maliyetlerini iyileştirmesine ve aşağıdaki görevleri gerçekleştirerek bütçelerde kalmalarına olanak sağlar:  

- VM 'lerde otomatik kapanmaya ve otomatik başlatma zamanlamalarını ayarlama.
- Kullanıcıların oluşturabileceğiniz VM sayısı için ilke ayarlama.
- Kullanıcıların seçim yapan VM boyutları ve Galeri görüntülerinde ilke ayarlama.
- Laboratuvarlarda izleme ve ayar hedefleri.
- Labs için yüksek oranda tasarlanan maliyetler hakkında bildirim almak için gerekli işlemleri yapabilirsiniz.

DevTest Labs, bulutta ortamları oluşturmak, yapılandırmak ve yönetmek için aşağıdaki avantajları sağlar.

## <a name="cost-control-and-governance"></a>Maliyet denetimi ve idare
DevTest Labs aşağıdaki görevleri gerçekleştirerek maliyetleri denetlemenizi kolaylaştırır:

- [Laboratuvarınızda](devtest-lab-set-lab-policy.md)Kullanıcı başına veya laboratuvar başına sanal makine sayısı gibi ilkeler ayarlayın. 
- Sanal makineleri [otomatik olarak kapatmak ve başlatmak için ilkeler](devtest-lab-set-lab-policy.md) oluşturun.
- Sanal makinelerde ve PaaS kaynaklarında bulunan maliyetleri izleyerek [bütçelerinize](devtest-lab-configure-cost-management.md)devam etmek için laboratuvarların içinde devam edin.
- Bunların dışındaki kaynakları kaçırmayın, laboratuvarlarınızın bağlamı dahilinde kalın.

## <a name="quickly-get-to-ready-to-test"></a>Hızlı bir şekilde çalışmaya kolayca başlayın
DevTest Labs, ekibinizin uygulama geliştirmek ve test etmek için ihtiyaç duyacağı her şeyi içeren önceden sağlanmış ortamlar oluşturmanızı sağlar. Yalnızca uygulamanızın en son iyi yapısının yüklendiği ve çalışmaya başlayacağı [ortamları talep](devtest-lab-add-claimable-vm.md) edin. Ya da daha da çok daha hızlı, çevre oluşturma ortamı oluşturmak için kapsayıcıları kullanın.

## <a name="create-once-use-everywhere"></a>Bir kez oluşturun, her yerde kullanın
Kolayca geliştirici ve test ortamları oluşturmak için, takımınızın veya kuruluşunuzun içinde PaaS [ortam şablonlarını](devtest-lab-create-environment-from-arm.md) ve [yapıtları](add-artifact-repository.md) (tüm kaynak denetimi) yakalayıp paylaşabilirsiniz.

## <a name="worry-free-self-service"></a>Sorunsuz Self Servis
DevTest Labs, geliştiricilerin ve test edenlerin, önceden yapılandırılmış bir küme kullanarak [IaaS VM 'leri](devtest-lab-add-vm.md) ve [PaaS kaynaklarını](devtest-lab-create-environment-from-arm.md) hızlı ve kolay bir şekilde oluşturmalarına olanak sağlar.

## <a name="use-iaas-and-paas-resources"></a>IaaS ve PaaS kaynaklarını kullanma 
Geliştiriciler ayrıca, Kaynak Yöneticisi şablonları kullanarak Azure Service Fabric kümeleri, Azure App Service ve SharePoint gruplarının Web Apps özelliği gibi PaaS kaynaklarını da çalıştırabilir. Laboratuvarlarda PaaS 'yi kullanmaya başlamak için, [genel ortam deposundaki](devtest-lab-configure-use-public-environments.md) şablonları kullanın veya [Laboratuvarı kendi git deponuza bağlayın](devtest-lab-create-environment-from-arm.md#configure-your-own-template-repositories). Ayrıca, bütçenize devam etmek için bu kaynaklardaki maliyetleri izleyebilirsiniz.

## <a name="integrate-with-your-existing-toolchain"></a>Mevcut araç zinciriyle tümleştirin
Doğrudan tercih ettiğiniz [sürekli tümleştirme (CI) aracından](devtest-lab-integrate-ci-cd.md), tümleşik geliştirme ORTAMıNDAN (IDE) veya otomatik sürüm işlem hattınızdan geliştirme/test ortamları sağlamak için önceden hazırlanmış EKLENTILER veya API kullanın. Kapsamlı komut satırı aracını da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- DevTest Labs hakkında daha fazla bilgi edinmek için bkz. [DevTest Labs kavramları](devtest-lab-concepts.md).
- Adım adım yönergeler içeren bir anlatım için bkz. [öğretici: Azure DevTest Labs kullanarak laboratuvar ayarlama](tutorial-create-custom-lab.md).