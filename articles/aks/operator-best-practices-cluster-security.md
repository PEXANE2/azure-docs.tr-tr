---
title: Operatör en iyi uygulamaları - Azure Kubernetes Hizmetlerinde küme güvenliği (AKS)
description: Azure Kubernetes Hizmeti'nde (AKS) küme güvenliği ve yükseltmeleri yönetme ye yönelik küme operatörünün en iyi uygulamalarını öğrenin
services: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: f02f6588946e2b63a1a092aba15603d1685e8207
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594813"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) küme güvenliği ve yükseltmeleri için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeleri yönetirken, iş yüklerinizin ve verilerinizin güvenliği önemli bir husustur. Özellikle mantıksal yalıtım kullanarak çok kiracılı kümeler çalıştırdığınızda, kaynaklara ve iş yüklerine erişimi güvence altına almanız gerekir. Saldırı riskini en aza indirmek için, en son Kubernetes ve düğüm işletim sistemi güvenlik güncelleştirmelerini uyguladığınıza da emin olmanız gerekir.

Bu makalede, AKS kümenizi nasıl güvene alabildiğiniz üzerinde duruluyor. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * API sunucusu erişimini sağlamak için Azure Etkin Dizini ve rol tabanlı erişim denetimlerini kullanma
> * Düğüm kaynaklarına güvenli kapsayıcı erişimi
> * Bir AKS kümesini en son Kubernetes sürümüne yükseltin
> * Düğümleri güncel tutun ve güvenlik yamaları otomatik olarak uygulayın

Ayrıca [konteyner görüntü yönetimi][best-practices-container-image-management] ve pod [güvenliği][best-practices-pod-security]için en iyi uygulamaları okuyabilirsiniz.

Tehditleri algılamaya ve AKS kümelerinizi güvence altına almak için önerileri görüntülemeye yardımcı olmak için [Azure Kubernetes Hizmetleri tümleştirmesini Güvenlik Merkezi ile][security-center-aks] de kullanabilirsiniz.

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>API sunucusuna ve küme düğümlerine güvenli erişim

**En iyi uygulama kılavuzu** - Kubernetes API-Server'a erişimi güvence altına almak, kümenizi güvence altına almak için yapabileceğiniz en önemli şeylerden biridir. API sunucusuna erişimi denetlemek için Kubernetes rol tabanlı erişim denetimini (RBAC) Azure Active Directory ile tümleştirin. Bu denetimler, AKS'yi Azure aboneliklerinize erişimi güvence altına aldığınız gibi güvenli hale almanızı sağlar.

Kubernetes API sunucusu, bir küme içinde eylem gerçekleştirmek için istekleri n için tek bir bağlantı noktası sağlar. API sunucusuna erişimi güvence altına almak ve denetlemek için erişimi sınırlandırın ve gereken en az ayrıcalıklı erişim izinlerini sağlayın. Bu yaklaşım Kubernetes'e özgü değildir, ancak AKS kümesi çok kiracılı kullanım için mantıksal olarak yalıtıldığında özellikle önemlidir.

Azure Active Directory (AD), AKS kümeleriyle tümlebilen kurumsal kullanıma hazır bir kimlik yönetimi çözümü sağlar. Kubernetes bir kimlik yönetimi çözümü sağlamadığından, API sunucusuna erişimi kısıtlamak için ayrıntılı bir yol sağlamak zor olabilir. AKS'deki Azure AD tümleşik kümeleriyle, kullanıcıların API sunucusuna kimliğini doğrulamak için varolan kullanıcı ve grup hesaplarınızı kullanırsınız.

![AKS kümeleri için Azure Active Directory tümleştirmesi](media/operator-best-practices-cluster-security/aad-integration.png)

