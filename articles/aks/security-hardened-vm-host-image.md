---
title: AKS sanal makine ana bilgisayarlarında güvenlik sertleştirmesi
description: AKS VM host OS'de güvenlik sertliği hakkında bilgi edinin
services: container-service
author: saudas
ms.topic: article
ms.date: 09/11/2019
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: d4105a9fba3c40c563198040afb811625727ead0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594389"
---
# <a name="security-hardening-in-aks-virtual-machine-hosts"></a>AKS sanal makine ana bilgisayarlarında güvenlik sertleştirmesi 

Azure Kubernetes Service (AKS), SOC, ISO, PCI DSS ve HIPAA standartlarına uygun güvenli bir hizmettir. Bu makale, AKS sanal makine ana bilgisayarlarına uygulanan güvenlik sertleştirmesini kapsar. AKS güvenliği hakkında daha fazla bilgi için [Azure Kubernetes Hizmeti'ndeki (AKS) uygulamalar ve kümeler için Güvenlik kavramlarına](https://docs.microsoft.com/azure/aks/concepts-security)bakın.

AKS kümeleri, güvenlik için optimize edilmiş işletim sistemi çalıştıran ana bilgisayar sanal makinelerinde dağıtılır. Bu ana bilgisayar işletim sistemi şu anda bir ubuntu 16.04.LTS görüntüsünü ve bir dizi ek güvenlik sertleştirme adımını uygulamalıdır (bkz.   

Güvenlik sertleştirilmiş ana bilgisayar işletim sistemi amacı saldırı yüzey alanını azaltmak ve güvenli bir şekilde konteyner dağıtım ını sağlamaktır. 

> [!Important]
> Güvenlik sertleştirilmiş işletim sistemi BDT kıyaslanmış DeğİlDIR. BDT kriterleri ile çakışmalar olsa da, amaç BDT uyumlu olmak değildir. Ana bilgisayar işletim sistemi sertleştirmesinin amacı, Microsoft'un kendi iç ana bilgisayar güvenlik standartlarıyla tutarlı bir güvenlik düzeyinde yakınsamayapmaktır. 

## <a name="security-hardening-features"></a>Güvenlik sertleştirme özellikleri 

* AKS varsayılan olarak bir güvenlik optimize ana bilgisayar işletim sistemi sağlar. Alternatif bir işletim sistemi seçmek için geçerli bir seçenek yoktur. 

* Azure, AKS sanal makine ana bilgisayarlarına günlük düzeltme emaları (güvenlik düzeltme emültleri dahil) uygular. Bu düzeltme eteklerinden bazıları yeniden başlatmayı gerektirirken, diğerleri bunu gerektirmez. Aks VM ana bilgisayar yeniden başlatmalarını gerektiği gibi planlamaktan siz sorumlusunuz. AKS yama otomatikleştirmek için nasıl rehberlik için [aks düğümleri yama](https://docs.microsoft.com/azure/aks/node-updates-kured)bakın.

Aşağıda güvenlik optimize ana işletim sistemi üretmek için AKS-Engine uygulanan görüntü sertleştirme çalışmalarının bir özetidir. Çalışma [bu GitHub projesinde](https://github.com/Azure/aks-engine/projects/7)uygulanmıştır.  

AKS-Engine şu anda herhangi bir özel güvenlik standardına destek vermez veya bunlara uymaz, ancak BD (Internet Security Merkezi) denetim idiler, uygun olduğu durumlarda kolaylık sağlamak için sağlanır. 

## <a name="whats-configured"></a>Ne yapılandırıldı?

| Bdt  | Denetim açıklaması| 
|---|---|
| 1.1.1.1 |Cramfs filesystems montaj devre dışı olduğundan emin olun|
| 1.1.1.2 |Freevxfs filesistemlerinin montajının devre dışı bırakıldığından emin olun|
| 1.1.1.3 |jffs2 file sistemlerinin montajının devre dışı bırakıldığından emin olun|
| 1.1.1.4 |HFS dosya sistemlerinin montajının devre dışı bırakıldığından emin olun|
| 1.1.1.5 |HFS Plus file sistemlerinin montajının devre dışı bırakıldığından emin olun|
|1.4.3 |Tek kullanıcı modu için gerekli kimlik doğrulamayı sağlama |
|1.7.1.2 |Yerel oturum açma uyarı afişinin düzgün şekilde yapılandırıldığından emin olun |
|1.7.1.3 |Uzaktan giriş uyarı başlığının düzgün şekilde yapılandırıldığından emin olun |
|1.7.1.5 |/etc/issue ile ilgili izinlerin yapılandırıldığından emin olun |
|1.7.1.6 |/etc/issue.net üzerindeki izinlerin yapılandırıldığından emin olun |
|2.1.5 |--akış-bağlantı-boşta zaman ayarı 0 olarak ayarlanmadığından emin olun |
|3.1.2 |Paket yönlendirme göndermenin devre dışı bırakıldığından emin olun |
|3.2.1 |Kaynak yönlendirilen paketlerin kabul edilmeydiğinden emin olun |
|3.2.2 |ICMP yönlendirmelerinin kabul edilmeydiğinden emin olun |
|3.2.3 |Güvenli ICMP yönlendirmelerinin kabul edilmeydiğinden emin olun |
|3.2.4 |Şüpheli paketlerin günlüğe kaydedilmesini sağlamak |
|3.3.1 |IPv6 yönlendirici reklamlarının kabul edilmeydiğinden emin olun |
|3.5.1 |DCCP'nin devre dışı bırakıldığından emin olun |
|3.5.2 |SCTP'nin devre dışı bırakıldığından emin olun |
|3.5.3 |RDS'nin devre dışı bırakıldığından emin olun |
|3.5.4 |TIPC'nin devre dışı bırakıldığından emin olun |
|4.2.1.2 |Günlüğe kaydetmenin yapılandırıldığından emin olun |
|5.1.2 |/etc/crontab üzerindeki izinlerin yapılandırıldığından emin olun |
|5.2.4 |SSH X11 yönlendirmenin devre dışı bırakıldığından emin olun |
|5.2.5 |SSH MaxAuthTries'in 4 veya daha az olarak ayarlandığından emin olun |
|5.2.8 |SSH kök girişinin devre dışı bırakıldığından emin olun |
|5.2.10 |SSH İzniKullanıcı Ortamının devre dışı bırakıldığından emin olun |
|5.2.11 |Yalnızca onaylanmış MAX algoritmalarının kullanıldığından emin olun |
|5.2.12 |SSH Boşta Zaman Aralığının yapılandırıldığından emin olun |
|5.2.13 |SSH LoginGraceTime'ın bir dakika veya daha az olarak ayarlandığından emin olun |
|5.2.15 |SSH uyarı afişinin yapılandırıldığından emin olun |
|5.3.1 |Parola oluşturma gereksinimlerinin yapılandırıldığından emin olun |
|5.4.1.1 |Parola süresinin 90 gün veya daha kısa olduğundan emin olun |
|5.4.1.4 |Etkin olmayan parola kilidinin 30 gün veya daha az olduğundan emin olun |
|5.4.4 |Varsayılan kullanıcı umask 027 veya daha kısıtlayıcı olduğundan emin olun |
|5.6 |Su komutuna erişimin kısıtlandığını emin olun|

## <a name="additional-notes"></a>Ek notlar
 
* Saldırı yüzey alanını daha da azaltmak için, işletim sistemi bazı gereksiz çekirdek modülü sürücüleri devre dışı bırakıldı. 

* Güvenlik sertleştirilmiş işletim sistemi AKS platformu dışında desteklenmez. 

## <a name="next-steps"></a>Sonraki adımlar  

AKS güvenliği hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 

[Azure Kubernetes Hizmeti (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)

[AKS güvenlik hususları](https://docs.microsoft.com/azure/aks/concepts-security)

[AKS en iyi uygulamaları](https://docs.microsoft.com/azure/aks/best-practices)
