---
title: Geliştirici en iyi uygulamalar-Azure Kubernetes Hizmetleri 'nde (AKS) Pod güvenliği
description: Azure Kubernetes Service 'te (aks) Pod 'nin güvenliğini sağlama hakkında geliştirici için en iyi uygulamaları öğrenin
services: container-service
author: zr-msft
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: zarhoads
ms.openlocfilehash: 3a62dcbbec90ec73ded722a6efbbd5907fb21f9f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84674049"
---
# <a name="best-practices-for-pod-security-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ' de Pod güvenliği için en iyi uygulamalar

Azure Kubernetes Service 'te (AKS) uygulama geliştirip çalıştırırken, ayırımlarınızın güvenliği önemli bir konudur. Uygulamalarınız, gereken en az sayıda ayrıcalıkların ilkesine göre tasarlanmalıdır. Özel verilerin güvende tutulması, müşteriler için göz önünde bulundurmaktan en iyi seçenektir. Veritabanı bağlantı dizeleri, anahtarlar veya gizlilikler gibi kimlik bilgilerini ve bir saldırganın kötü amaçlı olarak bu gizli dizileri avantajlarından faydalanabilecek dış dünyaya açık sertifikaları istemezsiniz. Bunları kodunuza eklemeyin veya kapsayıcı yansımalarına gömmeyin. Bu yaklaşım, kapsayıcı görüntülerinin yeniden oluşturulması gerektiği için bu kimlik bilgilerini döndürme olasılığını ortadan kaldırabilecek bir risk oluşturur.

Bu en iyi yöntemler makalesinde, aks 'de Pod 'nin güvenliğini sağlama konusunda odaklanılır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Süreçler ve hizmetler veya ayrıcalık yükseltme işlemlerine erişimi sınırlandırmak için pod güvenlik bağlamını kullanın
> * Pod tarafından yönetilen kimlikleri kullanarak diğer Azure kaynaklarıyla kimlik doğrulama
> * Azure Key Vault gibi bir dijital kasadan kimlik bilgilerini isteyin ve alın

[Küme güvenliği][best-practices-cluster-security] ve [kapsayıcı görüntüsü yönetimi][best-practices-container-image-management]için en iyi yöntemleri de okuyabilirsiniz.

## <a name="secure-pod-access-to-resources"></a>Kaynaklara güvenli Pod erişimi

**En iyi Yöntem Kılavuzu** -farklı bir kullanıcı veya grup olarak çalıştırmak ve temel alınan düğüm işlemlerine ve hizmetlerine erişimi sınırlandırmak için pod güvenlik bağlamı ayarlarını tanımlayın. Gereken en az ayrıcalık sayısını atayın.

Uygulamalarınızın doğru çalışması için, Pod 'lerin, *kök*olarak değil, tanımlı kullanıcı veya grup olarak çalışması gerekir. `securityContext`Pod veya Container için, uygun izinleri varsaymak üzere *RunAsUser* veya *fsgroup* gibi ayarları tanımlamanızı sağlar. Yalnızca gerekli Kullanıcı veya grup izinlerini atayın ve ek izinleri varsaymak için bir yol olarak güvenlik bağlamını kullanmayın. *RunAsUser*, ayrıcalık yükseltme ve diğer Linux özellikleri ayarları yalnızca Linux düğümlerinde ve pods 'de kullanılabilir.

Kök olmayan bir kullanıcı olarak çalıştırdığınızda, kapsayıcılar 1024 altındaki ayrıcalıklı bağlantı noktalarına bağlanamaz. Bu senaryoda, Kubernetes Hizmetleri, bir uygulamanın belirli bir bağlantı noktası üzerinde çalıştığı gerçeğini gizleyebilen şekilde kullanılabilir.

Pod güvenlik bağlamı, işlemlere ve hizmetlere erişim için ek yetenekler veya izinler de tanımlayabilir. Aşağıdaki ortak güvenlik bağlamı tanımları ayarlanabilir:

