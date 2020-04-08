---
title: Pod güvenlik en iyi uygulamaları
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmetinde (AKS) bölmelerin nasıl güvenli hale gelebildiğini öğrenmek için geliştiricinin en iyi uygulamalarını öğrenin
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: zarhoads
ms.openlocfilehash: 1f093b5276ee7ab334043e57f97a108267c32c87
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804393"
---
# <a name="best-practices-for-pod-security-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) pod güvenliği için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) uygulama geliştirip çalıştırdığınızda, bölmelerinizin güvenliği önemli bir husustur. Uygulamalarınız, gereken en az sayıda ayrıcalık ilkesine göre tasarlanmalıdır. Özel verileri güvende tutmak müşteriler için en üst düzey. Veritabanı bağlantı dizeleri, anahtarlar veya sırlar ve sertifikalar gibi kimlik bilgilerini, saldırganın bu sırlardan kötü amaçlı olarak yararlanabileceği dış dünyaya açık olmasını istemezsiniz. Bunları kodunuza eklemeyin veya kapsayıcı resimlerinize gömmeyin. Bu yaklaşım, pozlama için bir risk oluşturur ve kapsayıcı görüntülerinyeniden oluşturulması gerekeceği için bu kimlik bilgilerini döndürme yeteneğini sınırlandıracaktır.

Bu en iyi uygulamalar makale aks bölmeleri güvenli nasıl odaklanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İşlemlere ve hizmetlere erişimi veya ayrıcalık yükseltmeyi sınırlamak için bölme güvenlik bağlamını kullanın
> * Bölme yönetilen kimlikleri kullanarak diğer Azure kaynaklarıyla kimlik doğrulaması
> * Azure Anahtar Kasası gibi dijital kasadan kimlik bilgilerini isteyin ve alın

Küme [güvenliği][best-practices-cluster-security] ve [kapsayıcı görüntü yönetimi][best-practices-container-image-management]için en iyi uygulamaları da okuyabilirsiniz.

## <a name="secure-pod-access-to-resources"></a>Kaynaklara güvenli pod erişimi

**En iyi uygulama kılavuzu** - Farklı bir kullanıcı veya grup olarak çalıştırmak ve temel düğüm işlemlerine ve hizmetlerine erişimi sınırlamak için pod güvenlik bağlamı ayarlarını tanımlayın. Gereken en az sayıda ayrıcalık atayın.

Uygulamalarınızın doğru çalışması için bölmelerin *kök*olarak değil, tanımlanmış bir kullanıcı veya grup olarak çalışması gerekir. Bir `securityContext` bölme veya kapsayıcı için uygun izinleri almak için *runAsUser* veya *fsGroup* gibi ayarları tanımlamanızı sağlar. Yalnızca gerekli kullanıcı veya grup izinlerini atayın ve güvenlik bağlamını ek izinleri üstlenmek için bir araç olarak kullanmayın. *runAsUser*, ayrıcalık yükseltme ve diğer Linux yetenekleri ayarları yalnızca Linux düğümleri ve bölmelerinde kullanılabilir.

Kök olmayan bir kullanıcı olarak çalıştırdığınızda, kapsayıcılar 1024 altındaki ayrıcalıklı bağlantı noktalarına bağlanamaz. Bu senaryoda, Kubernetes Hizmetleri, bir uygulamanın belirli bir bağlantı noktasında çalıştığı gerçeğini gizlemek için kullanılabilir.

Pod güvenlik bağlamı, işlemlere ve hizmetlere erişmek için ek özellikler veya izinler de tanımlayabilir. Aşağıdaki ortak güvenlik bağlamı tanımları ayarlanabilir:

