---
title: Azure Container Registry SKU 'Ları
description: Azure Container Registry bulunan farklı hizmet katmanlarını karşılaştırın.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 05/06/2019
ms.author: danlep
ms.openlocfilehash: bf620178a0c10661126b3e52c7b908ccc9a90d89
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68311897"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry SKU 'Ları

Azure Container Registry (ACR), SKU 'Lar olarak bilinen birden çok hizmet katmanında kullanılabilir. Bu SKU 'Lar, Azure 'daki özel Docker kayıt defterinizin kapasitesini ve kullanım düzenlerini hizalamak için öngörülebilir fiyatlandırma ve çeşitli seçenekler sağlar.

| SKU | Yönetilen | Açıklama |
| --- | :-------: | ----------- |
| **Temel** | Evet | Azure Container Registry hakkında bilgi edinen geliştiriciler için düşük maliyetli bir giriş noktası. Temel kayıt defterleri standart ve Premium ile aynı programlı yeteneklere sahiptir (Azure Active Directory [kimlik doğrulaması tümleştirmesi](container-registry-authentication.md#individual-login-with-azure-ad), [görüntü silme][container-registry-delete]ve [Web kancaları][container-registry-webhook]gibi). Ancak, dahil edilen depolama ve görüntü üretimi en düşük kullanım senaryoları için uygundur. |
| **Standart** | Evet | Standart kayıt defterleri, artırılmış dahil edilen depolama ve görüntü işleme özelliklerine sahip temel ile aynı özellikleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılayabilir. |
| **Premium** | Evet | Premium kayıt defterleri, yüksek hacimli senaryoları etkinleştirerek en yüksek miktarda dahil edilen depolama ve eşzamanlı işlem sağlar. Premium, daha yüksek görüntü işleme özelliklerine ek olarak, birden çok bölgede tek bir kayıt defterini yönetmek için [coğrafi çoğaltma][container-registry-geo-replication] , resim etiketi imzalama için [içerik güveni](container-registry-content-trust.md) ve [güvenlik duvarları ve sanal ağlar (Önizleme)](container-registry-vnet.md) gibi özellikler ekler. kayıt defterine erişimi kısıtlayın. |
|  Klasik (*2019 Nisan 'dan sonra kullanılamaz*) | Hayır | Bu SKU, Azure 'da Azure Container Registry hizmetinin ilk sürümünü etkinleştirdi. Klasik kayıt defterleri, Azure 'un aboneliğinizde oluşturduğu bir depolama hesabı tarafından desteklenir ve ACR 'nin daha yüksek aktarım hızı ve coğrafi çoğaltma gibi daha üst düzey yetenekler sağlamasına imkan tanır. |

> [!IMPORTANT]
> Klasik kayıt defteri SKU 'SU **kullanımdan kaldırılmıştır**ve **2019 Nisan**'dan sonra kullanılamayacaktır. Tüm yeni kayıt defterleri için temel, standart veya Premium kullanmanızı öneririz. Mevcut tüm klasik kayıt defterleri 2019 Nisan 'dan önce yükseltilmelidir. Yükseltme bilgileri için bkz. [Klasik bir kayıt defterini yükseltme][container-registry-upgrade].

Temel, standart ve Premium SKU 'Lar (toplu olarak *yönetilen kayıt defterleri*olarak adlandırılır) hepsi aynı programlı özellikleri sağlar. Ayrıca, tamamen Azure tarafından yönetilen [görüntü depolamadan][container-registry-storage] de faydalanır. Daha yüksek düzeyde bir SKU seçilmesi daha fazla performans ve ölçek sağlar. Birden çok hizmet katmanı ile, temel ile çalışmaya başlayabilir ve kayıt defteri kullanımınız arttıkça standart ve Premium 'a dönüştürebilirsiniz.

## <a name="sku-feature-matrix"></a>SKU özelliği matrisi

Aşağıdaki tabloda temel, standart ve Premium hizmet katmanlarının özellikleri ve limitleri ayrıntılı olarak verilmiştir.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKU 'Ları değiştirme

Azure CLı ile bir kayıt defteri SKU 'sunu veya Azure portal değiştirebilirsiniz. Geçiş yaptığınız SKU 'nun gerekli maksimum depolama kapasitesine sahip olduğu sürece, yönetilen SKU 'Lar arasında serbestçe geçiş yapabilirsiniz. Klasik bir şekilde yönetilen SKU 'Lardan birine geçtiğinizde, klasik ' e geri dönemezsiniz. tek yönlü bir dönüştürmedir.

### <a name="azure-cli"></a>Azure CLI

Azure CLı 'daki SKU 'Lar arasında geçiş yapmak için [az ACR Update][az-acr-update] komutunu kullanın. Örneğin, Premium 'a geçmek için:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure portal

Azure portal kapsayıcı kayıt defterine **genel bakış** ' da **Güncelleştir**' i seçin ve ardından SKU açılır listesinden yeni bir **SKU** seçin.

![Azure portal içinde kapsayıcı kayıt defteri SKU 'SU güncelleştirme][update-registry-sku]

Klasik bir kayıt defteriniz varsa Azure portal içinde yönetilen SKU seçemezsiniz. Bunun yerine, önce yönetilen bir kayıt defterine [yükseltmeniz][container-registry-upgrade] gerekir.

## <a name="pricing"></a>Fiyatlandırma

Azure Container Registry SKU 'Larının her biri hakkında fiyatlandırma bilgileri için bkz. [Container Registry fiyatlandırması][container-registry-pricing].

Veri aktarımlarına ilişkin fiyatlandırma hakkında ayrıntılı bilgi için bkz. [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Sonraki adımlar

**Azure Container Registry yol haritası**

Hizmette yaklaşan özellikler hakkında bilgi edinmek için GitHub 'daki [ACR yol haritasını][acr-roadmap] ziyaret edin.

**UserVoice Azure Container Registry**

[ACR UserVoice][container-registry-uservoice]'ta yeni özellik önerilerini gönderir ve oylayın.

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
