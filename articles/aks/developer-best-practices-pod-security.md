---
title: Geliştirici en iyi uygulamalar - Pod güvenlik Azure Kubernetes Hizmetleri (AKS)
description: Geliştirici için en iyi pod'ları Azure Kubernetes Service (AKS) güvenli hale getirmeyi öğrenin
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: zarhoads
ms.openlocfilehash: eaeb81d7f93124f1f3dedf9676314b1b786d8571
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79252993"
---
# <a name="best-practices-for-pod-security-in-azure-kubernetes-service-aks"></a>Pod güvenlik Azure Kubernetes Service (AKS) için en iyi uygulamalar

Geliştirmek ve Azure Kubernetes Service (AKS) uygulamaları çalıştırma gibi podlarınız güvenliğini önemli bir noktadır. Uygulamalarınız, gereken en az sayıda ayrıcalıkların ilkesine göre tasarlanmalıdır. Özel verilerini güvende tutmanın üstünde müşteriler için göz önünde yer alır. Veritabanı bağlantı dizeleri, anahtarlar veya gizlilikler gibi kimlik bilgilerini ve bir saldırganın kötü amaçlı olarak bu gizli dizileri avantajlarından faydalanabilecek dış dünyaya açık sertifikaları istemezsiniz. Bunları kodunuza ekleyin veya kapsayıcı görüntülerinizi katıştırmanız kullanmayın. Bu yaklaşım bir maruz kalma riskini oluşturun ve ancak kapsayıcı görüntülerini yeniden açmanız gerekeceğinden bu kimlik bilgilerini döndürmek için özelliğini sınırlayabilir.

Bu en iyi yöntemler makalesinde, aks 'de Pod 'nin güvenliğini sağlama konusunda odaklanılır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İşlemler ve hizmetler veya ayrıcalık yükseltme saldırısı erişimi sınırlamak için pod güvenlik bağlamını kullanır.
> * Pod yönetilen kimliklerle diğer Azure kaynaklarıyla kimlik doğrulaması
> * Azure Key Vault gibi dijital bir kasa kimlik bilgilerini almak ve istek

[Küme güvenliği][best-practices-cluster-security] ve [kapsayıcı görüntüsü yönetimi][best-practices-container-image-management]için en iyi yöntemleri de okuyabilirsiniz.

## <a name="secure-pod-access-to-resources"></a>Pod kaynaklarına güvenli erişim

**En iyi Yöntem Kılavuzu** -farklı bir kullanıcı veya grup olarak çalıştırmak ve temel alınan düğüm işlemlerine ve hizmetlerine erişimi sınırlandırmak için pod güvenlik bağlamı ayarlarını tanımlayın. En az atama ayrıcalıklarına sayısı.

Uygulamalarınızın doğru çalışması için, Pod 'lerin, *kök*olarak değil, tanımlı kullanıcı veya grup olarak çalışması gerekir. Pod veya kapsayıcı için `securityContext`, uygun izinleri varsaymak üzere *RunAsUser* veya *fsgroup* gibi ayarları tanımlamanızı sağlar. Yalnızca gerekli kullanıcı veya grup izinleri atayın ve güvenlik bağlamı bir şekilde ek izinler varsaymak kullanmayın. *RunAsUser*, ayrıcalık yükseltme ve diğer Linux özellikleri ayarları yalnızca Linux düğümlerinde ve pods 'de kullanılabilir.

Kök olmayan kullanıcı olarak çalıştırdığınızda, kapsayıcılar ayrıcalıklı bağlantı noktalarına altında 1024 bağlanamıyor. Bu senaryoda, Kubernetes Hizmetleri bir uygulamanın belirli bir bağlantı noktası üzerinde çalıştığı olgu gizlemek için kullanılabilir.

Pod güvenlik bağlamı da ek özellikler veya işlemleri ve Hizmetleri erişmek için izinler tanımlayabilirsiniz. Aşağıdaki ortak güvenlik bağlamı tanımları ayarlanabilir:

