---
title: Azure Kubernetes Service'te desteklenen Kubernetes sürümleri
description: Azure Kubernetes Service 'teki (AKS) Kubernetes sürüm destek ilkesini ve kümelerin yaşam döngüsünü anlayın
services: container-service
ms.topic: article
ms.date: 07/08/2020
author: palma21
ms.author: jpalma
ms.openlocfilehash: 273c41a523de8b2776982e5229c5a8b618b82c19
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475201"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti’nde (AKS) desteklenen Kubernetes sürümleri

Kubernetes topluluğu, her üç ayda bir ikincil sürümleri kabaca yayınlar. Bu sürümler yeni özellikler ve geliştirmeler içerir. Düzeltme Eki sürümleri daha sık (bazen haftalık) ve yalnızca küçük bir sürümde kritik hata düzeltmeleri için tasarlanmıştır. Bu düzeltme eki sürümleri, güvenlik açıklarına veya önemli hatalara yönelik düzeltmeler içerir.

## <a name="kubernetes-versions"></a>Kubernetes sürümleri

Kubernetes standart [anlamsal sürüm](https://semver.org/) oluşturma şemasını kullanır, bu da her bir Kubernetes sürümünün bu numaralandırma düzenini izlediği anlamına gelir:

```
[major].[minor].[patch]

Example:
  1.17.7
  1.17.8
```

Sürümdeki her bir sayı, önceki sürümle genel uyumluluğu gösterir:

* Büyük sürümler uyumsuz API değişiklikleri veya geriye dönük uyumluluk bozuk olduğunda değişir.
* Küçük sürümler, diğer küçük yayınlar ile geriye doğru uyumlu olan işlevsellik değişiklikleri yapıldığında değişir.
* Geriye dönük olarak uyumlu hata düzeltmeleri yapıldığında düzeltme eki sürümleri değişir.

Kullanıcılar, çalıştırdığı alt sürümün en son düzeltme eki sürümünü çalıştırmalıdır. Örneğin, üretim kümeniz açık **`1.17.7`** ve **`1.17.8`** *1,17* serisi için kullanılabilir en son düzeltme eki sürümündedir, **`1.17.8`** kümenizin tamamen düzeltme eki uygulanmış ve desteklendiğinden emin olmak için ' a yükseltmeniz gerekir.

## <a name="kubernetes-version-support-policy"></a>Kubernetes sürüm desteği ilkesi

AKS, tüm SLO veya SLA ölçümlerine etkin bir sürüm olarak ve tüm bölgelerde kullanılabilir olduğunda, genel olarak kullanılabilen bir sürümü tanımlar. AKS, Kubernetes 'in üç GA sürümlerini destekler:

* AKS içinde yayınlanan en son GA alt sürümü (N olarak adlandırılıyoruz). 
* Önceki iki alt sürüm. 
* Desteklenen her bir alt sürüm ayrıca en fazla iki (2) kararlı düzeltme eki destekler.
* AKS, açıkça etiketlenmiş ve [Önizleme hüküm ve koşullarına][preview-terms]tabi olan önizleme sürümlerini de destekleyebilir.

> [!NOTE]
> AKS, aşamalı bölge dağıtımını içeren güvenli dağıtım uygulamalarını kullanır. Bu, yeni bir sürüm veya yeni bir sürümün tüm bölgelerde kullanılabilir olması için 10 iş günü sürebileceği anlamına gelir.

AKS üzerinde Kubernetes sürümlerinin desteklenen penceresi "N-2" olarak bilinir: (N (en son sürüm)-2 (ikincil sürümler)).

Örneğin, AKS *1.17.* bugün, aşağıdaki sürümler için destek sağlanır:

Yeni ikincil sürüm    |    Desteklenen sürüm listesi
-----------------    |    ----------------------
1.17. a               |    1.17. a, 1.17. b, 1.16. c, 1.16. d, 1.15. e, 1.15. f

Burada ". letter", düzeltme eki sürümlerinin temsilcisidir.

Yeni bir alt sürüm ortaya çıkarıldığında, desteklenen en eski ikincil sürüm ve düzeltme eki sürümleri kullanım dışıdır ve kaldırılır. Örneğin, geçerli desteklenen sürüm listesi şu ise:

```
1.17.a
1.17.b
1.16.c
1.16.d
1.15.e
1.15.f
```

Ve AKS yayımları 1,18. \* Bu, tüm 1,15. \* sürümlerin kaldırılabileceği ve 30 gün içinde destek dışı bırakılacak.

> [!NOTE]
> Müşteriler desteklenmeyen bir Kubernetes sürümü çalıştırıyorsa, bu, küme için destek istendiğinde yükseltmeniz istenir. Desteklenmeyen Kubernetes yayınları çalıştıran kümeler [aks destek ilkeleri](./support-policies.md)kapsamında değildir.

Yukarıdaki ' a ek olarak, AKS, belirli bir alt sürümün en fazla iki **Düzeltme Eki** sürümünü destekler. Bu nedenle, aşağıdaki desteklenen sürümler verilmiştir:

```
Current Supported Version List
------------------------------
1.17.8, 1.17.7, 1.16.10, 1.16.9
```

AKS yayınları `1.17.9` ve `1.16.11` , en eski düzeltme eki sürümleri kullanım dışıdır ve kaldırılır ve desteklenen sürüm listesi şu şekilde olur:

```
New Supported Version List
----------------------
1.17.*9*, 1.17.*8*, 1.16.*11*, 1.16.*10*
```

### <a name="supported-kubectl-versions"></a>Desteklenen `kubectl` sürümler

`kubectl` [Kubectl Için Kubernetes destek ilkesiyle](https://kubernetes.io/docs/setup/release/version-skew-policy/#kubectl)tutarlı olan *kuin-apiserver* sürümünüze göre daha eski veya daha yeni bir sürümünü kullanabilirsiniz.

Örneğin, *ku1,18* to *-apiserver* *1,17*ise, *1.16* `kubectl` Bu *kuin-apiserver*ile 1,16 arası sürümlerini kullanabilirsiniz.

Sürümünüzü yüklemek veya güncelleştirmek için `kubectl` ' i çalıştırın `az aks install-cli` .

## <a name="release-and-deprecation-process"></a>Serbest bırakma ve kullanımdan kaldırma işlemi

[Aks Kubernetes sürüm takviminde](#aks-kubernetes-release-calendar)yaklaşan sürüm yayınlarına ve kullanım dışı bırakılmaları için başvuru yapabilirsiniz.

Kubernetes 'in yeni **İkincil** sürümleri için
1. AKS, yeni sürüm sürümünün planlı tarihi ile bir ön duyuru yayımlar ve ilgili eski sürüm, kaldırma işleminden önce en az 30 gün önce, [aks sürüm notlarını](https://aka.ms/aks/releasenotes) kullanımdan kaldırır.
2. AKS, AKS ve Portal erişimi olan tüm kullanıcılar için kullanılabilir bir [hizmet durumu bildirimi](../service-health/service-health-overview.md) yayımlar ve abonelik yöneticilerine planlanan sürüm kaldırma tarihleriyle bir e-posta gönderir.
3. Kullanıcıların, destek almaya devam etmek için desteklenen bir ikincil sürüm sürümüne yükseltilmesi için sürüm kaldırma işleminden **30 gün** daha vardır.

Kubernetes 'in yeni **Düzeltme Eki** sürümleri için
  * Düzeltme Eki sürümlerinin acil doğası nedeniyle, bu hizmetler kullanılabilir hale geldiğinde hizmete tanıtıabilir.
  * Genel olarak, AKS yeni yama sürümlerinin yayını için geniş bir iletişim yapmaz. Ancak, AKS 'ler sürekli olarak bunları düzenli bir şekilde desteklemek için kullanılabilir CVE düzeltme eklerini izler ve doğrular. Kritik bir düzeltme eki bulunursa veya Kullanıcı eylemi gerekliyse, AKS kullanıcıları yeni kullanılabilir düzeltme ekine yükseltmek üzere bilgilendirilir.
  * Kullanıcıların desteklenen bir düzeltme ekine yükseltmek ve destek almaya devam etmek için, bir yama sürümünün AKS 'den kaldırıldığı zamandan **30 gün** daha vardır.

### <a name="supported-versions-policy-exceptions"></a>Desteklenen sürümler ilkesi özel durumları

AKS, hata veya güvenlik sorunlarını önceden etkilemeden bir veya daha fazla kritik üretime sahip olmak için tanımlanmış yeni/mevcut sürümlerini ekleme veya kaldırma hakkını saklı tutar.

Hatanın veya güvenlik sorununun önem derecesine bağlı olarak belirli düzeltme eki sürümleri atlanabilir veya dağıtım hızlandırılır.

## <a name="azure-portal-and-cli-versions"></a>Azure portal ve CLı sürümleri

Portal 'da veya Azure CLı ile bir AKS kümesi dağıttığınızda, küme, N-1 alt sürümüne ve en son düzeltme ekine varsayılan olarak ayarlanır. Örneğin, AKS, *1.17. a*, *1.17. b*, *1.16. c*, *1.16. d*, *1.15. e*ve *1.15. f*' i destekliyorsa, seçilen varsayılan sürüm *1.16. c*' dir.

Aboneliğiniz ve bölgeniz için şu anda hangi sürümlerin kullanılabildiğini öğrenmek için [az aks get-versions][az-aks-get-versions] komutunu kullanın. Aşağıdaki örnek *EastUS* bölgesi Için kullanılabilir Kubernetes sürümlerini listeler:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

## <a name="aks-kubernetes-release-calendar"></a>AKS Kubernetes Yayın takvimi

Son sürüm geçmişi için [buraya](https://en.wikipedia.org/wiki/Kubernetes#History)bakın.

|  K8s sürümü | Yukarı akış yayını  | AKS önizlemesi  | AKS GA  | Yaşam sonu |
|--------------|-------------------|--------------|---------|-------------|
| 1,17  | Ara-09-19  | 19 Ocak   | 20 Temmuz  | 1,20 GA | 
| 1,18  | Mar-23-20  | 20 Mayıs   | 20 Ağu  | 1,21 GA | 
| 1,19  | Ağu-04-20  | 20 Ağu   | 20 Kas  | 1,22 GA | 
| 1,20  | * Kas 20    | * Ara 20   | * 21 Ocak  | 1,23 GA | 

\*Bekleyen yukarı akış Yayın tarihi onayı.

## <a name="faq"></a>SSS

**Bir Kullanıcı bir Kubernetes kümesini desteklenmeyen küçük bir sürümle yükseltdiğinde ne olur?**

*N-3* veya daha eski bir sürüm kullanıyorsanız, destek dışında olduğunuz ve yükseltmeniz istenecek demektir. N-3 sürümünden n-2 ' ye yükseltme başarılı olursa, destek ilkeleriniz dahilinde geri dönebilirsiniz. Örnek:

- Desteklenen en eski AKS sürümü *1.15. a* ise ve *1.14. b* veya daha eski bir sürümdaysanız, destek dışında olursunuz.
- *1.14. b* 'den 1.15 'e yükseltme yapıldığında, ya da daha yüksek *bir* başarılı olduğunda, destek ilkilerimize geri dönebilirsiniz.

Eski sürüme düşürme işlemleri desteklenmez.

**' Destek dışında ' ne anlama geliyor?**

' Destek dışında ', çalıştırdığınız sürümün desteklenen sürümler listesinin dışında olduğu anlamına gelir ve sürüm kullanımdan kaldırıldıktan sonra 30 günlük yetkisiz kullanım süresi içinde olmadığınız takdirde, desteği talep edildiğinde kümeyi desteklenen bir sürüme yükseltmeniz istenir. Ayrıca, AKS, desteklenen sürümler listesinin dışındaki kümeler için herhangi bir çalışma zamanı veya başka garanti yapmaz.

**Bir Kullanıcı bir Kubernetes kümesini desteklenmeyen küçük bir sürümle ölçeklendirdiğinde ne olur?**

AKS tarafından desteklenmeyen alt sürümler için, ölçek genişletme veya küçültme özelliği çalışmaya devam etmelidir, ancak hizmet garantisi kalitesi yoktur; bu nedenle, kümenizi yeniden desteğe getirmek için yükseltme yapmanız önemle önerilir.

**Bir Kullanıcı bir Kubernetes sürümünde sonsuza kadar kalabilir mi?**

Bir kümenin üçten fazla (3) alt sürümü desteği tükenmiştir ve güvenlik riskleri taşıyan bir güvenlik riski bulunursa, Azure, kümenizi proaktif olarak yükseltmeniz için size iletişim kurar. Daha fazla işlem yapmazsanız, Azure, kümenizi sizin adınıza otomatik olarak yükseltme hakkını saklı tutar.

**Düğüm havuzu desteklenen AKS sürümlerinden birinde değilse, denetim düzlemi hangi sürümü destekler?**

Denetim düzlemi tüm düğüm havuzlarındaki sürümlerin bir penceresi içinde olmalıdır. Denetim düzlemi veya düğüm havuzlarını yükseltme hakkında daha fazla bilgi için, [düğüm havuzlarını yükseltme](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools)hakkındaki belgeleri ziyaret edin.

**Küme yükseltmesi sırasında birden çok AKS sürümünü atlayabilir miyim?**

Desteklenen bir AKS kümesini yükselttiğinizde, Kubernetes ikincil sürümleri atlanamaz. Örneğin, *1.12. x*  ->  *1.13. x* veya *1.13. x*  ->  *1.14. x* arasındaki yükseltmelere izin verilir, ancak *1.12. x*  ->  *1.14. x* değildir.

Yükseltmek için, *1.12. x*  ->  *1.14. x*sürümünden önce *1.12. x*  ->  *1.13. x*sürümünden yükseltme yapın ve ardından *1.13. x*  ->  *1.14. x*'den yükseltme yapın.

Birden çok sürüm atlanması yalnızca desteklenmeyen bir sürümden desteklenen bir sürüme yükseltilirken yapılabilir. Örneğin, desteklenmeyen bir *1,10. x* sürümünden yükseltme > desteklenen bir *1.15. x* tamamlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kümenizi yükseltme hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) kümesini yükseltme][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
