---
title: Azure Özel DNS nedir?
description: Bu makalede, Microsoft Azure'daki özel DNS barındırma hizmetine genel bir bakış la başlayın.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: rohink
ms.openlocfilehash: 97b266398b3ea46d09b04524dad34922f21b1a95
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76939280"
---
# <a name="what-is-azure-private-dns"></a>Azure Özel DNS nedir?

Alan Adı Sistemi veya DNS, bir hizmet adının IP adresine çevrilme (veya çözümlenmesinden) sorumludur.  Azure DNS, Microsoft Azure altyapısını kullanarak ad çözümlemesi sağlayan DNS etki alanları için bir barındırma hizmetidir. Azure DNS, Internet'e bakan DNS alan adlarını desteklemenin yanı sıra özel DNS bölgelerini de destekler.

Azure Private DNS, özel bir DNS çözümü eklemeye gerek kalmadan sanal ağdaki alan adlarını yönetmek ve çözmek için güvenilir ve güvenli bir DNS hizmeti sağlar. Özel DNS bölgelerini kullanarak, bugün kullanılabilen Azure tarafından sağlanan adlar yerine kendi özel alan adlarınızı kullanabilirsiniz. Özel alan adları kullanmak, sanal ağ mimarinizi kuruluşunuzun gereksinimlerine en uygun şekilde uyarlamanıza yardımcı olur. Sanal ağ içinde ve sanal ağlar arasında sanal makineler (VM' ler) için ad çözünürlüğü sağlar. Ayrıca, özel ve ortak bir DNS bölgesinin adı paylaşmasına olanak tanıyan bölünmüş ufuk görünümüyle bölge adlarını yapılandırabilirsiniz.

Sanal ağınızdan özel bir DNS bölgesinin kayıtlarını çözmek için sanal ağı bölgeye bağlamanız gerekir. Bağlantılı sanal ağlar tam erişime sahiptir ve özel bölgede yayınlanan tüm DNS kayıtlarını çözebilir. Ayrıca, sanal ağ bağlantısında otomatik kaydı da etkinleştirebilirsiniz. Sanal ağ bağlantısında otomatik kaydolunmasını etkinleştiriseniz, bu sanal ağdaki sanal makinelerin DNS kayıtları özel bölgeye kaydedilir. Otomatik kayıt etkinleştirildiğinde, Azure DNS sanal bir makine oluşturulduğunda, IP adresini değiştirdiğinde veya silindiğinde bölge kayıtlarını da güncelleştirir.

![DNS'ye genel bakış](./media/private-dns-overview/scenario.png)

> [!NOTE]
> En iyi uygulama olarak, özel DNS bölgeniz için *.yerel* etki alanı kullanmayın. Tüm işletim sistemleri bunu desteklemez.

## <a name="benefits"></a>Avantajlar

Azure Özel DNS aşağıdaki avantajları sağlar:

* **Özel DNS çözümleri gereksinimini ortadan kaldırır.** Daha önce, birçok müşteri sanal ağındaKi DNS bölgelerini yönetmek için özel DNS çözümleri oluşturmüştü. Artık, özel DNS çözümleri oluşturma ve yönetme yükünü ortadan kaldıran yerel Azure altyapısını kullanarak DNS bölgelerini yönetebilirsiniz.

* **Tüm yaygın DNS kayıt türlerini kullanın.** Azure DNS, A, AAAA, CNAME, MX, PTR, SOA, SRV ve TXT kayıtlarını destekler.

* **Otomatik ana bilgisayar adı kayıt yönetimi**. Azure, özel DNS kayıtlarınızı barındırmanın yanı sıra, belirtilen sanal ağlardaki Sanal M'ler için ana bilgisayar kayıtları otomatik olarak tutar. Bu senaryoda, özel DNS çözümleri oluşturmaya veya uygulamaları değiştirmeye gerek kalmadan kullandığınız alan adlarını en iyi duruma getirebilirsiniz.

* **Sanal ağlar arasında ana bilgisayar adı çözünürlüğü.** Azure tarafından sağlanan ana bilgisayar adlarının aksine, özel DNS bölgeleri sanal ağlar arasında paylaşılabilir. Bu özellik, sanal ağ eşleme gibi çapraz ağ ve hizmet bulma senaryolarını basitleştirir.

* **Tanıdık araçlar ve kullanıcı deneyimi.** Öğrenme eğrisini azaltmak için bu hizmet, köklü Azure DNS araçlarını (Azure portalı, Azure PowerShell, Azure CLI, Azure Kaynak Yöneticisi şablonları ve REST API) kullanır.

* **Split-horizon DNS desteği.** Azure DNS ile, sanal ağ içinden ve genel internetten farklı yanıtları çözen aynı ada sahip bölgeler oluşturabilirsiniz. Bölünmüş ufuk DNS için tipik bir senaryo, sanal ağınızda kullanılmak üzere bir hizmetin özel bir sürümünü sağlamaktır.

* **Tüm Azure bölgelerinde kullanılabilir.** Azure DNS özel bölgeler özelliği, Azure genel bulutundaki tüm Azure bölgelerinde kullanılabilir.

## <a name="capabilities"></a>Özellikler

Azure DNS aşağıdaki özellikleri sağlar:

* Otomatik kayıt etkin özel **bir bölgeye bağlı sanal bir ağdan sanal makinelerin otomatik kayıt.** Sanal makineler özel bölge lerine özel IP adreslerini gösteren Bir kayıt olarak kaydedilir (eklenir). Otomatik kayıt etkinleştirilmiş sanal ağ bağlantısındaki sanal bir makine silindiğinde, Azure DNS ilgili DNS kaydını da bağlantılı özel bölgeden otomatik olarak kaldırır.

* **İleri DNS çözünürlüğü, özel bölgeye bağlı sanal ağlar da desteklenir.** Sanal ağlar arası DNS çözümü için, sanal ağların birbiriyle bakması gibi açık bir bağımlılık yoktur. Ancak, diğer senaryolar için sanal ağlara eş vermek isteyebilirsiniz (örneğin, HTTP trafiği).

* **Ters DNS araması sanal ağ kapsamı içinde desteklenir.** Özel bir bölgeye atanan sanal ağ içinde özel bir IP aramaters DNS ana bilgisayar / kayıt adı ve sonek olarak bölge adını içeren FQDN döndürür.

## <a name="other-considerations"></a>Diğer konular

Azure DNS'nin aşağıdaki sınırlamaları vardır:

* VM DNS kayıtlarının otomatik kaydı etkinse, belirli bir sanal ağ yalnızca bir özel bölgeye bağlanabilir. Ancak, birden çok sanal ağı tek bir DNS bölgesine bağlayabilirsiniz.
* Ters DNS yalnızca bağlantılı sanal ağdaki özel IP alanı için çalışır
* Bağlı bir sanal ağ için özel bir IP adresi için Ters DNS, sanal makine için varsayılan sonek olarak *internal.cloudapp.net* döndürür. Otomatik kayıt etkinleştirilmiş özel bir bölgeye bağlanan sanal ağlar için, özel bir IP adresi için ters DNS iki FQDN döndürür: biri varsayılan sonek *internal.cloudapp.net* ve diğeri özel bölge sonekiyle.
* Koşullu iletme şu anda yerel olarak desteklenmez. Azure ve şirket içi ağlar arasında çözümü etkinleştirmek için. [VM'ler ve rol örnekleri için Ad çözünürlüğüne](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) bakın
 
## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için Azure [DNS](https://azure.microsoft.com/pricing/details/dns/)Fiyatlandırması'na bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI'yi](./private-dns-getstarted-cli.md)kullanarak Azure DNS'de nasıl özel bir bölge oluşturabilirsiniz öğrenin.

* Azure DNS'deki özel bölgelerle gerçekleştirilebilecek bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlem türleri için bekleyebileceğiniz belirli davranışlar da dahil olmak üzere Azure DNS'deki özel bölgelerle ilgili sık sorulan sorular ve yanıtlar için [Bkz.](./dns-faq-private.md)

* DNS bölgeleri ve kayıtları genel bakış ziyaret ederek [DNS bölgeleri ve](dns-zones-records.md)kayıtları hakkında bilgi edinin.

* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
