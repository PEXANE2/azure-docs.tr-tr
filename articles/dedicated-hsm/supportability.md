---
title: Desteklenebilirlik - Azure Özel HSM | Microsoft Dokümanlar
description: Farklı senaryolarda Azure Özel HSM için destek seçenekleri ve sorumluluk alanları
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70881012"
---
# <a name="azure-dedicated-hsm-supportability"></a>Azure Özel HSM Desteklenebilirliği

Azure Özel HSM Hizmeti, tüm yönetim kontrolü ve yönetim sorumluluğuyla tek müşteri kullanımı için fiziksel bir aygıt sağlar. Cihaz kullanılabilir hale [bir Gemalto SafeNet Luna 7 HSM modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Microsoft, bir izleme rolü olarak fiziksel seri bağlantı noktası eki dışında, bir müşteri tarafından sağlanan bir kez hiçbir yönetim erişimine sahip olacaktır.  Erişim olmadan, Microsoft'un devam eden yazılım düzeyi bakımı veya sistem yönetimi sorumlulukları olamaz. Sonuç olarak, müşteriler tipik operasyonel faaliyetlerden sorumludur.
Müşteriler HSM'leri kullanan uygulamalardan tamamen sorumludur ve destek veya danışmanlık tabanlı yardım için Gemalto ile birlikte çalışmalıdır. Operasyonel hijyenin müşteri sahipliğinin kapsamı nedeniyle, Microsoft'un bu hizmet için herhangi bir yüksek kullanılabilirlik garantisi sunması mümkün değildir. Uygulamalarının yüksek kullanılabilirlik elde etmek için doğru şekilde yapılandırıldığından emin olmak müşterinin sorumluluğundadır. Microsoft, aygıt durumunu ve ağ bağlantısını izleyecek ve koruyacaktır.

## <a name="getting-support"></a>Destek alma

Özel HSM için müşteri desteği, Microsoft ve Gemalto'nun ortak çabasıdır. Herhangi bir donanım sorunları veya ağ yolu sorunları Microsoft tarafından giderilecek ve yapılandırma, yazılım, firmware ve uygulama geliştirme gibi gerçek HSM ile ilgili her şey Gemalto tarafından ele alınacaktır. Bu destek modeli, en etkili desteğe giden en hızlı yolu sağlar. Belirli bir sorunla ilgili şüpheniz varsa, Microsoft'a bir destek isteği getirin ve size uygun şekilde yönlendirilmesini sağlayacağız. Microsoft tüm destek senaryolarına dahil olacak ve müşterilerimiz için en iyi destek deneyimini yaşamaya çalışacaktır.

## <a name="gemalto-support"></a>Gemalto desteği

Özel HSM hizmetini kullanan müşteriler, Plus Destek Planı uyarınca Gemalto'dan destek almaya hak kazanır. Bu sadece Gemalto destek portalı kullanarak bir kayıt süreci gerektirir. Özel HSM hizmetine erişmek için Microsoft ile olan ilk etkileşimin bir parçası olarak bunun için bir Müşteri Kimliği ve yönergeler sağlanacaktır. Gemalto'dan destek alma mekanizması [müşteri destek portalı](https://supportportal.gemalto.com/csm/)üzerindendir.
Önemli bir nokta Gemalto müşteri destek portalı nda indiryoluyla HSM (örneğin, istemci erişim yazılımı ve SDKs) kullanmak için gerekli tüm yazılım ve belgeleri sağlayacaktır.

### <a name="software-components"></a>Yazılım bileşenleri

HSM aygıtlarının yapılandırmasında çeşitli yazılım bileşenleri kullanılır:

* İstemci yazılımı
* SDK
* Araçlar

### <a name="guidance"></a>Rehber

Gemalto, [müşteri destek portalı](https://supportportal.gemalto.com/csm/)üzerinden yönetim ve yapılandırma rehberliği sunar. Geçerli bir müşteri kimliği kullanılarak oturum açtıktan sonra, bu belgeler indirilebilir. Gemalto ayrıca farklı senaryolar ve yazılım entegrasyonları ile müşterilerine yardımcı olmak için entegrasyon kılavuzları bir dizi sağlar. Daha fazla bilgi [için Microsoft için Gemalto iş ortağı sitesine](https://safenet.gemalto.com/partners/microsoft/)bakın.

### <a name="support"></a>Destek

HSM'lerin Özel HSM hizmetinin bir parçası olarak kullanılmasıyla ilgili herhangi bir yazılım düzeyindesorun veya soru doğrudan Gemalto desteğine iletilmelidir. Yukarıda listelenen tüm yazılım bileşenleri ve sağlama sonrası herhangi bir özel HSM yapılandırması Gemalto tarafından ele alınacaktır. Daha fazla bilgi için [Gemalto müşteri destek portalına](https://supportportal.gemalto.com/csm/)bakın.

### <a name="consulting-services"></a>Danışmanlık hizmetleri

HSM'yi kullanan özel uygulamaların tasarımı, geliştirilmesi ve dağıtımıyla ilgili herhangi bir yardım için Gemalto hesap temsilcinize başvurun.

## <a name="microsoft-support"></a>Microsoft desteği

Microsoft, fiziksel HSM aygıtlarının ağa erişilebilir olmasını ve tek bir müşterinin özel kullanımı için operasyonel durumda olmasını sağlar. Müşteriler aygıtın yapılandırma, yönetim ve yönetiminden sorumludur. Microsoft'un sorumlulukları şunlardır:

* Cihazın güç ve soğutma ya da soğutma gücüne sahip olduğundan emin olun
* HSM'nin çalışma durumunu koruma (örneğin, kesme/düzeltme senaryoları)
* Aygıta ağ üzerinden erişilebilir.

Aşağıdaki gibi sorunlar Microsoft'a bildirilmelidir:

* Bileşen hataları
* Tam cihaz arızası
* Ağ erişim sorunları
* Sağlanması ve deprovisioning sorunları.

Microsoft, temel sistem durumu telemetrisini sağlayan bir izleme rolü (yani yönetimsel rol değil) aracılığıyla aygıta fiziksel seri bağlantı noktası erişimine sahiptir.  Bu, müşteri bu izni kısıtlamayı seçmediği sürece Microsoft'un müşteriye sorunlarhakkında proaktif bildirim de sunmasını sağlar. 

### <a name="provisioning-and-decommissioning"></a>Sağlama ve hizmet dışı bırakma

Bir müşteri Özel HSM hizmeti için onaylı bir kaydı olduktan sonra, HSM kaynakları oluşturabilir (şu anda Azure portalı değil PowerShell veya komut satırı arabirimi üzerinden). Kaynak, belirli bir bölgedeki fiziksel aygıtı müşterinin önceden tanımlanmış sanal ağına (VNet) eşleyen bir ayırma işleminden geçer. Bir VNet'te görünür hale geldiğinde, müşteri aygıta erişebilir ve gereksinimlere göre daha da yapılandırabilir. Müşteriler Gemalto istemci yazılım ve araçlarını kullanarak özel HSM'lerine erişir. Kaynak oluşturma işlemi Microsoft tarafından desteklenir. Özel yapılandırma süreci ve ötesi Gemalto tarafından desteklenir. (bkz. yukarıdaki Gemalto desteği). Bir müşteri HSM kullanmayı bitirdiğinde, verilerin kalıcı olmasını sağlamak için sıfırlanmalıdır (veya sıfırlanmalıdır). Aygıtı sıfırlama işlemi tüm özel yapılandırmayı ve verileri kaldırır. Microsoft, aygıtı niçin bulunduğuna ve bozulmamış bir durumda havuza geri döndürür. Bu, aygıt havuza döndürüldüğünde önceki müşteri etkinliğine dair bir kanıt olmadığı anlamına gelir. 

### <a name="hardware-issues"></a>Donanım sorunları

HSM cihazı yedekli ve değiştirilebilir güç kaynakları ve fan ünitelerine sahiptir.  Ancak, fan birimi kaldırma hala bir kurcalama olayına neden olacaktır. Bir bileşen hatası oluştuğunda, Microsoft, bileşen düzeyi sorununu en az kesintiye ve müşterilerimizin hizmet kullanılabilirliği için en düşük riske neden olacak şekilde gidermek için en uygun işlemi kullanır.
Cihazın daha ciddi bir şekilde arızalanması, bu aygıtın ücretsiz havuzdan yeni bir cihazla değiştirilmesine neden olur. Müşteri, yeni aygıtı senkronize edinve tam operasyonel duruma döndürmek için mevcut HA çiftine içerir. Başarısız olan cihaz, veri taşıyan aygıtlarını veri merkezinde yerinde kaldırıp parçalatacaktır. Sadece şasi geri dönüşüm için Gemalto'ya iade edilecektir.


### <a name="networking-issues"></a>Ağ sorunları

Müşteriler HSM aygıtına erişim ağı sorunları yaşarsa, Microsoft desteğine başvurmaları gerekir. Ağ erişimi için basit bir test, HSM aygıtına bağlanmak için SSH kullanmaktır. Bu başarısız olursa, Microsoft desteğine başvurun.

## <a name="service-level-expectations-for-support"></a>Destek için hizmet düzeyi beklentileri

Microsoft destek hizmeti düzeyleri için [Azure destek planına](https://azure.microsoft.com/support/plans/)bakın.
Gemalto destek hizmet düzeyleri için [Gemalto Destek Essentials](https://azure.microsoft.com/support/plans/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz sağlama ve uygulama tasarımı veya dağıtımından önce yüksek kullanılabilirlik ve güvenlik gibi temel kavramların iyi anlaşılması önerilir.

* [Dağıtım Mimarisi](deployment-architecture.md)
* [Yüksek Kullanılabilirlik](high-availability.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)

