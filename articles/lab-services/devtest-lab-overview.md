---
title: Azure Devtest Labs Hakkında | Microsoft Dokümanlar
description: DevTest Laboratuvarlarının Azure sanal makinelerini oluşturmayı, yönetmeyi ve izlemeyi nasıl kolaylaştırabileceğini öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1b9eed3b-c69a-4c49-a36e-f388efea6f39
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: b7cd6bb1fd0377ca1440d9c667453df922aacbd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60561464"
---
# <a name="about-azure-devtest-labs"></a>Azure DevTest Labs hakkında
Azure DevTest Labs, ekiplerdeki geliştiricilerin onay beklemeden sanal makineleri (VM' ler) ve PaaS kaynaklarını verimli bir şekilde kendi kendine yönetmelerini sağlar.

DevTest Labs, önceden yapılandırılmış tabanlardan veya Azure Kaynak Yöneticisi şablonlarından oluşan laboratuvarlar oluşturur. Bunlar, ortam oluşturmak için kullanabileceğiniz tüm gerekli araçlara ve yazılımlara sahiptir. Saatler veya günler yerine birkaç dakika içinde ortamlar oluşturabilirsiniz.

DevTest Labs'ı kullanarak, aşağıdaki görevleri yaparak uygulamalarınızın en son sürümlerini test edebilirsiniz:

- Yeniden kullanılabilir şablonlar ve yapılar kullanarak Windows ve Linux ortamlarını hızla sağlar.
- İsteğe bağlı ortamlar sağlamak için, dağıtım işlem hattınızı DevTest Labs ile kolayca tümleştirin.
- Birden fazla test aracısı sağlayarak yük testinizi ölçeklendirin ve eğitim ve demolar için önceden sağlanmış ortamlar oluşturun.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/What-is-Azure-DevTest-Labs/player]

## <a name="capabilities"></a>Özellikler
DevTest Labs, VM'lerle çalışan geliştiricilere aşağıdaki özellikleri sağlar:

- Beşten az basit adımı izleyerek vm'leri hızlı bir şekilde oluşturun.
- Takım müşteri adayı veya merkezi BT tarafından yapılandırılan, onaylanan ve yetkilendirilen VM üslerinin seçilmiş bir listesinden seçim yapın.
- Tüm yazılım ve araçları zaten yüklü olan önceden oluşturulmuş özel görüntülerden VM'ler oluşturun. 
- VM'ler oluşturulduğunda yüklenen yazılımın en son yapılarıyla birleştirilmiş özel görüntüler olan formüllerden VM'ler oluşturun. 
- VM'lerde dağıtılan uzantılar olan yapıları, bunlar sağlandıktan sonra yükleyin.
- VM'lerde otomatik kapatma ve otomatik başlatma zamanlamalarını ayarlayın.
- Oluşturma işleminden geçmeden önceden oluşturulmuş bir VM talep edin.

DevTest Labs, PaaS ortamlarıyla çalışan geliştiricilere aşağıdaki özellikleri sağlar:

- Üçten az basit adımı izleyerek PaaS ortamlarını hızla oluşturmak için Kaynak Yöneticisi'ni kullanın.
- Takım müşteri adayı veya merkezi BT tarafından yapılandırılan ve yetkilendirilen Kaynak Yöneticisi şablonları listesinden seçim yapın.
- Bir laboratuvar bağlamında Azure'u keşfetmek için Kaynak Yöneticisi şablonu kullanarak boş bir kaynak grubunu (sandbox) döndürün.

DevTest Labs ayrıca merkezi BT'nin atıkları kontrol etmesini, kaynaklardaki maliyetleri optimize etmesini ve aşağıdaki görevleri yaparak bütçeler dahilinde kalmasını sağlar:  

- VM'lerde otomatik kapatma ve otomatik başlatma zamanlamalarını ayarlama.
- Kullanıcıların oluşturabileceği VM sayısına ilişkin ilkeleri ayarlama.
- Kullanıcıların seçtiği VM'lerin boyutları na ve galeri resimlerine ilişkin ilkeleri ayarlama.
- Maliyetleri izleme ve laboratuvarlarda hedefler belirleme.
- Gerekli eylemleri yapabilmeniz için laboratuvarlar için yüksek öngörülen maliyetler hakkında bilgilendirilme.

