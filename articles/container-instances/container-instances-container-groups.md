---
title: Kapsayıcı gruplarına giriş
description: Azure Container Instances içinde kapsayıcı grupları hakkında bilgi edinin, bir yaşam döngüsü ve depolama ve ağ gibi kaynakları paylaşan bir örnek koleksiyonu
ms.topic: article
ms.date: 11/01/2019
ms.custom: mvc
ms.openlocfilehash: 19fa50f83a2593b8914931e25fa99cb2e4896227
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75770280"
---
# <a name="container-groups-in-azure-container-instances"></a>Azure Container Instances kapsayıcı grupları

Azure Container Instances en üst düzey kaynak *kapsayıcı grubudur*. Bu makalede, kapsayıcı gruplarının ne olduğu ve etkinleştirildikleri senaryo türleri açıklanmaktadır.

## <a name="what-is-a-container-group"></a>Kapsayıcı grubu nedir?

Kapsayıcı grubu, aynı konak makinesinde zamanlanmış bir kapsayıcı koleksiyonudur. Bir kapsayıcı grubundaki kapsayıcılar bir yaşam döngüsü, kaynak, yerel ağ ve depolama birimi paylaşır. Prototip, [Kubernetes][kubernetes-pod]'te *Pod* ile benzerdir.

Aşağıdaki diyagramda birden çok kapsayıcı içeren bir kapsayıcı grubu örneği gösterilmektedir:

![Kapsayıcı grupları diyagramı][container-groups-example]

Bu örnek kapsayıcı grubu:

* Tek bir konak makinesinde zamanlanır.
* Bir DNS ad etiketi atanır.
* Tek bir genel IP adresini, kullanıma sunulan tek bir bağlantı noktasıyla birlikte sunar.
* İki kapsayıcıdan oluşur. Bir kapsayıcı 80 numaralı bağlantı noktasını dinler, diğer bir deyişle bağlantı noktası 5000 ' de dinleme yapılır.
* , Birim bağlama olarak iki Azure dosya paylaşımı içerir ve her kapsayıcı paylaşımların birini yerel olarak takar.

