---
title: Azure Lab Services Hakkında | Microsoft Docs
description: Lab Services’in geliştiriciler, test uzmanları, eğitimciler, öğrenciler ve diğerleri tarafından kullanılabilen sanal makinelerle laboratuvar oluşturma, yönetme ve güvenli hale getirmeyi nasıl kolaylaştırabildiğini öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/03/2020
ms.author: spelluru
ms.openlocfilehash: ef296035ee8f0c80b4ee078303639d7aedf5a91d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76986640"
---
# <a name="an-introduction-to-azure-lab-services"></a>Azure Lab Services’a giriş
Azure'da bulutta laboratuvar ortamları ayarlamanızı sağlayan iki hizmet vardır. 

- **Azure DevTest Labs** - Bu hizmet, ekibiniz için hızlı bir ortam (örneğin: geliştirme ortamı veya buluttaki test ortamı) ayarlamanızı sağlar. Laboratuvar sahibi laboratuvarı oluşturur, Windows veya Linux sanal makineleri sağlar, gerekli yazılım ve araçları yükler ve laboratuvar kullanıcıları için kullanılabilir hale getirir. Laboratuvar kullanıcıları laboratuvardaki sanal makinelere (VM) bağlanır ve bunları günlük işleri, kısa vadeli projeleri için kullanır. Kullanıcılar laboratuvardaki kaynakları kullanmaya başladıktan sonra, laboratuvar yöneticisi birden fazla laboratuvardaki maliyet ve kullanımı analiz edebilir ve kuruluşunuzun veya takımınızın maliyetlerini en iyi duruma getirmeye yönelik kapsayıcı ilkeler ayarlayabilir.
- **Azure Laboratuvar Hizmetleri** - Bu hizmet, yönetilen laboratuvar türleri oluşturmanıza olanak tanır. Şu anda sınıf laboratuvarları, Azure Lab Hizmetleri tarafından desteklenen tek yönetilen laboratuvar türüdür. Hizmetin kendisi, VM'leri döndürmeden hataları işlemeye ve altyapıyı ölçeklemelere kadar yönetilen bir laboratuvar türü için tüm altyapı yönetimini işler. BT yöneticisi Azure Lab Hizmetleri'nde bir laboratuvar hesabı oluşturduktan sonra, bir eğitmen sınıfı için hızlı bir şekilde bir laboratuvar oluşturabilir, sınıfta egzersiz yapmak için gereken VM sayısını ve türünü belirtebilir ve kullanıcıları sınıfa ekleyebilir. Bir kullanıcı sınıfa kaydolduktan sonra, kullanıcı sınıf için alıştırmalar yapmak için VM'ye erişebilir.  

## <a name="key-capabilities"></a>Temel işlevler

Bu hizmetler (Azure DevTest Labs ve Azure Lab Hizmetleri) aşağıdaki temel özellikleri/özellikleri destekler:

- **Hızlı ve esnek bir laboratuvar kurulumu**. Laboratuvar sahipleri Azure Lab Services’i kullanarak gereksinimlerine uygun bir laboratuvarı hızlıca ayarlayabilir. Hizmet, yönetilen laboratuvar türleri için tüm Azure altyapı çalışmalarını üstlenme veya laboratuvar sahiplerinin laboratuvar sahibiaboneliğinde altyapıyı kendi kendini yönetmesini ve özelleştirmesini sağlama seçeneği sunar. Hizmet, sizin yerinize yönettiği laboratuvarlar için yerleşik ölçeklendirme ve esneklik özelliği sağlar.
- **Laboratuvar kullanıcıları için basitleştirilmiş deneyim**. Sınıf laboratuarı gibi yönetilen bir laboratuvar türünde, laboratuvar kullanıcıları kayıt kodu olan bir laboratuvara kaydolabilir ve laboratuvar kaynaklarını kullanmak için laboratuvara her zaman erişebilir. DevTest Labs hizmetinde oluşturulan bir laboratuvarda laboratuvar sahibi, laboratuvar kullanıcılarına sanal makine oluşturma ve sanal makinelere erişme, veri disklerini yönetme ve yeniden kullanma ve yeniden kullanılabilir gizli diziler ayarlama izinleri verebilir.  
- **Maliyet iyileştirme ve analizi**. Laboratuvar sahibi, sanal makineleri otomatik olarak kapatmak ve başlatmak için laboratuvar zamanlamaları ayarlayabilir. Laboratuvar sahibi, laboratuvarın sanal makinelerine kullanıcılar tarafından erişilebildiğinde zaman dilimlerini belirlemek üzere bir zamanlama ayarlayabilir, maliyeti iyileştirmek için kullanıcı ya da laboratuvar başına kullanım ilkeleri belirleyebilir ve bir laboratuvardaki kullanım ve etkinlik eğilimlerini analiz edebilir. Sınıf laboratuvarları gibi yönetilen laboratuvar türleri için, şu anda maliyet optimizasyonu ve analiz seçeneklerinin daha küçük bir alt kümesi mevcuttur.
- **Yerleşik güvenlik**. Laboratuvar sahibi, özel bir sanal ağ ve bir laboratuvar alt ağı oluşturabilir ve paylaşılan bir genel IP adresini etkinleştirebilir. Laboratuvar kullanıcıları, ExpressRoute veya siteden siteye VPN ile yapılandırılan sanal ağı kullanarak kaynaklara güvenle erişebilir. (şu anda yalnızca DevTest Labs ile kullanılabilir)
- **İş akışlarınız ve araçlarınızla tümleştirme**. Azure Lab Services, laboratuvarları kuruluşunuzun web sitesi ve yönetim sistemleri ile tümleştirme olanağı sağlar. Ortamları sürekli tümleştirme/sürekli dağıtım (CI/CD) araçlarınızın içinden otomatik olarak sağlayabilirsiniz. (şu anda yalnızca DevTest Labs ile kullanılabilir)

