---
title: Geliştirme bilgisayarınızı AKS kümenize bağlama çalışması
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Geliştirme bilgisayarınızı Azure Kubernetes hizmet kümenize bağlamak için Azure Dev Spaces kullanma işlemini açıklar
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: a74a5a623006ccd64441023c2c4bc9ad3dcb517e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80241718"
---
# <a name="how-connecting-your-development-computer-to-your-aks-cluster-works"></a>Geliştirme bilgisayarınızı AKS kümenize bağlama çalışması

Azure Dev Spaces ile geliştirme bilgisayarınızı AKS kümenize bağlayabilirsiniz ve bu kodda, geliştirme bilgisayarınızda, küme üzerinde çalışıyor gibi bir kod çalıştırmanıza ve hata ayıklamanıza olanak sağlayabilirsiniz. Azure Dev Spaces, kuruluşunuzda geliştirme makineniz ve küme arasında trafiği yeniden yönlendirmek için uzak bir aracı görevi gören bir pod çalıştırarak bağlı AKS kümeniz arasında trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirme, AKS kümenizde çalışan geliştirme ve hizmetinizdeki kodların aynı AKS kümenizle gibi iletişim kurmasına olanak tanır. Bu bağlantı Ayrıca, geliştirme bilgisayarınızda bir kapsayıcı ile veya olmadan kodu çalıştırmanıza ve hata ayıklamanıza olanak tanır. Geliştirme bilgisayarınızı kümenize bağlamak, uygulamanızı hızlı bir şekilde geliştirmenize ve uçtan uca test gerçekleştirmenize yardımcı olur.

## <a name="connecting-to-your-cluster"></a>Kümenize bağlanma

MacOS veya Windows 10 ' da çalışan [Azure dev Spaces][azds-vs-code] uzantısı ile [Visual Studio Code][vs-code] kullanarak mevcut aks kümenize bağlanırsınız. Bir bağlantı oluşturduğunuzda, tüm trafiği kümedeki yeni veya var olan bir pod 'dan geliştirme bilgisayarınıza yönlendirme seçeneğiniz vardır.

> [!NOTE]
> Visual Studio Code kullanırken, kümenize bağlanmak için Azure Dev Spaces uzantısı size bir hizmeti geliştirme bilgisayarınıza yönlendirme seçeneği sunar. Bu seçenek, yeniden yönlendirme için bir pod belirlemek için uygun bir yoldur. AKS kümeniz ile geliştirme bilgisayarınız arasındaki tüm yeniden yönlendirme bir pod içindir.

Kümenize bağlanmak için kümenizde Azure Dev Spaces etkinleştirilmiş olması gerekmez. Bunun yerine, Azure Dev Spaces uzantısı kümenize bir bağlantı kurduğunda:

* AKS kümesindeki Pod 'daki kapsayıcıyı, trafiği geliştirme bilgisayarınıza yönlendiren bir uzak Aracı kapsayıcısı ile değiştirir. Yeni bir pod yeniden yönlendirilirken Azure Dev Spaces, uzak aracı ile AKS kümenizde yeni bir pod oluşturur.
* Geliştirme bilgisayarınızdan gelen trafiği kümenizde çalışan uzak aracıya iletmek için [kubectl bağlantı noktasını][kubectl-port-forward] geliştirme bilgisayarınızda ilet olarak çalıştırır.
* Uzak aracıyı kullanarak kümenizdeki ortam bilgilerini toplar. Bu ortam bilgileri ortam değişkenlerini, görünür Hizmetleri, birim takmaları ve gizli takmaları içerir.
* Geliştirme Bilgisayarınızdaki hizmetin, kümede çalışıyor gibi aynı değişkenlere erişebilmesi için Visual Studio Code terminalinde ortamı ayarlar.  
* Hosts dosyanızı, AKS kümenizdeki hizmetleri geliştirme bilgisayarınızdaki yerel IP adresleriyle eşlenecek şekilde güncelleştirir. Bu ana bilgisayar dosya girdileri, geliştirme bilgisayarınızda çalışan kodun kümenizde çalışan diğer hizmetlere istek yapmasına izin verir. Hosts dosyanızı güncelleştirmek için Azure Dev Spaces, kümenize bağlanırken geliştirme bilgisayarınızda yönetici erişimi ister.

