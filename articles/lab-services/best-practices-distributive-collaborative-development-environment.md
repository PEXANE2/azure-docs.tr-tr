---
title: Azure DevTest Labs kaynaklarının dağıtılmış işbirliği ne kadar gelişmiş
description: DevTest Labs kaynaklarını geliştirmek için dağıtılmış ve işbirliğine dayalı bir geliştirme ortamı ayarlamak için en iyi uygulamaları sağlar.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 9469591b1945adaffca973828d619d5d06655262
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170115"
---
# <a name="best-practices-for-distributed-and-collaborative-development-of-azure-devtest-labs-resources"></a>Azure DevTest Labs kaynaklarının dağıtılmış ve işbirliğine dayalı gelişimi için en iyi uygulamalar
Dağıtılmış işbirlikçi geliştirme, farklı ekiplerin veya kişilerin bir kod tabanı geliştirmesine ve sürdürmesine olanak tanır. Başarılı olmak için geliştirme süreci, bilgi oluşturma, paylaşma ve tümleştirme yeteneğine bağlıdır. Bu temel geliştirme ilkesi Azure DevTest Labs'da kullanılabilir. Bir laboratuvarda, kuruluş içindeki farklı laboratuvarlar arasında yaygın olarak dağıtılan çeşitli kaynak türleri vardır. Farklı kaynak türleri iki alana odaklanır:

- Laboratuvarda dahili olarak depolanan kaynaklar (laboratuvar tabanlı)
- [Laboratuvara bağlı dış depolarda](devtest-lab-add-artifact-repo.md) depolanan kaynaklar (kod deposu tabanlı). 

Bu belge, tüm düzeylerde özelleştirme ve kalite sağlarken birden çok takım arasında işbirliği ve dağıtıma izin veren bazı en iyi uygulamaları açıklar.

## <a name="lab-based-resources"></a>Laboratuvar tabanlı kaynaklar

### <a name="custom-virtual-machine-images"></a>Özel sanal makine görüntüleri
Laboratuvarlara her gece dağıtılan özel görüntülerin ortak bir kaynağına sahip olabilirsiniz. Ayrıntılı bilgi için [Bkz. Resim fabrikası.](image-factory-create.md)    

### <a name="formulas"></a>Formüller
[Formüller](devtest-lab-manage-formulas.md) laboratuvara özgüdir ve dağıtım mekanizması yoktur. Laboratuvar üyeleri formüllerin tüm geliştirme yapmak. 

## <a name="code-repository-based-resources"></a>Kod deposu tabanlı kaynaklar
Kod depolarını, yapıları ve ortamları temel alan iki farklı özellik vardır. Bu makale, kullanılabilir yapıları ve ortamları kuruluş düzeyinde veya takım düzeyinde özelleştirme olanağı sağlamak için depoları ve iş akışını en etkili şekilde nasıl ayarlayacaklarını ve özelliklerin üzerinden geçer.  Bu iş akışı standart [kaynak kodu denetimi dallandırma stratejisini](/azure/devops/repos/tfvc/branching-strategies-with-tfvc?view=azure-devops)temel adamaktadır. 

### <a name="key-concepts"></a>Önemli kavramlar
Yapılar için kaynak bilgileri meta veriler, komut dosyaları içerir. Ortamlar için kaynak bilgileri, PowerShell komut dosyaları, DSC komut dosyaları, Zip dosyaları gibi destekleyici dosyaları içeren meta veriler ve Kaynak Yöneticisi şablonlarını içerir.  

### <a name="repository-structure"></a>Depo yapısı  
Kaynak kodu denetimi (SCC) için en yaygın yapılandırma, laboratuarlarda kullanılan kod dosyalarını (Kaynak Yöneticisi şablonları, meta veriler ve komut dosyaları) depolamak için çok katmanlı bir yapı ayarlamaktır. Özellikle, işletmenin farklı düzeylerinde yönetilen kaynakları depolamak için farklı depolar oluşturun:   

- Şirket çapında kaynaklar.
- İş birimi/Bölüm çapındaki kaynaklar
- Takıma özel kaynaklar.

Bu düzeylerin her biri, ana dalı üretim kalitesinde olması gereken farklı bir depoya bağlanır. Her depodaki [dallar,](/azure/devops/repos/git/git-branching-guidance?view=azure-devops) bu belirli kaynakların (yapılar veya şablonlar) geliştirilmesi için olacaktır. Birden çok depoyu ve birden çok dalı aynı anda kuruluşun laboratuvarlarına kolayca bağlayabildiğiniz için bu yapı DevTest Labs ile iyi bir şekilde hizalanır. Depo adı, aynı adlar, açıklamalar ve yayımcı olduğunda karışıklığı önlemek için kullanıcı arabirimine (Kullanıcı Arabirimi) dahildir.
     
Aşağıdaki diyagram iki depo gösterir: BT Bölümü tarafından korunan bir şirket deposu ve R&D bölümü tarafından korunan bir bölüm deposu.

![Örnek dağıtım ve işbirliğine dayalı geliştirme ortamı](./media/best-practices-distributive-collaborative-dev-env/distributive-collaborative-dev-env.png)
   
Bu katmanlı yapı, ana dalda daha yüksek bir kalite düzeyini tutarken, bir laboratuvara bağlı birden fazla depoya sahip olmak daha fazla esneklik sağlarken, geliştirmeyi sağlar.

## <a name="next-steps"></a>Sonraki adımlar    
Aşağıdaki makalelere bakın:

- [Azure portalını](devtest-lab-add-artifact-repo.md) kullanarak veya [Azure Kaynak Yönetimi şablonu](add-artifact-repository.md) aracılığıyla laboratuvara depo ekleme
- [DevTest Labs eserler](devtest-lab-artifact-author.md)
- [DevTest Labs ortamları.](devtest-lab-create-environment-from-arm.md)