> [!NOTE]
> Çoklu kapsayıcı grupları Şu anda yalnızca Linux kapsayıcılarını destekliyor. Windows kapsayıcıları için Azure Container Instances yalnızca tek bir örnek dağıtımını destekler. Tüm özellikleri Windows kapsayıcılarına getirmek için çalıştık, ancak geçerli platform farklılıklarını hizmete [genel bakış](container-instances-overview.md#linux-and-windows-containers)bölümünde bulabilirsiniz.

## <a name="deployment"></a>Kurulum

Çok kapsayıcılı bir grubu dağıtmanın iki yaygın yolu aşağıda verilmiştir: bir [Kaynak Yöneticisi şablonu][resource-manager template] veya [YAML dosyası][yaml-file]kullanın. Kapsayıcı örneklerini dağıtırken ek Azure hizmet kaynakları (örneğin, bir [Azure dosyaları paylaşma][azure-files]) dağıtmanız gerektiğinde, bir kaynak yöneticisi şablonu önerilir. YAML biçiminin daha kısa olmasından dolayı, dağıtımınız yalnızca kapsayıcı örnekleri içerdiğinde YAML dosyası önerilir. Ayarlayabileceğiniz özelliklerle ilgili ayrıntılar için [Kaynak Yöneticisi Şablon başvurusu](/azure/templates/microsoft.containerinstance/containergroups) veya [YAML başvuru](container-instances-reference-yaml.md) belgelerine bakın.

Bir kapsayıcı grubunun yapılandırmasını korumak için, [az kapsayıcı dışarı aktarma][az-container-export]Azure CLI komutunu kullanarak bir YAML dosyasına yapılandırmayı dışarı aktarabilirsiniz. Dışarı aktarma, kapsayıcı grubu yapılandırmalarını "kod olarak yapılandırma" için sürüm denetiminde depolamanıza olanak tanır. Ya da, YAML 'de yeni bir yapılandırma geliştirirken, bir başlangıç noktası olarak, dışarıya aktarılmış dosyayı kullanın.



## <a name="resource-allocation"></a>Kaynak ayırma

Azure Container Instances, gruptaki örneklerin [kaynak isteklerini][resource-requests] ekleyerek CPU, bellek ve Isteğe bağlı [GPU 'lar][gpus] (Önizleme) gibi kaynakları çok kapsayıcılı bir gruba ayırır. CPU kaynaklarını örnek olarak alma Örneğin, her biri 1 CPU isteyen iki örneğe sahip bir kapsayıcı grubu oluşturursanız, kapsayıcı grubuna 2 CPU tahsis edilir.

### <a name="resource-usage-by-instances"></a>Örneklere göre kaynak kullanımı

Bir gruptaki her kapsayıcı örneği, kaynak isteğinde belirtilen kaynakları tahsis edilir. Ancak, bir gruptaki bir örnek tarafından kullanılan en fazla kaynak, isteğe bağlı [kaynak sınırı][resource-limits] özelliğini yapılandırırsanız farklı olabilir. Bir örneğin kaynak sınırı zorunlu [kaynak isteği][resource-requests] özelliğinden büyük veya buna eşit olmalıdır.

* Kaynak sınırı belirtmezseniz, örneğin en büyük kaynak kullanımı kaynak isteğiyle aynı olur.

* Bir örnek için bir sınır belirtirseniz, örneğin en büyük kullanım süresi istekten daha büyük olabilir ve bu, ayarladığınız sınıra kadar fazladır. Karşılık gelen, gruptaki diğer örneklere göre kaynak kullanımı azalabilir. Bir örnek için ayarlayabileceğiniz maksimum kaynak sınırı, gruba ayrılan toplam kaynaktır.
    
Örneğin, her biri 1 CPU isteyen iki örneğe sahip bir grupta, kapsayıcılarınızın biri diğer CPU 'ların çalışmasını gerektiren bir iş yükü çalıştırabilir.

Bu senaryoda, örnek için 2 CPU kaynak sınırı ayarlayabilirsiniz. Bu yapılandırma, kapsayıcının, varsa tam 2 CPU 'ya kadar kullanmasına izin verir.

### <a name="minimum-and-maximum-allocation"></a>En düşük ve en yüksek ayırma

* Bir kapsayıcı grubuna **en az** 1 CPU ve 1 GB bellek ayırın. Bir grup içindeki bağımsız kapsayıcı örnekleri 1 ' den az CPU ve 1 GB bellek ile sağlanabilir. 

* Bir kapsayıcı grubundaki **en fazla** kaynak için, dağıtım bölgesindeki Azure Container Instances [kaynak kullanılabilirliğine][region-availability] bakın.

## <a name="networking"></a>Networking (Ağ İletişimi)

Kapsayıcı grupları, bir dış IP adresini, bu IP adresinde bir veya daha fazla bağlantı noktasını ve tam etki alanı adı (FQDN) olan bir DNS etiketini paylaşabilir. Dış istemcilerin Grup içindeki bir kapsayıcıya ulaşmasını sağlamak için, bağlantı noktasını IP adresinde ve kapsayıcıdan kullanıma sunmalısınız. Grup içindeki kapsayıcılar bir bağlantı noktası ad alanını paylaştığından, bağlantı noktası eşleştirmesi desteklenmez. Kapsayıcı grubu silindiğinde kapsayıcı grubunun IP adresi ve FQDN serbest bırakılır. 

Bir kapsayıcı grubu içinde, kapsayıcılar örnekleri, bu bağlantı noktaları grubun IP adresinde veya kapsayıcıdan dışarıdan sunulmasa bile, herhangi bir bağlantı noktasında localhost aracılığıyla birbirlerine ulaşabilir.

Kapsayıcıların sanal ağdaki diğer kaynaklarla güvenli bir şekilde iletişim kurmasına izin vermek için, isteğe bağlı olarak kapsayıcı gruplarını bir [Azure sanal ağına][virtual-network] (Önizleme) dağıtın.

## <a name="storage"></a>Depolama

Bir kapsayıcı grubuna bağlamak için dış birimler belirtebilirsiniz. Desteklenen birimler şunlardır:
* [Azure dosya paylaşımı][azure-files]
* [Gizli dizi][secret]
* [Boş Dizin][empty-directory]
* [Kopyalanmış git deposu][volume-gitrepo]

Bu birimleri bir gruptaki tek kapsayıcılar içindeki belirli yollarla eşleyebilirsiniz. 

## <a name="common-scenarios"></a>Genel senaryolar

Çok Kapsayıcılı gruplar, tek bir işlevsel görevi az sayıda kapsayıcı görüntülerine bölmek istediğiniz durumlarda faydalıdır. Bu görüntüler daha sonra farklı takımlar tarafından teslim edilebilir ve ayrı kaynak gereksinimlerine sahip olabilir.

Örnek kullanım şunları içerebilir:

* Bir Web uygulamasına hizmet veren bir kapsayıcı ve bir kapsayıcı, kaynak denetiminden en son içeriği çekiliyor.
* Uygulama kapsayıcısı ve günlüğe kaydetme kapsayıcısı. Günlüğe kaydetme kapsayıcısı, ana uygulamanın günlüklerini ve ölçüm çıkışını toplar ve bunları uzun süreli depolamaya yazar.
* Uygulama kapsayıcısı ve izleme kapsayıcısı. İzleme kapsayıcısı düzenli aralıklarla, çalıştığından ve doğru şekilde yanıt verdiğinden emin olmak için uygulamaya bir istek yapar ve değilse bir uyarı oluşturur.
* Ön uç kapsayıcısı ve arka uç kapsayıcısı. Ön uç, arka uç ile verileri almak için bir hizmet çalıştıran bir Web uygulaması sunabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Resource Manager şablonuyla çok kapsayıcılı bir kapsayıcı grubunu dağıtmayı öğrenin:

> [!div class="nextstepaction"]
> [Kapsayıcı grubu dağıtma][resource-manager template]

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/

<!-- LINKS - Internal -->
[resource-manager template]: container-instances-multi-container-group.md
[yaml-file]: container-instances-multi-container-yaml.md
[region-availability]: container-instances-region-availability.md
[resource-requests]: /rest/api/container-instances/containergroups/createorupdate#resourcerequests
[resource-limits]: /rest/api/container-instances/containergroups/createorupdate#resourcelimits
[resource-requirements]: /rest/api/container-instances/containergroups/createorupdate#resourcerequirements
[azure-files]: container-instances-volume-azure-files.md
[virtual-network]: container-instances-vnet.md
[secret]: container-instances-volume-secret.md
[volume-gitrepo]: container-instances-volume-gitrepo.md
[gpus]: container-instances-gpu.md
[empty-directory]: container-instances-volume-emptydir.md
[az-container-export]: /cli/azure/container#az-container-export
