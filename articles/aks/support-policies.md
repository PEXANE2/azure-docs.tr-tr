---
title: Azure Kubernetes Hizmeti (AKS) için destek ilkeleri
description: Azure Kubernetes Hizmeti (AKS) destek ilkeleri, paylaşılan sorumluluk ve önizlemede olan özellikler (veya alfa veya beta) hakkında bilgi edinin.
services: container-service
author: jnoller
ms.topic: article
ms.date: 01/24/2020
ms.author: jenoller
ms.openlocfilehash: a5d90106a85a61cbf499c4c08130392b922a45f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593589"
---
# <a name="support-policies-for-azure-kubernetes-service"></a>Azure Kubernetes Hizmeti için destek ilkeleri

Bu makalede, Azure Kubernetes Hizmeti (AKS) için teknik destek ilkeleri ve sınırlamaları hakkında ayrıntılı bilgi verilmektedir. Makalede ayrıca işçi düğümü yönetimi, yönetilen denetim düzlembileşenleri, üçüncü taraf açık kaynak bileşenleri ve güvenlik veya yama yönetimi ayrıntıları.

## <a name="service-updates-and-releases"></a>Hizmet güncelleştirmeleri ve sürümleri

* Sürüm bilgileri için [AKS sürüm notlarına](https://github.com/Azure/AKS/releases)bakın.
* Önizlemedeki özellikler hakkında daha fazla bilgi için [AKS önizleme özellikleri ve ilgili projelere](https://github.com/Azure/AKS/blob/master/previews.md)bakın.

## <a name="managed-features-in-aks"></a>AKS'de yönetilen özellikler

Bilgi işlem veya ağ bileşenleri gibi hizmet (IaaS) bulut bileşenleri olarak temel altyapı, kullanıcılara düşük düzeyli denetimlere ve özelleştirme seçeneklerine erişim sağlar. Bunun aksine AKS, müşterilere ihtiyaç duydukları ortak yapılandırmalar ve yetenekler kümesini sağlayan anahtar teslim bir Kubernetes dağıtımı sağlar. AKS müşterilerinin özelleştirme, dağıtım ve diğer seçenekleri sınırlıdır. Bu müşterilerin Kubernetes kümeleri hakkında doğrudan endişelenmelerine veya yönetmelerine gerek yoktur.

AKS ile müşteri tam olarak yönetilen bir *kontrol düzlemi*alır. Kontrol düzlemi, müşterinin çalışması ve son kullanıcılara Kubernetes kümeleri sağlaması için gereken tüm bileşenleri ve hizmetleri içerir. Tüm Kubernetes bileşenleri Microsoft tarafından korunur ve işletilir.

Microsoft, denetim bölmesi aracılığıyla aşağıdaki bileşenleri yönetir ve izler:

* Kubelet veya Kubernetes API sunucuları
* Etcd veya uyumlu bir anahtar değer deposu, Hizmet Kalitesi (QoS), ölçeklenebilirlik ve çalışma süresi sağlayan
* DNS hizmetleri (örneğin, kube-dns veya CoreDNS)
* Kubernetes proxy veya ağ

AKS tamamen yönetilen bir küme çözümü değildir. İşçi düğümleri gibi bazı bileşenler, kullanıcıların AKS kümesini korumaya yardımcı olması gereken *sorumluluğu paylaştı.* Kullanıcı girişi, örneğin, bir alt düğüm işletim sistemi (OS) güvenlik yama uygulamak için gereklidir.

Hizmetler, Microsoft ve AKS ekibinin dağıtdığı, çalıştırdığı ve hizmet kullanılabilirliği ve işlevselliğinden sorumlu olduğu anlamında *yönetilir.* Müşteriler bu yönetilen bileşenleri değiştiremez. Microsoft, tutarlı ve ölçeklenebilir bir kullanıcı deneyimi sağlamak için özelleştirmeyi sınırlar. Tamamen özelleştirilebilir bir çözüm için [AKS Engine'e](https://github.com/Azure/aks-engine)bakın.

> [!NOTE]
> AKS alt düğümleri Azure portalında normal Azure IaaS kaynakları olarak görünür. Ancak bu sanal makineler özel bir Azure kaynak grubuna\\dağıtılır (MC *ile önceden belirlenmiştir). AKS işçi düğümlerini değiştirmek mümkündür. Örneğin, AKS işçi düğümlerini normal sanal makineleri değiştirme şeklinizi değiştirmek için Güvenli Kabuk 'u (SSH) kullanabilirsiniz (ancak temel işletim sistemi görüntüsünü değiştiremezsiniz ve değişiklikler bir güncelleştirme veya yeniden başlatma yoluyla devam etmeyebilir) ve aks'a diğer Azure kaynaklarını ekleyebilirsiniz işçi düğümleri. Ancak *bant yönetimi ve özelleştirme dışında* değişiklikler yaptığınızda, AKS kümesi desteklenmeyen hale gelebilir. Microsoft Destek sizi değişiklik yapmaya yönlendirmedikçe çalışan düğümlerini değiştirmekten kaçının.

Tüm aracı düğümlerinin bant deallocation dışında gibi yukarıda tanımlandığı gibi desteklenmeyen işlemleri veren küme desteklenmeyen hale getirir. AKS, destek yönergeleri dışında 30 güne eşit ve daha uzun süreler boyunca yapılandırılan kontrol düzlemlerini arşivleme hakkını saklı tutar. AKS, küme vb. meta verilerin yedeklerini korur ve kümeyi kolayca yeniden tahsis edebilir. Bu yeniden ayırma, kümeyi etkin aracı düğümlerine yükseltme veya ölçek gibi destek olarak geri getiren herhangi bir PUT işlemi tarafından başlatılabilir.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

Bir küme oluşturulduğunda, müşteri AKS'nin oluşturduğu Kubernetes işçi düğümlerini tanımlar. Müşteri iş yükleri bu düğümlerde yürütülür. Müşteriler, alt düğümleri görebilir veya değiştirebilir.

Müşteri küme düğümleri özel kodu yürüttürün ve hassas verileri depoladığı için Microsoft Destek bunlara yalnızca sınırlı bir şekilde erişebilir. Microsoft Destek, açık müşteri izni veya yardımı olmadan bu düğümler için oturum açamaz, komutları yürütemez veya günlükleri görüntüalamaz.

Çalışan düğümleri hassas olduğundan, Microsoft arka plan yönetimlerini sınırlamak için büyük özen ilerler. Çoğu durumda, Kubernetes ana düğümleri, etcd ve microsoft tarafından yönetilen diğer bileşenler başarısız olsa bile iş yükünüz çalışmaya devam eder. Dikkatsizce değiştirilmiş alt düğümler veri ve iş yükü kayıplarına neden olabilir ve kümeyi desteklenmeyen hale getirebilir.

## <a name="aks-support-coverage"></a>AKS destek kapsamı

Microsoft aşağıdakiler için teknik destek sağlar:

* Api sunucusu gibi Kubernetes hizmetinin sağladığı ve desteklediği tüm Kubernetes bileşenlerine bağlantı.
* Kubernetes denetim düzlem ihizmetleri (Örneğin Kubernetes ana düğümleri, API sunucusu, etcd ve kube-dns) yönetimi, çalışma süresi, QoS ve işlemleri.
* Etcd. Destek, afet planlaması ve küme durumu geri yüklemesi için her 30 dakikada bir tüm ETCD verilerinin otomatik, saydam yedeklemelerini içerir. Bu yedeklemeler doğrudan müşteriler veya kullanıcılar tarafından kullanılamaz. Veri güvenilirliği ve tutarlılığı sağlar.
* Kubernetes için Azure bulut sağlayıcısı sürücüsündeki tümleştirme noktaları. Bunlar arasında yük dengeleyicileri, kalıcı birimler veya ağ (Kubernetes ve Azure CNI) gibi diğer Azure hizmetlerine tümleştirmeler yer alır.
* Kubernetes API sunucusu, etcd ve kube-dns gibi kontrol düzlemi bileşenlerinin özelleştirilmesi yle ilgili sorular veya sorunlar.
* Azure CNI, kubenet veya diğer ağ erişimi ve işlevsellik sorunları gibi ağ la ilgili sorunlar. Sorunlar Arasında DNS çözümlemesi, paket kaybı, yönlendirme ve benzeri olabilir. Microsoft çeşitli ağ senaryolarını destekler:
  * Küme ve ilişkili bileşenler içinde Kubenet (temel) ve gelişmiş ağ (Azure CNI)
  * Diğer Azure hizmetlerine ve uygulamalarına bağlantı
  * Giriş denetleyicileri ve giriş veya yük dengeleyici konfigürasyonları
  * Ağ performansı ve gecikme

Microsoft aşağıdakiler için teknik destek sağlamaz:

* Kubernetes nasıl kullanılacağı hakkında sorular. Örneğin, Microsoft Destek özel giriş denetleyicileri oluşturma, uygulama iş yüklerini kullanma veya üçüncü taraf veya açık kaynak yazılım paketleri veya araçları uygulama hakkında tavsiye sağlamaz.
  > [!NOTE]
  > Microsoft Destek, AKS küme işlevselliği, özelleştirme ve aetme (örneğin, Kubernetes işlem sorunları ve yordamları) hakkında tavsiyelerde bulunabilir.
* Kubernetes denetim düzleminin bir parçası olarak sağlanmayan veya AKS kümeleriyle dağıtılan üçüncü taraf açık kaynak projeleri. Bu projeler Istio, Helm, Elçi veya diğerleri içerebilir.
  > [!NOTE]
  > Microsoft, Helm ve Kured gibi üçüncü taraf açık kaynak projeleri için en iyi çabayı sağlayabilir. Üçüncü taraf açık kaynak aracının Kubernetes Azure bulut sağlayıcısı veya AKS'ye özgü diğer hatalarla tümleştiği durumlarda, Microsoft Microsoft belgelerinden örnekler ve uygulamalar destekler.
* Üçüncü taraf kapalı kaynak yazılımları. Bu yazılım, güvenlik tarama araçlarını ve ağ aygıtlarını veya yazılımlarını içerebilir.
* Çok bulutlu veya çok satıcılı yapılarla ilgili sorunlar. Örneğin, Microsoft federe çok genel bulut satıcısı çözümü çalıştırmayla ilgili sorunları desteklemez.
* [AKS belgelerinde](https://docs.microsoft.com/azure/aks/)listelenenler dışındaki ağ özelleştirmeleri.
  > [!NOTE]
  > Microsoft, ağ güvenlik grupları (NSG'ler) ile ilgili sorunları ve hataları destekler. Örneğin, Microsoft Desteği, NSG'nin güncelleştirilmemesi veya beklenmeyen bir NSG veya yük dengeleyici davranışıyla ilgili soruları yanıtlayabilir.

## <a name="aks-support-coverage-for-worker-nodes"></a>Aks destek kapsamı işçi düğümleri için

### <a name="microsoft-responsibilities-for-aks-worker-nodes"></a>AKS işçi düğümleri için Microsoft sorumlulukları

Microsoft ve müşteriler, Kubernetes işçi düğümleri için sorumluluğu paylaşır:

* Temel işletim sistemi görüntüsü eklemeler (izleme ve ağ aracıları gibi) gerektir.
* İşçi düğümleri otomatik olarak işletim sistemi yamaları alır.
* Kubernetes denetim düzlemi bileşenleri ile ilgili sorunlar işçi düğümleri üzerinde çalışan otomatik olarak düzeltilir. Bileşenler şunlardır:
  * Kube-vekil
  * Kubernetes ana bileşenlerine iletişim yolları sağlayan ağ tünelleri
  * Kubelet
  * Docker veya Moby daemon

> [!NOTE]
> Bir çalışma düğümünde, bir denetim düzlemi bileşeni çalışmıyorsa, AKS ekibinin tek tek bileşenleri veya tüm alt düğümü yeniden başlatması gerekebilir. Bu yeniden başlatma işlemleri otomatiktir ve sık karşılaşılan sorunlar için otomatik düzeltme sağlar. Bu yeniden başlatmalar, acil bakım veya kesinti olmadığı sürece kümede değil, _yalnızca düğüm_ düzeyinde oluşur.

### <a name="customer-responsibilities-for-aks-worker-nodes"></a>AKS işçi düğümleri için müşteri sorumlulukları

Microsoft, işletim sistemi düzeyindeki düzeltme emleleri uygulamak için alt düğümleri otomatik olarak yeniden başlatmaz. İşletim sistemi düzeltme emaları işçi düğümlerine teslim edilebilse de, *müşteri* değişiklikleri uygulamak için işçi düğümlerini yeniden başlatmakla sorumludur. Paylaşılan kitaplıklar, katı hal hibrit sürücü (SSHD) gibi daemons ve sistem veya işletim sistemi düzeyindeki diğer bileşenler otomatik olarak yakalır.

Müşteriler Kubernetes yükseltmelerini yürütmekle yükümlüdür. Yükseltmeleri Azure denetim paneli veya Azure CLI üzerinden yürütebilirler. Bu, Kubernetes için güvenlik veya işlevsellik geliştirmeleri içeren güncelleştirmeler için geçerlidir.

> [!NOTE]
> AKS yönetilen bir *hizmet*olduğundan, son hedefleri, hizmet yönetimini daha eksiksiz ve uygulamalı hale getirmek için düzeltme etekleri, güncelleştirmeleri ve günlük toplama sorumluluğunu kaldırmayı içerir. Hizmetin uçlardan uca yönetim kapasitesi arttıkça, gelecekteki sürümler bazı işlevleri (örneğin, düğüm yeniden başlatma ve otomatik düzeltme) atlatır.

### <a name="security-issues-and-patching"></a>Güvenlik sorunları ve yama

AKS'nin bir veya daha fazla bileşeninde bir güvenlik kusuru bulunursa, AKS ekibi sorunu azaltmak için etkilenen tüm kümeleri yamalar. Alternatif olarak, takım kullanıcılara yükseltme kılavuzu verecektir.

Bir güvenlik kusurunun etkilediği işçi düğümleri için, sıfır kesinti süresi düzeltme eki varsa, AKS ekibi bu yama uygular ve değişikliği kullanıcılara bildirir.

Bir güvenlik yama işçi düğümü yeniden başlatmayı gerektiriyorsa, Microsoft bu gereksinimi müşterilere bildirir. Müşteri, küme yamasını almak için yeniden başlatmakveya güncelleştirmekten sorumludur. Kullanıcılar yamaları AKS kılavuzuna göre uygulamazsa, kümeleri güvenlik sorununa karşı savunmasız olmaya devam eder.

### <a name="node-maintenance-and-access"></a>Düğüm bakım ve erişim

İşçi düğümleri paylaşılan bir sorumluluktur ve müşterilere aittir. Bu nedenle, müşteriler işçi düğümlerinde oturum açma ve çekirdek güncelleştirmeleri ve paketleri yükleme veya kaldırma gibi zararlı olabilecek değişiklikler yapma olana sahiptir.

Müşteriler yıkıcı değişiklikler yaparsa veya denetim düzlemi bileşenlerinin çevrimdışı duruma gelmesine veya işlevsiz hale gelmesine neden olursa, AKS bu hatayı algılar ve çalışan düğüme otomatik olarak önceki çalışma durumuna geri yüklenir.

Müşteriler oturum açabilir ve işçi düğümlerini değiştirebilir, ancak değişiklikler bir kümeyi desteklenmeyen hale getirebileceğinden bunu yapmak önerilmez.

## <a name="network-ports-access-and-nsgs"></a>Ağ bağlantı noktaları, erişim ve NSG'ler

Yönetilen bir hizmet olarak AKS'nin belirli ağ ve bağlantı gereksinimleri vardır. Bu gereksinimler, normal IaaS bileşenleri için gereksinimlerden daha az esnektir. AKS'de, NSG kurallarını özelleştirme, belirli bir bağlantı noktasını engelleme (örneğin, giden bağlantı noktası 443'ü engelleyen güvenlik duvarı kurallarını kullanma) ve URL'leri beyaz listeleme gibi işlemler kümenizi desteklenmeyen yapabilir.

> [!NOTE]
> Şu anda, AKS tamamen küme çıkış trafiğini kilitlemek için izin vermez. Kümenizin giden trafik için kullanabileceği URL'ler ve bağlantı noktaları listesini denetlemek için [sınır çıkış trafiğini](limit-egress-traffic.md)görün.

## <a name="unsupported-alpha-and-beta-kubernetes-features"></a>Desteklenmeyen alfa ve beta Kubernetes özellikleri

AKS, yukarı kubernetes projesi nde yalnızca kararlı özellikleri destekler. Aksi belgelenmedikçe, AKS yukarı Kubernetes projesinde bulunan alfa ve beta özelliklerini desteklemez.

İki senaryoda, alfa veya beta özellikleri genel kullanıma sunulmadan önce kullanıma sunulabilir:

* Müşteriler AKS ürün, destek veya mühendislik ekipleri ile bir araya geldi ve bu yeni özellikleri denemek için istendi.
* Bu özellikler [bir özellik bayrağı tarafından etkinleştirilmiştir.](https://github.com/Azure/AKS/blob/master/previews.md) Müşteriler açıkça bu özellikleri kullanmayı tercih etmelidir.

## <a name="preview-features-or-feature-flags"></a>Özellikleri veya özellik bayraklarını önizleme

Genişletilmiş sınama ve kullanıcı geri bildirimi gerektiren özellikler ve işlevler için Microsoft, bir özellik bayrağının arkasında yeni önizleme özellikleri veya özellikleri yayımlar. Bu özellikleri yayın öncesi veya beta özellikleri olarak düşünün.

Önizleme özellikleri veya özellik bayrak özellikleri üretim için değildir. API'lerde ve davranışlarda devam eden değişiklikler, hata düzeltmeleri ve diğer değişiklikler kararsız kümelere ve kapalı kalma sürelerine neden olabilir.

Genel önizlemedeki özellikler önizlemede olduğu ve üretim için kullanılmadığı ve yalnızca iş saatlerinde AKS teknik destek ekipleri tarafından desteklendirilen 'en iyi çaba' desteği ne kadar dır? Daha fazla bilgi için lütfen bkz:

* [Azure Destek SSS](https://azure.microsoft.com/support/faq/)

> [!NOTE]
> Önizleme özellikleri Azure *abonelik* düzeyinde etkindir. Önizleme özelliklerini üretim aboneliğine yüklemeyin. Üretim aboneliğinde önizleme özellikleri varsayılan API davranışını değiştirebilir ve normal işlemleri etkileyebilir.

## <a name="upstream-bugs-and-issues"></a>Akış yukarı hataları ve sorunları

Upstream Kubernetes proje geliştirme hızı göz önüne alındığında, hatalar her zaman ortaya çıkar. Bu hataların bazıları aks sistemi içinde yamalı veya üzerinde çalışılamaz. Bunun yerine, hata düzeltmeleri yukarı akış projeleri (Kubernetes, düğüm veya alt işletim sistemleri ve çekirdekleri gibi) için daha büyük düzeltmeler gerektirir. Microsoft'un sahip olduğu bileşenler (Azure bulut sağlayıcısı gibi), AKS ve Azure personeli topluluktaki sorunları düzeltmeye kararlıdır.

Bir teknik destek sorunu bir veya daha fazla yukarı akış hatadan kaynaklanıyorsa, AKS desteği ve mühendislik ekipleri şunları yapacaktır:

* Bu sorunun kümenizi veya iş yükünüzü neden etkilediğini açıklamaya yardımcı olmak için, yukarı akış hatalarını destekleyen ayrıntılarla tanımlayın ve ilişkilendirin. Müşteriler, sorunları izleyebilmeleri ve yeni bir sürümün ne zaman düzeltmeler sağlayacağını görebilmeleri için gerekli depolara bağlantılar alırlar.
* Olası geçici çözüm veya azaltıcı etkenler sağlayın. Sorun azaltılabilirse, [AKS](https://github.com/Azure/AKS/issues?q=is%3Aissue+is%3Aopen+label%3Aknown-issue) deposunda bilinen bir sorun açılır. Bilinen sorun dosyalama açıklar:
  * Akış yukarı hatalarına bağlantılar da dahil olmak üzere sorun.
  * Geçici çözüm ve bir yükseltme veya çözümün başka bir kalıcılığı hakkında ayrıntılar.
  * Sorunun dahil edilmesi için kaba zaman çizelgeleri, upstream sürüm cadence dayalı.
