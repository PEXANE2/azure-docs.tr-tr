---
title: Kapsayıcı örnekleri için güvenlik
description: Azure Kapsayıcı Örnekleri için görüntüleri ve sırları güvenli hale getirmek için öneriler ve herhangi bir kapsayıcı platformu için genel güvenlik konuları
ms.topic: article
ms.date: 01/10/2020
ms.custom: ''
ms.openlocfilehash: b5f2c4d9ca80318574e288110fd4ce7f490af00d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76260506"
---
# <a name="security-considerations-for-azure-container-instances"></a>Azure Kapsayıcı Örnekleri için güvenlik hususları

Bu makalede, kapsayıcı uygulamaları çalıştırmak için Azure Kapsayıcı Örnekleri kullanmak için güvenlik konuları tanıtıştır. Konu başlıkları şunlardır:

> [!div class="checklist"]
> * Azure Kapsayıcı Örnekleri için görüntüleri ve sırları yönetmek için **güvenlik önerileri**
> * Herhangi bir konteyner platformu için konteyner yaşam döngüsü boyunca **konteyner ekosistemi** için dikkat edilmesi gerekenler

## <a name="security-recommendations-for-azure-container-instances"></a>Azure Kapsayıcı Örnekleri için güvenlik önerileri

### <a name="use-a-private-registry"></a>Özel kayıt defteri kullanma

