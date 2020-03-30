---
title: Azure Kubernetes Service'te desteklenen Kubernetes sürümleri
description: Azure Kubernetes Hizmeti'ndeki (AKS) kümelerin Kubernetes sürüm destek ilkesini ve yaşam döngüsünü anlayın
services: container-service
author: sauryadas
ms.topic: article
ms.date: 12/09/2019
ms.author: saudas
ms.openlocfilehash: 77d64391888957a1697a5823a1485413686682d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593453"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti’nde (AKS) desteklenen Kubernetes sürümleri

Kubernetes topluluk kabaca her üç ayda bir küçük sürümleri bültenleri. Bu sürümler yeni özellikler ve geliştirmeler içerir. Yama sürümleri daha sık (bazen haftalık) ve yalnızca küçük bir sürümde kritik hata düzeltmeleri için tasarlanmıştır. Bu yama sürümleri, güvenlik açıkları için düzeltmeler veya Kubernetes'e dayalı olarak üretimde çalışan çok sayıda müşteri ve ürünü etkileyen büyük hataları içerir.

AKS, yeni Kubernetes sürümlerini, yayının istikrarına bağlı olarak, bir upstream sürümünden sonraki 30 gün içinde onaylamayı ve serbest bırakmayı hedeflemektedir.

## <a name="kubernetes-versions"></a>Kubernetes sürümleri

