---
title: Küme güvenliği için en iyi uygulamalar
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service (AKS) ' de küme güvenliğini ve yükseltmelerini yönetme için en iyi küme işletmeni uygulamalarını öğrenin
services: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 72808f315f28a996a88e6cc56ae232a136726451
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85298030"
---
# <a name="best-practices-for-cluster-security-and-upgrades-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde küme güvenliği ve yükseltmeleri için en iyi uygulamalar

Azure Kubernetes Service (AKS) içindeki kümeleri yönetirken, iş yüklerinizin ve verilerinizin güvenliği önemli bir konudur. Özellikle, mantıksal yalıtım kullanarak çok kiracılı kümeler çalıştırdığınızda, kaynaklara ve iş yüklerine erişimi güvence altına almanız gerekir. Saldırı riskini en aza indirmek için, en son Kubernetes ve Node OS güvenlik güncelleştirmelerini de uyguladığınızdan emin olmanız gerekir.

Bu makalede AKS kümenizin güvenliğini sağlama konusuna odaklanılmaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * API sunucusu erişimini güvenli hale getirmek için Azure Active Directory ve rol tabanlı erişim denetimleri kullanma
> * Düğüm kaynaklarına güvenli kapsayıcı erişimi
> * AKS kümesini en son Kubernetes sürümüne yükseltme
> * Düğümleri güncel tut ve güvenlik düzeltme eklerini otomatik olarak Uygula

Ayrıca, [kapsayıcı görüntüsü yönetimi][best-practices-container-image-management] ve [Pod güvenliği][best-practices-pod-security]için en iyi yöntemleri okuyabilirsiniz.

Tehditleri algılamaya ve AKS kümelerinizi korumaya yönelik önerileri görüntülemenize yardımcı olması için [Güvenlik Merkezi Ile Azure Kubernetes Hizmetleri tümleştirmesini][security-center-aks] da kullanabilirsiniz.

## <a name="secure-access-to-the-api-server-and-cluster-nodes"></a>API sunucusuna ve küme düğümlerine güvenli erişim

**En iyi Yöntem Kılavuzu** -Kubernetes API-Server erişimini güvenli hale getirmek, kümenizin güvenliğini sağlamak için yapabileceğiniz en önemli işlemlerden biridir. API sunucusuna erişimi denetlemek için Kubernetes rol tabanlı erişim denetimini (RBAC) Azure Active Directory ile tümleştirin. Bu denetimler, Azure aboneliklerinize erişiminizi güvenli hale getirmenin aynı şekilde güvenliğini sağlamanıza olanak tanır.

Kubernetes API sunucusu, bir küme içinde eylemleri gerçekleştirmek için istekler için tek bir bağlantı noktası sağlar. API sunucusuna erişimi güvenli hale getirmek ve denetlemek için erişimi sınırlayın ve gereken en az ayrıcalıklı erişim izinlerini sağlayın. Bu yaklaşım Kubernetes için benzersiz değildir, ancak AKS kümesi çok kiracılı kullanım için mantıksal olarak yalıtılmışsa özellikle önemlidir.

Azure Active Directory (AD), AKS kümeleriyle tümleştirilen kurumsal özellikli bir kimlik yönetimi çözümü sağlar. Kubernetes bir kimlik yönetimi çözümü sunmazsa, API sunucusuna erişimi kısıtlamak için parçalı bir yol sağlamak zor olabilir. AKS 'deki Azure AD ile tümleşik kümeler sayesinde, API sunucusuna kullanıcıların kimliğini doğrulamak için mevcut kullanıcı ve grup hesaplarınızı kullanırsınız.

![AKS kümeleri için Azure Active Directory tümleştirme](media/operator-best-practices-cluster-security/aad-integration.png)

API sunucusunu güvenli hale getirmek ve tek bir ad alanı gibi kapsamlı bir kaynak kümesine gereken en az sayıda izin sağlamak için Kubernetes RBAC ve Azure AD-Integration kullanın. Azure AD 'deki farklı kullanıcılara veya gruplara farklı RBAC rolleri verilebilir. Bu ayrıntılı izinler, API sunucusuna erişimi kısıtlamanıza ve gerçekleştirilen eylemlerin net bir denetim izini sağlamanıza olanak tanır.

Önerilen en iyi yöntem, dosyalara ve klasörlere tek tek kimlikler için erişim sağlamak üzere grupları kullanmak, kullanıcıları bireysel *Kullanıcılar*yerine RBAC rollerine bağlamak IÇIN Azure AD *Grup* üyeliği kullanmaktır. Bir kullanıcının grup üyeliği değiştikçe, AKS kümesindeki erişim izinleri buna göre değişir. Kullanıcıyı doğrudan bir role bağlarsanız, iş işlevi değişebilir. Azure AD grup üyelikleri güncelleştirebilir, ancak AKS kümesindeki izinler bunu yansıtmaz. Bu senaryoda, kullanıcıya bir kullanıcının gerektirdiğinden daha fazla izin verilmeye sona eriyor.

