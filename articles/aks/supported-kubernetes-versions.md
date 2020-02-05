---
title: Azure Kubernetes hizmetinde desteklenen Kubernetes sürümleri
description: Azure Kubernetes Service 'teki (AKS) Kubernetes sürüm destek ilkesini ve kümelerin yaşam döngüsünü anlayın
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: article
ms.date: 12/09/2019
ms.author: saudas
ms.openlocfilehash: 8c144fb0c9818e21c2ca5bd61525067b485531bb
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026124"
---
# <a name="supported-kubernetes-versions-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde desteklenen Kubernetes sürümleri

Kubernetes topluluğu, küçük sürümleri yaklaşık üç ayda bir yayınlamaktadır. Bu yayınlar yeni özellikler ve geliştirmeler içerir. Düzeltme eki yayınları daha sıktır (bazen haftalık) ve yalnızca bir küçük sürümdeki kritik hata düzeltmelerine yöneliktir. Bu düzeltme eki sürümleri, Kubernetes temelinde üretimde çalışan çok sayıda müşteriyi ve ürünü etkileyen güvenlik açıklarına veya önemli hatalara yönelik düzeltmeler içerir.

Bir yukarı akış sürümünün 30 gün içinde yeni Kubernetes sürümlerini onaylamak ve serbest bırakmak, yayının kararlılığını ele almak için AKS aks.

## <a name="kubernetes-versions"></a>Kubernetes sürümleri

