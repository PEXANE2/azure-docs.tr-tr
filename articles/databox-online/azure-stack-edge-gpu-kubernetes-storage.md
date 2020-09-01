---
title: Azure Stack Edge cihazında Kubernetes depolama yönetimini anlayın | Microsoft Docs
description: Kubernetes depolama yönetiminin Azure Stack Edge cihazında nasıl oluştuğunu açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/27/2020
ms.author: alkohli
ms.openlocfilehash: 57574b66ddb20e592a5979a4b827347f7c8e09af
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268100"
---
# <a name="kubernetes-storage-management-on-your-azure-stack-edge-gpu-device"></a>Azure Stack Edge GPU cihazınızda Kubernetes depolama yönetimi

Azure Stack Edge cihazınızda, işlem rolünü yapılandırırken bir Kubernetes kümesi oluşturulur. Kubernetes kümesi oluşturulduktan sonra kapsayıcılı uygulamalar, pods 'deki Kubernetes kümesine dağıtılabilir. Kubernetes kümenizdeki yığınlara depolama sağlamaya yönelik ayrı yollar vardır. 

Bu makalede, genel olarak bir Kubernetes kümesinde depolama sağlama yöntemleri ve özellikle de Azure Stack Edge cihazınızın bağlamı açıklanmaktadır. 

## <a name="storage-requirements-for-kubernetes-pods"></a>Kubernetes pod için depolama gereksinimleri

Kubernetes Pod durumsuz, ancak çalıştırdıkları uygulamalar genellikle durum bilgisi değildir. Pod kısa süreli olabilir ve yeniden başlatma, yük devretme veya Kubernetes düğümleri arasında geçiş yaptığından, Pod ile ilişkili depolama için aşağıdaki gereksinimlerin karşılanması gerekir. 

Depolama alanı şu şekilde olmalıdır:

- Pod dışında canlı.
- Pod yaşam döngüsünün bağımsız olması.
- Tüm Kubernetes düğümlerinden erişilebilir olmalıdır.

Depolama alanının Kubernetes için nasıl yönetildiğini anlamak için, bir iki API kaynağını anlamanız gerekir: 

- **Persistentvolume (BD)**: Bu, Kubernetes kümesindeki bir depolama parçasıdır. Kubernetes depolaması, statik olarak sağlanabilir `PersistentVolume` . Ayrıca, olarak dinamik olarak sağlanabilir  `StorageClass` .

- **Persistentvolumeclaim (PVC)**: Bu, bir kullanıcı tarafından bir depolama isteğidir. PVC 'ler BD kaynaklarını kullanır. PVC 'ler, belirli boyut ve erişim modları isteyebilir. 

    Kullanıcılar `PersistentVolumes` farklı sorunlar için değişen özelliklere ihtiyaç duyduğundan, küme yöneticilerinin `PersistentVolumes` yalnızca boyut ve erişim modlarında farklı şekillerde farklılık gösterebilmesi gerekir. Bu gereksinimler için kaynağa ihtiyacınız vardır `StorageClass` .

Depolama sağlama statik veya dinamik olabilir. Sağlama türlerinin her biri, aşağıdaki bölümlerde ele alınmıştır.

## <a name="staticprovisioning"></a>Statik sağlama

Kubernetes küme yöneticileri, depolamayı statik olarak sağlayabilir. Bunu yapmak için, SMB/NFS dosya sistemlerine göre depolama arka ucu kullanabilir veya şirket içi bir ortamda ağ üzerinden yerel olarak eklenen Iscsı disklerini kullanabilir ya da Bulutta Azure dosyalarını veya Azure disklerini kullanabilirsiniz. Bu depolama türü varsayılan olarak sağlanmaz ve Cluster Admins 'in bu sağlamayı planlayıp yönetmesi gerekir. 
 
İşte, Kubernetes 'de statik olarak sağlanan depolamanın nasıl tüketildiğini gösteren bir diyagram. 

