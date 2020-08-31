---
title: Microsoft Azure Stack Edge cihazında Kubernetes kümesine genel bakış | Microsoft Docs
description: Kubernetes 'in Azure Stack Edge cihazınızda nasıl uygulandığını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: b85586a431a20102035e253537fc45c8a8a54796
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89086992"
---
# <a name="kubernetes-on-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızda Kubernetes

Kubernetes, Kapsayıcılı uygulamaları düzenlemek için popüler bir açık kaynaklı platformdur. Bu makale, Kubernetes 'e genel bakış sağlar ve ardından Azure Stack Edge cihazınızda Kubernetes 'in nasıl çalıştığını açıklar. 

## <a name="about-kubernetes"></a>Kubernetes hakkında 

Kubernetes kapsayıcı tabanlı uygulamaları ve bunların ilişkili ağ ve depolama bileşenlerini yönetmek için kolay ve güvenilir bir platform sağlar. Kubernetes ile Kapsayıcılı uygulamaları hızla oluşturabilir, sunabilir ve ölçeklendirebilirsiniz.

Açık bir platform olarak, tercih ettiğiniz programlama dili, işletim sistemi kitaplıkları veya mesajlaşma veri yolu ile uygulama oluşturmak için Kubernetes kullanabilirsiniz. Kubernetes, yayınları planlamak ve dağıtmak için mevcut sürekli tümleştirme ve sürekli teslim araçlarıyla tümleştirilebilir.