* **Allowprivilegeilerletme** , Pod 'un *kök* ayrıcalıkları varsayabilir olup olmadığını tanımlar. Bu ayarın her zaman *false*olarak ayarlanması için uygulamalarınızı tasarlayın.
* **Linux özellikleri** , Pod 'un temel alınan düğüm işlemlerine erişmesini sağlar. Bu özellikleri atamaya özen gösterin. Gereken en az ayrıcalık sayısını atayın. Daha fazla bilgi için bkz. [Linux özellikleri][linux-capabilities].
* **SELinux etiketleri** , hizmetler, süreçler ve dosya sistemi erişimi için erişim ilkeleri tanımlamanıza olanak sağlayan bir Linux çekirdek güvenlik modülüdür. Yine, gereken en az ayrıcalık sayısını atayın. Daha fazla bilgi için bkz. [Kubernetes 'Te SELinux seçenekleri][selinux-labels]

Aşağıdaki örnek Pod YAML bildirimi, güvenlik bağlamı ayarlarını belirlemek için ayarlar:

* Pod, Kullanıcı KIMLIĞI *1000* ve grup kimliği *2000* ' nin bir parçası olarak çalışır
* Kullanım ayrıcalıkları yükseltilemedi`root`
* Linux yeteneklerinin ağ arabirimlerine ve konağın gerçek zamanlı (donanım) saate erişmesine izin verir

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

Hangi güvenlik bağlamı ayarlarına ihtiyacınız olduğunu belirlemek için küme işleçle çalışın. Diğer izinleri en aza indirmek için uygulamalarınızı tasarlamayı deneyin ve pod 'un gerektirdiği erişimi yapın. AppArmor ve seccomp (güvenli bilgi işlem) kullanarak erişimi, küme işleçleri tarafından uygulanabilecek şekilde sınırlamak için ek güvenlik özellikleri vardır. Daha fazla bilgi için bkz. [kaynaklara yönelik güvenli kapsayıcı erişimi][apparmor-seccomp].

## <a name="limit-credential-exposure"></a>Kimlik bilgisi pozlamasını sınırla

**En iyi Yöntem Kılavuzu** -uygulama kodunuzda kimlik bilgilerini tanımlamayın. Pod 'nizin diğer kaynaklara erişmesine izin vermek için Azure kaynakları için Yönetilen kimlikler kullanın. Dijital anahtar ve kimlik bilgilerini depolamak ve almak için Azure Key Vault gibi bir dijital kasa da kullanılmalıdır. Pod tarafından yönetilen kimlikler yalnızca Linux Pod ve kapsayıcı görüntüleri ile kullanılmaya yöneliktir.

Uygulama kodunuzda açığa çıkarılan kimlik bilgileri riskini sınırlamak için, sabit veya paylaşılan kimlik bilgilerinin kullanılmasını önleyin. Kimlik bilgileri veya anahtarlar doğrudan kodunuza dahil edilmemelidir. Bu kimlik bilgileri açığa çıkarılabiliyorsa, uygulamanın güncelleştirilmesi ve yeniden dağıtılması gerekir. Daha iyi bir yaklaşım, kendi kimliklerini doğrulamak veya dijital bir kasadan kimlik bilgilerini otomatik olarak almak için bir yol vermektir.

### <a name="use-azure-container-compute-upstream-projects"></a>Azure Kapsayıcı Işlem yukarı akış projelerini kullanma

> [!IMPORTANT]
> İlişkili AKS açık kaynak projeleri, Azure teknik desteği tarafından desteklenmez. Bunlar, kullanıcıların kümeye kendi kendine yüklemeleri ve topluluğumuza geri bildirim toplamaları için sağlanır.

Aşağıdaki [ilişkili AKS açık kaynak projeleri][aks-associated-projects] , dijital bir kasadan otomatik olarak kimlik doğrulama veya kimlik bilgileri ve anahtar istekleri yapmanızı sağlar. Bu projeler Azure Kapsayıcı Işlem yukarı akış ekibi tarafından saklanır ve [kullanılabilecek daha geniş bir proje listesi](https://github.com/Azure/container-compute-upstream/blob/master/README.md#support)parçasıdır.

 * [Azure Active Directory Pod kimliği][aad-pod-identity]
 * [Gizli anahtar deposu için Azure Key Vault sağlayıcısı CSı sürücüsü](https://github.com/Azure/secrets-store-csi-driver-provider-azure#usage)

#### <a name="use-pod-managed-identities"></a>Pod tarafından yönetilen kimlikleri kullanma

Azure kaynakları için yönetilen bir kimlik, bir pod 'ın kendisini destekleyen, depolama veya SQL gibi Azure hizmetlerinde kimlik doğrulamasını sağlar. Pod 'a Azure Active Directory kimlik doğrulaması yapan bir Azure kimliği atanır ve bir dijital belirteç alırlar. Bu dijital belirteç, Pod 'ın hizmete erişme yetkisi olup olmadığını denetleyen ve gerekli eylemleri gerçekleştiren diğer Azure hizmetlerine sunulabilir. Bu yaklaşım, örneğin veritabanı bağlantı dizeleri için gizli dizi gerekmediği anlamına gelir. Pod yönetilen kimliği için Basitleştirilmiş iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure 'da Pod yönetilen kimliği için Basitleştirilmiş iş akışı](media/developer-best-practices-pod-security/basic-pod-identity.png)

Yönetilen bir kimlikle, uygulama kodunuzun Azure depolama gibi bir hizmete erişmek için kimlik bilgilerini içermesi gerekmez. Her pod kendi kimliğiyle kimlik doğruladığında, erişimi denetleyebilir ve gözden geçirebilirsiniz. Uygulamanız diğer Azure hizmetleriyle bağlanıyorsa, kimlik bilgilerinin yeniden kullanımını ve etkilenme riskini sınırlamak için Yönetilen kimlikler kullanın.

Pod kimlikleri hakkında daha fazla bilgi için bkz. [BIR AKS kümesini Pod tarafından yönetilen kimlikleri ve uygulamalarınızla birlikte kullanmak Için yapılandırma][aad-pod-identity]

#### <a name="use-azure-key-vault-with-secrets-store-csi-driver"></a>Gizli dizi ile Azure Key Vault kullanma CSı sürücüsü

Pod Identity projesi kullanılması, Azure hizmetlerini desteklemeye karşı kimlik doğrulaması yapılmasını mümkün değildir. Azure kaynakları için yönetilen kimlikleri olmayan kendi hizmetleriniz veya uygulamalarınız için kimlik bilgilerini veya anahtarları kullanarak kimlik doğrulaması yapabilirsiniz. Bu gizli dizi içeriklerini depolamak için bir dijital kasa kullanılabilir.

Uygulamalar bir kimlik bilgisine ihtiyaç duyduklarında, dijital kasada iletişim kurar, en son gizli içeriği alır ve ardından gerekli hizmete bağlanır. Azure Key Vault bu dijital kasa olabilir. Pod tarafından yönetilen kimlikleri kullanarak Azure Key Vault bir kimlik bilgisi almaya yönelik Basitleştirilmiş iş akışı aşağıdaki diyagramda gösterilmiştir:

![Pod yönetilen kimliği kullanarak Key Vault kimlik bilgisini almak için Basitleştirilmiş iş akışı](media/developer-best-practices-pod-security/basic-key-vault.png)

Key Vault ile kimlik bilgileri, depolama hesabı anahtarları veya sertifikalar gibi gizli dizileri depolar ve düzenli olarak döndürebilirsiniz. [Gizli dizi için Azure Key Vault sağlayıcısını](https://github.com/Azure/secrets-store-csi-driver-provider-azure#usage)kullanarak bir aks kümesiyle Azure Key Vault tümleştirebilirsiniz. Gizli dizileri deposunun CSı sürücüsü, AKS kümesinin gizli içeriği Key Vault 'tan yerel olarak almasını ve yalnızca istekte bulunan Pod 'e güvenli bir şekilde sağlamasını sağlar. Gizli dizileri, AKS çalışan düğümlerine bağlamak için küme işleçle birlikte çalışın. Key Vault erişim istemek ve gizli dizi depolama CSı sürücüsü aracılığıyla gereken gizli içeriği almak için pod tarafından yönetilen bir kimlik kullanabilirsiniz.

Gizli dizileri içeren Azure Key Vault, bir Kubernetes sürümü 1,16 veya daha büyük olan Linux düğümleri ve düğüm 'lar için kullanılabilir. Windows düğümleri ve pod için 1,18 veya üzeri bir Kubernetes sürümü gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ayırımlarınızın güvenliğini sağlama konusunda odaklanılmıştır. Bu alanlardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [AKS ile Azure kaynakları için Yönetilen kimlikler kullanma][aad-pod-identity]
* [Azure Key Vault AKS ile tümleştirme][aks-keyvault-csi-driver]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity#demo
[aks-keyvault-csi-driver]: https://github.com/Azure/secrets-store-csi-driver-provider-azure#usage
[linux-capabilities]: http://man7.org/linux/man-pages/man7/capabilities.7.html
[selinux-labels]: https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#selinuxoptions-v1-core
[aks-associated-projects]: https://github.com/Azure/AKS/blob/master/previews.md#associated-projects

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-container-image-management]: operator-best-practices-container-image-management.md
[aks-pod-identities]: operator-best-practices-identity.md#use-pod-identities
[apparmor-seccomp]: operator-best-practices-cluster-security.md#secure-container-access-to-resources