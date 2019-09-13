---
title: Yaygın Azure Kubernetes hizmet sorunlarını giderme
description: Azure Kubernetes Service (AKS) kullanırken karşılaşılan yaygın sorunları giderme ve çözme hakkında bilgi edinin
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 6ff273236f9f8465de9ec0cda89ed3ff8996ecec
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70932655"
---
# <a name="aks-troubleshooting"></a>AKS sorunlarını giderme

Azure Kubernetes hizmeti (AKS) kümeleri oluştururken veya yönetirken, zaman zaman sorunlarla karşılaşabilirsiniz. Bu makalede bazı yaygın sorunlar ve sorun giderme adımları ayrıntılı olarak anlatılmaktadır.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Genel olarak, Kubernetes sorunlarını ayıklama hakkındaki bilgileri nerede bulabilirim?

[Kubernetes kümelerinde sorun gidermeye yönelik resmi kılavuzunu](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/)deneyin.
Ayrıca, pods, düğümler, kümeler ve diğer özelliklerle ilgili sorunları gidermeye yönelik bir Microsoft mühendis tarafından yayımlanan bir [sorun giderme kılavuzu](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md)vardır.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Oluşturma veya yükseltme sırasında "Kota aşıldı" hatası alıyorum. Ne yapmalıyım? 

[Çekirdek istemeniz](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request)gerekir.

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>AKS için düğüm başına en fazla düğüm sayısı ayarı nedir?

