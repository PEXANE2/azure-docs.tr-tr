---
title: Kavramlar - Azure Kubernetes Hizmetlerinde Depolama (AKS)
description: Hacimler, kalıcı birimler, depolama sınıfları ve talepler dahil olmak üzere Azure Kubernetes Hizmetinde (AKS) depolama hakkında bilgi edinin
services: container-service
ms.topic: conceptual
ms.date: 03/01/2019
ms.openlocfilehash: 4bb19d7da971a82aef9c0e1fc092cc648ac49c4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596003"
---
# <a name="storage-options-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) uygulamaları için depolama seçenekleri

Azure Kubernetes Hizmeti'nde (AKS) çalışan uygulamaların veri depolaması ve alması gerekebilir. Bazı uygulama iş yükleri için bu veri depolama, bakla silindiğinde artık gerekli olmayan düğümüzerinde yerel, hızlı depolama kullanabilir. Diğer uygulama iş yükleri, Azure platformunda daha düzenli veri birimlerinde kalıcı depolama gerektirebilir. Pod farklı bir düğüm üzerinde yeniden zamanlanmışsa, birden çok bölmenin aynı veri birimlerini paylaşması veya veri birimlerini yeniden eklemesi gerekebilir. Son olarak, hassas verileri veya uygulama yapılandırma bilgilerini bölmelere enjekte etmeniz gerekebilir.

![Azure Kubernetes Hizmetleri (AKS) kümesindeki uygulamalar için depolama seçenekleri](media/concepts-storage/aks-storage-options.png)

Bu makalede, AKS uygulamalarınıza depolama sağlayan temel kavramlar tanıtışlar:

