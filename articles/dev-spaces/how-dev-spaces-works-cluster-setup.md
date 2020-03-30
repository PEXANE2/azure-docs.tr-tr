---
title: Azure Geliştirme Alanları için küme oluşturma nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Geliştirme Alanları için Azure Kubernetes Hizmet kümesi kurmanın nasıl çalıştığını açıklar
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: 00f8262f3008ce9ba82726960f78d18395458a2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241731"
---
# <a name="how-setting-up-a-cluster-for-azure-dev-spaces-works"></a>Azure Geliştirme Alanları için küme oluşturma nasıl çalışır?

Azure Geliştirme Alanları, Kubernetes uygulamalarını hızla tekrarlayıp hata ayıklamanın ve bir Azure Kubernetes Hizmeti (AKS) kümesinde ekibinizle işbirliği yapmanın birden çok yolunu sağlar. Bunun bir yolu, aks kümenizde Azure Dev Alanları'nı etkinleştirmektir, böylece [hizmetleri doğrudan kümenizde çalıştırabilir][how-it-works-up] ve [ek ağ ve yönlendirme özelliklerini][how-it-works-routing]kullanabilirsiniz. Bu makalede, kümenizi hazırlayıp Azure Dev Alanları'nı etkinleştirdiğinizde neler olduğu açıklanmaktadır.

## <a name="prepare-your-aks-cluster"></a>AKS kümenizi hazırlayın

AKS kümenizi Dev Spaces'e hazırlamak için, AKS kümenizin [Azure Dev Spaces tarafından desteklenen][supported-regions] bir bölgede olduğunu ve Kubernetes 1.10.3 veya daha sonra çalıştırdığınızı doğrulayın. Azure portalından kümenizde Azure Dev Alanları'nı etkinleştirmek için, kümenize gidin, *Dev Spaces'i*tıklatın, Dev Spaces'i *Evet'e* *çevirin* ve *Kaydet'i*tıklatın. Ayrıca, Azure CLI'den Azure Dev `az aks use-dev-spaces`Spaces'i çalıştırarak etkinleştirebilirsiniz.

Dev Spaces için bir AKS kümesi ayarlama örneği [için, takım geliştirme hızlı başlatın][quickstart-team]bakın.

AKS kümenizde Azure Dev Spaces etkinleştirildiğinde, kümenizin denetleyicisini yükler. Denetleyici AKS kümenizin dışında bulunur. İstemci tarafı takımlama ile AKS kümesi arasındaki davranışı ve iletişimi yönlendirir. Etkinleştirildikten sonra, istemci tarafı takımını kullanarak denetleyiciyle etkileşimkurabilirsiniz.

Denetleyici aşağıdaki eylemleri gerçekleştirir:

* Dev alan oluşturma ve seçimi yönetir.
* Uygulamanızın Dümen grafiğini yükler ve Kubernetes nesnelerini oluşturur.
* Uygulamanızın kapsayıcı görüntüsünü oluşturur.
* Uygulamanızı AKS'ye dağıtıyor.
* Kaynak kodunuz değiştiğinde artımlı yapılar oluşturur ve yeniden başlatır.
* Günlükleri ve HTTP izlerini yönetir.
* Müşteri tarafındaki takıma doğru sayılsa ve stderr' i iletin.
* Bir alan içindeki uygulamaların yanı sıra üst ve alt alanlarda ki uygulamalar için yönlendirmeyi yapılandırır.

Denetleyici, kümenizin dışında ayrı bir Azure kaynağıdır ve kümenizdeki kaynaklariçin aşağıdakileri yapar:

* Dev alanı olarak kullanmak üzere bir Kubernetes ad alanı oluşturur veya belirler.
* Varsa *azds*adlı herhangi bir Kubernetes ad alanını kaldırır ve yeni bir ad oluşturur.
* Bir Kubernetes webhook yapılandırması dağıtir.
* Bir webhook giriş sunucusu dağıtıyor.

AKS kümenizin diğer Azure Dev Spaces bileşenlerine hizmet çağrıları yapmak için kullandığı hizmet ilkesini kullanır.

![Azure Dev Spaces küme hazırlamak](media/how-dev-spaces-works/prepare-cluster.svg)

Azure Geliştirme Alanları'nı kullanabilmek için en az bir dev alanı olması gerekir. Azure Dev Spaces, dev alanlar için AKS kümenizdeki Kubernetes ad alanlarını kullanır. Bir denetleyici yüklendiğinde, yeni bir Kubernetes ad alanı oluşturmanızı veya ilk dev alanınız olarak kullanmak üzere varolan bir ad alanı seçmenize izin ister. Varsayılan olarak, denetleyici varolan *varsayılan* Kubernetes ad alanını ilk dev alanınıza yükseltmeyi teklif eder.

Bir ad alanı dev alanı olarak atandığında, denetleyici bu ad alanına *azds.io/space=true* etiketini eklayarak onu dev alanı olarak tanımlar. Oluşturduğunuz veya belirlediğiniz ilk dev alanı, kümenizi hazırladıktan sonra varsayılan olarak seçilir. Bir alan seçildiğinde, yeni iş yükleri oluşturmak için Azure Dev Spaces tarafından kullanılır.

İstemci tarafındaki aracı, yeni dev alanları oluşturmak ve varolan geliştirme alanlarını kaldırmak için kullanabilirsiniz. Kubernetes'teki bir sınırlama nedeniyle *varsayılan* dev alanı kaldırılamaz. Denetleyici ayrıca istemci tarafı takım tarafından kullanılan `azds` komutile çakışmaları önlemek için *azds* adlı varolan Kubernetes ad boşluklarını kaldırır.

