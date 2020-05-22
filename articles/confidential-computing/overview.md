---
title: Azure gizli bilgi Işlem genel bakış
description: Azure gizli (ACC) Bilgi Işlem 'ye Genel Bakış
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: workloads
ms.topic: overview
ms.date: 04/06/2020
ms.author: JenCook
ms.openlocfilehash: 44006bdfd9ffe6e78380adefe9271f42c0a76f84
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773288"
---
# <a name="confidential-computing-on-azure"></a>Azure 'da gizli bilgi işlem

Azure gizli bilgi işlem, bulutta işlendiği sırada hassas verilerinizi yalıtmanızı sağlar. Birçok sektör, verilerini korumak için gizli bilgi işlem kullanır. Bu iş yükleri şunları içerir:

- Finansal verilerin güvenliğini sağlama
- Hasta bilgilerini koruma
- Hassas bilgiler üzerinde makine öğrenimi süreçlerini çalıştırma
- Birden çok kaynaktan şifrelenmiş veri kümelerinde algoritmalar gerçekleştirme


## <a name="overview"></a>Genel Bakış
<p><p>


> [!VIDEO https://www.youtube.com/embed/rT6zMOoLEqI]

Bulut verilerinizin güvenliğini güvenli hale getirmenin önemli olduğunu biliyoruz. Kaygılarınızı duyduk. Müşterilerimiz hassas iş yüklerini buluta taşırken yalnızca birkaç soru aşağıda verilmiştir: 

- Nasıl yaparım? Microsoft 'un şifrelenmeyen verilere erişemediğinden emin olun.
- Nasıl yaparım? şirketimin içindeki ayrıcalıklı yöneticilerden güvenlik tehditleri önlemektir mi?
- Üçüncü tarafların hassas müşteri verilerine erişmesini Engelleyebildiğim daha fazla yol var mı?

Microsoft Azure, saldırı yüzeyinizi daha güçlü veri koruma kazanmak için en aza indirmenize yardımcı olur Azure, istemci tarafı şifreleme ve sunucu tarafı şifreleme gibi modellerle [**bekleyen verileri**](../security/fundamentals/encryption-atrest.md) korumak için zaten birçok araç sunmaktadır. Ayrıca, Azure, [**iletim içindeki VERILERI**](../security/fundamentals/data-encryption-best-practices.md#protect-data-in-transit) TLS ve https gibi güvenli protokoller aracılığıyla şifrelemek için mekanizmalar sunar. Bu sayfada veri şifrelemenin üçüncü bir tarafı tanıtılmıştır. **Kullanımdaki verilerin**şifrelenmesi.


## <a name="introduction-to-confidential-computing"></a>Gizli bilgi işlem konusuna giriş<a id="intro to acc"></a>

Gizli bilgi işlem, gizli bilgi işlem [Consortium](https://confidentialcomputing.io/) (CCC) tarafından tanımlanan bir sektör terimidir. Bu, gizli bilgi işlem benimsemeyi tanımlamaya ve hızlandırmaya ayrılmış bir temel hizmettir. CCC, donanım tabanlı bir güvenilir yürütme ortamında (t) hesaplamalar gerçekleştirerek kullanılan verilerin korunması olarak gizli bilgi işlem tanımlar.

T, yalnızca yetkili kodun yürütülmesini zorlayan bir ortamdır. T 'deki herhangi bir veri, bu ortamın dışındaki herhangi bir kodla okunamaz veya üzerinde oynanamaz.

### <a name="enclaves"></a>Kuşkukları

Kuşanın güvenliği, donanım işlemcisinin ve belleğinin güvenli bir kısımındadır. Bir hata ayıklayıcı ile birlikte, şifreleme içinde veri veya kod görüntülemenin bir yolu yoktur. Güvenilmeyen kod, şifreleme belleğindeki içeriği değiştirme girişiminde bulunursa ortam devre dışı bırakılır ve işlemler reddedilir.

Uygulama geliştirirken, kodunuzun ve verilerinizin parçalarını kuşkuya içinde korumak için [yazılım araçlarını](#oe-sdk) kullanabilirsiniz. Bu araçlar, kodunuzun ve verilerinizin güvenilir ortam dışındaki kimseler tarafından görüntülenememesini veya değiştirilmesini güvence altına alacak. 

Temelde, bir kuşatma 'yı siyah kutu olarak düşünün. Şifrelenmiş kod ve verileri kutuya yerleştirebilirsiniz. Kutunun dışından, hiçbir şey göremezsiniz. Şifreleme, verilerin şifresini çözmek için bir anahtar verirsiniz, daha sonra veriler, kuşdan gönderilmeden önce işlenir ve yeniden şifrelenir.

### <a name="attestation"></a>Kanıtlama

Güvenilir ortamınızın güvenli olduğunu doğrulama ve doğrulama almak isteyeceksiniz. Bu doğrulama, kanıtlama işlemidir. 

Kanıtlama, bağlı olan tarafın yazılımın (1) bir kuşsa ve (2) tarafından güncel ve güvenli olduğunu arttığı güvenlere erişmesini sağlar. Örneğin, bir kuşatma, temeldeki donanımın platformda bulunduğu kanıtı içeren bir kimlik bilgisi oluşturmasını ister. Rapor daha sonra raporun aynı platformda oluşturulduğunu doğrulayan ikinci bir kuşya verilebilir.

Kanıtlama, sistem yazılımıyla ve Silicon ile uyumlu olan güvenli bir kanıtlama hizmeti kullanılarak uygulanmalıdır. [Intel 'in kanıtlama ve sağlama hizmetleri](https://software.intel.com/sgx/attestation-services) , Azure gizli bilgi işlem sanal makineleri ile uyumludur.


## <a name="using-azure-for-cloud-based-confidential-computing"></a>Bulut tabanlı gizli bilgi işlem için Azure kullanma<a id="cc-on-azure"></a>

Azure gizli bilgi işlem, sanallaştırılmış bir ortamda gizli bilgi işlem özelliklerinden yararlanmanızı sağlar. Artık güvenli donanımın üzerine derlemek için araçlar, yazılımlar ve bulut altyapısını kullanabilirsiniz. 

### <a name="virtual-machines"></a>Sanal Makineler

Azure, sanallaştırılmış bir ortamda gizli bilgi işlem olanağı sunan ilk bulut sağlayıcıdır. Donanım ve uygulamanız arasında bir soyutlama katmanı görevi gören sanal makineler geliştirdik. İş yüklerini ölçekli ve artıklık ve kullanılabilirlik seçenekleriyle çalıştırabilirsiniz.  

#### <a name="intel-sgx-enabled-virtual-machines"></a>Intel SGX özellikli sanal makineler

Azure gizli bilgi işlem sanal makinelerinde, CPU 'nun bir parçası, uygulamanızdaki kod ve verilerin bir bölümü için ayrılmıştır. Bu kısıtlı bölüm kuşkudır. 

![VM modeli](media/overview/hardware-backed-enclave.png)

Azure gizli bilgi işlem altyapısı şu anda çok sayıda sanal makinenin (VM) bir SKU 'sundan oluşur. Bu VM 'Ler Software Guard uzantısı (Intel SGX) ile Intel işlemcilerde çalışır. [INTEL SGX](https://intel.com/sgx) , gizli bilgi işlem ile daha iyi koruma sağlayan bir bileşendir. 

Günümüzde Azure, donanım tabanlı şifreleme oluşturma için Intel SGX teknolojisini temel alan [DCsv2 serisi](https://docs.microsoft.com/azure/virtual-machines/dcv2-series) sunmaktadır. Uygulama verilerinizi ve kullanımda olan kodları korumak için DCsv2 serisi VM 'lerde çalışacak güvenli şifreleme tabanlı uygulamalar oluşturabilirsiniz. 

Azure gizli bilgi işlem sanal makinelerini donanım tabanlı güvenilen şifrelerle dağıtma hakkında [daha fazla](virtual-machine-solutions.md) bilgi edinebilirsiniz.

## <a name="application-development"></a>Uygulama geliştirme<a id="application-development"></a>

Şifreleme ve yalıtılmış ortamların gücünden yararlanmak için, gizli bilgi işlem desteği olan araçları kullanmanız gerekir. Şifreleme uygulaması geliştirmeyi destekleyen çeşitli araçlar vardır. Örneğin, bu açık kaynaklı çerçeveleri kullanabilirsiniz: 

- [Açık şifreleme yazılım geliştirme seti (SDK)](https://github.com/openenclave/openenclave)
- [Gizli konsorsiyum çatısı (CCF)](https://github.com/Microsoft/CCF)

### <a name="overview"></a>Genel Bakış

Şifreleme ile oluşturulmuş bir uygulama iki şekilde bölümlenir:
1. "Güvenilmeyen" bir bileşen (konak)
1. "Güvenilir" bir bileşen (şifreleme)

**Ana bilgisayar** , şifreleme uygulamanızın en üstünde çalıştığı ve güvenilmeyen bir ortam olduğu yerdir. Konakta dağıtılan şifreleme koduna konak tarafından erişilemiyor. 

**Şifreleme,** uygulama kodunun ve önbelleğe alınmış verilerinin/belleğin çalıştırıldığı yerdir. Güvenli hesaplamalar, gizli dizileri ve hassas verileri sağlamak için kuşlar halinde gerçekleşmelidir, korumalı kalır. 

Uygulama tasarımı sırasında, uygulamanın hangi bölümünün kuşkuta çalıştırılması gerektiğini belirlemek ve belirlemek önemlidir. Güvenilen bileşene yerleştirilmesi için seçtiğiniz kod uygulamanızın geri kalanından yalıtılmıştır. Şifreleme başlatıldıktan ve kod belleğe yüklendikten sonra, bu kod güvenilmeyen bileşenlerden okunamaz veya değiştirilemez. 

### <a name="open-enclave-software-development-kit-oe-sdk"></a>Açık şifreleme yazılım geliştirme seti (OE SDK)<a id="oe-sdk"></a>

Bir kuşın içinde çalışan bir kod yazmak istiyorsanız, sağlayıcınız tarafından desteklenen bir kitaplık veya çerçeveyi kullanın. [Open Enclave SDK](https://github.com/openenclave/openenclave) (OE SDK), farklı gizli bilgi işlem özellikli donanımlar üzerinde soyutlama sağlayan açık KAYNAKLı bir SDK 'dir. 

OE SDK 'Sı, herhangi bir CSP üzerinde herhangi bir donanım üzerinde tek bir soyutlama katmanı olacak şekilde oluşturulmuştur. OE SDK 'Sı, Azure gizli bilgi işlem sanal makinelerinin en üstünde, en üstte uygulamalar oluşturmak ve çalıştırmak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

DCsv2 serisi bir sanal makine dağıtın ve bu makineye OE SDK 'Yı yükler.

> [!div class="nextstepaction"]
> [Azure Marketi 'nde gizli bilgi Işlem VM 'si dağıtma](quick-create-marketplace.md)