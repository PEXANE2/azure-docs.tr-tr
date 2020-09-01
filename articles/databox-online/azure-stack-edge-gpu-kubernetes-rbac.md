---
title: Azure Stack Edge cihazında Kubernetes rol tabanlı Access Control anlayın | Microsoft Docs
description: Bir Azure Stack Edge cihazında Kubernetes rol tabanlı Access Control nasıl oluştuğunu açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/27/2020
ms.author: alkohli
ms.openlocfilehash: 697c686b61a86cb01327364ad73f30f88e2e151d
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268083"
---
# <a name="kubernetes-role-based-access-control-on-your-azure-stack-edge-gpu-device"></a>Kubernetes rol tabanlı Access Control Azure Stack Edge GPU cihazınızda


Azure Stack Edge cihazınızda, işlem rolünü yapılandırırken bir Kubernetes kümesi oluşturulur. Kubernetes rol tabanlı erişim denetimi 'ni (RBAC), cihazınızdaki küme kaynaklarıyla erişimi sınırlandırmak için kullanabilirsiniz.

Bu makaleler, Kubernetes tarafından sağlanan RBAC sistemine genel bir bakış sağlar ve Azure Stack Edge cihazınızda Kubernetes RBAC 'in nasıl uygulandığını açıklar. 

## <a name="rbac-for-kubernetes"></a>Kubernetes için RBAC

Kubernetes RBAC kullanıcıları veya Kullanıcı gruplarını atamanıza izin verir, kaynak oluşturma veya değiştirme gibi işlemleri yapma veya çalışan uygulama iş yüklerinden günlükleri görüntüleme izni verir. Bu izinler tek bir ad alanı kapsamına alınmış olabilir veya tüm küme genelinde verilebilir. 

Kubernetes kümesini ayarlarken, bu kümeye karşılık gelen tek bir Kullanıcı oluşturulur ve Küme Yönetici kullanıcısı olarak adlandırılır.  Bir `kubeconfig` Dosya, Küme Yöneticisi kullanıcısı ile ilişkilendirilir. `kubeconfig`Dosya, kullanıcının kimliğini doğrulamak için kümeye bağlanmak için gereken tüm yapılandırma bilgilerini içeren bir metin dosyasıdır.

## <a name="namespaces-types"></a>Ad alanı türleri

Pod ve dağıtımlar gibi Kubernetes kaynakları, mantıksal olarak bir ad alanı halinde gruplandırılır. Bu gruplandırmalar, bir Kubernetes kümesini mantıksal olarak bölmek ve kaynakları oluşturmak, görüntülemek veya yönetmek için erişimi kısıtlamak için bir yol sağlar. Kullanıcılar yalnızca atanan ad alanları içindeki kaynaklarla etkileşime girebilirler.

Ad alanları, birden çok takıma veya projeye yayılan birçok kullanıcı olan ortamlarda kullanılmak üzere tasarlanmıştır. Az sayıda kullanıcısı olan kümeler için, ad alanlarını oluşturmanız veya düşünmek zorunda değilsiniz. Sağladıkları özelliklere ihtiyacınız olduğunda ad alanlarını kullanmaya başlayın.

Daha fazla bilgi için bkz. [Kubernetes ad alanları](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).


Azure Stack Edge cihazınız aşağıdaki ad alanlarına sahiptir:

- **Sistem ad alanı** -bu ad alanı, DNS ve proxy gibi ağ özellikleri veya Kubernetes panosu gibi çekirdek kaynakların bulunduğu yerdir. Genellikle kendi uygulamalarınızı bu ad alanına dağıtmazsınız. Kubernetes küme sorunlarını ayıklamak için bu ad alanını kullanın. 

    Cihazınızda birden fazla sistem ad alanı vardır ve bu sistem ad alanlarına karşılık gelen adlar ayrılmıştır. Ayrılmış sistem ad alanlarının listesi aşağıdadır: 
    - kuin-sistem
    - meta allb-sistem
    - din-Namespace
    - default
    - Kubernetes-Pano
    - default
    - kuin düğüm kirası
    - KUIN-genel
    - iotedge
    - Azure-yay

    Oluşturduğunuz Kullanıcı ad alanları için ayrılmış adlar kullanmayın. 
<!--- **default namespace** - This namespace is where pods and deployments are created by default when none is provided and you have admin access to this namespace. When you interact with the Kubernetes API, such as with `kubectl get pods`, the default namespace is used when none is specified.-->

- **Kullanıcı ad alanı** -uygulamaları yerel olarak dağıtmak için **kubectl** aracılığıyla oluşturabileceğiniz ad boşluklardır.
 
- **IoT Edge ad alanı** - `iotedge` IoT Edge aracılığıyla uygulama dağıtmak için bu ad alanına bağlanırsınız.

- **Azure Arc ad alanı** - `azure-arc` Azure Arc aracılığıyla uygulamaları dağıtmak için bu ad alanına bağlanırsınız. 

## <a name="namespaces-and-users"></a>Ad alanları ve kullanıcılar

Gerçek dünyada, kümeyi birden çok ad alanına bölmek önemlidir. 

- **Birden çok Kullanıcı**: birden fazla kullanıcınız varsa, birden çok ad alanı, bu kullanıcıların her biri kendi belirli ad alanlarındaki uygulama ve hizmetlerini birbirinden yalıtımına dağıtmalarına olanak sağlar. 
- **Tek Kullanıcı**: tek bir Kullanıcı olsa bile, birden çok ad alanı söz konusu kullanıcının aynı Kubernetes kümesinde uygulamaların birden çok sürümünü çalıştırmasına izin verir.

### <a name="roles-and-rolebindings"></a>Roller ve RoleBindings

