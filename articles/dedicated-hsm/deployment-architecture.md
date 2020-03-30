---
title: Dağıtım mimarisi - Azure Özel HSM | Microsoft Dokümanlar
description: Uygulama mimarisinin bir parçası olarak Azure Özel HSM kullanırken göz önünde bulundurulması gereken temel tasarım konuları
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: mbaldwin
ms.openlocfilehash: 89e3bf95a6b048e5e97cfb151ef9302b70eac1c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77048561"
---
# <a name="azure-dedicated-hsm-deployment-architecture"></a>Azure Ayrılmış HSM dağıtım mimarisi

Azure Özel HSM, Azure'da şifreleme anahtar depolaması sağlar. Sıkı güvenlik gereksinimlerini karşılar. Müşteriler şunları yaparlarsa Azure Özel HSM'yi kullanmaktan yararlanacaktır:

* FIPS 140-2 Seviye 3 sertifikasını karşılamalı
* HSM'ye özel erişimleri olmasını zorunlu kılmasını zorunlu kılmasını
* cihazlarının tam denetimine sahip olmalıdır

HSM'ler Microsoft'un veri merkezlerine dağıtılır ve yüksek kullanılabilir bir çözümün temeli olarak kolayca bir çift aygıt olarak kullanılabilir. Ayrıca, afete dayanıklı bir çözüm için bölgeler arasında da dağıtılabilirler. Özel HSM'nin şu anda kullanılabilen bölgeleri şunlardır:

* Doğu ABD
* Doğu ABD 2
* Batı ABD
* Orta Güney ABD
* Güneydoğu Asya
* Doğu Asya
* Hindistan Orta
* Hindistan Güney
* Doğu Japonya
* Batı Japonya
* Kuzey Avrupa
* Batı Avrupa
* Güney Birleşik Krallık
* Batı Birleşik Krallık
* Orta Kanada
* Doğu Kanada
* Doğu Avustralya
* Güneydoğu Avustralya

Bu bölgelerin her birinde iki bağımsız veri merkezinde veya en az iki bağımsız kullanılabilirlik bölgesinde dağıtılan HSM rafları vardır. Güney Doğu Asya'da üç kullanılabilirlik bölgesi ve Doğu ABD 2'de iki tane bulunmaktadır. Avrupa, Asya ve ABD'de özel HSM hizmeti sunan toplam sekiz bölge bulunmaktadır. Azure bölgeleri hakkında daha fazla bilgi için resmi [Azure bölgeleri bilgilerine](https://azure.microsoft.com/global-infrastructure/regions/)bakın.
Özel HSM tabanlı herhangi bir çözüm için bazı tasarım faktörleri konum/gecikme süresi, yüksek kullanılabilirlik ve diğer dağıtılmış uygulamalar için destektir.

## <a name="device-location"></a>Cihaz konumu

Optimal HSM cihaz konumu, şifreleme işlemleri gerçekleştiren uygulamalara en yakın konumdadır. Bölge içinde gecikmenin tek basamaklı milisaniye olması beklenir. Bölgeler arası gecikme, bundan 5 ila 10 kat daha yüksek olabilir.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Yüksek kullanılabilirlik elde etmek için, bir müşteri yüksek kullanılabilirlik çifti olarak Gemalto yazılımı kullanılarak yapılandırılan bir bölgede iki HSM aygıtı kullanmalıdır. Bu tür bir dağıtım, tek bir aygıt anahtar işlemlerini işlemesini engelleyen bir sorunla karşılanınıyorsa anahtarların kullanılabilirliğini sağlar. Ayrıca güç kaynağı değişimi gibi kesme/düzeltme bakımlarını gerçekleştirirken riski önemli ölçüde azaltır. Bir tasarımın her türlü bölgesel düzeydeki başarısızlığı hesaba katması önemlidir. Kasırgalar, seller veya depremler gibi doğal afetler olduğunda bölgesel düzeyde arızalar meydana gelebilir. Bu tür olaylar, başka bir bölgede HSM aygıtları sağlayarak azaltılmalıdır. Başka bir bölgede dağıtılan aygıtlar Gemalto yazılım yapılandırması ile eşlenebilir. Bu, yüksek kullanılabilir ve olağanüstü durum esnekliğine dayanıklı bir çözüm için en az dağıtımın iki bölgede dört HSM aygıtı olduğu anlamına gelir. Bölgeler arasında yerel artıklık ve artıklık, gecikmeyi, kapasiteyi desteklemek veya uygulamaya özgü diğer gereksinimleri karşılamak için başka HSM aygıt dağıtımları eklemek için temel olarak kullanılabilir.

## <a name="distributed-application-support"></a>Dağıtılmış uygulama desteği

Özel HSM aygıtları genellikle anahtar depolama ve anahtar alma işlemlerini gerçekleştirmesi gereken uygulamaları desteklemek üzere dağıtılır. Özel HSM aygıtları bağımsız uygulama desteği için 10 bölüme sahiptir. Aygıt konumu, hizmeti kullanması gereken tüm uygulamaların bütünsel görünümüne dayanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım mimarisi belirlendikten sonra, bu mimariyi uygulamak için yapılandırma etkinliklerinin çoğu Gemalto tarafından sağlanacaktır. Bu, aygıt yapılandırmanın yanı sıra uygulama tümleştirme senaryolarını da içerir. Daha fazla bilgi için [Gemalto müşteri destek](https://supportportal.gemalto.com/csm/) portalını kullanın ve yönetim ve yapılandırma kılavuzlarını indirin. Microsoft iş ortağı sitesinde çeşitli tümleştirme kılavuzları vardır.
Örneğin yüksek kullanılabilirlik ve güvenlik gibi hizmetin tüm temel kavramlarının, aygıt sağlama veya uygulama tasarımı ve dağıtımından önce iyi anlaşılması önerilir.
Diğer kavram düzeyi konuları:

* [Yüksek Kullanılabilirlik](high-availability.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