* **allowPrivilegeEscalation,** bölmenin *kök* ayrıcalıklarını varsayabiliyorsa tanımlar. Uygulamalarınızı, bu ayar her zaman *yanlış*olarak ayarlanması için tasarla.
* **Linux yetenekleri,** bölmenin alttaki düğüm işlemlerine erişmesini sağlar. Bu yetenekleri atamaya dikkat edin. Gereken en az sayıda ayrıcalık atayın. Daha fazla bilgi için [Bkz. Linux özellikleri.][linux-capabilities]
* **SELinux etiketleri,** hizmetler, işlemler ve dosya sistemi erişimi için erişim ilkelerini tanımlamanıza olanak tanıyan bir Linux çekirdeği güvenlik modülüdür. Yine, gereken en az sayıda ayrıcalık atayın. Daha fazla bilgi için [Kubernetes'teki SELinux seçeneklerine][selinux-labels] bakın

Aşağıdaki örnek pod YAML bildirimi tanımlamak için güvenlik bağlamı ayarlarını ayarlar:

* Pod kullanıcı kimliği *1000* ve grup kimliği *2000* parçası olarak çalışır
* Kullanmak için ayrıcalıkları artıramaz`root`
* Linux yeteneklerinin ağ arabirimlerine ve ana bilgisayarın gerçek zamanlı (donanım) saatine erişmesine olanak tanır

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

Hangi güvenlik bağlamı ayarlarına ihtiyacınız olduğunu belirlemek için küme operatörünüzle birlikte çalışın. Ek izinleri en aza indirmek ve bölme nin gerektirdiği neşredilene erişmek için uygulamalarınızı tasarlamaya çalışın. Cluster operatörleri tarafından uygulanabilen AppArmor ve seccomp (güvenli bilgi işlem) kullanarak erişimi sınırlamak için ek güvenlik özellikleri vardır. Daha fazla bilgi için [bkz.][apparmor-seccomp]

## <a name="limit-credential-exposure"></a>Kimlik bilgisi maruziyetini sınırlama

**En iyi uygulama kılavuzu** - Uygulama kodunuzdaki kimlik bilgilerini tanımlamayın. Pod'unuzun diğer kaynaklara erişim istemesine izin vermek için Azure kaynakları için yönetilen kimlikleri kullanın. Azure Key Vault gibi dijital kasa, dijital anahtarları ve kimlik bilgilerini depolamak ve almak için de kullanılmalıdır. Pod yönetilen kimlikler yalnızca Linux bölmeleri ve konteyner görüntüleriyle kullanılmak üzere tasarlanmıştır.

Uygulama kodunuzda kimlik bilgilerinin maruz kalma riskini sınırlamak için, sabit veya paylaşılan kimlik bilgilerinin kullanımından kaçının. Kimlik bilgileri veya anahtarlar doğrudan kodunuza dahil edilmemelidir. Bu kimlik bilgileri açıklanırsa, uygulamanın güncelleştirilip yeniden dağıtılması gerekir. Daha iyi bir yaklaşım, bölmelere kendi kimliklerini ve kimliklerini doğrulamanın yolunu vermek veya kimlik bilgilerini dijital kasadan otomatik olarak almaktır.

Aşağıdaki [ilişkili AKS açık kaynak projeleri,][aks-associated-projects] bölmelerin otomatik olarak doğrulamasını veya dijital kasadan kimlik bilgileri ve anahtarlar istemenizi sağlar:

* Azure kaynakları için yönetilen kimlikler ve
* Azure Key Vault FlexVol sürücüsü

İlişkili AKS açık kaynak projeleri Azure teknik desteği yle desteklenmez. Onlar bizim toplumdan geribildirim ve hata toplamak için sağlanır. Bu projeler üretim kullanımı için önerilmez.

### <a name="use-pod-managed-identities"></a>Bölme yönetilen kimlikleri kullanma

Azure kaynakları için yönetilen bir kimlik, bir bölmenin kendisini depolama veya SQL gibi destekleyen Azure hizmetlerine karşı kimliğini niçin doğrulamasını sağlar. Bölmeye, Azure Etkin Dizin'ine kimlik doğrulamalarını ve dijital belirteç almalarını sağlayan bir Azure Kimliği atanır. Bu dijital belirteç, bölmenin hizmete erişmeye ve gerekli eylemleri gerçekleştirmeye yetkili olup olmadığını kontrol eden diğer Azure hizmetlerine sunulabilir. Bu yaklaşım, veritabanı bağlantı dizeleri için hiçbir sır gerekli olduğu anlamına gelir, örneğin. Pod yönetilen kimlik için basitleştirilmiş iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure'da pod yönetilen kimlik için basitleştirilmiş iş akışı](media/developer-best-practices-pod-security/basic-pod-identity.png)

