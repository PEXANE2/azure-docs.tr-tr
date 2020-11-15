---
title: Azure Stack Edge Pro cihazında Kubernetes rol tabanlı erişim denetimini anlayın | Microsoft Docs
description: Azure Stack Edge Pro cihazında Kubernetes rol tabanlı erişim denetiminin nasıl oluştuğunu açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: alkohli
ms.openlocfilehash: 9a9625dcf40ae7d11e1154fc89b7f04652c8ca16
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2020
ms.locfileid: "94635849"
---
# <a name="kubernetes-role-based-access-control-on-your-azure-stack-edge-pro-gpu-device"></a>Azure Stack Edge Pro GPU cihazınızda Kubernetes rol tabanlı erişim denetimi


Azure Stack Edge Pro cihazınızda, işlem rolünü yapılandırırken bir Kubernetes kümesi oluşturulur. Cihazınızdaki küme kaynaklarıyla erişimi sınırlandırmak için Kubernetes rol tabanlı erişim denetimi (Kubernetes RBAC) kullanabilirsiniz.

Bu makaleler, Kubernetes tarafından sağlanan Kubernetes RBAC sistemi için bir genel bakış sağlar ve Azure Stack Edge Pro cihazınızda Kubernetes RBAC 'in nasıl uygulandığı. 

## <a name="kubernetes-rbac"></a>Kubernetes RBAC

Kubernetes RBAC kullanıcıları veya Kullanıcı gruplarını atamanıza izin verir, kaynak oluşturma veya değiştirme gibi işlemleri yapma veya çalışan uygulama iş yüklerinden günlükleri görüntüleme izni verir. Bu izinler tek bir ad alanı kapsamına alınmış olabilir veya tüm küme genelinde verilebilir. 

Kubernetes kümesini ayarlarken, bu kümeye karşılık gelen tek bir Kullanıcı oluşturulur ve Küme Yönetici kullanıcısı olarak adlandırılır.  Bir `kubeconfig` Dosya, Küme Yöneticisi kullanıcısı ile ilişkilendirilir. `kubeconfig`Dosya, kullanıcının kimliğini doğrulamak için kümeye bağlanmak için gereken tüm yapılandırma bilgilerini içeren bir metin dosyasıdır.

## <a name="namespaces-types"></a>Ad alanı türleri

Pod ve dağıtımlar gibi Kubernetes kaynakları, mantıksal olarak bir ad alanı halinde gruplandırılır. Bu gruplandırmalar, bir Kubernetes kümesini mantıksal olarak bölmek ve kaynakları oluşturmak, görüntülemek veya yönetmek için erişimi kısıtlamak için bir yol sağlar. Kullanıcılar yalnızca atanan ad alanları içindeki kaynaklarla etkileşime girebilirler.