Kubernetes, bir ad alanı düzeyinde ve bir küme düzeyinde Kullanıcı veya kaynaklara izin vermenizi sağlayan rol ve rol bağlama kavramıdır. 

- **Roller**: kullanıcılar için Izinleri bir **rol** olarak tanımlayabilir ve ardından **Roller** kullanarak bir ad alanı içinde izinler verebilirsiniz. 
- **Rolebindings**: rolleri tanımladıktan sonra, belirli bir ad alanı için rol atamak üzere **rolebindings** kullanabilirsiniz. 

Bu yaklaşım, tek bir Kubernetes kümesini mantıksal olarak ayırt etmenizi sağlar, böylece kullanıcılar yalnızca atanan ad alanındaki uygulama kaynaklarına erişebilir. 

## <a name="rbac-on-azure-stack-edge"></a>Azure Stack Edge üzerinde RBAC

RBAC 'in geçerli uygulamasında Azure Stack Edge, kısıtlanmış bir PowerShell çalışma alanından aşağıdaki eylemleri almanıza olanak sağlar:

- Ad alanları oluşturun.  
- Ek kullanıcılar oluşturun.
- Oluşturduğunuz ad alanlarına yönetici erişimi verin. Küme Yöneticisi rolüne veya küme genelindeki kaynakların görünümüne erişiminizin olmadığı göz önünde bulundurun.
- `kubeconfig`Kubernetes kümesine erişmek için bilgileri içeren dosyayı alın.


Azure Stack Edge cihazında birden çok sistem ad alanı vardır ve `kubeconfig` Bu ad alanlarına erişmek için dosyalarla birlikte Kullanıcı ad alanları oluşturabilirsiniz. Kullanıcılar bu ad alanları üzerinde tam denetime sahiptir ve Kullanıcı oluşturabilir veya değiştirebilir veya kullanıcılara erişim izni verebilir. Yalnızca küme yöneticisinin sistem ad alanları ve küme genelinde kaynaklara tam erişimi vardır. `aseuser`, Sistem ad alanlarına salt okuma erişimi vardır.

İşte Azure Stack Edge cihazında RBAC uygulamasını gösteren bir diyagram.

![Azure Stack Edge cihazında RBAC](./media/azure-stack-edge-gpu-kubernetes-rbac/rbac-view-1.png)

Bu diyagramda, Gamze, Bob ve Chuck yalnızca atanan kullanıcı ad alanlarına erişebilir, bu durumda, `ns1` `ns2` ve `ns3` sırasıyla. Bu ad alanlarında yönetici erişimi vardır. Diğer yandan küme yöneticisinin sistem ad alanları ve küme genelinde kaynaklara yönetici erişimi vardır.

`kubectl`Ad alanları oluşturmak, kullanıcı atamak, kullanıcı atamak veya dosyaları indirmek için komutları kullanabilirsiniz `kubeconfig` . Yüksek düzey bir iş akışı aşağıda verilmiştir:

1. Bir ad alanı ve kullanıcı oluşturun.  

    `New-HcsKubernetesNamespace -Namespace`  

2. Bir kullanıcı oluşturun.  

    `New-HcsKubernetesUser -UserName`  

3. Ad alanını oluşturduğunuz kullanıcıyla ilişkilendirin.  

    `Grant-HcsKubernetesNamespaceAccess -Namespace -UserName`  

4. Kullanıcı yapılandırmasını ' ye kaydedin `C:\Users\<username>\.kube` .  

5. `kubectl`Uygulamaları ' ye yükleyip dağıtmaya başlayın `kubectl` . 

Ayrıntılı adım adım yönergeler için [Azure Stack kenarınızdan kuebctl aracılığıyla Kubernetes kümesine erişim](azure-stack-edge-gpu-create-kubernetes-cluster.md)bölümüne gidin.


Azure Stack Edge cihazlarınızdaki ad alanları ve kullanıcılarla çalışırken aşağıdaki uyarılar geçerlidir:

- Sistem ad alanlarından herhangi biri için Kullanıcı oluşturma, Kullanıcı adına ad alanı erişimi verme veya iptal etme gibi işlemleri gerçekleştirmenize izin verilmez. Sistem ad alanları örnekleri şunlardır,,,, `kube-system` `metallb-system` `kubernetes-dashboard` `default` `kube-node-lease` , `kube-public` . Sistem ad alanları, `iotedge` (IoT Edge ad alanı) ve `azure-arc` (Azure Arc ad alanı) gibi dağıtım türleri için ayrılan ad alanlarını da içerir.
- Kullanıcı ad alanları oluşturabilir ve bu ad alanları içinde ek kullanıcılar oluşturabilir ve bu kullanıcılara ad alanı erişimi verebilir veya iptal edebilirsiniz.
- Herhangi bir sistem ad alanı için olanlarla aynı adlara sahip ad alanı oluşturma izniniz yok. Sistem ad alanları için adlar ayrılmıştır.  
- Diğer Kullanıcı ad alanları tarafından zaten kullanılan adlara sahip hiçbir Kullanıcı ad alanı oluşturmanıza izin verilmiyor. Örneğin, oluşturduğunuz bir tane varsa `test-ns` , başka bir `test-ns` ad alanı oluşturamazsınız.
- Zaten ayrılmış adlara sahip kullanıcılar oluşturmanıza izin verilmiyor. Örneğin, `aseuser` ayrılmış bir küme yöneticsahiptir ve kullanılamaz.


## <a name="next-steps"></a>Sonraki adımlar

Bir Kullanıcı oluşturma, ad alanı oluşturma ve ad alanına kullanıcı erişimi verme işlemlerini anlamak için bkz. [kubectl aracılığıyla bir Kubernetes kümesine erişme](azure-stack-edge-gpu-create-kubernetes-cluster.md).