Daha fazla bilgi için bkz. [Kubernetes 'In nasıl çalıştığı](https://www.youtube.com/watch?v=q1PcAawa4Bg&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=2&t=0s).

## <a name="kubernetes-on-azure-stack-edge"></a>Azure Stack Edge üzerinde Kubernetes

Azure Stack Edge cihazınızda, işlem yapılandırarak bir Kubernetes kümesi oluşturabilirsiniz. İşlem rolü yapılandırıldığında, ana ve çalışan düğümlerin dahil olduğu Kubernetes kümesinin tümü dağıtılır ve sizin için yapılandırılır. Bu küme daha sonra `kubectl` , IoT Edge veya Azure Arc aracılığıyla iş yükü dağıtımı için kullanılır.

Azure Stack Edge cihazı, altyapı kümesini oluşturan 1 düğümlü bir yapılandırma olarak kullanılabilir. Kubernetes kümesi altyapı kümesinden ayrıdır ve altyapı kümesinin üzerine dağıtılır. Altyapı kümesi, Kubernetes kümesi yalnızca uygulama düzenlemesi için sorumlu olduğunda Azure Stack Edge cihazınız için kalıcı depolama alanı sağlar. 

Bu durumda Kubernetes kümesinin bir ana düğümü ve bir çalışan düğümü vardır. Bir kümedeki Kubernetes düğümleri, uygulamalarınızı ve bulut iş akışlarınızı çalıştıran sanal makinelerdir. 

Kubernetes ana düğümü, kümenizin istenen durumunun korunmasından sorumludur. Ana düğüm aynı zamanda Kapsayıcılı uygulamaları çalıştıran çalışan düğümünü de denetler. 

Aşağıdaki diyagramda, Kubernetes 'in 1 düğümlü Azure Stack Edge cihazında uygulanması gösterilmektedir. 1 düğümlü cihaz yüksek oranda kullanılabilir değildir ve tek düğüm başarısız olursa cihaz kapanır. Kubernetes kümesi de aşağı gider.

![1 düğümlü Azure Stack Edge cihazı için Kubernetes mimarisi](media/azure-stack-edge-gpu-kubernetes-overview/kubernetes-architecture-1-node.png)

Kubernetes küme mimarisi hakkında daha fazla bilgi için [Kubernetes temel kavramları](https://kubernetes.io/docs/concepts/architecture/)' ne gidin.


<!--The Kubernetes cluster control plane components make global decisions about the cluster. The control plane has:

- *kubeapiserver* that is the front end of the Kubernetes API and exposes the API.
- *etcd* that is a highly available key value store that backs up all the Kubernetes cluster data.
- *kube-scheduler* that makes scheduling decisions.
- *kube-controller-manager* that runs controller processes such as those for node controllers, replications controllers, endpoint controllers, and service account and token controllers. -->

## <a name="storage-volume-provisioning"></a>Depolama birimi sağlama

Uygulama iş yüklerini desteklemek için, Azure Stack Edge cihaz paylaşımlarınız üzerinde kalıcı veriler için depolama birimleri bağlayabilirsiniz. Statik ve dinamik birimler kullanılabilir. 

Daha fazla bilgi için, [Azure Stack Edge cihazınız Için Kubernetes depolamadaki](azure-stack-edge-gpu-kubernetes-storage.md)uygulamalar için depolama sağlama seçenekleri bölümüne bakın.

## <a name="networking"></a>Ağ

Kubernetes ağı, kapsayıcıda kapsayıcı ağ, Pod-Pod ağı, Pod-to-Service Networking ve Internet-to-Service Networking gibi Kubernetes ağınız içindeki iletişimi yapılandırmanızı sağlar. Daha fazla bilgi için, [Azure Stack Edge cihazınız Için Kubernetes ağındaki](azure-stack-edge-gpu-kubernetes-networking.md)ağ modeline bakın.

## <a name="updates"></a>Güncelleştirmeler

Yeni Kubernetes sürümleri kullanılabilir hale geldiğinde, kümeniz Azure Stack Edge cihazınız için kullanılabilen standart güncelleştirmeler kullanılarak yükseltilebilir. Yükseltme adımları için, bkz. [Azure Stack Edge için güncelleştirmeleri uygulama](azure-stack-edge-gpu-install-update.md).

## <a name="access-monitoring"></a>Erişim, izleme

Azure Stack Edge cihazındaki Kubernetes kümesi rol tabanlı erişim denetimine (RBAC) izin verir. Daha fazla bilgi için, [Azure Stack Edge cihazınızda Kubernetes kümesi Için rol tabanlı erişim denetimi](azure-stack-edge-gpu-kubernetes-rbac.md)konusuna bakın.

Ayrıca, Kubernetes panosu aracılığıyla kümenizin ve kaynaklarınızın durumunu izleyebilirsiniz. Kapsayıcı günlükleri de kullanılabilir. Daha fazla bilgi için bkz. [Azure Stack Edge cihazınızda Kubernetes kümesi sistem durumunu izlemek Için Kubernetes panosunu kullanma](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md).

Azure Izleyici, kapsayıcılardan, düğümlerde ve denetleyicilerden sağlık verileri toplamak için bir eklenti olarak da kullanılabilir. Daha fazla bilgi için bkz. [Azure izlemeye genel bakış](../azure-monitor/overview.md)

<!--## Private container registry

Kubernetes on Azure Stack Edge device allows for the private storage of your images by providing a local container registry.-->

## <a name="application-management"></a>Uygulama yönetimi

Azure Stack Edge cihazınızda bir Kubernetes kümesi oluşturulduktan sonra, aşağıdaki yöntemlerden herhangi birini kullanarak bu kümede dağıtılan uygulamaları yönetebilirsiniz:

- Aracılığıyla yerel erişim `kubectl`
- IoT Edge 
- Azure Arc

Bu yöntemler aşağıdaki bölümlerde açıklanmıştır.


### <a name="kubernetes-and-kubectl"></a>Kubernetes ve kubectl

Kubernetes kümesi dağıtıldıktan sonra, kümede dağıtılan uygulamaları bir istemci makineden yerel olarak yönetebilirsiniz. Uygulamalarla etkileşimde bulunmak için komut satırı aracılığıyla *kubectl* gibi yerel bir araç kullanın. 

Kubernetes kümesi dağıtma hakkında daha fazla bilgi için [Azure Stack Edge cihazınızda bir Kubernetes kümesi dağıtma](azure-stack-edge-gpu-create-kubernetes-cluster.md)konusuna gidin. Yönetim hakkında daha fazla bilgi için, [Azure Stack Edge cihazınızda Kubernetes kümesini yönetmek için kubectl kullanma](azure-stack-edge-gpu-create-kubernetes-cluster.md)bölümüne gidin.


### <a name="kubernetes-and-iot-edge"></a>Kubernetes ve IoT Edge

Kubernetes, Kubernetes 'in ölçek sağladığı ve ekosistem ve IoT 'nin IoT merkezli ekosistemi sağladığı Azure Stack Edge cihazında IoT Edge iş yükleri ile de tümleştirilebilir. Kubernetes katmanı, Azure IoT Edge iş yüklerini dağıtmak için bir altyapı katmanı olarak kullanılır. Modül ömrü ve Ağ Yükü Dengeleme, Kubernetes tarafından yönetilir, ancak uç uygulama platformu IoT Edge tarafından yönetilir.

IoT Edge aracılığıyla Kubernetes kümenizdeki uygulamaları dağıtma hakkında daha fazla bilgi için şuraya gidin: 

- [Azure Stack Edge cihazında durum bilgisiz uygulamaları IoT Edge aracılığıyla kullanıma sunun](azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module.md).


### <a name="kubernetes-and-azure-arc"></a>Kubernetes ve Azure yay

Azure Arc, Kubernetes kümelerinizde uygulamalar dağıtmanıza imkan tanıyan bir karma yönetim aracıdır. Azure Arc, kümelerinizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyici kullanmanıza da olanak tanır. Daha fazla bilgi için [Azure-Arc etkin Kubernetes nedir?](https://docs.microsoft.com/azure/azure-arc/kubernetes/overview)bölümüne bakın. Azure Arc fiyatlandırması hakkında bilgi için [Azure Arc fiyatlandırması](https://azure.microsoft.com/services/azure-arc/#pricing)' na gidin.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge cihazında](azure-stack-edge-gpu-kubernetes-storage.md)Kubernetes depolaması hakkında daha fazla bilgi edinin.
- [Azure Stack Edge cihazında](azure-stack-edge-gpu-kubernetes-networking.md)Kubernetes ağ modelini anlayın.
- Azure portal [Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md) dağıtın.