API sunucusunun güvenliğini sağlamak ve tek bir ad alanı gibi kapsamlı bir kaynak kümesi için gereken en az sayıda izin sağlamak için Kubernetes RBAC ve Azure AD tümleştirmesini kullanın. Azure AD'deki farklı kullanıcılara veya gruplara farklı RBAC rolleri verilebilir. Bu parçalı izinler, API sunucusuna erişimi kısıtlamanızı ve gerçekleştirilen eylemlerin net bir denetim izinini sağlamanıza izin verir.

Önerilen en iyi yöntem, dosyaları ve klasörlere ve tek tek kimliklere erişim sağlamak için grupları kullanmak, kullanıcıları tek tek *kullanıcılar*yerine RBAC rollerine bağlamak için Azure AD *grup* üyeliğini kullanmaktır. Kullanıcının grup üyeliği değiştikçe, AKS kümesindeki erişim izinleri buna göre değişir. Kullanıcıyı doğrudan bir role bağlarsanız, iş işlevi değişebilir. Azure AD grup üyelikleri güncellenir, ancak AKS kümesindeki izinler bunu yansıtmaz. Bu senaryoda, kullanıcıya bir kullanıcının gerektirdiğinden daha fazla izin verilir.

Azure AD tümleştirmesi ve RBAC hakkında daha fazla bilgi için [AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][aks-best-practices-identity]bakın.

## <a name="secure-container-access-to-resources"></a>Kaynaklara güvenli kapsayıcı erişimi

**En iyi uygulama kılavuzu** - Kapsayıcıların gerçekleştirebileceği eylemlere erişimi sınırlandırın. En az sayıda izin sağlayın ve kök / ayrıcalıklı yükseltme kullanımını önleyebilirsiniz.

Kullanıcılara veya gruplara gereken en az sayıda ayrıcalık vermeniz gerektiği gibi, kapsayıcılar da yalnızca gereksinim duydukları eylem ve işlemlerle sınırlı olmalıdır. Saldırı riskini en aza indirmek için, artırılmış ayrıcalıklar veya kök erişimi gerektiren uygulamaları ve kapsayıcıları yapılandırmayın. Örneğin, pod `allowPrivilegeEscalation: false` bildiriminde ayarlayın. Bu *bölme güvenlik bağlamları* Kubernetes'te yerleşiktir ve çalışacak kullanıcı veya grup veya hangi Linux yeteneklerini ortaya çıkarabileceğiniz gibi ek izinler tanımlamanıza olanak sağlar. Daha iyi uygulamalar için [kaynaklara güvenli pod erişimi][pod-security-contexts]bakın.

Kapsayıcı eylemlerinin daha ayrıntılı denetimi için *AppArmor* ve *seccomp*gibi yerleşik Linux güvenlik özelliklerini de kullanabilirsiniz. Bu özellikler düğüm düzeyinde tanımlanır ve daha sonra bir bakla bildirimi aracılığıyla uygulanır. Yerleşik Linux güvenlik özellikleri yalnızca Linux düğümleri ve bölmelerinde kullanılabilir.

> [!NOTE]
> Kubernetes ortamları, AKS veya başka bir yerde, düşmanca çok kiracı kullanımı için tamamen güvenli değildir. *AppArmor,* *seccomp,* *Pod Security İlkeleri*veya düğümler için daha ince taneli rol tabanlı erişim denetimleri (RBAC) gibi ek güvenlik özellikleri açıkları daha zor hale getirmektedir. Ancak, düşmanca çok kiracılı iş yüklerini çalıştırırken gerçek güvenlik için, bir hipervizör güvenmeniz gereken tek güvenlik düzeyidir. Kubernetes için güvenlik etki alanı tek bir düğüm değil, tüm küme olur. Bu tür çok kiracılı çok kiracılı iş yükleri için fiziksel olarak yalıtılmış kümeler kullanmalısınız.

### <a name="app-armor"></a>Uygulama Zırhı

