---
title: Azure Stack Edge cihazında Kubernetes iş yükü yönetimini anlayın | Microsoft Docs
description: Kubernetes iş yüklerinin Azure Stack Edge cihazında nasıl yönetilebileceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/12/2020
ms.author: alkohli
ms.openlocfilehash: 21845b51fdd108221d5e1bce50e953b79084d17d
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89086985"
---
# <a name="kubernetes-workload-management-on-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızda Kubernetes iş yükü yönetimi

Azure Stack Edge cihazınızda, işlem rolünü yapılandırırken bir Kubernetes kümesi oluşturulur. Kubernetes kümesi oluşturulduktan sonra kapsayıcılı uygulamalar, pods 'deki Kubernetes kümesine dağıtılabilir. Kubernetes kümenizde iş yüklerini dağıtmanın farklı yolları vardır. 

Bu makalede, Azure Stack Edge cihazınızda iş yüklerini dağıtmak için kullanılabilecek çeşitli yöntemler açıklanmaktadır.

## <a name="workload-types"></a>İş yükü türleri

Azure Stack Edge cihazınıza dağıtabileceğiniz iki ortak iş yükü türü durum bilgisiz uygulamalardır ve durum bilgisi olan uygulamalardır.

- **Durum bilgisiz uygulamalar** durumlarını korumaz ve kalıcı depolamaya veri kaydetmez. Tüm Kullanıcı ve oturum verileri istemcisiyle kalır. Durum bilgisi olmayan uygulamalara bazı örnekler NGINX gibi Web ön uçları ve diğer Web uygulamaları içerir.

    Bir Kubernetes dağıtımı oluşturarak, kümenizde durum bilgisiz bir uygulamayı dağıtabilirsiniz. 

- Durum **bilgisi olan uygulamalar** , durumunun kaydedilmesini gerektirir. Durum bilgisi olan uygulamalar, verileri sunucu veya diğer kullanıcılar tarafından kullanılmak üzere kaydetmek için kalıcı birimler gibi kalıcı depolama alanı kullanır. Durum bilgisi olan uygulamalara örnek olarak MongoDB gibi veritabanları dahildir.

    Durum bilgisi olan bir uygulamayı dağıtmak için bir Kubernetes dağıtımı oluşturabilirsiniz. 

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

 
## <a name="deployment-types"></a>Dağıtım türleri

İş yüklerinizi dağıtmanın üç temel yolu vardır. Bu dağıtım yöntemlerinin her biri, cihazdaki ayrı bir ad alanına bağlanmanıza ve sonra durum bilgisiz veya durum bilgisi olmayan uygulamalar dağıtmanıza olanak tanır.

![Kubernetes iş yükü dağıtımı](./media/azure-stack-edge-gpu-kubernetes-workload-management/kubernetes-workload-management-1.png)

- **Yerel dağıtım**: Bu, K8 dağıtmanıza olanak sağlayan gibi komut satırı erişim aracıdır `kubectl` `yamls` . Dosyayı kullanarak oluşturduğunuz Azure Stack Kenarlarınızın K8 kümesine bağlanırsınız `kubeconfig` . Daha fazla bilgi için, [kubectl aracılığıyla bir Kubernetes kümesine erişme](azure-stack-edge-gpu-create-kubernetes-cluster.md)konusuna gidin.

- **IoT Edge dağıtımı**: Bu, Azure IoT Hub 'e bağlanan IoT Edge. Ad alanı aracılığıyla Azure Stack Edge cihazınızdan K8 kümesine bağlanırsınız `iotedge` . Bu ad alanında dağıtılan IoT Edge aracıları Azure bağlantısı sağlanmasından sorumludur. `IoT Edge deployment.json`Yapılandırmayı Azure DevOps CI/CD kullanarak uygularsınız. Ad alanı ve IoT Edge yönetimi, bulut operatörü aracılığıyla yapılır.

- **Azure/Arc dağıtımı**: Azure Arc, K8 kümelerinizde uygulamalar dağıtmanıza imkan tanıyan bir karma yönetim aracıdır. Azure Stack Edge cihazınızdan K8 kümesini aracılığıyla bağlanırsınız `azure-arc namespace` .  Aracılar, bu ad alanında Azure bağlantısının sorumlu olduğu bir şekilde dağıtılır. Dağıtım yapılandırmasını, Gilar tabanlı yapılandırma yönetimini kullanarak uygularsınız. Azure Arc, kümelerinizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyicisini kullanmanıza da imkan tanır. Daha fazla bilgi için [Azure-Arc etkin Kubernetes nedir?](https://docs.microsoft.com/azure/azure-arc/kubernetes/overview)bölümüne bakın.

## <a name="choose-the-deployment-type"></a>Dağıtım türünü seçin

Uygulamaları dağıtma sırasında aşağıdaki bilgileri göz önünde bulundurun:

- **Tek veya birden çok tür**: tek bir dağıtım seçeneği veya farklı dağıtım seçenekleri karışımı seçebilirsiniz.
- **Bulutta yerel**olarak: uygulamalarınıza bağlı olarak, kubectl veya bulut dağıtımı aracılığıyla IoT Edge ve Azure Arc aracılığıyla yerel dağıtım seçeneğini belirleyebilirsiniz. 
    - Yerel dağıtım, geliştirme senaryoları için daha uygundur. Yerel bir dağıtım seçtiğinizde, Azure Stack Edge cihazınızın dağıtıldığı ağla sınırlı olursunuz.
    - Dağıtabileceğiniz bir bulut aracınız varsa, bulut operatörüzü dağıtmanız ve bulut yönetimi kullanmanız gerekir.
- **IoT vs Azure Arc**: dağıtım seçimi, ürün senaryolarınızın amacına de bağlıdır. IoT veya IoT ekosistemi ile daha derin tümleştirme sağlayan uygulamalar veya kapsayıcılar dağıtıyorsanız, uygulamaları dağıtmanın IoT Edge yolunu seçmeniz gerekir. Mevcut Kubernetes dağıtımlarınız varsa, Azure Arc tercih edilen seçenek olacaktır.


## <a name="next-steps"></a>Sonraki adımlar

Bir uygulamayı kubectl aracılığıyla yerel olarak dağıtmak için bkz.:

- [Kubectl aracılığıyla Azure Stack kenarınızdan durum bilgisiz bir uygulamayı dağıtın](azure-stack-edge-j-series-deploy-stateless-application-kubernetes.md).

IoT Edge aracılığıyla bir uygulama dağıtmak için, bkz.:

- [IoT Edge aracılığıyla Azure Stack kenarınızdan örnek bir modül dağıtın](azure-stack-edge-gpu-deploy-sample-module.md).

Azure Arc aracılığıyla bir uygulama dağıtmak için, bkz.:

- [Azure Arc kullanarak bir uygulamayı dağıtın](azure-stack-edge-gpu-deploy-sample-module.md).
