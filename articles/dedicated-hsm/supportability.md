---
title: Desteklenebilirlik-Azure ayrılmış HSM | Microsoft Docs
description: Farklı senaryolarda Azure ayrılmış HSM için destek seçenekleri ve sorumluluk bölgeleri
services: dedicated-hsm
author: johndaw
manager: rkarlin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.custom: seodec18
ms.date: 03/27/2019
ms.author: mbaldwin
ms.openlocfilehash: d83d688707baf6098d63dfde9b4181eb04fb9729
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "70881012"
---
# <a name="azure-dedicated-hsm-supportability"></a>Azure ayrılmış HSM desteklenebilirliği

Azure ayrılmış HSM hizmeti, tek müşteri kullanımı için tüm yönetim denetimi ve yönetim sorumluluğuna sahip fiziksel bir cihaz sağlar. Kullanılabilir cihaz, bir [Gemalto SafeNet Luna 7 HSM modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Microsoft, fiziksel seri bağlantı noktası ekinin ötesinde bir izleme rolü olarak bir müşteri tarafından sağlandıktan sonra yönetim erişimi olmayacaktır.  Microsoft, erişim olmadan hiçbir sürekli yazılım düzeyinde bakım veya sistem yönetimi sorumluluğu içermez. Sonuç olarak, müşteriler tipik işlem etkinliklerinden sorumludur.
Müşteriler, HSM 'leri kullanan uygulamalardan tamamen sorumludur ve destek veya danışmanlık tabanlı yardım için Gemalle birlikte çalışmalıdır. İşlemsel Hygiene 'ın müşteri sahipliğinin uzatması nedeniyle Microsoft 'un bu hizmet için herhangi bir tür yüksek kullanılabilirlik garantisi sunabileceği için bu mümkün değildir. Uygulamanın yüksek kullanılabilirliğe sahip olacak şekilde doğru şekilde yapılandırılmasını sağlamak müşterinin sorumluluğundadır. Microsoft, cihaz sistem durumu ve ağ bağlantısını izler ve korur.

## <a name="getting-support"></a>Destek alma

Adanmış HSM için müşteri desteği, Microsoft ve Gemalto arasındaki birleşme çabadır. Herhangi bir donanım sorunu veya ağ yolu sorunları Microsoft tarafından ele alınacaktır ve yapılandırma, yazılım, bellenim ve uygulama geliştirme gibi gerçek HSM ile yapılan her türlü şey Gemalto tarafından ele alınacaktır. Bu destek modeli en etkin desteğe en hızlı yol sağlar. Belirli bir sorunla karşılaşırsanız, Microsoft ile bir destek isteği oluşturup uygun şekilde yönlendirildiğinizden emin olursunuz. Microsoft tüm destek senaryolarından sorumlu olmaya devam eder ve müşterilerimiz için en iyi destek deneyimine yöneliktir.

## <a name="gemalto-support"></a>Gemalto desteği

Adanmış HSM hizmetini kullanan müşteriler, Gemallerinden ve Destek planına kadar olan destek için uygun olan kullanıcıları destekler. Bu, yalnızca Gemalto Destek portalını kullanarak bir kayıt işlemi gerektirir. Bu, adanmış HSM hizmetine erişim kazanmak için Microsoft ile ilk katılım 'nin bir parçası olarak bu bir müşteri KIMLIĞI ve yönergeler sağlanacaktır. Gemalto 'dan destek almaya yönelik mekanizma, [Müşteri Destek Portalı](https://supportportal.gemalto.com/csm/)aracılığıyla yapılır.
Bir anahtar noktası, Gemalto 'ın, müşteri destek portalı 'nda indirme yoluyla HSM 'yi (örneğin, istemci erişim yazılımı ve SDK 'lar) kullanması için gereken tüm yazılım ve belgeleri sağlayacağına yönelik bir örnektir.

### <a name="software-components"></a>Yazılım bileşenleri

HSM cihazlarının yapılandırmasında çeşitli yazılım bileşenleri kullanılır:

* İstemci yazılımı
* SDK
* Araçlar

### <a name="guidance"></a>Rehber

Gemalto, [Müşteri Destek Portalı](https://supportportal.gemalto.com/csm/)aracılığıyla kullanılabilir yönetim ve yapılandırma kılavuzu sağlar. Geçerli bir müşteri KIMLIĞI kullanılarak oturum açıldıktan sonra bu belgeler indirilemez. Gemalto, müşterilerin farklı senaryolar ve yazılım tümleştirmelerine yardımcı olmak için bir dizi tümleştirme kılavuzu sağlar. Daha fazla bilgi için bkz. [Microsoft Için Gemalto iş ortağı sitesi](https://safenet.gemalto.com/partners/microsoft/).

### <a name="support"></a>Destek

Özel HSM hizmeti 'nin bir parçası olarak HSM 'leri kullanmayla ilgili herhangi bir yazılım düzeyi sorunu veya sorusu, doğrudan desteklemek üzere bildirilmelidir. Yukarıda listelenen tüm yazılım bileşenleri ve sağlama sonrası olan tüm özel HSM yapılandırmaları, Gemalto tarafından giderilecektir. Daha fazla bilgi için bkz. [Gemalto müşteri destek portalı](https://supportportal.gemalto.com/csm/).

### <a name="consulting-services"></a>Danışmanlık hizmetleri

HSM 'yi kullanan özel uygulamaların tasarımına, geliştirilmesine ve dağıtımına ilişkin herhangi bir yardım için, Gemalto hesap temsilcinizle iletişime geçin.

## <a name="microsoft-support"></a>Microsoft desteği

Microsoft, fiziksel HSM cihazlarının ağa erişilebilir ve tek bir müşterinin özel kullanımı için işlemsel bir durumda olmasını sağlayacaktır. Müşteriler, cihazın yapılandırmasından, yönetiminden ve yönetiminden sorumludur. Microsoft sorumlulukları şunları içerir:

* Cihazın güç ve soğutma içerdiğinden emin olma
* HSM 'nin işletimsel durumunu koruma (örneğin, kesme/çözme senaryoları)
* Cihaza ağ üzerinden erişilebilir.

Aşağıdakiler gibi sorunlar Microsoft 'a bildirilmesi gerekir:

* Bileşen sorunları
* Tam cihaz hatası
* Ağ erişimi sorunları
* Sağlama ve sağlamayı kaldırma sorunları.

Microsoft, temel sistem durumu telemetrisini sağlayan bir izleme rolü (yönetim rolü olmayan) aracılığıyla cihaza fiziksel seri bağlantı noktası erişimine sahiptir.  Bu, müşteri tarafından bu izni kısıtlamayı seçmedikçe Microsoft 'un müşteriye yönelik sorunlara yönelik öngörülü bildirimler sağlamasına imkan tanır. 

### <a name="provisioning-and-decommissioning"></a>Sağlama ve yetki alma

Bir müşterinin adanmış HSM hizmeti için onaylanmış bir kaydı olduktan sonra, HSM kaynakları (Şu anda PowerShell veya komut satırı arabirimi aracılığıyla Azure portal değil,) oluşturabilebilecektir. Kaynak, belirtilen bölgedeki fiziksel bir cihazı müşterinin önceden tanımlanmış sanal ağına (VNet) eşleyen bir ayırma işleminden geçer. Bir VNet üzerinde görünür hale getirildikten sonra müşteri cihaza erişebilir ve gereksinim başına daha fazla şekilde yapılandırabilir. Müşteriler, Gemalto istemci yazılımını ve araçlarını kullanarak kendi adanmış HSM 'lerine erişir. Kaynak oluşturma işlemi Microsoft tarafından desteklenir. Özel yapılandırma işlemi ve ötesinde, Gemalto tarafından desteklenir. (bkz. Gemalto desteği). Bir müşteri bir HSM kullanmayı bitirdiğinde, verilerin kalıcı olmamasını sağlamak için sıfırlanmalıdır (veya sıfırlanmış olmalıdır). Cihazı sıfırlama işlemi tüm özel yapılandırma ve verileri kaldırır. Microsoft, cihazı ayırır ve bir Pristine durumundaki havuza geri döndürür. Bu, cihaz havuza döndürüldüğünde önceki müşteri etkinliğinin kanıtı olmadığı anlamına gelir. 

### <a name="hardware-issues"></a>Donanım sorunları

HSM cihazında gereksiz ve değiştirilebilir güç kaynakları ve fan birimleri vardır.  Ancak, fan birimi kaldırma işlemi yine de bir yetkisiz olay oluşmasına neden olur. Bir bileşen hatası oluştuğunda, Microsoft, bileşen düzeyi sorununa en uygun süreci kullanarak müşterilerin hizmet kullanılabilirliğine en düşük düzeyde kesinti ve en düşük risklere neden olur.
Cihazda daha ciddi bir hata olması, bu cihazın boş havuzdan yeni bir tane tarafından değiştirilmesine neden olur. Müşteri, ister yeni cihazı, hem de tam işlemsel duruma dönmesi için mevcut HA çiftine dahil eder. Başarısız olan cihazın verileri, veri merkezindeki sitede kaldırılan ve çıkarılan cihazları alır. Geri dönüştürme için yalnızca kasa Gemalto 'a döndürülür.


### <a name="networking-issues"></a>Ağ sorunları

Müşteriler HSM cihazına ağ erişimi sorunları yaşlarsa Microsoft desteği 'ne başvurmalıdır. Ağ erişimi için basit bir test, HSM cihazına bağlanmak için SSH kullanmaktır. Bu başarısız olursa, Microsoft desteği 'ne başvurun.

## <a name="service-level-expectations-for-support"></a>Destek için hizmet düzeyi beklentileri

Microsoft destek hizmet düzeyleri için [Azure Destek planına](https://azure.microsoft.com/support/plans/)bakın.
Hizmet düzeylerini desteklemek için, bkz. [Gemalto destek temelleri](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi temel kavramların cihaz sağlama ve uygulama tasarımı ya da dağıtımdan önce iyi anlaşıldığından emin olmanız önerilir.

* [Dağıtım mimarisi](deployment-architecture.md)
* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)