Azure portal bir AKS kümesi dağıtırsanız, düğüm başına en fazla düğüm sayısı ayarı varsayılan olarak 30 ' dur.
Azure CLı 'de bir AKS kümesi dağıtırsanız, düğüm başına en fazla düğüm ayarı varsayılan olarak 110 ' dir. (Azure CLı 'nın en son sürümünü kullandığınızdan emin olun). Bu varsayılan ayar, `–-max-pods` `az aks create` komutunda bayrağı kullanılarak değiştirilebilir.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Gelişmiş ağlarla AKS kümesi dağıtma sırasında insufficientSubnetSize hatası alıyorum. Ne yapmalıyım?

Azure CNı (Gelişmiş ağ) kullanılıyorsa, AKS, yapılandırılmış düğüm başına "en büyük pods" temelinde IP 'yi önceden ayırır. Bir AKS kümesindeki düğümlerin sayısı 1 ile 110 arasında bir yerde olabilir. Düğüm başına yapılandırılmış en fazla düğüm sayısına bağlı olarak, alt ağ boyutu "düğüm sayısının ürünü ve düğüm başına en fazla Pod" değerinden büyük olmalıdır. Aşağıdaki temel denklem şunları özetler:

Alt ağ boyutu > kümedeki düğümlerin sayısı (gelecekteki ölçeklendirme gereksinimlerini dikkate alarak) * düğüm başına en fazla Pod.

Daha fazla bilgi için bkz. [kümeniz IÇIN IP adresleme planlaması](configure-azure-cni.md#plan-ip-addressing-for-your-cluster).

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Pod My CrashLoopBackOff modunda takılmış. Ne yapmalıyım?

Pod 'un bu modda takılmasının çeşitli nedenleri olabilir. Şöyle görünebilir:

* Kullanarak `kubectl describe pod <pod-name>`Pod kendisini.
* Kullanılarak `kubectl log <pod-name>`Günlükler.

Pod sorunlarını giderme hakkında daha fazla bilgi için bkz. [uygulamalarda hata ayıklama](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Mevcut bir kümede RBAC 'yi etkinleştirmeye çalışıyorum. Bunu nasıl yapabilirim?

Ne yazık ki, mevcut kümelerde rol tabanlı erişim denetimi 'ni (RBAC) etkinleştirmek Şu anda desteklenmiyor. Açıkça yeni kümeler oluşturmanız gerekir. CLı kullanıyorsanız, RBAC varsayılan olarak etkindir. AKS portalını kullanıyorsanız, oluşturma iş akışında RBAC 'yi etkinleştirmek için iki durumlu bir düğme bulunur.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Azure CLı 'yi varsayılan olarak veya Azure portal kullanarak ve şimdi Kubernetes panosunda çok sayıda uyarı görmem için RBAC ile etkinleştirilen bir küme oluşturdum. Herhangi bir uyarı olmadan çalışmak için kullanılan Pano. Ne yapmalıyım?

Panodaki uyarıların nedeni, kümenin RBAC ile etkin hale gelir ve erişim varsayılan olarak devre dışı bırakılmıştır. Genel olarak bu yaklaşım iyi bir uygulamadır çünkü panonun tüm kullanıcıları için varsayılan olarak pozlaması güvenlik tehditlerine neden olabilir. Panoyu hala etkinleştirmek istiyorsanız, [Bu blog gönderisine](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/)ilişkin adımları izleyin.

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Panoya bağlanamıyorum. Ne yapmalıyım?

Küme dışında hizmetinize erişmenin en kolay yolu `kubectl proxy`, ana bilgisayar bağlantı noktası 8001 ile Kubernetes API sunucusuna gönderilen isteklerin proxy 'sidir. Buradan, API sunucusu hizmetinize proxy gönderebilir: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Kubernetes panosunu görmüyorsanız, `kube-proxy` Pod 'un `kube-system` ad alanında çalışıp çalışmadığını denetleyin. Çalışır durumda değilse, Pod 'yi silin ve yeniden başlatılır.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Kubectl günlüklerini kullanarak günlükleri alamıyor veya API sunucusuna bağlanamıyorum. "Sunucudan hata: arka uç ararken hata: TCP ara..." hatasını alıyorum. Ne yapmalıyım?

Varsayılan ağ güvenlik grubunun değiştirilmediğinden ve API sunucusuyla bağlantı için 22 ve 9000 bağlantı noktasının açık olduğundan emin olun. Pod 'ın, `kubectl get pods --namespace kube-system` komutunu kullanarak *kuin-System* ad alanında çalışıp çalışmadığını denetleyin. `tunnelfront` Değilse, Pod 'ın silinmesini zorla ve yeniden başlatılır.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Yükseltmeye veya ölçeklendirmeye çalışıyorum ve bir "ileti alıyorum: ' ImageReference ' özelliğinin değiştirilmesine izin verilmiyor "hatası. Nasıl yaparım? bu sorun düzeltilsin mi?

AKS kümesi içindeki aracı düğümlerinde bulunan etiketleri değiştirdiğiniz için bu hatayı alabilirsiniz. MC_ * kaynak grubundaki kaynakların ve diğer özelliklerinin değiştirilmesi ve silinmesi, beklenmeyen sonuçlara neden olabilir. AKS kümesindeki MC_ * grubu altındaki kaynakları değiştirmek, hizmet düzeyi hedefini (SLO) keser.

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Kümemin başarısız durumunda olduğunu ve yükseltme ya da ölçeklendirmeyi düzeltilinceye kadar çalışmayacak

*Bu sorun giderme yardımı şuradan yönlendirilir https://aka.ms/aks-cluster-failed*

Bu hata, kümeler birden çok nedenden dolayı başarısız bir durum girerken oluşur. Daha önce başarısız olan işlemi yeniden denemeden önce kümenizin başarısız durumunu çözümlemek için aşağıdaki adımları izleyin:

1. Kümenin `failed` durumuolmadığından`scale`veişlemlerbaşarılıolmayacaktır. `upgrade` Ortak kök sorunları ve çözümleri şunları içerir:
    * **Yetersiz işlem (CRP) kotasıyla**ölçekleme. Çözümlemek için, önce kümenizi kotanın içindeki kararlı bir hedef durumuna ölçeklendirin. Ardından, ilk kota limitlerinin ötesinde daha fazla ölçeklendirmeyi denemeden önce [bir işlem kotası artışı istemek için bu adımları](../azure-supportability/resource-manager-core-quotas-request.md) izleyin.
    * Gelişmiş ağ ve **yetersiz alt ağ (ağ) kaynaklarıyla**bir kümeyi ölçeklendirin. Çözümlemek için, önce kümenizi kotanın içindeki kararlı bir hedef durumuna ölçeklendirin. Ardından ilk kota limitlerinin ötesinde daha fazla ölçeklendirmeyi denemeden önce [bir kaynak kotası artışı istemek için bu adımları](../azure-resource-manager/resource-manager-quota-errors.md#solution) izleyin.
2. Yükseltme hatasının temeldeki nedeni çözümlendikten sonra, kümenizin başarılı bir durumda olması gerekir. Başarılı bir durum doğrulandıktan sonra, özgün işlemi yeniden deneyin.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Bu durumun yükseltilme veya ölçeklendirilmesi sırasında hata alıyorum, bu durum kümemdeki Şu anda yükseltilmekte veya yükseltme başarısız oldu

*Bu sorun giderme yardımı şuradan yönlendirilir https://aka.ms/aks-pending-upgrade*

Tek düğümlü havuz veya [birden çok düğüm](use-multiple-node-pools.md) havuzu içeren bir küme ile bir kümede yükseltme ve ölçeklendirme işlemleri birbirini dışlıyor. Aynı anda yükseltme ve ölçeklendirme için bir küme veya düğüm havuzunuz olamaz. Bunun yerine, her işlem türünün aynı kaynaktaki bir sonraki istekten önce hedef kaynakta tamamlaması gerekir. Sonuç olarak, etkin yükseltme veya ölçeklendirme işlemleri gerçekleşirken veya denendiğinde ve daha sonra başarısız olduğunda işlemler sınırlıdır. 

Kümenizde ayrıntılı durum almak `az aks show -g myResourceGroup -n myAKSCluster -o table` için sorunu tanılamaya yardımcı olmak için. Sonuca göre:

* Küme etkin bir şekilde yükseltildiğinde, işlem sonlanana kadar bekleyin. Başarılı olduysa, daha önce başarısız olan işlemi yeniden deneyin.
* Kümede yükseltme başarısız olursa, önceki bölümde açıklanan adımları izleyin.

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>Kümemi farklı bir aboneliğe veya kümeme yeni bir kiracıya sahip aboneliğime taşıyabilir miyim?

AKS kümenizi farklı bir aboneliğe veya kümeye sahip olan aboneliğe yeni bir kiracıya taşıdıysanız, rol atamalarının ve hizmet sorumlusu haklarının kaybolması nedeniyle küme işlevselliği kaybeder. Aks 'ler, bu kısıtlama nedeniyle **kümelerin abonelikler veya kiracılar arasında taşınmasını desteklemez** .

## <a name="im-receiving-errors-trying-to-use-features-that-require-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri gerektiren özellikleri kullanmaya çalışırken hata alıyorum

*Bu sorun giderme yardımı aka.ms/aks-vmss-enablement 'ten yönlendirilir*

AKS kümenizin, aşağıdaki örnekte olduğu gibi bir sanal makine ölçek kümesi üzerinde olmadığını belirten hatalar alabilirsiniz:

**AgentPool ' agentpool ' otomatik ölçeklendirmeyi etkin olarak ayarladı, ancak sanal makine ölçek kümelerinde değil**

Küme otomatik veya birden çok düğüm havuzu gibi özellikleri kullanmak için, sanal makine ölçek kümelerini kullanan AKS kümelerinin oluşturulması gerekir. Sanal makine ölçek kümelerine bağımlı olan özellikleri kullanmayı denerseniz ve normal, sanal olmayan bir makine ölçek kümesi AKS kümesini hedeflerseniz hatalar döndürülür. Sanal makine ölçek kümesi desteği şu anda AKS 'de önizlemededir.

Sanal makine ölçek kümesi özelliği önizlemesine doğru kaydolmak ve bir AKS kümesi oluşturmak için uygun belge içindeki *başlamadan önce* ' i izleyin:

* [Küme otomatik Scaler 'ı kullanma](cluster-autoscaler.md)
* [Birden çok düğüm havuzu oluşturma ve kullanma](use-multiple-node-pools.md)
 
## <a name="what-naming-restrictions-are-enforced-for-aks-resources-and-parameters"></a>AKS kaynakları ve parametreleri için hangi adlandırma kısıtlamaları zorlanır?

*Bu sorun giderme yardımı aka.ms/aks-naming-rules 'ten yönlendirilir*

Adlandırma kısıtlamaları hem Azure platformu hem de AKS tarafından uygulanır. Bir kaynak adı veya parametresi bu kısıtlamaların birini keserse, farklı bir giriş sağlamanızı isteyen bir hata döndürülür. Aşağıdaki ortak adlandırma yönergeleri geçerlidir:

* AKS *Mc_* kaynak grubu adı, kaynak grubu adını ve kaynak adını birleştirir. Otomatik olarak `MC_resourceGroupName_resourceName_AzureRegion` oluşturulan sözdiziminin 80 karakterden büyük olmaması gerekir. Gerekirse, kaynak grubu adınızın veya AKS kümesi adınızın uzunluğunu azaltın.
* *Dnspredüzeltmesinin* alfasayısal değerlerle başlaması ve bitmesi gerekir. Geçerli karakterler alfasayısal değerleri ve kısa çizgileri (-) içerir. *Dnspredüzeltmesini* nokta (.) gibi özel karakterler içeremez.

## <a name="im-receiving-errors-when-trying-to-create-update-scale-delete-or-upgrade-cluster-that-operation-is-not-allowed-as-another-operation-is-in-progress"></a>Küme oluşturmaya, güncelleştirmeye, ölçeklendirmeye, silmeye veya yükseltmeye çalışırken hata alıyorum, devam eden başka bir işlem olduğundan bu işleme izin verilmiyor.

*Bu sorun giderme yardımı aka.ms/aks-pending-operation 'ten yönlendirilir*

Önceki bir işlem devam ederken küme işlemleri sınırlıdır. Kümenizin ayrıntılı durumunu almak için `az aks show -g myResourceGroup -n myAKSCluster -o table` komutunu kullanın. Gerektiğinde kendi kaynak grubunuzu ve AKS küme adınızı kullanın.

Küme durumunun çıkışına göre:

* Küme, *başarılı* veya *başarısız*dışında bir sağlama durumundaysa, Işlem (*yükseltme/güncelleştirme/oluşturma/ölçeklendirme/silme/geçirme*) bitene kadar bekleyin. Önceki işlem tamamlandığında, en son küme işleminizi yeniden deneyin.

* Kümenin başarısız bir yükseltmesi varsa, ana belirtilen adımları izleyerek [Kümemin başarısız durumunda olduğunu ve yükseltme ya da ölçeklendirmeyi düzeltilene kadar çalışmayacaktır](#im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed).

## <a name="im-receiving-errors-that-my-service-principal-was-not-found-when-i-try-to-create-a-new-cluster-without-passing-in-an-existing-one"></a>Mevcut bir kümeyi geçirmeden yeni bir küme oluşturmaya çalıştığımda hizmet sorumlumun bulunamadığını belirten hatalar alıyorum.

Bir AKS kümesi oluştururken sizin adınıza kaynak oluşturmak için hizmet sorumlusu gerekir. AKS, küme oluşturma sırasında yeni bir tane oluşturma özelliği sunar, ancak bu, kümenin oluşturma işleminin başarılı olması için yeni hizmet sorumlusunu uygun bir süre içinde tam olarak yaymak için Azure Active Directory gerektirir. Bu yayma çok uzun sürerse, küme, kullanılabilir bir hizmet sorumlusu bulamadığından, oluşturulması için doğrulama başarısız olur. 

Bunun için aşağıdaki geçici çözümleri kullanın:
1. Bölgelere zaten yayılan ve küme oluşturma zamanında AKS 'e geçirilecek mevcut bir hizmet sorumlusunu kullanın.
2. Otomasyon betikleri kullanıyorsanız, hizmet sorumlusu oluşturma ve AKS kümesi oluşturma arasında zaman gecikmeleri ekleyin.
3. Azure portal kullanıyorsanız, oluşturma sırasında küme ayarlarına dönün ve birkaç dakika sonra doğrulama sayfasını yeniden deneyin.

## <a name="im-receiving-errors-after-restricting-my-egress-traffic"></a>Çıkış trafiğinizi kısıtladıktan sonra hata alıyorum

AKS kümesinden çıkış trafiği kısıtlandığında, [gerekli ve isteğe bağlı olarak önerilen](limit-egress-traffic.md) giden bağlantı noktaları/ağ kuralları ve aks için FQDN/uygulama kuralları vardır. Ayarlarınız bu kuralların herhangi biriyle çakışıyorsa, belirli `kubectl` komutları çalıştıramayabilir. AKS kümesi oluştururken de hata görebilirsiniz.

Ayarlarınızın gerekli veya isteğe bağlı önerilen giden bağlantı noktaları/ağ kuralları ve FQDN/uygulama kuralları ile çakışmadığından emin olun.