DevTest Labs, bulutta ortam oluşturma, yapılandırma ve yönetme de aşağıdaki avantajları sağlar.

## <a name="cost-control-and-governance"></a>Maliyet kontrolü ve yönetişim
DevTest Labs, aşağıdaki görevleri yapmanıza izin vererek maliyetleri kontrol etmeyi kolaylaştırır:

- [Laboratuvarlarınızda](devtest-lab-get-started-with-lab-policies.md)kullanıcı başına veya laboratuvar başına VM sayısı gibi ilkeler ayarlayın. 
- [Otomatik olarak kapatmak](devtest-lab-set-lab-policy.md) ve VM'leri başlatmak için ilkeler oluşturun.
- [Bütçeniz](devtest-lab-configure-cost-management.md)dahilinde kalmak için laboratuvarların içinde dönen VM'ler ve PaaS kaynaklarındaki maliyetleri izleyin.
- Laboratuvarlarınızın bağlamında kalın, böylece kaynakları bunların dışında çevirmeyin.

## <a name="quickly-get-to-ready-to-test"></a>Hızlı bir şekilde teste hazır olun
DevTest Labs, ekibinizin uygulamaları geliştirmesi ve test etmesi için ihtiyaç duyduğu her şeyle donatılmış önceden sağlanmış ortamlar oluşturmanıza olanak tanır. Uygulamanızın son iyi yapısının yüklü olduğu [ortamları talep](devtest-lab-add-claimable-vm.md) edin ve çalışmaya başlayın. Veya daha hızlı, daha yalın ortam oluşturma için kapları kullanın.

## <a name="create-once-use-everywhere"></a>Bir kez oluşturun, her yerde kullanın
Geliştirici ve test [ortamları](devtest-lab-create-environment-from-arm.md) oluşturmak için ekibinizde veya kuruluşunuzdaki PaaS ortam şablonlarını ve [yapılarını](add-artifact-repository.md) (hepsi kaynak denetiminde) yakalayın ve paylaşın.

## <a name="worry-free-self-service"></a>Sorunsuz self servis
DevTest Labs, geliştiricilerinizin ve test edenlerin önceden yapılandırılmış bir dizi kaynak kullanarak [IaaS VM'leri](devtest-lab-add-vm.md) ve [PaaS kaynaklarını](devtest-lab-create-environment-from-arm.md) hızlı ve kolay bir şekilde oluşturmalarını sağlar.

## <a name="use-iaas-and-paas-resources"></a>IaaS ve PaaS kaynaklarını kullanma 
Geliştiriciler ayrıca Kaynak Yöneticisi şablonlarını kullanarak Azure Hizmet Kumaşı kümeleri, Azure Uygulama Hizmeti'nin Web Uygulamaları özelliği ve SharePoint çiftlikleri gibi PaaS kaynaklarını da döndürebilir. Laboratuvarlarda PaaS'a başlamak [için, ortak ortam deposundan](devtest-lab-configure-use-public-environments.md) şablonları kullanın veya [laboratuarı kendi Git deponuza bağlayın.](devtest-lab-create-environment-from-arm.md#configure-your-own-template-repositories) Bütçeniz dahilinde kalmak için bu kaynakların maliyetlerini de izleyebilirsiniz.

## <a name="integrate-with-your-existing-toolchain"></a>Mevcut araç zincirinizle tümleştirme
Tercih ettiğiniz [sürekli tümleştirme (CI) aracı,](devtest-lab-integrate-ci-cd-vsts.md)entegre geliştirme ortamı (IDE) veya otomatik sürüm boru hattından doğrudan geliştirme/test ortamlarını sağlamak için önceden yapılmış eklentileri veya API'yi kullanın. Kapsamlı komut satırı aracını da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- DevTest Labs hakkında daha fazla bilgi edinmek için [DevTest Labs konseptlerine](devtest-lab-concepts.md)bakın.
- Adım adım yönergeleri içeren bir yol için [Bkz.](tutorial-create-custom-lab.md)