Azure AD tümleştirmesi ve RBAC hakkında daha fazla bilgi için bkz. [AKS 'de kimlik doğrulama ve yetkilendirme Için en iyi uygulamalar][aks-best-practices-identity].

## <a name="secure-container-access-to-resources"></a>Kaynaklara güvenli kapsayıcı erişimi

Kapsayıcının gerçekleştirebileceği eylemlere erişimi **en iyi yöntem kılavuzlarından** sınırlayın. En az sayıda izin sağlayın ve kök/ayrıcalıklı yükseltme kullanmaktan kaçının.

Kullanıcılara veya gruplara gereken en az ayrıcalık sayısını vermeniz gerektiği gibi, kapsayıcılar de yalnızca ihtiyaç duydukları eylemlerle ve işlemlerle sınırlandırılmalıdır. Saldırı riskini en aza indirmek için, ilerletilen ayrıcalıklar veya kök erişim gerektiren uygulamaları ve kapsayıcıları yapılandırmayın. Örneğin, `allowPrivilegeEscalation: false` Pod bildiriminde ayarlayın. Bu *Pod güvenlik bağlamları* , Kubernetes 'te yerleşiktir ve çalıştırılacak Kullanıcı veya grup gibi ek izinler tanımlamanızı veya kullanıma sunulacak Linux özelliklerini tanımlamanızı sağlar. Daha iyi uygulamalar için bkz. [kaynaklara güvenli Pod erişimi][pod-security-contexts].

Kapsayıcı eylemlerinin daha ayrıntılı denetimi için *AppArmor* ve *Seccomp*gibi yerleşik Linux güvenlik özelliklerini de kullanabilirsiniz. Bu özellikler düğüm düzeyinde tanımlanmıştır ve sonra Pod bildirimi aracılığıyla uygulanır. Yerleşik Linux güvenlik özellikleri yalnızca Linux düğümlerinde ve yığınların üzerinde kullanılabilir.

> [!NOTE]
> Kubernetes ortamları, AKS veya başka bir yerde, çok kiracılı Kullanıcı kullanımı için tamamen güvenli değildir. Düğümler için *AppArmor*, *seccomp*, *Pod güvenlik ilkeleri*veya daha AYRıNTıLı rol tabanlı erişim denetimleri (RBAC) gibi ek güvenlik özellikleri, güvenli hale getirme daha zordur. Ancak, çok kiracılı çoklu kiracı iş yüklerini çalıştırırken doğru güvenlik için bir hiper yönetici, güvenmeniz gereken tek güvenlik düzeyidir. Kubernetes güvenlik etki alanı, tek bir düğüm değil, tüm küme haline gelir. Bu tür çok kiracılı iş yükleri için, fiziksel olarak yalıtılmış kümeler kullanmanız gerekir.

### <a name="app-armor"></a>Uygulama koruma sağlamak

Kapsayıcıların gerçekleştirebileceği eylemleri sınırlandırmak için [AppArmor][k8s-apparmor] Linux çekirdek güvenlik modülünü kullanabilirsiniz. AppArmor, temel alınan AKS düğüm işletim sisteminin bir parçası olarak kullanılabilir ve varsayılan olarak etkindir. Okuma, yazma veya yürütme gibi eylemleri kısıtlayan AppArmor profilleri veya filesystems bağlama gibi sistem işlevleri oluşturabilirsiniz. Varsayılan AppArmor profilleri `/proc` , çeşitli ve konumlarına erişimi kısıtlar `/sys` ve temel alınan düğümden kapsayıcıları mantıksal olarak yalıtmak için bir yol sağlar. AppArmor, yalnızca Kubernetes pods değil, Linux üzerinde çalışan tüm uygulamalar için çalışır.

![Bir AKS kümesinde kullanılan AppArmor profilleri kapsayıcı eylemlerini sınırlandırmaya](media/operator-best-practices-container-security/apparmor.png)

AppArmor 'i görmek için aşağıdaki örnek, dosyalara yazmayı önleyen bir profil oluşturur. [SSH][aks-ssh] 'YI BIR aks düğümüne, ardından *deny-Write. Profile* adlı bir dosya oluşturun ve aşağıdaki içeriği yapıştırın:

```
#include <tunables/global>
profile k8s-apparmor-example-deny-write flags=(attach_disconnected) {
  #include <abstractions/base>
  
  file,
  # Deny all file writes.
  deny /** w,
}
```

AppArmor profilleri komutu kullanılarak eklenir `apparmor_parser` . Profili AppArmor öğesine ekleyin ve önceki adımda oluşturulan profilin adını belirtin:

```console
sudo apparmor_parser deny-write.profile
```

Profil doğru ayrıştırılırsa ve AppArmor 'e uygulanırsa hiçbir çıkış döndürülmedi. Komut istemine döndürülürsünüz.

Şimdi, yerel makinenizden *aks-AppArmor. YAML* adlı bir pod bildirimi oluşturun ve aşağıdaki içeriği yapıştırın. Bu bildirim, `container.apparmor.security.beta.kubernetes` önceki adımlarda oluşturulan *reddetme-yazma* profiline başvuru Ekle için bir ek açıklama tanımlar:

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

Pod ile dağıtılan bir dosyaya yazmak için [kubectl exec][kubectl-exec] komutunu kullanın. Komut aşağıdaki örnek çıktıda gösterildiği gibi yürütülemez:

```
$ kubectl exec hello-apparmor touch /tmp/test

touch: /tmp/test: Permission denied
command terminated with exit code 1
```

AppArmor hakkında daha fazla bilgi için bkz. [Kubernetes 'Te AppArmor profilleri][k8s-apparmor].

### <a name="secure-computing"></a>Güvenli bilgi işlem

AppArmor tüm Linux uygulamaları için çalışırken, [seccomp (*San*) işlem *comp*][seccomp] düzeyinde çalışmaktadır. Seccomp Ayrıca bir Linux çekirdek güvenlik modülüdür ve AKS düğümleri tarafından kullanılan Docker çalışma zamanı tarafından yerel olarak desteklenir. Seccomp ile, kapsayıcıların gerçekleştirebileceği işlem çağrıları sınırlıdır. İzin verilecek veya reddedilecek eylemleri tanımlayan filtreler oluşturun ve ardından seccomp filtresiyle ilişkilendirmek üzere Pod YAML bildiriminde ek açıklamaları kullanın. Bu, yalnızca kapsayıcıyı çalıştırmak için gereken en düşük izinleri ve daha fazlasını veren en iyi uygulamaya hizalanır.

Seccomp eylemini görmek için, bir dosyada izinleri değiştirmeyi önleyen bir filtre oluşturun. Bir AKS düğümüne [SSH][aks-ssh] ekleyin, ardından */var/lib/kubelet/seccomp/prevent-chmod* adlı bir seccomp filtresi oluşturun ve aşağıdaki içeriği yapıştırın:

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

Şimdi, yerel makinenizden *aks-seccomp. YAML* adlı bir pod bildirimi oluşturun ve aşağıdaki içeriği yapıştırın. Bu bildirim, için bir ek açıklama tanımlar `seccomp.security.alpha.kubernetes.io` ve önceki adımda oluşturulan *Önle-chmod* filtresine başvurur:

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

[Kubectl Get Pod][kubectl-get] komutunu kullanarak Pod 'nin durumunu görüntüleyin. Pod bir hata bildiriyor. `chmod`Aşağıdaki örnek çıktıda gösterildiği gibi komutun seccomp filtresi tarafından çalışması engellenir:

```
$ kubectl get pods

NAME                      READY     STATUS    RESTARTS   AGE
chmod-prevented           0/1       Error     0          7s
```

Kullanılabilir filtreler hakkında daha fazla bilgi için bkz. [Docker Için Seccomp güvenlik profilleri][seccomp].

## <a name="regularly-update-to-the-latest-version-of-kubernetes"></a>En son Kubernetes sürümüne düzenli olarak güncelleştir

**En iyi Yöntem Kılavuzu** -yeni özellikler ve hata düzeltmeleri üzerinde güncel kalmak için aks kümenizdeki Kubernetes sürümünü düzenli olarak yükseltin.

Kubernetes, daha geleneksel altyapı platformlarından daha hızlı bir şekilde yeni özellikler yayımlar. Kubernetes güncelleştirmeleri yeni özellikler ve hata veya güvenlik düzeltmelerini içerir. Yeni özellikler tipik olarak bir *Alfa* ve ardından *Beta* durumunu *kararlı* hale gelmeden önce ve üretim kullanımı için kullanılması önerilen bir şekilde taşır. Bu yayın döngüsünün, Kubernetes 'i düzenli olarak bozmadan veya dağıtımlarınızı ve şablonlarınızı ayarlamadan güncelleştirmenize izin vermeniz gerekir.

AKS, Kubernetes 'in dört küçük sürümünü destekler. Bu, yeni bir ikincil yama sürümü ortaya çıkdığında, desteklenen en eski alt sürüm ve düzeltme eki sürümlerinin kullanımdan kalktığını gösterir. Kubernetes 'e yönelik küçük güncelleştirmeler düzenli aralıklarla gerçekleşir. Destek dışı kalmaması için gereken şekilde denetim ve yükseltme yapmak üzere bir idare işlemi kullandığınızdan emin olun. Daha fazla bilgi için bkz. [desteklenen Kubernetes sürümleri AKS][aks-supported-versions]

Kümeniz için kullanılabilen sürümleri denetlemek için, aşağıdaki örnekte gösterildiği gibi [az aks Get-yükseltmeler][az-aks-get-upgrades] komutunu kullanın:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Daha sonra, [az aks Upgrade][az-aks-upgrade] komutunu kullanarak aks kümenizi yükseltebilirsiniz. Yükseltme işlemi aynı anda bir düğümü güvenli bir şekilde alır ve kapatır, kalan düğümlerde Pod 'yi zamanlar ve ardından en son işletim sistemi ve Kubernetes sürümlerini çalıştıran yeni bir düğüm dağıtır.

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version KUBERNETES_VERSION
```

AKS 'teki yükseltmeler hakkında daha fazla bilgi için bkz. [aks 'de desteklenen Kubernetes sürümleri][aks-supported-versions] ve [aks kümesini yükseltme][aks-upgrade].

## <a name="process-linux-node-updates-and-reboots-using-kured"></a>Linux düğüm güncelleştirmelerini işleme ve kured kullanarak yeniden başlatmalar

**En iyi Yöntem Kılavuzu** -aks her bir Linux düğümünde güvenlik düzeltmelerini otomatik olarak indirir ve yükler, ancak gerektiğinde otomatik olarak yeniden başlatılır. `kured`Bekleyen yeniden başlatmalar için izlemek üzere öğesini kullanın, ardından düğümün yeniden başlatılmasını sağlamak için düğümü güvenle kapatıp boşaltın, güncelleştirmeleri uygulayın ve işletim sistemine göre mümkün olduğunca güvenli hale getirin. Windows Server düğümleri için düzenli olarak bir AKS yükseltme işlemi gerçekleştirip, büyük/dışarı boşaltma ve güncelleştirilmiş düğümleri dağıtma işlemleri yapılır.

AKS içindeki her akşam Linux düğümü, kendi sahip oldukları güncelleştirme kanalıyla sunulan güvenlik düzeltme eklerini alır. Bu davranış, düğümler bir AKS kümesinde dağıtıldığında otomatik olarak yapılandırılır. Çalışma yüklerini çalıştırmaya yönelik kesintiyi ve olası etkiyi en aza indirmek için, bir güvenlik düzeltme eki veya çekirdek güncelleştirmesi gerektiriyorsa düğümler otomatik olarak yeniden başlatılır.

Açık kaynaklı [kured (KUbernetes önyükleme cini)][kured] projesi, bekleyen düğüm yeniden başlatmaları için izler. Bir Linux düğümü, yeniden başlatma gerektiren güncelleştirmeler uygularsa, düğüm, kümedeki diğer düğümlerde bulunan düğümleri taşımak ve zamanlamak için güvenli bir şekilde donmış ve taşınabilir. Düğüm yeniden başlatıldıktan sonra, kümeye geri eklenir ve Kubernetes bu, üzerinde yer alan zamanlamaya devam eder. Kesintiyi en aza indirmek için, tek seferde yalnızca bir düğümün tarafından yeniden başlatılması izin verilir `kured` .

![Kured kullanarak AKS düğümü yeniden başlatma işlemi](media/operator-best-practices-cluster-security/node-reboot-process.png)

Yeniden başlatmalar gerçekleştiğinde daha hassas bir denetim istiyorsanız, `kured` devam eden başka bakım olayları veya küme sorunları varsa, yeniden başlatmaları engellemek Için Prometheus ile tümleştirilebilir. Bu tümleştirme, diğer sorunları etkin bir şekilde giderirken düğümleri yeniden başlatarak ek karmaşıklıkları en aza indirir.

Düğüm yeniden başlatmaların nasıl işleneceği hakkında daha fazla bilgi için bkz. [AKS 'deki düğümlere güvenlik ve çekirdek güncelleştirmelerini uygulama][aks-kured].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümenizin güvenliğini sağlama konusunda odaklanılmıştır. Bu alanlardan bazılarını uygulamak için aşağıdaki makalelere bakın:

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
