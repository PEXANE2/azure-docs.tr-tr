---
title: AKS sanal makine konaklarında güvenlik sağlamlaştırma
description: AKS VM konak işletim sistemindeki güvenlik sağlamlaştırma hakkında bilgi edinin
services: container-service
author: mlearned
ms.topic: article
ms.date: 09/11/2019
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: b7552fc083c5ed340dc54c2a31160b0c8b4bd076
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80420893"
---
# <a name="security-hardening-for-aks-agent-node-host-os"></a>AKS aracı düğümü konak işletim sistemi için güvenlik sağlamlaştırma

Azure Kubernetes Service (AKS), SOC, ISO, PCI DSS ve HIPAA standartları ile uyumlu bir güvenli hizmettir. Bu makalede, AKS sanal makine konaklarına uygulanan güvenlik sağlamlaştırma ele alınmaktadır. AKS güvenliği hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (aks) içindeki uygulamalar ve kümeler Için güvenlik kavramları](https://docs.microsoft.com/azure/aks/concepts-security).

> [!Note]
> Bu belge yalnızca AKS 'deki Linux aracılarının kapsamına alınır.

AKS kümeleri, AKS üzerinde çalışan kapsayıcılar için kullanılan güvenlik için iyileştirilmiş bir işletim sistemi çalıştıran konak sanal makinelerinde dağıtılır. Bu konak işletim sistemi, ek güvenlik sağlamlaştırma ve iyileştirmeler uygulanmış bir **Ubuntu 16.04. LTS** görüntüsünü temel alır (bkz. güvenlik sağlamlaştırma ayrıntıları).

Güvenlik sıkı ana bilgisayar işletim sisteminin hedefi, saldırının yüzey alanını azaltmaktır ve kapsayıcıların güvenli bir şekilde dağıtılması için iyileştirmektir.

> [!Important]
> Güvenliği sağlamlaştırılmış işletim sistemi, CIS sınama olarak işaretlendi. CIS kıyaslamalarıyla örtüşmelere karşın, amaç CIS ile uyumlu değildir. Ana bilgisayar işletim sistemi sağlamlaştırma için hedef, Microsoft 'un kendi iç ana bilgisayar güvenlik standartları ile tutarlı bir güvenlik düzeyine yakınsamaktır.

## <a name="security-hardening-features"></a>Güvenlik sağlamlaştırma özellikleri

* AKS, varsayılan olarak güvenlik açısından iyileştirilmiş bir konak işletim sistemi sağlar. Alternatif bir işletim sistemi seçme seçeneği yoktur.

* Azure, AKS sanal makine konaklarına günlük düzeltme ekleri (güvenlik düzeltme ekleri dahil) uygular. Bu düzeltme eklerinin bazılarının yeniden başlatılması gerekir, diğerleri bu şekilde çalışmaz. Gerektiğinde AKS VM konağının yeniden başlatmaları zamanlamaktan siz sorumlusunuz. AKS düzeltme eki uygulama hakkında yönergeler için bkz. [aks düğümlerine düzeltme eki uygulama](https://docs.microsoft.com/azure/aks/node-updates-kured).

## <a name="what-is-configured"></a>Ne yapılandırıldı

| CI  | Denetim açıklaması|
|---|---|
| 1.1.1.1 |Cramfs dosya sistemleri 'ın takılmasını devre dışı bırakıldığından emin olun|
| 1.1.1.2 |Freevxfs dosya sistemleri 'ın takılmasını devre dışı bırakıldığından emin olun|
| 1.1.1.3 |JFFS2 dosya sistemleri 'ın takılmasını devre dışı bırakıldığından emin olun|
| 1.1.1.4 |HFS dosya sistemleri 'ın takılmasını devre dışı bırakıldığından emin olun|
| 1.1.1.5 |HFS ve dosya sistemlerinin takılmasını devre dışı bırakıldığından emin olun|
|1.4.3 |Tek Kullanıcı modu için kimlik doğrulamanın gerekli olduğundan emin olun |
|1.7.1.2 |Yerel oturum açma uyarısı başlığının düzgün yapılandırıldığından emin olun |
|1.7.1.3 |Uzaktan oturum açma uyarısı başlığının düzgün yapılandırıldığından emin olun |
|1.7.1.5 |/Etc/sorununun izinlerinin yapılandırıldığından emin olun |
|1.7.1.6 |/Etc/Issue.net izinlerinin yapılandırıldığından emin olun |
|2.1.5 |--Streaming-Connection-Idle-timeout değerinin 0 olarak ayarlı olmadığından emin olun |
|3.1.2 |Paket yeniden yönlendirme gönderimi devre dışı olduğundan emin olun |
|3.2.1 |Kaynak yönlendirilmiş paketlerin kabul edilmediğinden emin olun |
|3.2.2 |ICMP yeniden yönlendirmelerinin kabul edilmediğinden emin olun |
|3.2.3 |Güvenli ıCMP yeniden yönlendirmelerinin kabul edilmediğinden emin olun |
|3.2.4 |Şüpheli paketlerin günlüğe kaydedildiğinden emin olun |
|3.3.1 |IPv6 yönlendirici tanıtımlarının kabul edilmediğinden emin olun |
|3.5.1 |DCCP 'nin devre dışı olduğundan emin olun |
|3.5.2 |SCTP 'nin devre dışı olduğundan emin olun |
|3.5.3 |RDS 'nin devre dışı olduğundan emin olun |
|3.5.4 |TIPC 'nin devre dışı olduğundan emin olun |
|4.2.1.2 |Günlüğün yapılandırıldığından emin olun |
|5.1.2 |/Etc/crontab üzerinde izinlerin yapılandırıldığından emin olun |
|5.2.4 |SSH X11 iletiminin devre dışı olduğundan emin olun |
|5.2.5 |SSH Maxauthdenemeler 'in 4 veya daha az olarak ayarlandığından emin olun |
|5.2.8 |SSH kök oturum açmanın devre dışı olduğundan emin olun |
|5.2.10 |SSH PermitUserEnvironment 'ın devre dışı olduğundan emin olun |
|5.2.11 |Yalnızca onaylanan en fazla algoritmaların kullanıldığından emin olun |
|5.2.12 |SSH boşta zaman aşımı aralığının yapılandırıldığından emin olun |
|5.2.13 |SSH LoginGraceTime 'in bir dakika veya daha az olarak ayarlandığından emin olun |
|5.2.15 |SSH uyarı başlığının yapılandırıldığından emin olun |
|5.3.1 |Parola oluşturma gereksinimlerinin yapılandırıldığından emin olun |
|5.4.1.1 |Parola kullanım süresinin 90 gün veya daha az olduğundan emin olun |
|5.4.1.4 |Etkin olmayan parola kilidinin 30 gün veya daha az olduğundan emin olun |
|5.4.4 |Varsayılan Kullanıcı umask 'in 027 veya daha kısıtlayıcı olduğundan emin olun |
|5,6 |Su komutuna erişimin kısıtlanmasını sağlayın|

## <a name="additional-notes"></a>Ek notlar
 
* Saldırı yüzeyi alanını daha fazla azaltmak için, işletim sisteminde gereksiz bazı çekirdek modülü sürücüleri devre dışı bırakılmıştır.

* Güvenliği sağlamlaştırılmış işletim sistemi, AKS için özel olarak oluşturulup sürdürülür ve AKS platformunun dışında desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar  

AKS güvenliği hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 

[Azure Kubernetes Hizmeti (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)

[AKS güvenlik konuları](https://docs.microsoft.com/azure/aks/concepts-security)

[AKS en iyi uygulamaları](https://docs.microsoft.com/azure/aks/best-practices)