Yönetilen bir kimliğe sahip olan uygulama kodunuz, Azure Depolama gibi bir hizmete erişmek için kimlik bilgilerini içermelidir. Her bölme nin kendi kimliğiyle kimlik doğrulaması yaptığı ndan, erişimi denetleyebilir ve gözden geçirebilirsiniz. Uygulamanız diğer Azure hizmetlerine bağlanıyorsa, kimlik bilgisi nin yeniden kullanımını ve maruz kalma riskini sınırlamak için yönetilen kimlikleri kullanın.

Pod kimlikleri hakkında daha fazla bilgi için [bkz.][aad-pod-identity]

### <a name="use-azure-key-vault-with-flexvol"></a>FlexVol ile Azure Anahtar Kasası'nı kullanma

Yönetilen bölme kimlikleri, Azure hizmetlerini desteklemeye karşı kimlik doğrulama yapmak için harika çalışır. Azure kaynakları için yönetilen kimlikleri olmayan kendi hizmetleriniz veya uygulamalarınız için kimlik bilgilerini veya anahtarlarını kullanmaya devam emiyorsunuz. Bu kimlik bilgilerini depolamak için dijital kasa kullanılabilir.

Uygulamalar bir kimlik bilgisine ihtiyaç duyduğunda, dijital kasayla iletişim kurar, en son kimlik bilgilerini alır ve gerekli hizmete bağlanır. Azure Key Vault bu dijital kasa olabilir. Pod yönetilen kimlikleri kullanarak Azure Anahtar Kasası'ndan bir kimlik belgesi almak için basitleştirilmiş iş akışı aşağıdaki diyagramda gösterilmiştir:

![Pod yönetilen bir kimlik kullanarak Key Vault'tan kimlik bilgilerini almak için basitleştirilmiş iş akışı](media/developer-best-practices-pod-security/basic-key-vault-flexvol.png)

Key Vault ile kimlik bilgileri, depolama hesabı anahtarları veya sertifikalar gibi sırları düzenli olarak saklar ve düzenli olarak döndürür. FlexVolume kullanarak Azure Key Vault'u aks kümesiyle tümleştirebilirsiniz. FlexVolume sürücüsü, AKS kümesinin anahtar kasasından kimlik bilgilerini yerel olarak almasına ve bunları yalnızca istenen bölmeye güvenli bir şekilde sağlamasına olanak tanır. Key Vault FlexVol sürücüsünü AKS düğümlerine dağıtmak için küme operatörünüzle birlikte çalışın. Anahtar Kasası'na erişim istemek ve FlexVolume sürücüsü aracılığıyla ihtiyacınız olan kimlik bilgilerini almak için pod yönetilen bir kimlik kullanabilirsiniz.

FlexVol'lu Azure Key Vault, Linux bölmeleri ve düğümleri üzerinde çalışan uygulamalar ve hizmetlerle kullanılmak üzere tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bölmelerinizi nasıl güvenli hale getirmek için odaklanılabilen bir makale yer alanmıştır. Bu alanlardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [AKS ile Azure kaynakları için yönetilen kimlikleri kullanma][aad-pod-identity]
* [Azure Anahtar Kasası'nı AKS ile tümleştir][aks-keyvault-flexvol]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity#demo
[aks-keyvault-flexvol]: https://github.com/Azure/kubernetes-keyvault-flexvol
[linux-capabilities]: http://man7.org/linux/man-pages/man7/capabilities.7.html
[selinux-labels]: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#selinuxoptions-v1-core
[aks-associated-projects]: https://github.com/Azure/AKS/blob/master/previews.md#associated-projects

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[aks-pod-identities]: operator-best-practices-identity.md#use-pod-identities
[apparmor-seccomp]: operator-best-practices-cluster-security.md#secure-container-access-to-resources