Ad alanları, birden çok takıma veya projeye yayılan birçok kullanıcı olan ortamlarda kullanılmak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Kubernetes ad alanları](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

Azure Stack Edge Pro cihazınız aşağıdaki ad alanlarına sahiptir:

- **Sistem ad alanı** -bu ad alanı, DNS ve proxy gibi ağ özellikleri veya Kubernetes panosu gibi çekirdek kaynakların bulunduğu yerdir. Genellikle kendi uygulamalarınızı bu ad alanına dağıtmazsınız. Kubernetes küme sorunlarını ayıklamak için bu ad alanını kullanın. 

    Cihazınızda birden fazla sistem ad alanı vardır ve bu sistem ad alanlarına karşılık gelen adlar ayrılmıştır. Ayrılmış sistem ad alanlarının listesi aşağıdadır: 
    - kuin-sistem
    - meta allb-sistem
    - din-Namespace
    - default
    - Kubernetes-Pano
    - kuin düğüm kirası
    - KUIN-genel


    Oluşturduğunuz Kullanıcı ad alanları için ayrılmış adlar kullanmayın. 
<!--- **default namespace** - This namespace is where pods and deployments are created by default when none is provided and you have admin access to this namespace. When you interact with the Kubernetes API, such as with `kubectl get pods`, the default namespace is used when none is specified.-->

- **Kullanıcı ad alanı** -Bunlar, **kubectl** aracılığıyla oluşturabileceğiniz ad boşluklardır veya uygulamaların yerel olarak dağıtılması için cihazın PowerShell arabirimi aracılığıyla oluşturulabilir.
 
- **IoT Edge ad alanı** - `iotedge` IoT Edge aracılığıyla dağıtılan uygulamaları yönetmek için bu ad alanına bağlanırsınız.

- **Azure Arc ad alanı** - `azure-arc` Azure Arc aracılığıyla dağıtılan uygulamaları yönetmek için bu ad alanına bağlanırsınız. Azure Arc ile diğer kullanıcı ad alanlarında da uygulama dağıtabilirsiniz. 

## <a name="namespaces-and-users"></a>Ad alanları ve kullanıcılar

Gerçek dünyada, kümeyi birden çok ad alanına bölmek önemlidir. 

- **Birden çok Kullanıcı** : birden fazla kullanıcınız varsa, birden çok ad alanı, bu kullanıcıların her biri kendi belirli ad alanlarındaki uygulama ve hizmetlerini birbirinden yalıtımına dağıtmalarına olanak sağlar. 
- **Tek Kullanıcı** : tek bir Kullanıcı olsa bile, birden çok ad alanı söz konusu kullanıcının aynı Kubernetes kümesinde uygulamaların birden çok sürümünü çalıştırmasına izin verir.

### <a name="roles-and-rolebindings"></a>Roller ve RoleBindings

Kubernetes, bir ad alanı düzeyinde ve bir küme düzeyinde Kullanıcı veya kaynaklara izin vermenizi sağlayan rol ve rol bağlama kavramıdır. 

- **Roller** : kullanıcılar için Izinleri bir **rol** olarak tanımlayabilir ve ardından **Roller** kullanarak bir ad alanı içinde izinler verebilirsiniz. 
- **Rolebindings** : rolleri tanımladıktan sonra, belirli bir ad alanı için rol atamak üzere **rolebindings** kullanabilirsiniz. 

Bu yaklaşım, tek bir Kubernetes kümesini mantıksal olarak ayırt etmenizi sağlar, böylece kullanıcılar yalnızca atanan ad alanındaki uygulama kaynaklarına erişebilir. 

## <a name="kubernetes-rbac-on-azure-stack-edge-pro"></a>Kubernetes Azure Stack Edge Pro 'da RBAC

Kubernetes RBAC 'in geçerli uygulamasında Azure Stack Edge Pro, kısıtlanmış bir PowerShell çalışma alanından aşağıdaki eylemleri almanıza olanak sağlar:

- Ad alanları oluşturun.  
- Ek kullanıcılar oluşturun.
- Oluşturduğunuz ad alanlarına yönetici erişimi verin. Küme Yöneticisi rolüne veya küme genelindeki kaynakların görünümüne erişiminizin olmadığı göz önünde bulundurun.
- `kubeconfig`Kubernetes kümesine erişmek için bilgileri içeren dosyayı alın.


Azure Stack Edge Pro cihazında birden çok sistem ad alanı vardır ve `kubeconfig` Bu ad alanlarına erişmek için dosyalarla birlikte Kullanıcı ad alanları oluşturabilirsiniz. Kullanıcılar bu ad alanları üzerinde tam denetime sahiptir ve Kullanıcı oluşturabilir veya değiştirebilir veya kullanıcılara erişim izni verebilir. Yalnızca küme yöneticisinin sistem ad alanları ve küme genelinde kaynaklara tam erişimi vardır. `aseuser`, Sistem ad alanlarına salt okuma erişimi vardır.

İşte Azure Stack Edge Pro cihazında Kubernetes RBAC uygulamasını gösteren bir diyagram.

![Kubernetes Azure Stack Edge Pro cihazındaki RBAC](./media/azure-stack-edge-gpu-kubernetes-rbac/rbac-view-1.png)

Bu diyagramda, Gamze, Bob ve Chuck yalnızca atanan kullanıcı ad alanlarına erişebilir, bu durumda, `ns1` `ns2` ve `ns3` sırasıyla. Bu ad alanlarında yönetici erişimi vardır. Diğer yandan küme yöneticisinin sistem ad alanları ve küme genelinde kaynaklara yönetici erişimi vardır.

Kullanıcı olarak, ad alanları ve kullanıcılar oluşturabilir, kullanıcıları ad alanlarına atayabilir veya `kubeconfig` dosyaları indirebilirsiniz. Ayrıntılı adım adım yönergeler için [Azure Stack Edge Pro 'da kuebctl aracılığıyla Kubernetes kümesine erişim](azure-stack-edge-gpu-create-kubernetes-cluster.md)bölümüne gidin.


Azure Stack Edge Pro cihazlarınızda ad alanları ve kullanıcılarla çalışırken aşağıdaki uyarılar geçerlidir:

- Sistem ad alanlarından herhangi biri için Kullanıcı oluşturma, Kullanıcı adına ad alanı erişimi verme veya iptal etme gibi işlemleri gerçekleştirmenize izin verilmez. Sistem ad alanları örnekleri şunlardır,,,, `kube-system` `metallb-system` `kubernetes-dashboard` `default` `kube-node-lease` , `kube-public` . Sistem ad alanları, `iotedge` (IoT Edge ad alanı) ve `azure-arc` (Azure Arc ad alanı) gibi dağıtım türleri için ayrılan ad alanlarını da içerir.
- Kullanıcı ad alanları oluşturabilir ve bu ad alanları içinde ek kullanıcılar oluşturabilir ve bu kullanıcılara ad alanı erişimi verebilir veya iptal edebilirsiniz.
- Herhangi bir sistem ad alanı için olanlarla aynı adlara sahip ad alanı oluşturma izniniz yok. Sistem ad alanları için adlar ayrılmıştır.  
- Diğer Kullanıcı ad alanları tarafından zaten kullanılan adlara sahip hiçbir Kullanıcı ad alanı oluşturmanıza izin verilmiyor. Örneğin, oluşturduğunuz bir tane varsa `test-ns` , başka bir `test-ns` ad alanı oluşturamazsınız.
- Zaten ayrılmış adlara sahip kullanıcılar oluşturmanıza izin verilmiyor. Örneğin, `aseuser` ayrılmış bir Kullanıcı ve kullanılamaz.


## <a name="next-steps"></a>Sonraki adımlar

Bir Kullanıcı oluşturma, ad alanı oluşturma ve ad alanına kullanıcı erişimi verme işlemlerini anlamak için bkz. [kubectl aracılığıyla bir Kubernetes kümesine erişme](azure-stack-edge-gpu-create-kubernetes-cluster.md).

