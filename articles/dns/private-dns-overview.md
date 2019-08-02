---
title: Azure Özel DNS nedir?
description: Microsoft Azure üzerinde özel DNS barındırma hizmetine genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: 35a4c7410b36e9716d2ee9625ed98fcc34043e58
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742456"
---
# <a name="what-is-azure-private-dns"></a>Azure Özel DNS nedir?

> [!IMPORTANT]
> Azure Özel DNS şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Etki alanı adı sistemi veya DNS, bir hizmet adını IP adresine çevirmekten (veya çözümlemeden) sorumludur.  Azure DNS, Microsoft Azure altyapısını kullanarak ad çözümlemesi sağlayan DNS etki alanları için bir barındırma hizmetidir. İnternet 'e yönelik DNS etki alanlarını desteklemeye ek olarak, Azure DNS özel DNS bölgelerini de destekler.

Azure Özel DNS, bir sanal ağdaki etki alanı adlarını yönetmek ve çözümlemek için özel bir DNS çözümü eklemeye gerek kalmadan güvenilir, güvenli bir DNS hizmeti sağlar. Özel DNS bölgelerini kullanarak, bugün kullanılabilir olan Azure tarafından sağlanan adlar yerine kendi özel etki alanı adlarınızı kullanabilirsiniz. Özel etki alanı adları kullanmak, sanal ağ mimarinizi kuruluşunuzun ihtiyaçlarına en iyi şekilde uyarlamanıza yardımcı olur. Bir sanal ağ içinde ve sanal ağlar arasında sanal makineler (VM) için ad çözümlemesi sağlar. Ayrıca, bölge adlarını bir bölünmüş ufuk görünümüyle yapılandırarak özel ve ortak bir DNS bölgesinin adı paylaşmasına izin verir.

Özel bir DNS bölgesinin kayıtlarını sanal ağınızdan çözümlemek için, sanal ağı bölgeye bağlamanız gerekir. Bağlı sanal ağlar tam erişime sahiptir ve özel bölgede yayınlanan tüm DNS kayıtlarını çözümleyebilir. Ayrıca, bir sanal ağ bağlantısında de oto kaydı etkinleştirebilirsiniz. Bir sanal ağ bağlantısında oto kaydını etkinleştirirseniz, bu sanal ağdaki sanal makinelerin DNS kayıtları özel bölgeye kaydedilir. Oto kayıt etkinleştirildiğinde Azure DNS, bir sanal makine oluşturulduğunda bölge kayıtlarını da güncelleştirir, ' IP adresini değiştirir veya silinir.

![DNS genel bakış](./media/private-dns-overview/scenario.png)

> [!NOTE]
> En iyi uygulama olarak, özel DNS bölgeniz için bir *. Local* etki alanı kullanmayın. Tüm işletim sistemleri bunu desteklemez.

## <a name="benefits"></a>Avantajlar

Azure Özel DNS aşağıdaki avantajları sağlar:

* **Özel DNS çözümleri gereksinimini ortadan kaldırır**. Daha önce, birçok müşteri, sanal ağındaki DNS bölgelerini yönetmek için özel DNS çözümleri oluşturmuştur. Artık özel DNS çözümlerini oluşturma ve yönetme yükünü ortadan kaldıran yerel Azure altyapısını kullanarak DNS bölgelerini yönetebilirsiniz.

* **Tüm ortak DNS kayıtları türlerini kullanın**. Azure DNS, bir, AAAA, CNAME, MX, PTR, SOA, SRV ve TXT kayıtlarını destekler.

* **Otomatik ana bilgisayar adı kayıt yönetimi**. Özel DNS kayıtlarınızı barındırmakla birlikte, Azure belirtilen sanal ağlardaki VM 'Ler için ana bilgisayar adı kayıtlarını otomatik olarak korur. Bu senaryoda, özel DNS çözümleri oluşturmaya veya uygulamaları değiştirmeye gerek kalmadan kullandığınız etki alanı adlarını iyileştirebilirsiniz.

* **Sanal ağlar arasında konak adı çözümlemesi**. Azure tarafından sunulan ana bilgisayar adlarından farklı olarak, özel DNS bölgeleri sanal ağlar arasında paylaşılabilir. Bu özellik, sanal ağ eşlemesi gibi ağlar arası ve hizmet bulma senaryolarını basitleştirir.

* **Tanıdık araçlar ve Kullanıcı deneyimi**. Bu hizmet, öğrenme eğrisini azaltmak için iyi kurulu Azure DNS araçları (Azure portal, Azure PowerShell, Azure CLı, Azure Resource Manager şablonları ve REST API) kullanır.