Kubernetes standart [Anlamsal Sürüm](https://semver.org/) sürüm şemasını kullanır. Bu, Kubernetes'in her sürümünün bu numaralandırma düzenini izlediği anlamına gelir:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
```

Sürümdeki her sayı önceki sürümle genel uyumluluğu gösterir:

* Uyumsuz API değişiklikleri veya geriye dönük uyumluluk kırıldığında ana sürümler değişir.
* Diğer küçük sürümlere geriye dönük olarak uyumlu işlevsellik değişiklikleri yapıldığında küçük sürümler değişir.
* Yama sürümleri, geriye dönük uyumlu hata düzeltmeleri yapıldığında değişir.

Kullanıcılar, örneğin üretim kümeniz *1.12.14* ve *1.12.15'teyse,* çalıştırdıkları küçük sürümün en son yama sürümünü çalıştırmayı hedeflemelidir, örneğin *1.12* serisi için mevcut en son yama sürümüyse, kümenizin tam olarak yamave desteklenmediğinden emin olur olmaz *1.12.15'e* yükseltmeniz gerekir.

## <a name="kubernetes-version-support-policy"></a>Kubernetes sürüm destek ilkesi

AKS, Kubernetes'in üç küçük versiyonunu destekler:

* AKS (N) içinde yayımlanan geçerli küçük sürüm
* İki önceki küçük versiyon. Desteklenen her küçük sürüm de iki kararlı yamaları destekler.

Bu "N-2" olarak bilinir: (N (Son sürüm) - 2 (küçük sürümler)).

Örneğin, AKS bugün *1.15.a'yı tanıtırsa,* aşağıdaki sürümler için destek sağlanır:

Yeni küçük sürüm    |    Desteklenen Sürüm Listesi
-----------------    |    ----------------------
1.15.a               |    1.15.a, 1.15.b, 1.14.c, 1.14.d, 1.13.e, 1.13.f

".letter"ın yama versiyonlarını temsil ettiği yer.

Sürüm değişiklikleri ve beklentileri ile ilgili iletişimler hakkında ayrıntılı bilgi için aşağıdaki "İletişim" bölümüne bakın.

Yeni bir küçük sürüm tanıtıldığında, desteklenen en eski küçük sürüm ve yama sürümleri küçümsenir ve kaldırılır. Örneğin, geçerli desteklenen sürüm listesi:

```
1.15.a
1.15.b
1.14.c
1.14.d
1.13.e
1.13.f
```

Ve AKS bültenleri 1.16. *, bu demektir ki 1.13.* sürümler (tüm 1.13 sürümleri) kaldırılır ve destek dışıdır.

> [!NOTE]
> Müşteriler desteklenmeyen bir Kubernetes sürümü çalıştırıyorsa, küme için destek talep ederken yükseltmeleri istenecektir. Desteklenmeyen Kubernetes sürümlerini çalıştıran kümeler [AKS destek ilkeleri](https://docs.microsoft.com/azure/aks/support-policies)kapsamında değildir.

Küçük sürümlerinde yukarıdakiek olarak, AKS belirli bir küçük sürümün iki son **yama** sürümleri destekler. Örneğin, aşağıdaki desteklenen sürümler göz önüne alındığında:

```
Current Supported Version List
------------------------------
1.15.2, 1.15.1, 1.14.5, 1.14.4
```

Upstream Kubernetes 1.15.3 ve 1.14.6 yayımladı ve AKS bu yama sürümleri bültenleri, en eski yama sürümleri azat ve kaldırılır ve desteklenen sürüm listesi olur:

```
New Supported Version List
----------------------
1.15.*3*, 1.15.*2*, 1.14.*6*, 1.14.*5*
```

### <a name="communications"></a>İletişim

* Kubernetes'in yeni **küçük** versiyonları için
  * Tüm kullanıcılara yeni sürüm ve hangi sürümün kaldırılacağı hakkında genel olarak bilgilendirilir.
  * Yeni bir yama sürümü yayımlandığında, en eski yama sürümü aynı anda kaldırılır.
  * Müşterilerin desteklenen küçük bir sürüm sürümüne yükseltmek için genel bildirim tarihinden **itibaren 30 günü** var.
* Kubernetes'in yeni **yama** sürümleri için
  * Tüm kullanıcılar yeni yama sürümü yayımlanan ve en son yama sürümüne yükseltmek için bildirilir.
  * Kullanıcıların, en eski sürümü kaldırılmadan önce daha yeni, desteklenen bir yama sürümüne yükseltmek için **30 günü** vardır.

AKS, "serbest bırakılmış sürümü" tüm SLO / Hizmet Kalitesi ölçümlerinde etkin ve tüm bölgelerde kullanılabilen genel olarak kullanılabilen sürümler olarak tanımlar. AKS ayrıca, açıkça etiketlenmiş ve Önizleme hüküm ve koşullarına tabi önizleme sürümlerini de destekleyebilir.

#### <a name="notification-channels-for-aks-changes"></a>AKS değişiklikleri için bildirim kanalları

AKS, [GitHub'daki](https://github.com/Azure/AKS/releases)hizmette yayımlanan yeni Kubernetes sürümlerini, hizmet değişikliklerini ve bileşen güncelleştirmelerini özetleyen düzenli hizmet güncelleştirmeleri yayımlar.

Bu değişiklikler, yönetilen hizmetin bir parçası olarak sunulan düzenli bakımın bir parçası olarak tüm müşterilere yuvarlanır, bazıları açık yükseltmeler gerektirirken diğerleri hiçbir işlem gerektirmez.

Bildirimler de şu yoluyla gönderilir:

* [AKS Yayın notları](https://aka.ms/aks/releasenotes)
* Azure portalı bildirimleri
* [Azure güncelleştirme kanalı][azure-update-channel]

### <a name="supported-versions-policy-exceptions"></a>Desteklenen Sürümler İlke Özel Durumları

AKS, önceden haber vermeksizin hataları veya güvenlik sorunlarını etkileyen bir veya daha fazla kritik üretime sahip olduğu tespit edilen yeni/varolan sürümleri ekleme veya kaldırma hakkını saklı tutar.

Belirli yama sürümleri atlanabilir veya hatanın veya güvenlik sorununun önem derecesine bağlı olarak kullanıma hızlandırılabilir.

### <a name="azure-portal-and-cli-default-versions"></a>Azure portalı ve CLI varsayılan sürümleri

Portalda veya Azure CLI ile bir AKS kümesi dağıttığınızda, küme Varsayılan Olarak N-1 küçük sürümü ve en son düzeltme eki olarak sunulur. Örneğin, AKS *1.15.a*, *1.15.b*, *1.14.c*, *1.14.d*, *1.13.e*ve *1.13.f*destekler, seçilen varsayılan sürüm *1.14.c*.

AKS, müşterilere varsayılan olarak bilinen, kararlı ve yamalı bir sürüm sağlamak için N-1 varsayılanını seçer.

## <a name="list-currently-supported-versions"></a>Şu anda desteklenen sürümleri listele

Aboneliğiniz ve bölgeniz için şu anda hangi sürümlerin kullanılabilensürümlerin kullanılabileni öğrenmek için [az aks get-versions][az-aks-get-versions] komutunu kullanın. Aşağıdaki örnek, *EastUS* bölgesi için kullanılabilir Kubernetes sürümlerini listeler:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

## <a name="faq"></a>SSS

**Bir müşteri, desteklenmeyen küçük bir sürümle Bir Kubernetes kümesini yükseltirse ne olur?**

*N-3* sürümündeyseniz, desteğindışındasınız ve yükseltmeniz istenecektir. N-3 sürümünden n-2 sürümüne yükseltmeniz başarılı olursa, artık destek politikalarımızın içindesiniz. Örnek:

- Desteklenen en eski AKS sürümü *1.13.a* ise ve *1.12.b* veya daha büyükseniz, destek dışındasınızdır.
- *1.12.b'den* *1.13.a* veya üzeri yükseltme başarılı olursa, destek politikalarımıza geri dönersiniz.

Desteklenen *N-2* penceresinden daha eski sürümlere yapılan yükseltmeler desteklenmez. Bu gibi durumlarda, müşterilerin yeni AKS kümeleri oluşturmalarını ve desteklenen penceredeki sürümlerle iş yüklerini yeniden dağıtmalarını öneririz.

**'Destek Dışında' ne demek**

'Destek Dışında' çalıştırdığınız sürümün desteklenen sürümler listesinin dışında olduğu anlamına gelir ve destek isteğinde bulunularak kümeyi desteklenen bir sürüme yükseltmeniz istenir. Ayrıca, AKS desteklenen sürümler listesinin dışındaki kümeler için çalışma süresi veya başka garantiler sağlamaz.

**Bir müşteri, desteklenmeyen küçük bir sürümle Bir Kubernetes kümesini ölçeklendirdiğinde ne olur?**

AKS tarafından desteklenmeyen küçük sürümler için, ölçekleme veya dışarı çalışmaya devam etmelidir ancak kümenizi desteğe geri getirmek için yükseltmeniz önerilir.

**Bir müşteri sonsuza kadar Kubernetes sürümünde kalabilir mi?**

Evet. Ancak, küme AKS tarafından desteklenen sürümlerden birinde değilse, küme AKS destek ilkeleri dışındadır. Azure kümenizi otomatik olarak yükseltmez veya silmez.

**Düğüm havuzu desteklenen AKS sürümlerinden birinde değilse, denetim düzlemi hangi sürümü destekler?**

Denetim düzlemi, tüm düğüm havuzlarından bir sürüm penceresi içinde olmalıdır. Denetim düzlemi veya düğüm havuzlarını yükseltme hakkında daha fazla bilgi için düğüm havuzlarını yükseltme yle ilgili belgeleri ziyaret [edin.](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools)

## <a name="next-steps"></a>Sonraki adımlar

Kümenizi yükseltme hakkında daha fazla bilgi için azure [Kubernetes Hizmetini (AKS) yükseltme kümesine][aks-upgrade]bakın.

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
