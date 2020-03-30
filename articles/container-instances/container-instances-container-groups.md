---
title: Konteyner gruplarına giriş
description: Bir yaşam döngüsünü ve depolama ve ağ gibi kaynakları paylaşan bir örnek koleksiyonu olan Azure Kapsayıcı Örnekleri'ndeki kapsayıcı grupları hakkında bilgi edinin
ms.topic: article
ms.date: 11/01/2019
ms.custom: mvc
ms.openlocfilehash: 73781418321c3932bf3e0190b646dcd3bb178195
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247221"
---
# <a name="container-groups-in-azure-container-instances"></a>Azure Container Instances’taki kapsayıcı grupları

Azure Kapsayıcı Örnekleri'ndeki üst düzey kaynak *kapsayıcı grubudur.* Bu makalede, kapsayıcı gruplarının ne olduğu ve etkinleştirdikleri senaryo türleri açıklanmaktadır.

## <a name="what-is-a-container-group"></a>Konteyner grubu nedir?

Kapsayıcı grubu, aynı ana bilgisayar makinesinde zamanlanan kapsayıcılar topluluğudur. Kapsayıcı grubundaki kapsayıcılar yaşam döngüsünü, kaynakları, yerel ağı ve depolama birimlerini paylaşır. [Kubernetes'teki][kubernetes-pod]bir *pod'a* benzer.

Aşağıdaki diyagram, birden çok kapsayıcı içeren bir kapsayıcı grubunun bir örneğini gösterir:

![Konteyner grupları diyagramı][container-groups-example]

Bu örnek kapsayıcı grubu:

* Tek bir ana bilgisayar makinesinde zamanlanır.
* Bir DNS ad etiketi atanır.
* Tek bir ortak IP adresi, tek bir açık bağlantı noktası ile ortaya çıkarır.
* İki kapsayıcıdan oluşur. Bir konteyner port 80 dinler, diğer port 5000 dinler.
* Birim bağlar olarak iki Azure dosya paylaşımı içerir ve her kapsayıcı yerel paylaşımlardan birini bağlar.

> [!NOTE]
> Çoklu kapsayıcı grupları şu anda yalnızca Linux kapsayıcılarını desteklememektedir. Windows kapsayıcıları için Azure Kapsayıcı Örnekleri yalnızca tek bir kapsayıcı örneğinin dağıtımını destekler. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışırken, [hizmete Genel Bakış'ta](container-instances-overview.md#linux-and-windows-containers)güncel platform farklılıklarını bulabilirsiniz.

## <a name="deployment"></a>Dağıtım

Çoklu kapsayıcı grubunu dağıtmanın iki yaygın yolu vardır: [Kaynak Yöneticisi şablonu][resource-manager template] veya [YAML dosyası][yaml-file]kullanın. Kapsayıcı örneklerini dağıttığınızda ek Azure hizmet kaynaklarını (örneğin, [Azure Dosyaları paylaşımı)][azure-files]dağıtmanız gerektiğinde Kaynak Yöneticisi şablonu önerilir. YAML biçiminin daha özlü yapısı nedeniyle, dağıtımınızda yalnızca kapsayıcı örnekleri bulunduğunda bir YAML dosyası önerilir. Ayarlayabildiğiniz özelliklerle ilgili ayrıntılar için [Kaynak Yöneticisi şablon başvurusuna](/azure/templates/microsoft.containerinstance/containergroups) veya [YAML başvuru belgelerine](container-instances-reference-yaml.md) bakın.

Bir kapsayıcı grubunun yapılandırmasını korumak için, Azure CLI [komutaz kapsayıcı dışa aktarma][az-container-export]yı kullanarak yapılandırmayı BIR YAML dosyasına dışa aktarabilirsiniz. Dışa aktarma, kapsayıcı grup yapılandırmalarınızı "kod olarak yapılandırma" için sürüm denetiminde depolamanıza olanak tanır. Veya, YAML'de yeni bir yapılandırma geliştirirken dışa aktarılan dosyayı başlangıç noktası olarak kullanın.



## <a name="resource-allocation"></a>Kaynak ayırma

Azure Kapsayıcı Örnekleri, gruptaki örneklerin [kaynak isteklerini][resource-requests] ekleyerek, CPU'lar, bellek ve isteğe bağlı [GPU'lar][gpus] (önizleme) gibi kaynakları çok kapsayıcılı bir gruba ayırır. Örnek olarak CPU kaynaklarını ele alırsak, her biri 1 CPU isteyen iki kapsayıcı örneği içeren bir kapsayıcı grubu oluşturursanız, kapsayıcı grubu 2 CPU ayrılır.

### <a name="resource-usage-by-container-instances"></a>Kapsayıcı örneklerine göre kaynak kullanımı

Bir gruptaki her kapsayıcı örneği, kaynak isteğinde belirtilen kaynaklara tahsis edilir. Ancak, bir gruptaki kapsayıcı örneği tarafından kullanılan en büyük kaynaklar, isteğe bağlı [kaynak sınırı][resource-limits] özelliğini yapılandırırsanız farklı olabilir. Kapsayıcı örneğinin kaynak sınırı, zorunlu [kaynak isteği][resource-requests] özelliğinden büyük veya eşit olmalıdır.

* Bir kaynak sınırı belirtmezseniz, kapsayıcı örneğinin en büyük kaynak kullanımı kaynak isteğiyle aynıdır.

* Bir kapsayıcı örneği için bir sınır belirtirseniz, örneğin en büyük kullanımı, belirlediğiniz sınıra kadar istekten daha büyük olabilir. Buna bağlı olarak, gruptaki diğer kapsayıcı örnekleri tarafından kaynak kullanımı azalabilir. Kapsayıcı örneği için ayarlayabildiğiniz maksimum kaynak sınırı, gruba ayrılan toplam kaynaktır.
    
Örneğin, her biri 1 CPU isteyen iki kapsayıcı örneği olan bir grupta, kapsayıcılarınızdan biri diğerinden daha fazla CPU'nun çalıştırılması nı gerektiren bir iş yükü çalıştırabilir.

Bu senaryoda, kapsayıcı örneği için 2 CPU kaynak sınırı ayarlayabilirsiniz. Bu yapılandırma, kapsayıcı örneğinin varsa tam 2 CPU'yu kullanmasına olanak tanır.

### <a name="minimum-and-maximum-allocation"></a>Minimum ve maksimum ayırma

* Bir kapsayıcı grubuna **en az** 1 CPU ve 1 GB bellek ayırın. Bir grup içindeki tek tek kapsayıcı örnekleri 1 CPU'dan az ve 1 GB bellekle sağlanabilir. 

* Kapsayıcı grubundaki **maksimum** kaynaklar için dağıtım bölgesindeki Azure Kapsayıcı Örnekleri için [kaynak kullanılabilirliğine][region-availability] bakın.

## <a name="networking"></a>Ağ Oluşturma

Kapsayıcı grupları, harici bir IP adresini, bu IP adresinde bir veya daha fazla bağlantı noktasını ve tam nitelikli etki alanı adı (FQDN) olan bir DNS etiketini paylaşabilir. Dış istemcilerin grup içindeki bir kapsayıcıya ulaşmasını sağlamak için, bağlantı noktasını IP adresinde ve kapsayıcıdan çıkarmanız gerekir. Grup içindeki kapsayıcılar bir bağlantı noktası ad alanını paylaştığından, bağlantı noktası eşlemi desteklenmez. Kapsayıcı grubu silindiğinde bir kapsayıcı grubunun IP adresi ve FQDN serbest bırakılır. 

Bir kapsayıcı grubunda, bu bağlantı noktaları grubun IP adresinde veya kapsayıcıdan dışarıdan açıklanmasa bile, kapsayıcı örnekleri herhangi bir bağlantı noktasında yerel barındırma aracılığıyla birbirlerine ulaşabilir.

Kapsayıcıların sanal [ağdaki][virtual-network] diğer kaynaklarla güvenli bir şekilde iletişim kurabilmesi için kapsayıcı gruplarını isteğe bağlı olarak Azure sanal ağına dağıtın.

## <a name="storage"></a>Depolama

Bir kapsayıcı grubuna monte etmek için dış hacimleri belirtebilirsiniz. Desteklenen birimler şunlardır:
* [Azure dosya paylaşımı][azure-files]
* [Gizli dizi][secret]
* [Boş dizin][empty-directory]
* [Klonlanmış git repo][volume-gitrepo]

Bu birimleri bir gruptaki tek tek kapsayıcılar içindeki belirli yollara eşleyebilirsiniz. 

## <a name="common-scenarios"></a>Genel senaryolar

Çok kapsayıcılı gruplar, tek bir işlevsel görevi az sayıda kapsayıcı görüntüsüne bölmek istediğiniz durumlarda yararlıdır. Bu görüntüler daha sonra farklı ekipler tarafından teslim edilebilir ve ayrı kaynak gereksinimleri vardır.

Örnek kullanım şunları içerebilir:

* Bir web uygulaması sunan bir konteyner ve kaynak denetiminden en son içeriği çeken bir konteyner.
* Uygulama kapsayıcısı ve günlük kapsayıcısı. Günlük konteyneri, günlükleri ve ölçümler çıktısını ana uygulama tarafından toplar ve bunları uzun süreli depolamaalanına yazar.
* Bir uygulama kapsayıcısı ve bir izleme konteyneri. İzleme kabı, düzgün çalışmasını ve yanıt verdiğinden emin olmak için uygulama için düzenli aralıklarla bir istekte bulunmaz ve değilse bir uyarı yükseltir.
* Bir ön uç konteyner ve bir arka uç konteyner. Ön uç, arka uç veri almak için bir hizmet çalışan bir web uygulaması hizmet verebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure Kaynak Yöneticisi şablonu yla çok kapsayıcı kapsayıcı grubunu nasıl dağıtılayacağız öğrenin:

> [!div class="nextstepaction"]
> [Bir kapsayıcı grubu dağıtma][resource-manager template]

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
