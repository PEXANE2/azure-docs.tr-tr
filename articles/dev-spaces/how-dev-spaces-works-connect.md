---
title: Geliştirme bilgisayarınızı AKS kümenize bağlama nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Geliştirme bilgisayarınızı Azure Kubernetes Hizmet kümenize bağlamak için Azure Dev Spaces'i kullanan işlemleri açıklar
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: a74a5a623006ccd64441023c2c4bc9ad3dcb517e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241718"
---
# <a name="how-connecting-your-development-computer-to-your-aks-cluster-works"></a>Geliştirme bilgisayarınızı AKS kümenize bağlama nasıl çalışır?

Azure Dev Spaces ile geliştirme bilgisayarınızı AKS kümenize bağlayarak geliştirme bilgisayarınızda kümeüzerinde çalışıyormuş gibi kod çalıştırmanızı ve hata ayıklamanızı sağlayabilirsiniz. Azure Dev Spaces, geliştirme makineniz ile küme arasındaki trafiği yeniden yönlendirmek için uzak bir aracı görevi gören bir bölmeyi kümenizde çalıştırarak bağlı AKS kümeniz arasındaki trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirmesi, geliştirme bilgisayarınızdaki kodun ve AKS kümenizde çalışan hizmetlerin aynı AKS kümesindeymüş gibi iletişim kurmasına olanak tanır. Bu bağlantı ayrıca, geliştirme bilgisayarınızda bir kapsayıcı ile veya olmadan kod çalıştırmanızı ve hata ayıklamanızı sağlar. Geliştirme bilgisayarınızı kümenize bağlamak, uygulamanızı hızla geliştirmenize ve uçdan uca sınama gerçekleştirmenize yardımcı olur.

## <a name="connecting-to-your-cluster"></a>Kümenize bağlanma

MacOS veya Windows 10'da çalışan [Azure Dev Spaces][azds-vs-code] uzantısı ile [Visual Studio Kodu'nu][vs-code] kullanarak mevcut AKS kümenize bağlanırsınız. Bir bağlantı kurduğunuzda, kümedeki yeni veya varolan bir bölmedeki tüm trafiği geliştirme bilgisayarınıza yönlendirme seçeneğiniz var.

> [!NOTE]
> Kümenize bağlanmak için Visual Studio Code'u kullanırken, Azure Geliştirme Alanları uzantısı size bir hizmeti geliştirme bilgisayarınıza yönlendirme seçeneği sunar. Bu seçenek, yeniden yönlendirme için bir bölme tanımlamak için kullanışlı bir yoldur. AKS kümeniz ile geliştirme bilgisayarınız arasındaki tüm yeniden yönlendirme bir bölme içindir.

Kümenize bağlanmak, kümenizde Azure Dev Alanları'nın etkinleştirilmesini gerektirmez. Bunun yerine, Azure Geliştirme Alanları uzantısı kümenize bir bağlantı kurduğunda, aşağıdakileri

* AKS kümesindeki bölmedeki kapsayıcıyı, trafiği geliştirme bilgisayarınıza yönlendiren uzak bir aracı kapsayıcıyla değiştirir. Azure Dev Spaces, yeni bir bölmeyi yeniden yönlendirirken, uzak aracıyla AKS kümenizde yeni bir bölme oluşturur.
* Geliştirme bilgisayarınızdaki [kubectl bağlantı noktasını,][kubectl-port-forward] geliştirme bilgisayarınızdan kümenizde çalışan uzak aracıya iletmek için çalıştırın.
* Uzak aracıyı kullanarak kümenizden ortam bilgileri toplar. Bu ortam bilgileri çevre değişkenlerini, görünür hizmetleri, hacim montajlarını ve gizli bağlar içerir.
* Geliştirme bilgisayarınızdaki hizmetküme üzerinde çalışıyormuş gibi aynı değişkenlere erişebilsin diye Visual Studio Code terminalindeki ortamı ayarlar.  
* Ev sahipleri dosyanızı AKS kümenizdeki hizmetleri geliştirme bilgisayarınızdaki yerel IP adreslerine eşlemek üzere güncelleştirir. Bu ana bilgisayar dosya girişleri, geliştirme bilgisayarınızda çalışan kodun kümenizde çalışan diğer hizmetlere istekte bulunmasına izin verir. Azure Dev Spaces, ana bilgisayar dosyanızı güncelleştirmek için kümenize bağlanırken geliştirme bilgisayarınızda yönetici erişimi ister.