## <a name="scenarios"></a>Senaryolar

Azure DevTest Labs ve Azure Laboratuvar Hizmetleri'nin desteklediği senaryolardan bazıları şunlardır:

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Bulutta sınıfınız için yeniden boyutlandırılabilen bir bilgisayar laboratuvarı oluşturma  

- Yönetilen bir sınıf laboratuvarı oluşturun. Hizmete tam olarak ihtiyacınız olanları söylediğinizde laboratuvarın altyapısını sizin için oluşturup yönetir, böylece siz de laboratuvarın ayrıntılarına değil, ders vermeye odaklanabilirsiniz.
- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, sınıf çalışmaları için VM’leri kullanabilecekleri sınırlı sayıda saat verin.  
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin.
- İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin.

### <a name="use-devtest-labs-for-development-environments"></a>Geliştirme ortamları için DevTest Labs kullanma

Azure DevTest Labs, çok sayıda önemli senaryoyu uygulamak için kullanılabilir ancak başlıca senaryolardan biri, geliştiriciler için geliştirme makinelerini barındırmak üzere DevTest Labs kullanmayı içerir. Bu senaryoda DevTest Labs şu avantajları sağlar:

- Geliştiricilerin isteğe bağlı olarak geliştirme makinelerini hızlıca sağlaması.
- Yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamları sağlama
- Geliştiriciler, geliştirme makinelerini gerekli olan her durumda özelleştirebilir.
- Yöneticiler, geliştiricilerin geliştirme için gerektiğinde daha fazla VM alamamasını ve kullanımda olmadığında VM’lerin kapanmasını sağlayarak maliyetleri kontrol edebilir.

Daha fazla bilgi için bkz. [Geliştirme için DevTest Labs kullanma](devtest-lab-developer-lab.md).

### <a name="use-devtest-labs-for-test-environments"></a>Test ortamları için DevTest Labs kullanma

Azure DevTest Labs’i çok sayıda önemli senaryoyu uygulamak için kullanabilirsiniz ancak başlıca senaryolardan biri, test uzmanları için makineleri barındırmak üzere DevTest Labs kullanmayı içerir. Bu senaryoda DevTest Labs şu avantajları sağlar:

- Test uzmanları, yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızla sağlayabilir ve bu sayede uygulamalarının en son sürümünü test edebilir.
- Test uzmanları birden fazla test aracısı sağlayarak yük testlerinin ölçeğini artırabilirler.
- Yöneticiler, test uzmanlarının test için gerektiğinde daha fazla VM alamamasını ve kullanımda olmadığında VM’lerin kapanmasını sağlayarak maliyetleri kontrol edebilir.

Daha fazla bilgi için bkz. [Test için DevTest Labs kullanma](devtest-lab-test-env.md).

## <a name="types-of-labs"></a>Laboratuvar türleri
İki tür laboratuvar oluşturabilirsiniz: Azure Lab Hizmetleri ile **yönetilen laboratuvar türleri** ve Azure Lab Hizmetleri ile **laboratuvarlar.** Sadece bir laboratuvarda ihtiyacınız olanı girmeyi ve hizmetin laboratuvar için gerekli altyapıyı kurmasına ve yönetmesine izin vermek istiyorsanız, **yönetilen laboratuvar türlerinden**birini seçin. Şu **anda, sınıf laboratuarı** Azure Lab Hizmetleri ile oluşturabileceğiniz tek yönetilen laboratuvar türüdür. Kendi altyapınızı yönetmek istiyorsanız, **Azure DevTest Labs'ı**kullanarak bir laboratuvar oluşturun.

Aşağıdaki bölümlerde bu laboratuvarlar hakkında daha ayrıntılı bilgi verilmektedir. 

## <a name="managed-lab-types"></a>Yönetilen laboratuvar türleri
Azure Lab Services, altyapısı Azure tarafından yönetilen laboratuvarlar oluşturmanızı sağlar. Bu makalede, yönetilen laboratuvar türleri olarak ifade eder. Yönetilen laboratuvar türleri, özel gereksiniminize uygun farklı laboratuvar türleri sunar. Şu anda, desteklenen sadece yönetilen laboratuvar türü **sınıf laboratuarıdır.** 