* **Allowprivilegeilerletme** , Pod 'un *kök* ayrıcalıkları varsayabilir olup olmadığını tanımlar. Bu ayarın her zaman *false*olarak ayarlanması için uygulamalarınızı tasarlayın.
* **Linux özellikleri** , Pod 'un temel alınan düğüm işlemlerine erişmesini sağlar. Bu özellikler atama ile dikkatli olun. En az Ata ayrıcalıkları gerekli sayısı. Daha fazla bilgi için bkz. [Linux özellikleri][linux-capabilities].
* **SELinux etiketleri** , hizmetler, süreçler ve dosya sistemi erişimi için erişim ilkeleri tanımlamanıza olanak sağlayan bir Linux çekirdek güvenlik modülüdür. En az yeniden ata ayrıcalıkları gerekli sayısı. Daha fazla bilgi için bkz. [Kubernetes 'Te SELinux seçenekleri][selinux-labels]

Aşağıdaki örnek pod YAML bildirim güvenlik bağlamı ayarları tanımlamak için ayarlar:

* Pod, Kullanıcı KIMLIĞI *1000* ve grup kimliği *2000* ' nin bir parçası olarak çalışır
* Kullanım ayrıcalıkları `root` için yükseltilemedi
* Ağ arabirimleri ve ana bilgisayarın (Donanım) gerçek zamanlı saatini erişmek Linux özellikleri sağlar

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  containers:
    - name: security-context-demo
      image: nginx:1.15.5
    securityContext:
      runAsUser: 1000
      fsGroup: 2000
      allowPrivilegeEscalation: false
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```

Gereksinim duyduğunuz güvenlik bağlamı ayarları belirlemek için küme işleci ile çalışır. Ek izinler ve erişim pod gerektirir en aza indirmek için uygulamalarınızı tasarlamak deneyin. AppArmor ve küme operatörleri tarafından uygulanabilir (güvenli bilgi işlem) seccomp kullanarak erişimi kısıtlamak için ek güvenlik özellikleri vardır. Daha fazla bilgi için bkz. [kaynaklara yönelik güvenli kapsayıcı erişimi][apparmor-seccomp].

## <a name="limit-credential-exposure"></a>Kimlik bilgisi ifşa sınırı

**En iyi Yöntem Kılavuzu** -uygulama kodunuzda kimlik bilgilerini tanımlamayın. Azure kaynakları için yönetilen kimlikleri pod isteği erişiminizi diğer kaynaklara izin vermek için kullanın. Azure Key Vault gibi dijital bir kasa, depolamak ve dijital anahtarlarını ve kimlik bilgilerini almak için de kullanılmalıdır. Pod tarafından yönetilen kimlikler yalnızca Linux Pod ve kapsayıcı görüntüleri ile kullanılmaya yöneliktir.

Kimlik bilgileri uygulama kodunuzda maruz kalma riskini sınırlamak için sabit ya da paylaşılan kimlik bilgilerini kullanmaktan kaçının. Kimlik bilgilerine veya anahtarlara doğrudan kodunuza eklenmemelidir. Bu kimlik bilgilerinin ifşa edildiği imzalanmasını ve güncelleştirilmiş uygulama gerekir. Kendi kimlik ve kendi kimliğini doğrulamasını veya otomatik olarak dijital bir kasadan kimlik bilgilerini almak için yol pod'ların vermek daha iyi bir yaklaşımdır.

Aşağıdaki [ilişkili AKS açık kaynak projeleri][aks-associated-projects] , dijital bir kasadan otomatik olarak kimlik doğrulama veya kimlik bilgileri ve anahtar istekleri yapmanızı sağlar:

* Yönetilen Azure kaynakları için kimlikleri ve
* Azure anahtar kasası FlexVol sürücüsü

İlişkili AKS açık kaynak projeleri, Azure teknik desteği tarafından desteklenmez. Topluluğumuza geri bildirim ve hata toplamak için sağlanırlar. Bu projeler üretim kullanımı için önerilmez.

### <a name="use-pod-managed-identities"></a>Kullanım pod kimlikler yönetilen

Azure kaynakları için yönetilen bir kimlik, bir pod 'ın kendisini destekleyen, depolama veya SQL gibi Azure hizmetlerinde kimlik doğrulamasını sağlar. Pod, bunları Azure Active Directory kimlik doğrulaması ve dijital belirteç almasını sağlayan bir Azure kimlik atanır. Bu dijital belirteci pod hizmete erişmek ve gerekli eylemleri gerçekleştirmek için yetkili olup olmadığını denetleyen diğer Azure hizmetlerine sunulabilir. Bu yaklaşım, gizli dizi örnek veritabanı bağlantı dizeleri için gerekli olduğu anlamına gelir. Pod yönetilen kimlik için Basitleştirilmiş iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure'da kimlik pod için basitleştirilmiş bir iş akışı yönetilen](media/developer-best-practices-pod-security/basic-pod-identity.png)

Yönetilen bir kimlik ile uygulama kodunuzu Azure depolama gibi bir hizmete erişmek için kimlik bilgilerini eklemek gerekmez. Bu nedenle her pod kendi kimliği ile kimlik doğrulaması gibi denetim ve erişim gözden geçirebilirsiniz. Uygulamanız diğer Azure Hizmetleri ile bağlanıyorsa, kimlik bilgilerini yeniden sınırı ve ifşa riskini yönetilen kimlikleri kullanın.

Pod kimlikleri hakkında daha fazla bilgi için bkz. [BIR AKS kümesini Pod tarafından yönetilen kimlikleri ve uygulamalarınızla birlikte kullanmak Için yapılandırma][aad-pod-identity]

### <a name="use-azure-key-vault-with-flexvol"></a>Azure Key Vault ile FlexVol kullanma

Yönetilen pod kimlikler destekleyici Azure Hizmetleri karşı kimlik doğrulaması yapmak için harika çalışır. Kendi Hizmetleri veya Azure kaynakları için yönetilen kimlikleri olmadan uygulamalar için yine de kimlik bilgilerine veya anahtarlara kullanarak kimlik doğrulaması. Dijital bir kasa, bu kimlik bilgilerini depolamak için kullanılabilir.

Bunlar dijital kasa ile iletişim kurmak uygulamaların bir kimlik bilgisi gerektiğinde, en son kimlik bilgilerini almak ve daha sonra gerekli hizmete bağlanın. Azure Key Vault bu dijital kasası olabilir. Azure Key Vault'taki pod yönetilen kimliklerle bir kimlik bilgisi almak için basitleştirilmiş bir iş akışı aşağıdaki diyagramda gösterilmiştir:

![Yönetilen bir pod kullanarak Key Vault'tan bir kimlik bilgisi almak için basitleştirilmiş bir iş akışı kimliği](media/developer-best-practices-pod-security/basic-key-vault-flexvol.png)

Key Vault ile depolayın ve kimlik bilgilerini, depolama hesabı anahtarlarını veya sertifika gibi gizli dizileri düzenli olarak döndür. Azure Key Vault bir FlexVolume kullanarak bir AKS kümesi ile tümleştirebilirsiniz. FlexVolume sürücü yerel olarak kimlik Key Vault'tan almanız ve güvenli bir şekilde yalnızca isteyen pod'u sağlayacağını AKS kümesi sağlar. Anahtar kasası FlexVol sürücü AKS düğümleri üzerine dağıtmak için küme işleci ile çalışır. Pod yönetilen kimlik, anahtar kasası erişim istemek ve FlexVolume sürücüyü gereken kimlik bilgilerini almak için kullanabilirsiniz.

Esnek olarak Azure Key Vault, Linux Pod ve düğümlerinde çalışan uygulamalar ve hizmetlerle birlikte kullanılmak üzere tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, pod'ların güvenli nasıl odaklanır. Bu alanlardan bazıları uygulamak için aşağıdaki makalelere bakın:

* [AKS ile Azure kaynakları için Yönetilen kimlikler kullanma][aad-pod-identity]
* [Azure Key Vault AKS ile tümleştirme][aks-keyvault-flexvol]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity#demo-pod
[aks-keyvault-flexvol]: https://github.com/Azure/kubernetes-keyvault-flexvol
[linux-capabilities]: http://man7.org/linux/man-pages/man7/capabilities.7.html
[selinux-labels]: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#selinuxoptions-v1-core
[aks-associated-projects]: https://github.com/Azure/AKS/blob/master/previews.md#associated-projects

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[aks-pod-identities]: operator-best-practices-identity.md#use-pod-identities
[apparmor-seccomp]: operator-best-practices-cluster-security.md#secure-container-access-to-resources
