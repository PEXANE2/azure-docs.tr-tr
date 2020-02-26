---
title: İşleç en iyi uygulamalar - Azure Kubernetes Hizmetleri (AKS) küme güvenliği
description: Küme güvenliği ve yükseltmeleri Azure Kubernetes Service (AKS) nasıl yönetileceği küme işleci en iyi uygulamaları öğrenin
services: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: f02f6588946e2b63a1a092aba15603d1685e8207
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77594813"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Küme güvenliği ve yükseltmeler Azure Kubernetes Service (AKS) için en iyi uygulamalar

Azure Kubernetes Service (AKS) kümeleri yönetme gibi iş yüklerinde ve veri güvenliğini önemli bir noktadır. Özellikle, mantıksal yalıtım kullanarak çok kiracılı kümeleri çalıştırdığınızda, kaynaklar ve iş yüklerini güvenli erişim gerekir. Saldırı riskini en aza indirmek için Ayrıca düğümün işletim sistemi güvenlik güncelleştirmelerini ve en son Kubernetes geçerli olduğundan emin olmamız gerekir.

Bu makalede, AKS kümenizin güvenliğini sağlama konusunda odaklanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * API sunucusu erişimi güvenli hale getirmek için Azure Active Directory ve rol tabanlı erişim denetimleri kullanma
> * Kapsayıcı düğümü kaynaklarına güvenli erişim
> * Bir AKS kümesi son Kubernetes sürümüne yükseltmek için
> * Düğümleri güncel tut ve güvenlik düzeltme eklerini otomatik olarak Uygula

Ayrıca, [kapsayıcı görüntüsü yönetimi][best-practices-container-image-management] ve [Pod güvenliği][best-practices-pod-security]için en iyi yöntemleri okuyabilirsiniz.

Tehditleri algılamaya ve AKS kümelerinizi korumaya yönelik önerileri görüntülemenize yardımcı olması için [Güvenlik Merkezi Ile Azure Kubernetes Hizmetleri tümleştirmesini][security-center-aks] da kullanabilirsiniz.

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>API sunucu ile küme düğümleri erişimin güvenliğini sağlama

**En iyi Yöntem Kılavuzu** -Kubernetes API-Server erişimini güvenli hale getirmek, kümenizin güvenliğini sağlamak için yapabileceğiniz en önemli işlemlerden biridir. Kubernetes rol tabanlı erişim denetimi (RBAC) API sunucusuna erişimi denetlemek için Azure Active Directory ile tümleştirin. Bu denetimler, AKS Azure aboneliklerinize erişimi güvenli şekilde güvenli hale getirmenize olanak tanır.

Kubernetes API sunucusuna bir küme içindeki eylemleri gerçekleştirmek istekleri için bir tek bağlantı noktası sağlar. Güvenli ve API sunucusuna erişimi denetlemek için erişimi sınırlamak ve gereken en az ayrıcalıklı erişim izinleri sağlar. Bu yaklaşım, Kubernetes için benzersiz değildir, ancak çok kiracılı kullanılmak üzere mantıksal olarak yalıtılmış bir AKS kümesi olduğunda özellikle önemlidir.

Azure Active Directory (AD), AKS kümeleri ile tümleşen bir kurumsal kullanıma hazır kimlik yönetimi çözümü sağlar. Kubernetes, bir kimlik yönetimi çözümü sağlamayan gibi Aksi takdirde API sunucusuna erişimi kısıtlamak için ayrıntılı bir şekilde sağlamak zor olabilir. Azure AD ile tümleşik kümeleriyle aks'deki, API sunucusu için kullanıcıların kimliğini doğrulamak için mevcut kullanıcı ve grup hesapları kullanırsınız.

![AKS kümeleri için Azure Active Directory Tümleştirmesi](media/operator-best-practices-cluster-security/aad-integration.png)

Kubernetes RBAC ve Azure API sunucusunun güvenliğini sağlama ve az sağlamak için AD tümleştirme tek bir ad alanı gibi kaynaklar kapsamlı bir dizi gerekli izinler sayısı. Farklı kullanıcılar veya gruplar Azure AD'de farklı RBAC rollerini verilebilir. Bu ayrıntılı izinler API sunucusuna erişimi kısıtlamak ve gerçekleştirilen eylemlerin bir açık denetim izi sağlamak belirlemenizi sağlar.