Kubernetes webhook kabul sunucusu enstrümantasyon için dağıtım sırasında üç kapsayıcı ile bölmeenjekte etmek için kullanılır: bir devspaces-proxy kapsayıcı, bir devspaces-proxy-init konteyner ve devspaces-yapı konteyner. **Bu kapların üçü de AKS kümenizde kök erişimi yle çalışır.** Ayrıca, AKS kümenizin diğer Azure Dev Spaces bileşenlerine hizmet çağrıları yapmak için kullandığı hizmet ilkesini de kullanırlar.

![Azure Dev Spaces Kubernetes webhook kabul sunucusu](media/how-dev-spaces-works/kubernetes-webhook-admission-server.svg)

Devspaces-proxy kapsayıcı, uygulama konteynerinin içine ve dışına tüm TCP trafiğini işleyen ve yönlendirmenin gerçekleştirilmesine yardımcı olan bir kenar araç kapsayıcısır. Belirli boşluklar kullanılıyorsa, devspaces-proxy kapsayıcısı HTTP iletilerini yeniden yönlendirir. Örneğin, http iletilerini üst ve alt alanlardaki uygulamalar arasında yönlendirmeye yardımcı olabilir. TÜM HTTP olmayan trafik devspaces-proxy değiştirilmemiş geçer. Devspaces-proxy kapsayıcı da gelen ve giden TÜM HTTP iletileri günlükleri ve iz olarak istemci tarafı araç gönderir. Bu izlemeler daha sonra uygulamanın davranışını incelemek için geliştirici tarafından görüntülenebilir.

Devspaces-proxy-init kapsayıcıuygulamanızın kapsayıcısına alan hiyerarşisine dayalı ek yönlendirme kuralları ekleyen bir [init](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) kapsayıcısidir. Uygulama konteynerinin */etc/resolv.conf* dosyasını ve iptables yapılandırmasını başlamadan önce güncelleyerek yönlendirme kuralları ekler. */etc/resolv.conf* güncellemeleri, üst alanlardaki hizmetlerin DNS çözümüne olanak tanır. iptables yapılandırma güncelleştirmeleri, uygulamanın kapsayıcısına giren ve çıkan tüm TCP trafiğinin devspaces-proxy olsa da yönlendirilmesini sağlar. Devspaces-proxy-init'in tüm güncelleştirmeleri, Kubernetes'in eklediği kurallara ek olarak gerçekleşir.

Devspaces-yapı kapsayıcı bir init konteyner ve proje kaynak kodu ve Docker soket monte vardır. Proje kaynak kodu ve Docker'a erişim, uygulama kabının doğrudan pod tarafından inşa edilmesine olanak tanır.

> [!NOTE]
> Azure Dev Spaces, uygulamanızın kapsayıcısını oluşturmak ve çalıştırmak için aynı düğümü kullanır. Sonuç olarak, Azure Dev Spaces uygulamanızı oluşturmak ve çalıştırmak için harici bir kapsayıcı kayıt defterine gerek duymaz.

Kubernetes webhook kabul sunucusu AKS kümesinde oluşturulan herhangi bir yeni pod için dinler. Bu *bölme, azds.io/space=true* etiketiyle herhangi bir ad alanına dağıtılırsa, bu bölmeye ek kapsayıcılar enjekte eder. Devspaces yapı kapsayıcısı yalnızca uygulamanın kapsayıcısı istemci tarafı aracı kullanılarak çalıştırılırsa enjekte edilir.

AKS kümenizi hazırladıktan sonra, kodunuzu geliştirme alanınızda hazırlamak ve çalıştırmak için istemci tarafındaki aracı kullanabilirsiniz.

## <a name="client-side-tooling"></a>İstemci yan takım

İstemci tarafı aracı, kullanıcının şunları yapmasına olanak tanır:
* Uygulama için Dockerfile, Miğfer grafiği ve Azure Dev Spaces yapılandırma dosyası oluşturun.
* Üst ve alt dev alanları oluşturun.
* Denetleyiciye uygulamanızı oluşturmasını ve başlatmasını söyleyin.

Uygulamanız çalışırken, istemci tarafı aracı da:
* AKS'de çalışan uygulamanızdan stdout ve stderr alır ve görüntüler.
* Http:\//localhost'u kullanarak uygulamanıza web erişimine izin vermek için [ileri bağlantı noktasını](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) kullanır.
* AKS'deki çalışan uygulamanıza bir hata ayıklama ataşar.
* Artımlı yapılar için bir değişiklik algılandığında kaynak kodu dev alanınızla eşitleyerek hızlı yinelemesağlar.
* Geliştirici makinenizi doğrudan AKS kümenize bağlamanızı sağlar.

Komut satırından istemci tarafı aracını `azds` komutun bir parçası olarak kullanabilirsiniz. İstemci tarafı aracını aşağıdakilerle de kullanabilirsiniz:

* [Azure Dev Spaces uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)kullanarak Görsel Stüdyo Kodu.
* [Kubernetes için Visual Studio Araçları](https://aka.ms/get-vsk8stools)ile Visual Studio .

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzu geliştirme alanınızda hazırlamak ve çalıştırmak için istemci tarafı aracını kullanma hakkında daha fazla bilgi edinmek için Azure [Geliştirme Alanları için proje hazırlamanın nasıl çalıştığını][how-it-works-prep]öğrenin.

Ekip geliştirme için Azure Geliştirme Alanları'nı kullanmaya başlamak için [Azure Dev Spaces'te ekip geliştirmeye][quickstart-team] bakın.

[how-it-works-prep]: how-dev-spaces-works-prep.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[how-it-works-up]: how-dev-spaces-works-up.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[quickstart-team]: quickstart-team-development.md