Kapsayıcı, bir veya daha fazla depoda depolanan görüntülerden oluşturulur. Bu depolar [Docker Hub](https://hub.docker.com)gibi bir genel kayıt defterine veya özel bir kayıt defterine ait olabilir. Özel bir kayıt defteri örneği olarak şirket içinde veya sanal özel bulutta kullanılabilen [Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/) gösterilebilir. [Azure Kapsayıcı Kayıt Defteri](../container-registry/container-registry-intro.md)de dahil olmak üzere bulut tabanlı özel kapsayıcı kayıt defteri hizmetlerini de kullanabilirsiniz. 

Genel kullanıma açık bir kapsayıcı görüntüsü güvenliği garanti etmez. Kapsayıcı görüntüler birden çok yazılım katmanından oluşur ve her yazılım katmanında güvenlik açıkları olabilir. Saldırı tehdidini azaltmaya yardımcı olmak için, Azure Konteyner Kayıt Defteri veya Docker Trusted Registry gibi özel bir kayıt defterinden görüntüleri depolamalı ve almalısınız. Azure Kapsayıcı Kayıt Defteri, yönetilen bir özel kayıt defteri sağlamanın yanı sıra, temel kimlik doğrulama akışları için Azure Etkin Dizini aracılığıyla hizmet temel kimlik [doğrulamasını](../container-registry/container-registry-authentication.md) destekler. Bu kimlik doğrulaması, salt okunur (çekme), yazma (itme) ve diğer izinler için rol tabanlı erişimi içerir.

### <a name="monitor-and-scan-container-images"></a>Konteyner görüntülerini izleme ve tarayıp taz

Özel bir kayıt defterindeki kapsayıcı görüntülerini tarayabilir ve olası güvenlik açıklarını belirlemek için çözümlerden yararlanın. Farklı çözümlerin sağladığı tehdit algılamanın derinliğini anlamak önemlidir.

Örneğin, Azure Kapsayıcı Kayıt Defteri isteğe bağlı olarak [Azure Güvenlik Merkezi ile entegre edilip,](../security-center/azure-container-registry-integration.md) bir kayıt defterine itilen tüm Linux görüntülerini otomatik olarak tarar. Azure Güvenlik Merkezi'nin tümleşik Qualys tarayıcısı görüntü açıklarını algılar, sınıfa tanır ve düzeltme kılavuzu sağlar.

[Twistlock](https://azuremarketplace.microsoft.com/marketplace/apps/twistlock.twistlock?tab=Overview) ve [Aqua Security](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) gibi güvenlik izleme ve görüntü tarama çözümleri de Azure Marketi'nde kullanılabilir.  

### <a name="protect-credentials"></a>Kimlik bilgilerini koruma

Kapsayıcılar çeşitli kümelere ve Azure bölgelerine yayılabilir. Bu nedenle, parolalar veya belirteçler gibi oturum açma veya API erişimi için gereken kimlik bilgilerini güvence altına almanız gerekir. Yalnızca ayrıcalıklı kullanıcıların bu kapsayıcılara geçiş sırasında ve istirahatte erişebilmesini sağlayın. Tüm kimlik bilgilerini envantere çıkarın ve ardından geliştiricilerin kapsayıcı platformlar için tasarlanmış yeni ortaya çıkan sırlar yönetimi araçlarını kullanmalarını gerektirir.  Çözümünüzde şifreli veritabanları, aktarımsırasındaki sırlar için TLS şifreleme ve en az ayrıcalıklı [rol tabanlı erişim denetimi](../role-based-access-control/overview.md)bulunduğundan emin olun. [Azure Key Vault,](../key-vault/key-vault-secure-your-key-vault.md) kapsayıcı uygulamalar için şifreleme anahtarlarını ve sırlarını (sertifikalar, bağlantı dizeleri ve parolalar gibi) koruyan bir bulut hizmetidir. Bu veriler hassas ve iş açısından kritik olduğundan, yalnızca yetkili uygulamaların ve kullanıcıların bunlara erişebilmeleri için anahtar kasalarınıza güvenli erişim.

## <a name="considerations-for-the-container-ecosystem"></a>Konteyner ekosistemi için dikkat edilecek hususlar

İyi uygulanan ve etkin bir şekilde yönetilen aşağıdaki güvenlik önlemleri, konteyner ekosisteminizi güvence altına almanıza ve korumanıza yardımcı olabilir. Bu önlemler, geliştirmeden üretim dağıtımına ve bir dizi konteyner orkestratör, ana bilgisayar ve platforma kadar konteyner yaşam döngüsü boyunca geçerlidir. 

### <a name="use-vulnerability-management-as-part-of-your-container-development-lifecycle"></a>Konteyner geliştirme yaşam döngünüzün bir parçası olarak güvenlik açığı yönetimini kullanın 

Kapsayıcı geliştirme yaşam döngüsü boyunca etkili güvenlik açığı yönetimini kullanarak, daha ciddi bir sorun haline gelmeden önce güvenlik sorunlarını belirleme ve çözme olasılığınız artar. 

### <a name="scan-for-vulnerabilities"></a>Güvenlik açıklarını taramaya 

Her zaman yeni güvenlik açıkları keşfedilir, bu nedenle güvenlik açıklarını tarama ve tanımlama sürekli bir işlemdir. Kapsayıcı yaşam döngüsü boyunca güvenlik açığı taramasını birleştirin:

* Geliştirme ardışık alanınızda son bir denetim olarak, görüntüleri genel veya özel bir kayıt defterine itmeden önce kapsayıcılarda bir güvenlik açığı tayini gerçekleştirmeniz gerekir. 
* Hem geliştirme sırasında gözden kaçırılan kusurları belirlemek hem de kapsayıcı görüntülerinde kullanılan kodda bulunan yeni keşfedilen güvenlik açıklarını gidermek için kayıt defterindeki kapsayıcı görüntülerini taramaya devam edin.  

### <a name="map-image-vulnerabilities-to-running-containers"></a>Çalışan kapsayıcılara görüntü açıklarını haritalamak 

Güvenlik sorunlarının azaltılaabilmesi veya çözülebilmeleri için kapsayıcı görüntülerinde tanımlanan güvenlik açıklarını çalıştıran kapsayıcılara eşleme aracı olmanız gerekir.  

### <a name="ensure-that-only-approved-images-are-used-in-your-environment"></a>Ortamınızda yalnızca onaylı görüntülerin kullanıldığından emin olun 

Bilinmeyen konteynerlere izin vermeden bir konteyner ekosisteminde yeterli değişim ve değişkenlik vardır. Yalnızca onaylı kapsayıcı görüntülerine izin verin. Onaylanmamış kapsayıcı görüntülerinin kullanımını izlemek ve önlemek için araçlara ve işlemlere sahip olmak. 

Saldırı yüzeyini azaltmanın ve geliştiricilerin kritik güvenlik hataları yapmasını önlemenin etkili bir yolu, konteyner görüntülerinin geliştirme ortamınıza akışını denetlemektir. Örneğin, olası saldırılar için yüzeyi en aza indirmek için tek bir Linux dağıtımını temel görüntü olarak, tercihen yalın (Ubuntu yerine Alp veya CoreOS) olarak onaylayabilirsiniz. 

Görüntü imzalama veya parmak izi, kapsayıcıların bütünlüğünü doğrulamanızı sağlayan bir gözetim zinciri sağlayabilir. Örneğin, Azure Kapsayıcı Kayıt Defteri, görüntü yayıncılarının bir kayıt defterine itilen görüntüleri imzalamasına ve tüketicilerin yalnızca imzalı görüntüleri çekmesine olanak tanıyan Docker'ın [içerik güven](https://docs.docker.com/engine/security/trust/content_trust) modelini destekler.

### <a name="permit-only-approved-registries"></a>Yalnızca onaylı kayıt defterlerine izin verme 

Ortamınızın yalnızca onaylı görüntüler kullanmasını sağlamanın bir uzantısı, yalnızca onaylı konteyner kayıtlarının kullanımına izin vermektir. Onaylı konteyner kayıtlarının kullanılması, bilinmeyen güvenlik açıklarının veya güvenlik sorunlarının ortaya çıkması potansiyelini sınırlayarak riske maruz kalmanızı azaltır. 

### <a name="ensure-the-integrity-of-images-throughout-the-lifecycle"></a>Yaşam döngüsü boyunca görüntülerin bütünlüğünü sağlama 

Kapsayıcı yaşam döngüsü boyunca güvenliği yönetmenin bir parçası da, kayıt defterindeki kapsayıcı görüntülerinin bütünlüğünü sağlamak ve bunlar değiştirildikçe veya üretime dağıtılındığından emin olmaktır. 

* Güvenlik açıkları olan görüntülerin, küçük de olsa, üretim ortamında çalışmasına izin verilmemelidir. İdeal olarak, üretimde dağıtılan tüm görüntüler, belirli birkaç kişi tarafından erişilebilen özel bir kayıt defterine kaydedilmelidir. Etkili bir şekilde yönetilebildiklerinden emin olmak için üretim görüntülerinin sayısını küçük tutun.

* Yazılımın kaynağını genel olarak kullanılabilen bir kapsayıcı görüntüsünden saptamak zor olduğundan, katmanın kökeni hakkında bilgi sahibi olmak için kaynaktan görüntüler oluşturun. Kendi oluşturdukları bir kapsayıcı görüntüsünde bir güvenlik açığı olduğunda müşteriler çözüme daha hızlı ulaşabilir. Herkese açık bir görüntüyle, müşterilerin bunu düzeltmek veya yayımcıdan başka bir güvenli görüntü almak için herkese açık bir resmin kökünü bulmaları gerekir. 

* Üretimde dağıtılan tam olarak taranmış bir görüntünün uygulamanın kullanım ömrü boyunca güncel olacağı garanti edilmez. Görüntünün önceden bilinmeyen veya üretim dağıtımından sonra sunulan katmanlarında güvenlik açıkları bildirilebilir. 

  Güncel liğini yitirmiş veya bir süredir güncelleştirilemeyen görüntüleri belirlemek için üretimde dağıtılan görüntüleri düzenli olarak denetler. Kapsayıcı görüntüleri kapalı kalmadan güncelleştirmek için mavi-yeşil dağıtım yöntemlerini ve yuvarlanma yükseltme mekanizmalarını kullanabilirsiniz. Önceki bölümde açıklanan araçları kullanarak görüntüleri tyapabilirsiniz. 

* Güvenli görüntüler oluşturmak ve bunları özel kayıt defterinize itmek için entegre güvenlik taraması içeren sürekli bir tümleştirme (CI) ardışık hattı kullanın. CI çözümüne yerleşik güvenlik açığı taraması, tüm sınamaları geçen görüntülerin üretim iş yüklerinin dağıtıldığı özel kayıt defterine gönderilmesini sağlar. 

  CI boru hattı hatası, güvenlik açığı olan görüntülerin üretim iş yükü dağıtımları için kullanılan özel kayıt defterine itilmemesini sağlar. Ayrıca, önemli sayıda görüntü varsa görüntü güvenliği taramaotomatikleştirir. Aksi durumda, görüntüleri güvenlik açıkları için el ile denetlemek çok zor olabileceği gibi hata riskini de artırır. 

### <a name="enforce-least-privileges-in-runtime"></a>Çalışma zamanında en az ayrıcalıkları zorla 

En az ayrıcalık kavramı, kapsayıcılar için de geçerli olan temel bir güvenlik en iyi uygulamasıdır. Bir güvenlik açığından yararlanıldığında, genellikle saldırgana erişim ve gizliliği ihlal edilen uygulama veya işlemle eşit ayrıcalıklar sağlar. Kapsayıcıların en düşük ayrıcalıklarla çalışmasını ve işi halletmek için gereken erişimin riske maruz kalmanızı azaltır. 

### <a name="reduce-the-container-attack-surface-by-removing-unneeded-privileges"></a>Gereksiz ayrıcalıkları kaldırarak konteyner saldırı yüzeyini azaltın 

Ayrıca, kullanılmayan veya gereksiz işlemleri veya ayrıcalıkları kapsayıcı çalışma zamanından kaldırarak olası saldırı yüzeyini en aza indirebilirsiniz. Ayrıcalıklı kapsayıcılar kök olarak çalıştırın. Kötü amaçlı bir kullanıcı veya iş yükü ayrıcalıklı bir kapsayıcıda kaçarsa, kapsayıcı bu sistemde kök olarak çalışır.

### <a name="preapprove-files-and-executables-that-the-container-is-allowed-to-access-or-run"></a>Kapsayıcının erişmesine veya çalıştırmasına izin verilen dosyaları ve yürütülebilir leri önceden onaylama 

Değişken veya bilinmeyen sayısını azaltmak, istikrarlı ve güvenilir bir ortamı korumanıza yardımcı olur. Kapsayıcılara yalnızca önceden onaylanmış veya güvenli listelenmiş dosyalara ve yürütülebilir dosyalara erişebilmeleri veya çalıştırabilmeleri için sınırlama, riske maruzkalmanın sınırlandırılması için kanıtlanmış bir yöntemdir.  

Bir güvenli listeyi en başından itibaren yönetmek çok daha kolaydır. Güvenli bir liste, uygulamanın düzgün çalışması için hangi dosyaların ve yürütülebilir lerin gerekli olduğunu öğrendikçe, denetim ve yönetilebilirlik ölçüsü sağlar. 

Bir safelist sadece saldırı yüzeyini azaltır ama aynı zamanda anomaliler için bir temel sağlayabilir ve "gürültülü komşu" ve konteyner koparma senaryoları kullanımını önlemek. 

### <a name="enforce-network-segmentation-on-running-containers"></a>Çalışan kapsayıcılarda ağ bölümlemesini zorlama  

Bir alt ağdaki kapsayıcıları başka bir alt ağdaki güvenlik risklerinden korumaya yardımcı olmak için ağ bölümlemasını (veya nano segmentasyonunu) veya çalışan kapsayıcılar arasında ayrım yapmayı koruyun. Uyumluluk görevlerini yerine getirmek için gerekli olan endüstrilerde kapsayıcıları kullanmak için ağ bölümlemesi de gerekebilir.  

Örneğin, ortak araç [Aqua](https://azuremarketplace.microsoft.com/marketplace/apps/aqua-security.aqua-security?tab=Overview) nano-segmentasyon için otomatik bir yaklaşım sağlar. Aqua, konteyner ağ etkinliklerini çalışma zamanında izler. Diğer kapsayıcılara, hizmetlere, IP adreslerine ve genel internete gelen ve giden tüm ağ bağlantılarını tanımlar. Nano segmentasyon, izlenen trafiğe göre otomatik olarak oluşturulur. 

### <a name="monitor-container-activity-and-user-access"></a>Kapsayıcı etkinliğini ve kullanıcı erişimini izleme 

Herhangi bir BT ortamında olduğu gibi, şüpheli veya kötü amaçlı etkinlikleri hızlı bir şekilde tanımlamak için etkinliği ve konteyner ekosisteminize kullanıcı erişimini sürekli olarak izlemeniz gerekir. Azure, şunları içeren kapsayıcı izleme çözümleri sağlar:

* [Kapsayıcılar için Azure Monitörü,](../azure-monitor/insights/container-insights-overview.md) Azure Kubernetes Hizmeti'nde (AKS) barındırılan Kubernetes ortamlarına dağıtılan iş yüklerinizin performansını izler. Kapsayıcılar için Azure Monitör, Ölçümler API'si aracılığıyla Kubernetes'te bulunan denetleyicilerden, düğümlerden ve kapsayıcılardan bellek ve işlemci ölçümleri toplayarak performans görünürlüğü sağlar. 

* [Azure Kapsayıcı İzleme çözümü,](../azure-monitor/insights/containers.md) diğer Docker ve Windows kapsayıcı ana bilgisayarlarını tek bir konumda görüntülemenize ve yönetmenize yardımcı olur. Örnek:

  * Kapsayıcılarla birlikte kullanılan komutları gösteren ayrıntılı denetim bilgilerini görüntüleyin. 
  * Docker veya Windows ana bilgisayarlarını uzaktan görüntülemek zorunda kalmadan merkezi günlükleri görüntüleyerek ve arayarak kapsayıcıları sorun giderin.  
  * Gürültülü olabilecek kapsayıcıları bulun ve ana bilgisayarda fazla kaynakları tüketin.
  * Kapsayıcılar için merkezi cpu, bellek, depolama ve ağ kullanımını ve performans bilgilerini görüntüleyin.  

  Çözüm, Docker Swarm, DC/OS, yönetilmeyen Kubernetes, Service Fabric ve Red Hat OpenShift gibi konteyner orkestratörlerini destekler. 

### <a name="monitor-container-resource-activity"></a>Kapsayıcı kaynak etkinliğini izleme 

Kapsayıcılarınızın eriştisi dosyalar, ağ ve diğer kaynaklar gibi kaynak etkinliğinizi izleyin. Kaynak etkinliğini ve tüketimini izlemek hem performans izleme hem de güvenlik önlemi olarak yararlıdır. 

[Azure Monitor,](../azure-monitor/overview.md) ölçümlerin, etkinlik günlüklerinin ve tanılama günlüklerinin toplanmasına izin vererek Azure hizmetleri için temel izleme sağlar. Örneğin, etkinlik günlüğü size yeni kaynakların ne zaman oluşturulduğunu veya değiştirildiğini bildirir. 

  Farklı kaynaklar için ve hatta bir sanal makinenin içindeki işletim sistemi için bile performans istatistikleri sağlayan ölçümler kullanılabilir. Azure portalında gezginlerden biriyle bu verileri görüntüleyebilir ve bu ölçümlere dayalı uyarılar oluşturabilirsiniz. Azure İzleyici, en hızlı ölçüm işlem hattını (5 dakika ila 1 dakika arasında) sağlar, bu nedenle zaman açısından kritik uyarılar ve bildirimler için bunu kullanmalısınız. 

### <a name="log-all-container-administrative-user-access-for-auditing"></a>Denetim için tüm konteyner yönetim kullanıcı erişimini günlüğe kaydetme 

Kubernetes kümeniz, konteyner kayıt defteriniz ve konteyner görüntüleriniz de dahil olmak üzere konteyner ekosisteminize yönetime erişimin doğru bir denetim izini koruyun. Bu günlükler denetim amaçları için gerekli olabilir ve herhangi bir güvenlik olayından sonra adli kanıt olarak yararlı olacaktır. Azure çözümleri şunları içerir:

* Küme ortamının güvenlik yapılandırmasını izlemek ve güvenlik önerileri oluşturmak için [Azure Kubernetes Hizmetinin Azure Güvenlik Merkezi ile tümleştirilmesi](../security-center/azure-kubernetes-service-integration.md)
* [Azure Kapsayıcı İzleme çözümü](../azure-monitor/insights/containers.md)
* Azure Kapsayıcı Örnekleri ve [Azure Kapsayıcı Kayıt Defteri](../container-registry/container-registry-diagnostics-audit-logs.md) için kaynak [günlükleri](container-instances-log-analytics.md)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Güvenlik [Merkezi'ni](../security-center/container-security.md) konteyner ortamlarınızda gerçek zamanlı tehdit algılama için kullanma hakkında daha fazla bilgi edinin.

* [Twistlock](https://www.twistlock.com/solutions/microsoft-azure-container-security/) ve [Aqua Security](https://www.aquasec.com/solutions/azure-container-security/)çözümleriyle konteyner güvenlik açıklarını yönetme hakkında daha fazla bilgi edinin.