![PersistentVolumes aracılığıyla statik sağlama](./media/azure-stack-edge-gpu-kubernetes-storage/static-provisioning-persistent-volumes-1.png)

Aşağıdaki adımlar oluşur: 

1. **Depolama sağlama**: Küme Yöneticisi depolamayı sağlar. Bu örnekte, Küme Yöneticisi, bu paylaşımlara karşılık gelen Kubernetes kümesinde kalıcı birim nesnelerini otomatik olarak oluşturan bir veya daha fazla SMB paylaşımı oluşturur. 

1. **Talep depolama**: depolama ısteyen bir PVC dağıtımı gönderebilirsiniz. Bu depolama talebi, PersistentVolumeClaim (PVC) ' dir. BD 'in boyutu ve erişim modu PVC ile eşleşiyorsa, PVC, BD 'e bağlanır. PVC ve BD bire bir eşleştirin.

1. **PVC 'yi kapsayıcıya bağlama**: PVC, BD 'e bağlandıktan sonra, bu PVC 'yi kapsayıcıda bir yola bağlayabilirsiniz. Kapsayıcıdaki uygulama mantığı bu yoldan/içine okurken/yazarken, veriler SMB deposuna yazılır.
 

## <a name="dynamicprovisioning"></a>Dinamik sağlama

İşte, Kubernetes 'de statik olarak sağlanan depolamanın nasıl tüketildiğini gösteren bir diyagram. 

![StorageClasses aracılığıyla dinamik sağlama](./media/azure-stack-edge-gpu-kubernetes-storage/dynamic-provisioning-storage-classes-1.png)

Aşağıdaki adımlar oluşur: 

1. **Depolama sınıfını tanımlama**: Küme Yöneticisi, Kubernetes kümeniz için işletim ortamına bağlı olarak bir depolama sınıfı tanımlar. Küme Yöneticisi ayrıca, Kubernetes kümesinde dağıtılan başka bir pod veya uygulama olan bir provisioner dağıtır. Sağlayıcısı oluştur, paylaşımları dinamik olarak sağlamak için tüm ayrıntılara sahiptir.  

1. **Talep depolama**: depolama alanı talep eden bir uygulama gönderir. Bu depolama sınıfı başvurusuyla bir PVC oluşturulduktan sonra, sağlayıcısı oluştur çağrılır. 

1. **Depolama alanını dinamik olarak sağlayın**: sağlayıcısı oluştur, yerel disk depolamayla ilişkili olan paylaşımın dinamik olarak oluşturur. Paylaşma oluşturulduktan sonra, bu paylaşıma karşılık gelen otomatik olarak bir BD nesnesi de oluşturur.

1. **PVC 'yi kapsayıcıya bağlama**: PVC, BD 'e bağlandıktan sonra, PVC 'yi statik sağlama ve paylaşıma okuma ya da yazma ile aynı şekilde bir yola bağlayabilirsiniz.


## <a name="storage-provisioning-on-azure-stack-edge"></a>Azure Stack Edge üzerinde depolama sağlama

Azure Stack Edge cihazında, statik olarak sağlanan `PersistentVolumes` cihaz depolama özellikleri kullanılarak oluşturulur. Bir paylaşma sağladığınızda ve **kenar işlem ile paylaşma** seçeneği etkinken bu eylem, Kubernetes kümesinde otomatik olarak bir BD kaynağı oluşturur.

![Statik sağlama için Azure portal yerel paylaşma oluşturma](./media/azure-stack-edge-gpu-kubernetes-storage/static-provisioning-azure-portal-2.png)

Bulut katmanlaması kullanmak için, Edge işlem ile paylaşma seçeneğinin etkin olduğu bir uç bulut paylaşma oluşturabilirsiniz. Bu paylaşımda otomatik olarak bir BD oluşturulur. Edge paylaşımında yazdığınız herhangi bir uygulama verisi, buluta katmanlıdır. 