Yönetilen laboratuvar türleri, en az kurulumla hemen başlamanızı sağlar. Hizmet, VM’leri tasarlamaktan hataları işlemeye ve altyapıyı ölçeklendirmeye varan tüm laboratuvar altyapısı yönetimi konularını ele alır.Sınıf laboratuarı gibi yönetilen bir laboratuvar türü oluşturmak için önce kuruluşunuz için bir laboratuvar hesabı oluşturmanız gerekir. Laboratuvar hesabı, kuruluştaki tüm laboratuvarların yönetildiği merkezi hesap olarak görev yapar. 

Bu yönetilen laboratuvar türlerinde Azure kaynakları oluşturduğunuzda ve kullandığınızda, hizmet dahili Microsoft aboneliklerinde kaynak oluşturur ve yönetir. Bunlar sizin Azure aboneliğinizde oluşturulmaz. Hizmet bu kaynakların dahili Microsoft aboneliklerindeki kullanımını takip eder. Bu kullanım, laboratuvar hesabını içeren Azure aboneliğinize faturalanır.   

**Yönetilen laboratuvar türleri için kullanım durumlarından**bazıları şunlardır: 

- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, ev ödevi veya kişisel projeleri için VM’leri kullanabilecekleri sınırlı sayıda saat verin.
- İşlem yoğunluklu veya grafik yoğunluklu araştırmalar gerçekleştirmek üzere yüksek performanslı işlem VM’leri içeren bir havuz oluşturun. Gerektiğinde VM’leri çalıştırın ve işiniz bittikten sonra makineleri temizleyin. 
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin.  
- Bir hackathon barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 


## <a name="devtest-labs"></a>DevTest Labs
Tüm altyapıyı ve yapılandırmayı kendi başınıza, kendi aboneliğiniz içinde yönetmek istediğiniz senaryolar olabilir. Bunu yapmak için, Azure portalda Azure DevTest Labs ile bir laboratuvar oluşturabilirsiniz.Bu laboratuvarlar için bir laboratuvar hesabı oluşturmanız gerekmez. Bu laboratuvarlar laboratuar hesabında (yönetilen laboratuvar türleri için var olan) görünmüyor.  

Burada **DevTest Labs kullanmak için kullanım durumlardan**bazıları şunlardır: 

- Bir hackathon veya bir konferansta uygulamalı oturum barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 
- Uygulamanızla yapılandırılan bir VM havuzu oluşturun ve takımınızın yoğun hata ayıklama için bir sanal makineyi kolayca kullanmasını sağlayın.  
- Geliştiricilere, ihtiyaç duydukları tüm araçlarla yapılandırılmış sanal makineler sağlayın. Maliyeti en aza indirmek için otomatik başlatma ve kapatma zamanlayın. 
- Dağıtımınızın bir parçası olarak tekrarlanan şekilde bir test makineleri laboratuvarı oluşturun. En son bitleri test edin ve işiniz bittiğinde test makinelerini temizleyin. 
- Farklı şekilde yapılandırılmış çeşitli sanal makineler ve ölçeklendirme ile performans testi için birden fazla test aracısı ayarlayın. 
- Ürününüzün en son sürümü ile yapılandırılmış bir laboratuvar kullanarak müşterilerinize eğitim oturumları sunun. Her müşteriye laboratuvarda kullanmak üzere sınırlı sayıda saat verin. 


## <a name="managed-lab-types-vs-devtest-labs"></a>Yönetilen laboratuvar türleri ve DevTest Labs
Aşağıdaki tabloda Azure Lab Services tarafından desteklenen iki laboratuvar türü karşılaştırılmaktadır: 

| Özellikler | Yönetilen laboratuvar türleri | DevTest Labs |
| -------- | ----------------- | ---------- |
| Laboratuvarda Azure altyapısı yönetimi. |  Hizmet tarafından otomatik olarak yönetilir | Kendi başınıza yönetirsiniz  |
| Altyapı sorunlarında yerleşik esneklik | Hizmet tarafından otomatik olarak gerçekleştirilir | Kendi başınıza yönetirsiniz  |
| Abonelik yönetimi | Hizmet, hizmeti destekleyen Microsoft abonelikleri içinde kaynak ayırmayı gerçekleştirir. Ölçeklendirme hizmet tarafından otomatik olarak gerçekleştirilir. | Kendi Azure aboneliğinizde kendi başınıza yönetirsiniz. Aboneliklerin otomatik olarak ölçeklenmesi yok. |
| Laboratuvar içinde Azure Resource Manager dağıtımı | Kullanılamaz | Kullanılabilir |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın: 

- [Sınıf Laboratuvarları Hakkında](./classroom-labs/classroom-labs-overview.md)
- [DevTest Laboratuvarları Hakkında](devtest-lab-overview.md)