Kapsayıcıların gerçekleştirebileceği eylemleri sınırlamak için [AppArmor][k8s-apparmor] Linux çekirdek güvenlik modüllerini kullanabilirsiniz. AppArmor, temel AKS düğüm işletim sistemi'nin bir parçası olarak kullanılabilir ve varsayılan olarak etkinleştirilir. Okuma, yazma veya yürütme gibi eylemleri kısıtlayan AppArmor profilleri veya dosya sistemlerini montaj gibi sistem işlevleri oluşturursunuz. Varsayılan AppArmor profilleri çeşitli `/proc` konumlara ve `/sys` konumlara erişimi kısıtlar ve kapsayıcıları temel düğümden mantıksal olarak yalıtmak için bir araç sağlar. AppArmor Linux üzerinde çalışan herhangi bir uygulama için çalışır, sadece Kubernetes pods.

![Kapsayıcı eylemlerini sınırlamak için aks kümesinde kullanılan AppArmor profilleri](media/operator-best-practices-container-security/apparmor.png)

AppArmor'u iş başında görmek için aşağıdaki örnek, dosyalara yazmayı engelleyen bir profil oluşturur. Bir AKS düğümüne [SSH,][aks-ssh] ardından *deny-write.profile* adlı bir dosya oluşturun ve aşağıdaki içeriği yapıştırın:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

AppArmor profilleri komutu `apparmor_parser` kullanılarak eklenir. Profili AppArmor'a ekleyin ve önceki adımda oluşturulan profilin adını belirtin:

```console
sudo apparmor_parser deny-write.profile
```

Profil doğru bir şekilde ayrıştırılır ve AppArmor'a uygulanırsa hiçbir çıktı döndürülmez. Komut istemine döndürülürsün.

Yerel makinenizden, şimdi *aks-apparmor.yaml* adlı bir pod manifestosu oluşturun ve aşağıdaki içeriği yapıştırın. Bu bildirim, önceki adımlarda `container.apparmor.security.beta.kubernetes` oluşturulan *reddet-yazma* profili ekleme başvuruları için bir ek açıklama tanımlar:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
spec:
  containers:
  - name: hello
    image: busybox
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak örnek bölmeyi dağıtın:

```console
kubectl apply -f aks-apparmor.yaml
```

Pod dağıtıldı, bir dosyaya yazmak için [kubectl exec][kubectl-exec] komutunu kullanın. Aşağıdaki örnek çıktıda gösterildiği gibi komut yürütülemez:

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