![Statik sağlama için Azure portal bulut paylaşımının oluşturulması](./media/azure-stack-edge-gpu-kubernetes-storage/static-provisioning-azure-portal-1.png)

PVs 'yi Azure Stack Edge cihazını statik olarak sağlamak için hem SMB hem de NFS paylaşımları oluşturabilirsiniz. BD sağlandıktan sonra, bu depolama alanını talep etmek için bir PVC gönderilir. Aşağıda, `yaml` depolamayı talep eden ve sağladığınız paylaşımları kullanan BIR PVC dağıtımına örnek verilmiştir.


```yml
kind: PersistentVolumeClaim 
apiVersion: v1 
metadata: 
  name: pvc-smb-flexvol 
spec: 
  accessModes: 
  - ReadWriteMany 
  resources: 
    requests: 
      storage: 10Gi 
  volumeName: <nfs-or-smb-share-name-here> 
  storageClassName: ""
```

Daha fazla bilgi için bkz. [kubectl aracılığıyla Azure Stack kenarınızda statik sağlama aracılığıyla durum bilgisi olan bir uygulama dağıtma](azure-stack-edge-gpu-deploy-stateful-application-static-provision-kubernetes.md).

Azure Stack Edge `StorageClass` `ase-node-local` , Kubernetes düğümüne iliştirilmiş bir veri diski depolaması kullanan bir yerleşik olarak da bulunur. Bu, `StorageClass` dinamik sağlamayı destekler. `StorageClass`Pod uygulamalarında bir başvuru yapabilirsiniz ve bır BD sizin için otomatik olarak oluşturulur. Daha fazla bilgi için bkz. sorgu için [Kubernetes panosu](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md) `ase-node-local StorageClass` .

![Kubernetes panosundaki yerleşik depolama sınıfı](./media/azure-stack-edge-gpu-kubernetes-storage/dynamic-provisioning-builtin-storage-class-1.png)

Daha fazla bilgi için bkz. [kuebctl aracılığıyla Azure Stack kenarınızda dinamik sağlama aracılığıyla durum bilgisi içeren bir uygulama dağıtma](azure-stack-edge-gpu-deploy-stateful-application-dynamic-provision-kubernetes.md).

## <a name="choose-storage-type"></a>Depolama türünü seçin

Dağıttığınız iş yüküne bağlı olarak depolama türünü seçmeniz gerekebilir. 

- `ReadWriteMany` `PersistentVolumes` Birimlerin birçok düğüm tarafından okuma-yazma olarak bağlanması için erişim modunu ISTIYORSANıZ, SMB/NFS paylaşımları için statik sağlama kullanın.

- Dağıttığınız uygulamaların, örneğin MongoDB, PostgreSQL, MySQL veya promeus gibi uygulamalar gibi bir POSIX uyumluluk gereksinimi varsa, yerleşik StorageClass ' ı kullanın. Erişim modları `ReadWriteOnce` veya birim, tek bir düğüm tarafından okuma-yazma olarak bağlanır. 


Erişim modları hakkında daha fazla bilgi için bkz. [Kubernetes birimleri erişim modu](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes).


## <a name="next-steps"></a>Sonraki adımlar

Statik olarak nasıl sağlayabileceğinizi anlamak için `PersistentVolume` , bkz:

- [Azure Stack kenarınızda, kubectl aracılığıyla statik sağlama aracılığıyla durum bilgisi olan bir uygulamayı dağıtın](azure-stack-edge-gpu-deploy-stateful-application-static-provision-kubernetes.md).

Dinamik olarak nasıl sağlayabileceğinizi öğrenmek için `StorageClass` bkz.:

- [Kuebctl aracılığıyla Azure Stack kenarınızda dinamik sağlama aracılığıyla durum bilgisi olan bir uygulamayı dağıtın](azure-stack-edge-gpu-deploy-stateful-application-dynamic-provision-kubernetes.md).