Kubernetes, standart [anlamsal sürüm](https://semver.org/) oluşturma düzenini kullanır. Bu, her bir Kubernetes sürümünün bu numaralandırma düzenini izlediği anlamına gelir:

```
[major].[minor].[patch]

Example:
  1.12.14
  1.12.15
```

Sürümdeki her bir sayı, önceki sürümle genel uyumluluğu gösterir:

* Büyük sürümler uyumsuz API değişiklikleri veya geriye dönük uyumluluk bozuk olduğunda değişir.
* Küçük sürümler, diğer küçük yayınlar ile geriye doğru uyumlu olan işlevsellik değişiklikleri yapıldığında değişir.
* Geriye dönük olarak uyumlu hata düzeltmeleri yapıldığında düzeltme eki sürümleri değişir.

Kullanıcılar, çalıştırıldıkları ikincil sürümün en son düzeltme eki sürümünü çalıştırmalıdır. Örneğin, üretim kümeniz *1.12.14* üzerinde ve *1.12.15* , *1,12* serisi için kullanılabilen en son düzeltme eki sürümündedir, kümenizin tamamen düzeltme eki uygulanmış ve desteklenmiş olduğundan emin olmak için *1.12.15* sürümüne yükseltmeniz gerekir.

## <a name="kubernetes-version-support-policy"></a>Kubernetes sürüm desteği ilkesi

AKS, Kubernetes 'in üç küçük sürümünü destekler:

* AKS içinde Yayınlanan geçerli ikincil sürüm (N)
* Önceki iki alt sürüm. Desteklenen her küçük sürüm, ayrıca iki kararlı düzeltme eki de destekler.

Bu "N-2" olarak bilinir: (N (en son sürüm)-2 (ikincil sürümler)).

Örneğin, AKS *1.15.* bugün, aşağıdaki sürümler için destek sağlanır:

Yeni ikincil sürüm    |    Desteklenen sürüm listesi
-----------------    |    ----------------------
1.15. a               |    1.15. a, 1.15. b, 1.14. c, 1.14. d, 1.13. e, 1.13. f

Burada ". letter", düzeltme eki sürümlerinin temsilcisidir.

Sürüm değişiklikleri ve beklentileri hakkındaki iletişimlerle ilgili ayrıntılar için aşağıdaki "Iletişimler" bölümüne bakın.

Yeni bir alt sürüm ortaya çıkarıldığında, desteklenen en eski ikincil sürüm ve düzeltme eki sürümleri kullanım dışıdır ve kaldırılır. Örneğin, geçerli desteklenen sürüm listesi şu ise:

```
1.15.a
1.15.b
1.14.c
1.14.d
1.13.e
1.13.f
```

Ve AKS yayınları 1,16. *Bu, 1,13 anlamına gelir.* sürümler (tüm 1,13 sürümler) kaldırılır ve destek dışı bırakılır.

> [!NOTE]
> Müşteriler desteklenmeyen bir Kubernetes sürümü çalıştırıyorsa, bu, küme için destek istendiğinde yükseltmeniz istenir. Desteklenmeyen Kubernetes yayınları çalıştıran kümeler [aks destek ilkeleri](https://docs.microsoft.com/azure/aks/support-policies)kapsamında değildir.

AKS, yukarıdaki küçük sürümlere ek olarak, belirli bir alt sürümün en son **Düzeltme Eki** sürümlerini destekler. Örneğin, aşağıdaki desteklenen sürümler verildiğinde:

```
Current Supported Version List
------------------------------
1.15.2, 1.15.1, 1.14.5, 1.14.4
```

Yukarı akış Kubernetes 1.15.3 ve 1.14.6 ve AKS Bu düzeltme eki sürümlerini yayımlarsa, en eski düzeltme eki sürümleri kullanım dışıdır ve kaldırılır ve desteklenen sürüm listesi şu şekilde olur:

```
New Supported Version List
----------------------
1.15.*3*, 1.15.*2*, 1.14.*6*, 1.14.*5*
```

### <a name="communications"></a>Haberleşme

* Kubernetes 'in yeni **İkincil** sürümleri için
  * Tüm kullanıcılar, yeni sürümden ve hangi sürümün kaldırılabileceği herkese açık bir şekilde bildirilir.
  * Yeni bir yama sürümü yayınlandığında, en eski düzeltme eki sürümü aynı anda kaldırılır.
  * Müşterilerin desteklenen bir alt sürüm sürümüne yükseltilmesi için genel bildirim tarihinden itibaren **30 gün** daha vardır.
* Kubernetes 'in yeni **Düzeltme Eki** sürümleri için
  * Tüm kullanıcılara, yayımlanmakta olan yeni yama sürümü bildirilir ve en son düzeltme eki sürümüne yükseltilir.
  * Kullanıcıların daha yeni, desteklenen bir yama sürümüne yükseltilmesi **30 gün** daha vardır. Kullanıcıların, en eski kaldırılmadan önce desteklenen bir yama sürümüne yükseltilmesi **30 gün** daha vardır.

AKS, tüm SLO/Quality ve tüm bölgelerde kullanılabilen genel kullanıma açık olan sürümler olarak "yayınlanan sürümü" tanımlar. AKS, açıkça etiketlenmiş ve önizleme hüküm ve koşullarına tabi olan önizleme sürümlerini de destekleyebilir.

#### <a name="notification-channels-for-aks-changes"></a>AKS değişiklikleri için bildirim kanalları

AKS, [GitHub](https://github.com/Azure/AKS/releases)'da hizmette yayınlanan yeni Kubernetes sürümlerini, hizmet değişikliklerini ve bileşen güncelleştirmelerini özetleyen normal hizmet güncelleştirmelerini yayımlar.

Bu değişiklikler, yönetilen hizmetin bir parçası olarak sunulan düzenli bakımın bir parçası olarak tüm müşterilere alınır, bazıları herhangi bir işlem gerektirirken açık yükseltmeler gerektirir.

Bildirimler ile de gönderilir:

* [AKS sürüm notları](https://aka.ms/aks/releasenotes)
* Azure portalı bildirimleri
* [Azure güncelleştirme kanalı][azure-update-channel]

### <a name="supported-versions-policy-exceptions"></a>Desteklenen sürümler Ilkesi özel durumları

AKS, hata veya güvenlik sorunlarını önceden etkilemeden bir veya daha fazla kritik üretime sahip olmak için tanımlanmış yeni/mevcut sürümlerini ekleme veya kaldırma hakkını saklı tutar.

Hatanın veya güvenlik sorununun önem derecesine bağlı olarak belirli düzeltme eki sürümleri atlanabilir veya dağıtım hızlandırılır.

### <a name="azure-portal-and-cli-default-versions"></a>Azure portal ve CLı varsayılan sürümleri

Portal 'da veya Azure CLı ile bir AKS kümesi dağıttığınızda, küme, N-1 alt sürümüne ve en son düzeltme ekine varsayılan olarak ayarlanır. Örneğin, AKS, *1.15. a*, *1.15. b*, *1.14. c*, *1.14. d*, *1.13. e*ve *1.13. f*' i destekliyorsa, seçilen varsayılan sürüm *1.14. c*' dir.

AKS, müşterilere varsayılan olarak bilinen, kararlı ve düzeltme eki uygulanmış bir sürüm sağlamak için varsayılan N-1 ' i seçer.

## <a name="list-currently-supported-versions"></a>Şu anda desteklenen sürümleri Listele

Aboneliğiniz ve bölgeniz için şu anda hangi sürümlerin kullanılabildiğini öğrenmek için [az aks get-versions][az-aks-get-versions] komutunu kullanın. Aşağıdaki örnek *EastUS* bölgesi Için kullanılabilir Kubernetes sürümlerini listeler:

```azurecli-interactive
az aks get-versions --location eastus --output table
```

## <a name="faq"></a>SSS

**Bir müşteri bir Kubernetes kümesini desteklenmeyen küçük bir sürümle yükseltdiğinde ne olur?**

*N-3* sürümdaysanız, destek dışında olursunuz ve yükseltmeniz istenir. N-3 sürümünden n-2 ' ye yükseltme başarılı olursa, şimdi destek ilkeleriniz dahilinde olursunuz. Örneğin:

- Desteklenen en eski AKS sürümü *1.13. a* ve *1.12. b* veya daha eski bir sürümse, destek dışında olursunuz.
- *1.12. b* 'den *1.13* 'e yükseltme işlemi başarılı olursa, destek ilkilerimizin içinden geri dönebilirsiniz.

*N-2* ' nin desteklenen penceresinden daha eski sürümlere yükseltmeler desteklenmez. Bu gibi durumlarda, müşterilerin desteklenen penceredeki sürümlerle yeni AKS kümeleri oluşturmalarına ve iş yüklerini yeniden dağıtmanıza önerilir.

**' Destek dışı ' anlamına gelir**

' Destek dışında ', çalıştırdığınız sürümün desteklenen sürümler listesinin dışında olduğu anlamına gelir ve destek istemek için kümeyi desteklenen bir sürüme yükseltmeniz istenir. Ayrıca, AKS, desteklenen sürümler listesinin dışındaki kümeler için herhangi bir çalışma zamanı veya başka bir garanti yapmaz.

**Bir müşteri bir Kubernetes kümesini desteklenmeyen küçük bir sürümle ölçeklendiğinde ne olur?**

AKS tarafından desteklenmeyen alt sürümler için, ölçeklendirme ve küçültme özelliği çalışmaya devam etmelidir, ancak kümenizi yeniden desteğe getirmek için yükseltmesi önemle önerilir.

**Bir müşteri, bir Kubernetes sürümünde sonsuza kadar kalabilir mi?**

Evet. Ancak, küme AKS tarafından desteklenen sürümlerden birinde değilse, küme AKS destek ilkelerinden çıkar. Azure, kümenizi otomatik olarak yükseltmez veya silmez.

**Düğüm havuzu desteklenen AKS sürümlerinden birinde değilse, denetim düzlemi hangi sürümü destekler?**

Denetim düzlemi tüm düğüm havuzlarındaki sürümlerin bir penceresi içinde olmalıdır. Denetim düzlemi veya düğüm havuzlarını yükseltme hakkında daha fazla bilgi için, [düğüm havuzlarını yükseltme](use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools)hakkındaki belgeleri ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Kümenizi yükseltme hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) kümesini yükseltme][aks-upgrade].

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[azure-update-channel]: https://azure.microsoft.com/updates/?product=kubernetes-service

<!-- LINKS - Internal -->
[aks-upgrade]: upgrade-cluster.md
[az-aks-get-versions]: /cli/azure/aks#az-aks-get-versions