Kümenizde Azure Dev Spaces etkinse, [Azure dev Spaces tarafından sunulan trafik yeniden yönlendirmeyi][how-it-works-routing]kullanma seçeneğiniz de vardır. Azure Dev Spaces tarafından sunulan trafik yeniden yönlendirme, bir alt geliştirme alanında çalışan hizmetinizin bir kopyasına bağlanmanızı sağlar. Alt geliştirme alanının kullanılması, yalnızca alt alanın hizmet örneğini hedefleyen trafiği yeniden yönlendirdiklerinden ve hizmetin üst alan örneğini değiştirilmemiş şekilde bırakarak üst geliştirme alanında çalışan diğerlerinin kesintiye uğramasını önlemeye yardımcı olur.

Kümenize bağlandıktan sonra, hizmetinize geliştirme bilgisayarınızda çalışıp çalışmadığını göz önüne alarak trafik geliştirme bilgisayarınıza yönlendirilir.

## <a name="running-code-on-your-development-computer"></a>Geliştirme bilgisayarınızda kod çalıştırma

AKS kümenize bir bağlantı kurduktan sonra, herhangi bir kodu kapsayıcısız bir şekilde bilgisayarınızda çalıştırabilirsiniz. Uzak aracının aldığı tüm ağ trafiği, bağlantı sırasında belirtilen yerel bağlantı noktasına yönlendirilir, böylece yerel olarak çalışan kodunuz bu trafiği kabul edebilir ve işleyebilir. Kümenizin ortam değişkenleri, birimleri ve gizli dizileri, geliştirme bilgisayarınızda çalışan kod için kullanılabilir hale getirilir. Ayrıca, ana bilgisayar dosya girişleri ve bağlantı noktası iletimi Azure Dev Spaces tarafından geliştirici bilgisayarınıza eklenmiş olması nedeniyle, kodunuz kümenizdeki hizmet adlarını kullanarak kümenizde çalışan hizmetlere ağ trafiği gönderebilir ve bu trafik kümenizde çalışan hizmetlere iletilir.

Kodunuz geliştirme bilgisayarınızda çalıştığından, kendi kodunuzu çalıştırmak ve hata ayıklamak için genellikle geliştirme amacıyla kullandığınız herhangi bir aracı kullanma esnekliği vardır. Geliştirme bilgisayarınız ile kümeniz arasında, bağlandığınız zaman trafik yönlendirilir. Bu kalıcı bağlantı, bir bağlantıyı yeniden oluşturmaya gerek kalmadan kodunuzu başlatmanıza, durdurabilir ve yeniden başlatmanıza olanak tanır.

Ayrıca Azure Dev Spaces, *azds-Local. env* dosyası aracılığıyla geliştirme bilgisayarınızdaki aks kümenizdeki ortam değişkenlerini ve bağlı dosyaları bir arada çoğaltmak için bir yol sağlar. Bu dosyayı Ayrıca yeni ortam değişkenleri ve birim bağlama oluşturmak için de kullanabilirsiniz.

## <a name="additional-configuration-with-azds-localenv"></a>Azds-Local. env ile ek yapılandırma

*Azds-Local. env* dosyası, aks kümenizdeki ortam değişkenlerini ve takılı dosyaları kümelerinizi çoğaltmanıza olanak sağlar. Bir *azds-Local. env* dosyasında aşağıdaki eylemleri belirtebilirsiniz:

* Bir birim indirin ve bu birimin yolunu ortam değişkeni olarak ayarlayın.
* Bir birimden tek bir dosyayı veya dosya kümesini indirin ve geliştirme bilgisayarınıza bağlayın.
* Bağlandığınız kümeden bağımsız olarak bir hizmeti kullanılabilir hale getirin.

Aşağıda örnek bir *azds-Local. env* dosyası verilmiştir:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development computer,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development computer.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}

# This makes the myapp1 service available to your development computer
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.

MYAPP1_SERVICE_HOST=${services.myapp1}

# This makes the service myapp2 in namespace mynamespace available to your 
# development computer regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP2_SERVICE_HOST.

MYAPP2_SERVICE_HOST=${services.mynamespace.myapp2}
```

Varsayılan bir *azds-Local. env* dosyası otomatik olarak oluşturulmaz, bu sayede dosyayı projenizin kökünde el ile oluşturmanız gerekir.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

AKS kümenize bağlanıldığında, kümenizdeki tanılama günlükleri geliştirme bilgisayarınızın [geçici dizinine][azds-tmp-dir]kaydedilir. Visual Studio Code kullanarak, AKS kümenizdeki geçerli ortam değişkenlerini ve DNS girdilerini yazdırmak için *Tanılama bilgilerini göster* komutunu da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yerel geliştirme bilgisayarınızı AKS kümenize bağlamaya başlamak için bkz. [geliştirme bilgisayarınızı BIR aks kümesine bağlama][connect].

[azds-tmp-dir]: troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[connect]: how-to/connect.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[vs-code]: https://code.visualstudio.com/download