- [Birim](#volumes)
- [Kalıcı birimler](#persistent-volumes)
- [Depolama sınıfları](#storage-classes)
- [Kalıcı birim talepleri](#persistent-volume-claims)

## <a name="volumes"></a>Birimler

Uygulamaların genellikle veri depolayıp alabilmeleri gerekir. Kubernetes genellikle tek tek kullanımlık kaynaklar olarak ele aldığı için, uygulamaların verileri kullanması ve gerektiğinde devam etmesi için farklı yaklaşımlar mevcuttur. *Birim,* bölmeler arasında ve uygulama yaşam döngüsü nde verileri depolamanın, almanın ve kalıcı olarak saklamanın bir yolunu temsil eder.

Verileri depolamak ve almak için geleneksel birimler, Azure Depolama tarafından desteklenen Kubernetes kaynakları olarak oluşturulur. Bu veri hacimlerini doğrudan bölmelere atanacak şekilde el ile oluşturabilir veya Kubernetes'in bunları otomatik olarak oluşturmasını sağlayabilirsiniz. Bu veri birimleri Azure Diskleri veya Azure Dosyaları kullanabilir:

- *Azure Diskleri* bir Kubernetes *DataDisk* kaynağı oluşturmak için kullanılabilir. Diskler, yüksek performanslı SSD'ler tarafından desteklenen Azure Premium depolama alanını veya normal HDD'ler tarafından desteklenen Azure Standart depolama alanını kullanabilir. Çoğu üretim ve geliştirme iş yükü için Premium depolama alanını kullanın. Azure Diskler *ReadWriteOnce*olarak monte edilir, bu nedenle yalnızca tek bir bölme için kullanılabilir. Aynı anda birden çok bölmeyle erişilebilen depolama birimleri için Azure Dosyaları'nı kullanın.
- *Azure Dosyaları,* Bir Azure Depolama hesabı tarafından desteklenen bir SMB 3.0 paylaşımını bölmelere takmak için kullanılabilir. Dosyalar, verileri birden çok düğüm ve bölme arasında paylaşmanıza izin eder. Dosyalar, normal HDD'ler tarafından desteklenen Azure Standart depolama alanını veya yüksek performanslı SSD'ler tarafından desteklenen Azure Premium depolama alanını kullanabilir.
> [!NOTE] 
> Azure Dosyaları, Kubernetes 1,13 veya daha yüksek çalıştıran AKS kümelerinde premium depolamayı destekler.

Kubernetes'te birimler, bilgilerin depolanabileceği ve alınabildiği geleneksel bir diskten daha fazlasını temsil edebilir. Kubernetes hacimleri, kapsayıcılar tarafından kullanılmak üzere bir bölmeye veri enjekte etmenin bir yolu olarak da kullanılabilir. Kubernetes'te yaygın ek birim türleri şunlardır:

- *emptyDir* - Bu birim genellikle bir bölme için geçici alan olarak kullanılır. Bir bölmeiçindeki tüm kapsayıcılar birimdeki verilere erişebilir. Bu birim türüne yazılan veriler yalnızca bölmenin ömrü boyunca kalır - bölme silindiğinde, birim silinir. Bu birim genellikle altta yatan yerel düğüm disk depolama kullanır, ancak yalnızca düğümün belleğinde de bulunabilir.
- *gizli* - Bu birim, parolalar gibi bölmelere hassas verileri enjekte etmek için kullanılır. Önce Kubernetes API'sini kullanarak bir Gizli oluşturursunuz. Bölmenizi veya dağıtımınızı tanımladığınızda, belirli bir Gizli istenebilir. Sırlar yalnızca bunu gerektiren zamanlanmış bir bölmeye sahip düğümlere sağlanır ve Gizli, diske yazılmayan *tmpfs'de*depolanır. Gizli gerektiren bir düğümdeki son bölme silindiğinde, Gizli düğümün tmpfs'sinden silinir. Sırlar belirli bir ad alanı içinde depolanır ve yalnızca aynı ad alanı içindeki bölmeler tarafından erişilebilir.
- *configMap* - Bu birim türü, uygulama yapılandırma bilgileri gibi bölmelere anahtar değeri çifti özelliklerini enjekte etmek için kullanılır. Kapsayıcı görüntüsünde uygulama yapılandırma bilgilerini tanımlamak yerine, bunu kolayca güncelleştirilebilen ve dağıtılırken yeni bölme örneklerine uygulanabilen bir Kubernetes kaynağı olarak tanımlayabilirsiniz. Gizli kullanmak gibi, önce Kubernetes API'sini kullanarak bir ConfigMap oluşturursunuz. Bu ConfigMap daha sonra bir bölme veya dağıtım tanımladığınızda istenebilir. ConfigMaps belirli bir ad alanı içinde saklanır ve yalnızca aynı ad alanı içinde bölmeler tarafından erişilebilir.

## <a name="persistent-volumes"></a>Kalıcı birimler

Pod yaşam döngüsünün bir parçası olarak tanımlanan ve oluşturulan birimler yalnızca bölme silinene kadar bulunur. Podlar genellikle bir bölme, özellikle StatefulSets'te bir bakım olayı sırasında farklı bir ana bilgisayarda yeniden zamanlanırsa depolama alanlarının kalmasını bekler. *Kalıcı birim* (PV), Kubernetes API tarafından oluşturulan ve yönetilen ve tek bir bölmenin ömrü dışında var olabilecek bir depolama kaynağıdır.

Kalıcı Birim'i sağlamak için Azure Diskleri veya Dosyaları kullanılır. Birimler'in önceki bölümünde belirtildiği gibi, Disk veya Dosya seçimi genellikle verilere veya performans katmanına eşzamanlı erişim gereksinimine göre belirlenir.

![Azure Kubernetes Hizmetleri (AKS) kümesinde kalıcı birimler](media/concepts-storage/persistent-volumes.png)

PersistentVolume statik *bir* küme yöneticisi tarafından oluşturulabilir veya Dinamik *Olarak* Kubernetes API sunucusu tarafından oluşturulabilir. Bir bölme zamanlanmışsa ve şu anda kullanılamayan depolama yı talep ederse, Kubernetes altta yatan Azure Disk veya Dosyalar depolama alanını oluşturabilir ve bölmeye ekleyebilir. Dinamik sağlama, ne tür bir Azure depolama alanı oluşturulması gerektiğini belirlemek için bir *StorageClass* kullanır.

## <a name="storage-classes"></a>Depolama sınıfları

Premium ve Standart gibi farklı depolama katmanları tanımlamak için bir *StorageClass*oluşturabilirsiniz. StorageClass ayrıca geri *alma Politikası'nı*da tanımlar. Bu geri alma Politikası, bölme silindiğinde ve kalıcı birim artık gerekmediğinde, temel Azure depolama kaynağının davranışını denetler. Temel depolama kaynağı silinebilir veya gelecekteki bir bölmeyle kullanılmak üzere saklanabilir.

AKS'de iki ilk Depolama Sınıfı oluşturulur:

- *varsayılan* - Yönetilen Disk oluşturmak için Azure Standart depolama alanını kullanır. Geri alma ilkesi, kullanılan kalıcı birim silindiğinde temel Azure Diskinin silindiğini gösterir.
- *yönetilen premium* - Yönetilen Disk'i oluşturmak için Azure Premium depolama alanını kullanır. Geri alma ilkesi, kullanılan kalıcı birim silindiğinde temel Azure Diskinin silindiğini yine gösterir.

Kalıcı bir birim için StorageClass belirtilmemişse, varsayılan StorageClass kullanılır. İhtiyacınız olan uygun depolama alanını kullanmaları için kalıcı hacimler isterken dikkatli olun. Ek ihtiyaçlar için bir StorageClass oluşturabilirsiniz. `kubectl` Aşağıdaki örnekte Premium Yönetilen Diskler kullanılır ve bölme silindiğinde temel Azure Diskinin *korunması* gerektiğini belirtir:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: managed-premium-retain
provisioner: kubernetes.io/azure-disk
reclaimPolicy: Retain
parameters:
  storageaccounttype: Premium_LRS
  kind: Managed
```

## <a name="persistent-volume-claims"></a>Kalıcı birim talepleri

PersistentVolumeClaim, belirli bir StorageClass'ın Disk veya Dosya depolamasını, erişim modunu ve boyutunu ister. Kubernetes API sunucusu, tanımlanan StorageClass'ı temel alan talebi yerine getirmek için varolan bir kaynak yoksa Azure'daki temel depolama kaynağını dinamik olarak sağlayabilir. Bölme tanımı, birim bölmeye bağlandıktan sonra ses montajını içerir.

![Azure Kubernetes Hizmetleri (AKS) kümesinde kalıcı birim talepleri](media/concepts-storage/persistent-volume-claims.png)

Kalıcı Birim, kullanılabilir bir depolama kaynağı isteğiolan bölmeye atandıktan sonra Kalıcı Birim Talebine *bağlanır.* Taleplere kalıcı birimlerin 1:1 eşlemi vardır.

Aşağıdaki örnek YAML bildirimi, *yönetilen premium* StorageClass'ı kullanan ve disk 5Gi boyutunda bir Disk *5Gi* isteyen kalıcı bir topluluk iddiasını gösterir:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

Bir pod tanımı oluşturduğunuzda, kalıcı birim talebi istenen depolama istemek için belirtilir. Ayrıca, uygulamalarınızın veri okuması ve yazması için *ciltMount'u* da belirtin. Aşağıdaki örnek YAML bildirimi, önceki kalıcı birim talebinin */mnt/azure'a*bir birim monte etmek için nasıl kullanılabileceğini gösterir:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar [için, AKS'deki depolama ve yedekleme ler için en iyi uygulamalara][operator-best-practices-storage]bakın.

Azure Diskleri veya Azure Dosyaları kullanan dinamik ve statik birimlerin nasıl oluşturulup oluşturultamalarını görmek için aşağıdaki nasıl yapılBilen makalelere bakın:

- [Azure Diskleri kullanarak statik bir birim oluşturma][aks-static-disks]
- [Azure Dosyalarını kullanarak statik bir birim oluşturma][aks-static-files]
- [Azure Diskleri kullanarak dinamik bir birim oluşturma][aks-dynamic-disks]
- [Azure Dosyalarını kullanarak dinamik bir birim oluşturma][aks-dynamic-files]

Çekirdek Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümeleri ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS kimliği][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS ölçeği][aks-concepts-scale]

<!-- EXTERNAL LINKS -->

<!-- INTERNAL LINKS -->
[aks-static-disks]: azure-disk-volume.md
[aks-static-files]: azure-files-volume.md
[aks-dynamic-disks]: azure-disks-dynamic-pv.md
[aks-dynamic-files]: azure-files-dynamic-pv.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-storage]: operator-best-practices-storage.md