AppArmor hakkında daha fazla bilgi için [Kubernetes'teki AppArmor profillerine][k8s-apparmor]bakın.

### <a name="secure-computing"></a>Güvenli bilgi işlem

AppArmor herhangi bir Linux uygulaması için çalışırken, [*seccomp (sec*ure *comp*uting)][seccomp] işlem düzeyinde çalışır. Seccomp aynı zamanda bir Linux çekirdeği güvenlik modülüdür ve aks düğümleri tarafından kullanılan Docker çalışma zamanı tarafından yerel olarak desteklenir. Seccomp ile, kapsayıcıların gerçekleştirebileceği işlem çağrıları sınırlıdır. Hangi eylemlerin izin verilip reddedilemeyeceğini tanımlayan filtreler oluşturur sunuz ve ardından seccomp filtresiyle ilişkilendirmek için bir bölme YAML bildiriminde ek açıklamalar kullanırsınız. Bu, yalnızca kapsayıcıya çalışması için gereken en az izinleri ve daha fazlasını verme nin en iyi uygulamasına hizalanır.

Eylem de seccomp görmek için, bir dosya üzerinde izinleri değiştirmeyi engelleyen bir filtre oluşturun. Bir AKS düğümüne [SSH,][aks-ssh] ardından */var/lib/kubelet/seccomp/prevent-chmod* adlı bir seccomp filtresi oluşturun ve aşağıdaki içeriği yapıştırın:

```
{
  "defaultAction": "SCMP_ACT_ALLOW",
  "syscalls": [
    {
      "name": "chmod",
      "action": "SCMP_ACT_ERRNO"
    }
  ]
}
```

Yerel makinenizden, şimdi *aks-seccomp.yaml* adlı bir pod manifestosu oluşturun ve aşağıdaki içeriği yapıştırın. Bu bildirim, önceki adımda `seccomp.security.alpha.kubernetes.io` oluşturulan *prevent-chmod* filtresi için bir ek açıklama tanımlar ve başvurur:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: chmod-prevented
  annotations:
    seccomp.security.alpha.kubernetes.io/pod: localhost/prevent-chmod
spec:
  containers:
  - name: chmod
    image: busybox
    command:
      - "chmod"
    args:
     - "777"
     - /etc/hostname
  restartPolicy: Never
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak örnek bölmeyi dağıtın:

```console
kubectl apply -f ./aks-seccomp.yaml
```

[Kubectl get pods][kubectl-get] komutunu kullanarak bölmelerin durumunu görüntüleyin. Pod bir hata bildirir. Komutun, `chmod` aşağıdaki örnek çıktıda gösterildiği gibi seccomp filtresi tarafından çalıştırılması engellenir:

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Kullanılabilir filtreler hakkında daha fazla bilgi için [Docker için Seccomp güvenlik profillerine][seccomp]bakın.

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Düzenli olarak Kubernetes en son sürümüne güncelleme

**En iyi uygulama kılavuzu** - Yeni özellikler ve hata düzeltmeleri üzerinde güncel kalmak için, AKS kümenizdeki Kubernetes sürümüne düzenli olarak yükseltin.

Kubernetes yeni özellikleri daha geleneksel altyapı platformlarından daha hızlı bir hızda yayınlar. Kubernetes güncelleştirmeleri yeni özellikler ve hata veya güvenlik düzeltmeleri içerir. Yeni özellikler genellikle *kararlı* hale gelmeden önce bir *alfa* ve daha sonra *beta* durumu ile hareket ve genellikle kullanılabilir ve üretim kullanımı için önerilir. Bu sürüm döngüsü, düzenli olarak çığır açan değişikliklerle karşılaşmadan veya dağıtımlarınızı ve şablonlarınızı ayarlamadan Kubernetes'i güncelleştirmenize olanak sağlar.

AKS, Kubernetes'in dört küçük versiyonunu destekler. Bu, yeni bir küçük yama sürümü tanıtıldığında, desteklenen en eski küçük sürüm ve yama sürümlerinin kullanımdan kaldırıldığı anlamına gelir. Kubernetes için küçük güncellemeler periyodik olarak olur. Destekten düşmemek için gerektiğinde kontrol etmek ve yükseltmek için bir yönetim sürecine sahip olduğundan emin olun. Daha fazla bilgi için, [desteklenen Kubernetes sürümleri AKS][aks-supported-versions] bakın

Kümeniz için kullanılabilen sürümleri denetlemek için aşağıdaki örnekte gösterildiği gibi [az aks get-upgrades][az-aks-get-upgrades] komutunu kullanın:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Daha sonra [az aks yükseltme][az-aks-upgrade] komutunu kullanarak AKS kümeyükseltebilirsiniz. Yükseltme işlemi, aynı anda bir düğümü güvenli bir şekilde kordon altına alıp boşaltır, kalan düğümlerde bölmeleri zamanlar ve ardından en son işletim sistemi ve Kubernetes sürümlerini çalıştıran yeni bir düğüm dağıtır.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version KUBERNETES_VERSION
```

AKS yükseltmeleri hakkında daha fazla bilgi için, [AKS'deki Desteklenen Kubernetes sürümlerine][aks-supported-versions] bakın ve [bir AKS kümesini yükseltin.][aks-upgrade]

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>Kured kullanarak Linux düğümü güncellemelerini ve yeniden başlatmaları işleme

**En iyi uygulama kılavuzu** - AKS otomatik olarak her Linux düğümünde güvenlik düzeltmeleri indirir ve yükler, ancak gerekirse otomatik olarak yeniden başlatmaz. Bekleyen `kured` yeniden başlatmaları izlemek için kullanın, ardından düğümügüvenli bir şekilde kordon altına alın ve düğümün yeniden başlatılmasına, güncelleştirmelerin uygulanmasına ve işletim sistemi yle ilgili olarak mümkün olduğunca güvenli olması için düğümü boşaltın. Windows Server düğümleri için (şu anda AKS'de önizlemede), bölmeleri güvenli bir şekilde kordon altına almak ve boşaltmak ve güncelleştirilmiş düğümleri dağıtmak için düzenli olarak bir AKS yükseltme işlemi gerçekleştirin.

Her akşam, AKS'deki Linux düğümleri dağıtım güncelleştirme kanalı aracılığıyla güvenlik yamaları elde eder. Düğümler bir AKS kümesinde dağıtıldığında bu davranış otomatik olarak yapılandırılır. İş yüklerinin çalıştırılmasındaki kesintiyi ve olası etkiyi en aza indirmek için, bir güvenlik düzeltme ekini veya çekirdek güncelleştirmesi gerektiriyorsa düğümler otomatik olarak yeniden başlatılmez.

Açık kaynak [kured (KUbernetes REboot Daemon)][kured] Weaveworks tarafından proje bekleyen düğüm yeniden başlatmaları için saatler. Bir Linux düğümü yeniden başlatma gerektiren güncelleştirmeler uyguladığında, düğüm güvenli bir şekilde kordon altına edilir ve kümedeki diğer düğümlerde bölmeleri taşımak ve zamanlamak için güvenli bir şekilde boşaltılır. Düğüm yeniden başlatıldıktan sonra kümeye geri eklenir ve Kubernetes üzerinde zamanlama bölmelerine devam eder. Bozulmayı en aza indirmek için, aynı anda yalnızca bir `kured`düğümün yeniden başlatılmasına izin verilir.

![KURED kullanarak AKS düğümü yeniden başlatma işlemi](media/operator-best-practices-cluster-security/node-reboot-process.png)

Yeniden başlatmalar ne zaman daha ince `kured` gren denetimi istiyorsanız, devam eden diğer bakım olayları veya küme sorunları varsa yeniden başlatmaları önlemek için Prometheus ile tümleştirebilirsiniz. Bu tümleştirme, siz etkin olarak diğer sorunları giderirken düğümleri yeniden başlatarak ek komplikasyonları en aza indirir.

Düğüm yeniden başlatmaları nasıl işlenir hakkında daha fazla bilgi için, [aks düğümlerine güvenlik ve çekirdek güncelleştirmeleri uygula'ya][aks-kured]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümenizin nasıl güvenli hale alındığı üzerinde duruluyor. Bu alanlardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [Azure Etkin Dizini AKS ile tümleştir][aks-aad]
* [Bir AKS kümesini Kubernetes'in en son sürümüne yükseltin][aks-upgrade]
* [Kured ile işlem güvenlik güncelleştirmeleri ve düğüm yeniden başlatma][aks-kured]

<!-- EXTERNAL LINKS -->
[kured]: https://github.com/weaveworks/kured
[k8s-apparmor]: https://kubernetes.io/docs/tutorials/clusters/apparmor/
[seccomp]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#seccomp
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[aks-supported-versions]: supported-kubernetes-versions.md
[aks-upgrade]: upgrade-cluster.md
[aks-best-practices-identity]: concepts-identity.md
[aks-kured]: node-updates-kured.md
[aks-aad]: azure-ad-integration.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[pod-security-contexts]: developer-best-practices-pod-security.md#secure-pod-access-to-resources
[aks-ssh]: ssh.md
[security-center-aks]: /azure/security-center/azure-kubernetes-service-integration