Önerilen en iyi yöntem, dosyalara ve klasörlere tek tek kimlikler için erişim sağlamak üzere grupları kullanmak, kullanıcıları bireysel *Kullanıcılar*yerine RBAC rollerine bağlamak IÇIN Azure AD *Grup* üyeliği kullanmaktır. Bir kullanıcının grup üyeliği değişiklikleri olarak bunların erişim izinlerini AKS kümesinde uygun şekilde değiştirirsiniz. Bir role kullanıcı bağlarsanız, kendi iş işlevi değiştirebilir. Azure AD grup üyeliklerini güncelleştirmeniz gerekir, ancak izinleri AKS kümesi, yansıtmıyor. Bu senaryoda, kullanıcı, bir kullanıcı gerektirir. daha fazla izin verilmeden sona erer.

Azure AD tümleştirmesi ve RBAC hakkında daha fazla bilgi için bkz. [AKS 'de kimlik doğrulama ve yetkilendirme Için en iyi uygulamalar][aks-best-practices-identity].

## <a name="secure-container-access-to-resources"></a>Kapsayıcı kaynaklarına güvenli erişim

Kapsayıcının gerçekleştirebileceği eylemlere erişimi **en iyi yöntem kılavuzlarından** sınırlayın. En az sağlayan izinleri sayısı ve kök kullanmaktan kaçının / ayrıcalıklı yükseltme.

En az kullanıcılar veya gruplar vermelisiniz aynı şekilde ayrıcalıkları sayısı gerekli, kapsayıcılar da eylemleri ve ihtiyaç duydukları işlemleri yalnızca sınırlı olmalıdır. Saldırı riskini en aza indirmek için uygulamaları ve ilerletilen ayrıcalıkları gerektiren veya kök erişim kapsayıcıları yapılandırmayın. Örneğin, Pod bildiriminde `allowPrivilegeEscalation: false` ayarlayın. Bu *Pod güvenlik bağlamları* , Kubernetes 'te yerleşiktir ve çalıştırılacak Kullanıcı veya grup gibi ek izinler tanımlamanızı veya kullanıma sunulacak Linux özelliklerini tanımlamanızı sağlar. Daha iyi uygulamalar için bkz. [kaynaklara güvenli Pod erişimi][pod-security-contexts].

Kapsayıcı eylemlerinin daha ayrıntılı denetimi için *AppArmor* ve *Seccomp*gibi yerleşik Linux güvenlik özelliklerini de kullanabilirsiniz. Bu özellikler düğümü düzeyinde tanımlanan ve ardından bir pod bildirimi aracılığıyla uygulanır. Yerleşik Linux güvenlik özellikleri yalnızca Linux düğümlerinde ve yığınların üzerinde kullanılabilir.

> [!NOTE]
> Kubernetes ortamları, AKS veya başka bir yerde, çok kiracılı Kullanıcı kullanımı için tamamen güvenli değildir. Düğümler için *AppArmor*, *seccomp*, *Pod güvenlik ilkeleri*veya daha AYRıNTıLı rol tabanlı erişim denetimleri (RBAC) gibi ek güvenlik özellikleri, güvenli hale getirme daha zordur. Ancak, çok kiracılı çoklu kiracı iş yüklerini çalıştırırken doğru güvenlik için bir hiper yönetici, güvenmeniz gereken tek güvenlik düzeyidir. Kubernetes güvenlik etki alanı, tek bir düğüm değil, tüm küme haline gelir. Bu tür çok kiracılı iş yükleri için, fiziksel olarak yalıtılmış kümeler kullanmanız gerekir.

### <a name="app-armor"></a>Uygulama Armor

Kapsayıcıların gerçekleştirebileceği eylemleri sınırlandırmak için [AppArmor][k8s-apparmor] Linux çekirdek güvenlik modülünü kullanabilirsiniz. AppArmor kullanılabilir işletim sistemi, temel alınan AKS düğümü bir parçası olarak ve varsayılan olarak etkindir. AppArmor eylemler gibi kısıtlama profillerini okuma, yazma veya yürütme ya da dosya sistemleri bağlama gibi sistem işlevlerini oluşturursunuz. Varsayılan AppArmor profilleri, çeşitli `/proc` ve `/sys` konumlarına erişimi kısıtlar ve temel alınan düğümden kapsayıcıları mantıksal olarak yalıtmak için bir yol sağlar. Yalnızca Kubernetes pod'larını Linux üzerinde çalışan herhangi bir uygulama için AppArmor çalışır.

![Kapsayıcı eylemlerine sınırlamak için bir AKS kümesi kullanımda AppArmor profilleri](media/operator-best-practices-container-security/apparmor.png)

AppArmor nasıl çalıştığını görmek için aşağıdaki örnek dosyalara yazma engelleyen bir profili oluşturur. [SSH][aks-ssh] 'YI BIR aks düğümüne, ardından *deny-Write. Profile* adlı bir dosya oluşturun ve aşağıdaki içeriği yapıştırın:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

AppArmor profilleri `apparmor_parser` komutu kullanılarak eklenir. Profili eklemek için AppArmor ve önceki adımda oluşturulan profilin adını belirtin:

```console
sudo apparmor_parser deny-write.profile
```

Profilin doğru bir şekilde ayrıştırılır ve AppArmor için uygulanan olursa çıktı yok. Komut istemine geri dönersiniz.

Şimdi, yerel makinenizden *aks-AppArmor. YAML* adlı bir pod bildirimi oluşturun ve aşağıdaki içeriği yapıştırın. Bu bildirim, önceki adımlarda oluşturulan *reddetme-yazma* profiline `container.apparmor.security.beta.kubernetes` Add başvuruları için bir ek açıklama tanımlar:

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

[Kubectl Apply][kubectl-apply] komutunu kullanarak örnek Pod 'u dağıtın:

```console
kubectl apply -f aks-apparmor.yaml
```

Pod ile dağıtılan bir dosyaya yazmak için [kubectl exec][kubectl-exec] komutunu kullanın. Aşağıdaki örnek çıktıda gösterildiği gibi komutu yürütülemiyor:

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

AppArmor hakkında daha fazla bilgi için bkz. [Kubernetes 'Te AppArmor profilleri][k8s-apparmor].

### <a name="secure-computing"></a>Bilgi işlem güvenliğini sağlama

AppArmor tüm Linux uygulamaları için çalışırken, [seccomp (*San*) işlem][seccomp] düzeyinde çalışmaktadır. Seccomp aynı zamanda bir Linux çekirdek güvenlik modülü olan ve AKS düğümleri tarafından kullanılan Docker çalışma zamanı tarafından yerel olarak desteklenir. Kapsayıcıları gerçekleştirebilirsiniz işlem çağrıları seccomp ile sınırlıdır. İzin verme veya reddetme eylemleri tanımlayın. filtre oluşturma ve seccomp filtresiyle ilişkilendirilebilmesi için ek açıklamalar içindeki bir pod YAML bildirimi'i kullanın. Bu, yalnızca kapsayıcı çalıştırmak için gereken en az düzeyde izinleri verme ve artık en iyi uygulama hizalar.

Seccomp iş başında görmek için değiştirme engelleyen bir filtre oluşturun. bir dosya üzerindeki izinleri. Bir AKS düğümüne [SSH][aks-ssh] ekleyin, ardından */var/lib/kubelet/seccomp/prevent-chmod* adlı bir seccomp filtresi oluşturun ve aşağıdaki içeriği yapıştırın:

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

Şimdi, yerel makinenizden *aks-seccomp. YAML* adlı bir pod bildirimi oluşturun ve aşağıdaki içeriği yapıştırın. Bu bildirim, `seccomp.security.alpha.kubernetes.io` için bir ek açıklama tanımlar ve önceki adımda oluşturulan *engelleme-chmod* filtresine başvurur:

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

[Kubectl Apply][kubectl-apply] komutunu kullanarak örnek Pod 'u dağıtın:

```console
kubectl apply -f ./aks-seccomp.yaml
```

[Kubectl Get Pod][kubectl-get] komutunu kullanarak Pod 'nin durumunu görüntüleyin. Pod bir hata bildirir. Aşağıdaki örnek çıktıda gösterildiği gibi, `chmod` komutunun seccomp filtresi tarafından çalışması engellenir:

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Kullanılabilir filtreler hakkında daha fazla bilgi için bkz. [Docker Için Seccomp güvenlik profilleri][seccomp].

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>Düzenli olarak kubernetes en son sürüme güncelleştirme

**En iyi Yöntem Kılavuzu** -yeni özellikler ve hata düzeltmeleri üzerinde güncel kalmak için aks kümenizdeki Kubernetes sürümüne düzenli olarak yükseltin.

Kubernetes, yeni özellikleri daha geleneksel altyapı platformları daha hızlı bir hızda serbest bırakır. Kubernetes güncelleştirmeleri, yeni özellikler ve hata veya güvenlik düzeltmeleri içerir. Yeni özellikler tipik olarak bir *Alfa* ve ardından *Beta* durumunu *kararlı* hale gelmeden önce ve üretim kullanımı için kullanılması önerilen bir şekilde taşır. Bu sürüm döngüsü Kubernetes düzenli olarak yeni değişiklikler karşılaşıldığında veya dağıtımları ve şablonları ayarlama olmadan güncelleştirmenize izin vermelidir.

AKS, dört alt Kubernetes sürümlerini destekler. Bu, yeni bir ikincil düzeltme eki sürümü eklendiğinde, desteklenen en eski ikincil sürüm ve yama sürümler kullanımdan anlamına gelir. Kubernetes için küçük güncelleştirmeler düzenli aralıklarla gerçekleşecek. Denetleyin ve destek kapsamı dışında kalan yoksa için gereken şekilde yükseltmek için idare işlemi olduğundan emin olun. Daha fazla bilgi için bkz. [desteklenen Kubernetes sürümleri AKS][aks-supported-versions]

Kümeniz için kullanılabilen sürümleri denetlemek için, aşağıdaki örnekte gösterildiği gibi [az aks Get-yükseltmeler][az-aks-get-upgrades] komutunu kullanın:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Daha sonra, [az aks Upgrade][az-aks-upgrade] komutunu kullanarak aks kümenizi yükseltebilirsiniz. Yükseltme işlemi güvenli bir şekilde cordons ve aynı anda bir düğümü boşaltır, pod'ların Kalan düğümlerde zamanlar ve sonra en son işletim sistemi ve Kubernetes sürümlerini çalıştıran yeni bir düğüm dağıtır.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version KUBERNETES_VERSION
```

AKS 'teki yükseltmeler hakkında daha fazla bilgi için bkz. [aks 'de desteklenen Kubernetes sürümleri][aks-supported-versions] ve [aks kümesini yükseltme][aks-upgrade].

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>Linux düğüm güncelleştirmelerini işleme ve kured kullanarak yeniden başlatmalar

**En iyi Yöntem Kılavuzu** -aks her bir Linux düğümünde güvenlik düzeltmelerini otomatik olarak indirir ve yükler, ancak gerektiğinde otomatik olarak yeniden başlatılır. Bekleyen yeniden başlatmalar için izlemek üzere `kured` kullanın, ardından düğümün yeniden başlatılmasını sağlamak için düğümü güvenle kapatıp boşaltın, güncelleştirmeleri uygulayın ve işletim sistemine göre mümkün olduğunca güvenli hale getirin. Windows Server düğümleri için (Şu anda AKS 'de önizlemededir), büyük bir şekilde bir AKS yükseltme işlemi gerçekleştirip, büyük/veya dışarı ve güncelleştirilmiş düğümleri dağıtır.

AKS içindeki her akşam Linux düğümü, kendi sahip oldukları güncelleştirme kanalıyla sunulan güvenlik düzeltme eklerini alır. Bu davranış, bir AKS kümesinde düğümlere dağıtılmış olarak otomatik olarak yapılandırılır. Kesintisi ve çalışan iş yükleri için olası etkisini en aza indirmek için düğümleri otomatik olarak bir güvenlik düzeltme eki, yeniden başlatılır değil veya çekirdek güncelleştirme gerektiriyor.

Açık kaynaklı [kured (KUbernetes önyükleme cini)][kured] projesi, bekleyen düğüm yeniden başlatmaları için izler. Bir Linux düğümü, yeniden başlatma gerektiren güncelleştirmeler uygularsa, düğüm, kümedeki diğer düğümlerde bulunan düğümleri taşımak ve zamanlamak için güvenli bir şekilde donmış ve taşınabilir. Düğüm yeniden başlatıldıktan sonra Kubernetes sürdürür üzerindeki pod'ları zamanlama ve küme yeniden içine eklenir. Kesintiyi en aza indirmek için, tek seferde yalnızca bir düğümün `kured`tarafından yeniden başlatılması izin verilir.

![Kured kullanarak AKS düğümü yeniden başlatma işlemi](media/operator-best-practices-cluster-security/node-reboot-process.png)

Yeniden başlatmalar gerçekleştiğinde daha fazla gren denetim istiyorsanız, devam eden başka bakım olayları veya küme sorunları varsa, yeniden başlatmaları engellemek için `kured` Prometheus ile tümleştirilebilir. Bu tümleştirme ek zorluklar diğer sorunlarını giderme etkin durumdayken düğümlerin yeniden başlatılması en aza indirir.

Düğüm yeniden başlatmaların nasıl işleneceği hakkında daha fazla bilgi için bkz. [AKS 'deki düğümlere güvenlik ve çekirdek güncelleştirmelerini uygulama][aks-kured].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümenizin güvenliğini sağlama konusunda odaklanır. Bu alanlardan bazıları uygulamak için aşağıdaki makalelere bakın:

* [Azure Active Directory AKS ile tümleştirme][aks-aad]
* [Bir AKS kümesini Kubernetes 'in en son sürümüne yükseltme][aks-upgrade]
* [Kured ile güvenlik güncelleştirmelerini ve düğüm yeniden başlatmaları işleme][aks-kured]

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