Kümenizde Azure Dev Spaces etkinleştirilmişse, [Azure Dev Spaces tarafından sunulan trafik yeniden yönlendirmesini][how-it-works-routing]de kullanma seçeneğiniz vardır. Azure Geliştirme Spaces tarafından sunulan trafik yeniden yönlendirmesi, hizmetinizin bir alt geliştirme alanında çalışan bir kopyasına bağlanmanızı sağlar. Alt öğe geliştirme alanı kullanmak, yalnızca alt alanın hizmetinizi hedefleyen trafiği yeniden yönlendirdiğiniz ve hizmetin üst alan örneğini değiştirilmeden bıraktığınız için, üst geliştirme alanında çalışan diğer lerini rahatsız etmekten kaçınmanıza yardımcı olur.

Kümenize bağlandıktan sonra, hizmetinizin geliştirme bilgisayarınızda çalışıp çalışmadığına bakılmaksızın trafik geliştirme bilgisayarınıza yönlendirilir.

## <a name="running-code-on-your-development-computer"></a>Geliştirme bilgisayarınızda kod çalıştırma

AKS kümenize bağlantı kurduktan sonra, herhangi bir kodu bilgisayarınızda kapsayıcılıktan çalıştırmadan yerel olarak çalıştırabilirsiniz. Uzak aracının aldığı herhangi bir ağ trafiği, yerel olarak çalışan kodunuz bu trafiği kabul edip işleyebilir diye bağlantı sırasında belirtilen yerel bağlantı noktasına yönlendirilir. Kümenizdeki ortam değişkenleri, birimleri ve sırları geliştirme bilgisayarınızda çalışan kodlara kullanılabilir hale getirilir. Ayrıca, Azure Dev Spaces tarafından geliştirici bilgisayarınıza eklenen ana bilgisayar dosya girişleri ve bağlantı noktası yönlendirmesi nedeniyle, kodunuz kümenizdeki servis adlarını kullanarak kümenizde çalışan hizmetlere ağ trafiği gönderebilir ve bu trafik kümenizde çalışan hizmetler.

Kodunuz geliştirme bilgisayarınızda çalışırken, normalde geliştirme için kullandığınız herhangi bir aracı kodunuzu çalıştırmak ve hata ayıklamak için kullanma esnekliğine sahipsiniz. Bağlı olduğunuz süre boyunca geliştirme bilgisayarınızla kümeniz arasında trafik yönlendirilir. Bu kalıcı bağlantı, bir bağlantıyı yeniden kurmak zorunda kalmadan kodunuzu istediğiniz kadar başlatmanızı, durdurmanızı ve yeniden başlatmanızı sağlar.

Buna ek olarak, Azure Dev *Spaces, azds-local.env* dosyası aracılığıyla geliştirme bilgisayarınızdaki AKS kümenizdeki bölmelerde bulunan ortam değişkenlerini ve monte edilmiş dosyaları çoğaltmanın bir yolunu sağlar. Bu dosyayı yeni ortam değişkenleri ve birim bağlantıları oluşturmak için de kullanabilirsiniz.

## <a name="additional-configuration-with-azds-localenv"></a>azds-local.env ile ek yapılandırma

*Azds-local.env* dosyası, AKS kümenizde bölmelerinizin kullanabileceği ortam değişkenlerini ve monte edilmiş dosyaları çoğaltmanızı sağlar. *Azds-local.env* dosyasında aşağıdaki eylemleri belirtebilirsiniz:

* Bir birim indirin ve bu birimin yolunu bir ortam değişkeni olarak ayarlayın.
* Tek bir dosyayı veya dosya kümesini bir birimden indirin ve geliştirme bilgisayarınıza monte edin.
* Bağlı olduğunuz kümeden bağımsız olarak bir hizmeti kullanılabilir hale getirin.

Burada bir örnek *azds-local.env* dosyası:

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

Varsayılan *azds-local.env* dosyası otomatik olarak oluşturulmaz, bu nedenle dosyayı projenizin kökünde el ile oluşturmanız gerekir.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

AKS kümenize bağlandığında, kümenizdeki tanılama günlükleri geliştirme bilgisayarınızın [geçici dizinine][azds-tmp-dir]kaydedilir. Visual Studio Code'u kullanarak, AKS kümenizden geçerli ortam değişkenlerini ve DNS girişlerini yazdırmak için *Tanı bilgisini göster* komutunu da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yerel geliştirme bilgisayarınızı AKS kümenize bağlamaya başlamak için geliştirme [bilgisayarınızı bir AKS kümesine bağlayın'a][connect]bakın.

[azds-tmp-dir]: troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[connect]: how-to/connect.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[vs-code]: https://code.visualstudio.com/download