* **Bölünmüş ufuk DNS desteği**. Azure DNS ile, bir sanal ağ içinden ve genel İnternet 'ten farklı yanıtlara çözüm veren aynı ada sahip bölgeler oluşturabilirsiniz. Bölünmüş ufku DNS için tipik bir senaryo, sanal ağınızın içinde kullanılmak üzere bir hizmetin adanmış bir sürümünü sağlamaktır.

* **Tüm Azure bölgelerinde kullanılabilir**. Azure DNS özel bölgeler özelliği, Azure genel bulutundaki tüm Azure bölgelerinde kullanılabilir.

## <a name="capabilities"></a>Özellikler

Azure DNS aşağıdaki özellikleri sağlar:

* **Otomatik kayıt etkin olan bir özel bölgeye bağlı bir sanal ağ sanal makinelerin otomatik olarak kaydı**. Sanal makineler, özel IP adreslerine işaret eden kayıtlar olarak özel bölgeye kaydedilir (eklenir). Bir sanal ağ bağlantısı içindeki otomatik kayıt etkinken bir sanal makine silindiğinde, ilgili DNS kaydını otomatik olarak bağlı özel bölgeden da kaldırır Azure DNS.

* **ILERI DNS çözümlemesi, özel bölgeye bağlı sanal ağlar arasında desteklenir**. Çapraz sanal ağ DNS çözümlemesi için, sanal ağların birbirleriyle eşlenme gibi açık bir bağımlılık yoktur. Bununla birlikte, sanal ağların diğer senaryolar için (örneğin, HTTP trafiği) eşdüzey olmasını isteyebilirsiniz.

* **Ters DNS araması, sanal ağ kapsamı içinde desteklenir**. Özel bir bölgeye atanan sanal ağ içindeki özel bir IP için ters DNS araması, ana bilgisayar/kayıt adını ve sonek olarak bölge adını içeren FQDN 'yi döndürür.

## <a name="known-issues"></a>Bilinen sorunlar
Aşağıdaki öğeler önizleme sürümündeki bilinen hatalar ve sorunlardır:
* Özel bir DNS bölgesine bağlı bir sanal ağı silerseniz, özel DNS bölgesinin bağlantılarını silmez. Sanal ağı aynı ad ve kaynak grubuyla yeniden oluşturup bir özel DNS bölgesine yeniden bağlamayı denerseniz bağlantı başarısız olur. Bu sorunu geçici olarak çözmek için, sanal ağı farklı bir kaynak grubunda veya aynı kaynak grubunda farklı bir adla oluşturun.
* Bir sanal ağı başka bir kaynak grubuna veya aboneliğe taşırsanız, özel DNS bölgesinin bağlantılarını güncelleştirmez. Taşınan sanal ağın ad çözümlemesi çalışmaya devam eder, ancak özel DNS bölgesinin sanal ağ bağlantılarını görüntülediğinizde sanal ağın eski ARM kimliklerini görürsünüz.
* Şu anda BAE Kuzey, BAE Orta, Güney Afrika Batı, Güney Afrika Kuzey, Kanada Doğu ve Fransa Güney barındırılan bağlı sanal ağlar başarısız olabilir ve aralıklı DNS çözümleme sorunları görebilirsiniz. 


## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Azure DNS aşağıdaki sınırlamalara sahiptir:

* Belirli bir sanal ağ, VM DNS kayıtlarının otomatik kaydı etkin olduğu gibi yalnızca bir özel bölgeye bağlanabilir. Ancak, birden çok sanal ağı tek bir DNS bölgesine bağlayabilirsiniz.
* Ters DNS yalnızca bağlı sanal ağdaki özel IP alanı için geçerlidir
* Bağlı bir sanal ağın özel IP 'si için ters DNS, sanal makine için varsayılan sonek olarak "internal.cloudapp.net" döndürür. Bir özel bölgeyle bağlantılı olan sanal ağlar için, bir özel IP için ters DNS, *internal.cloudapp.net* varsayılan sonekine ve diğeri de özel bölge sonekine sahip olan 2 FQDN döndürür.
* Koşullu iletme Şu anda yerel olarak desteklenmiyor. Azure ile şirket içi ağlar arasında çözünürlüğü etkinleştirmek için. Bkz. [VM 'ler ve rol örnekleri Için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)
 
## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz. [Azure DNS fiyatlandırması](https://azure.microsoft.com/pricing/details/dns/).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md)kullanarak Azure DNS bir özel bölge oluşturmayı öğrenin.

* Azure DNS özel bölgelerle gerçekleştirilen bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlemler için bekleneceğiniz belirli davranış dahil olmak üzere Azure DNS özel bölgeler hakkında sık sorulan sorular ve yanıtlar için, bkz. [özel DNS SSS](./dns-faq-private.md).

* DNS bölgelerini ve [kayıtları](dns-zones-records.md)ziyaret ederek DNS bölgeleri ve kayıtları hakkında bilgi edinin.

* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
