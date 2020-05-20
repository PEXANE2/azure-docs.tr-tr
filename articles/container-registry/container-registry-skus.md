---
title: Kayıt defteri hizmeti katmanları ve özellikleri
description: Azure Container Registry temel, standart ve Premium hizmet katmanlarında (SKU 'Lar) Özellikler ve sınırlar hakkında bilgi edinin.
ms.topic: article
ms.date: 05/18/2020
ms.openlocfilehash: 35f5d4ebd4a2b427aadc6e82e265a7da9b6409f8
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683444"
---
# <a name="azure-container-registry-service-tiers"></a>Azure Container Registry hizmet katmanları

Azure Container Registry birden çok hizmet katmanında (STB olarak da bilinir) kullanılabilir. Bu katmanlar, Azure 'daki özel Docker kayıt defterinizin kapasitesini ve kullanım düzenlerini hizalamak için öngörülebilir fiyatlandırma ve çeşitli seçenekler sağlar.

| Katman | Açıklama |
| --- | ----------- |
| **Temel** | Azure Container Registry hakkında bilgi edinen geliştiriciler için düşük maliyetli bir giriş noktası. Temel kayıt defterleri standart ve Premium ile aynı programlı yeteneklere sahiptir (Azure Active Directory [kimlik doğrulaması tümleştirmesi](container-registry-authentication.md#individual-login-with-azure-ad), [görüntü silme][container-registry-delete]ve [Web kancaları][container-registry-webhook]gibi). Ancak, dahil edilen depolama ve görüntü üretimi en düşük kullanım senaryoları için uygundur. |
| **Standart** | Standart kayıt defterleri, artırılmış dahil edilen depolama ve görüntü işleme özelliklerine sahip temel ile aynı özellikleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılayabilir. |
| **Premium** | Premium kayıt defterleri, yüksek hacimli senaryoları etkinleştirerek en yüksek miktarda dahil edilen depolama ve eşzamanlı işlem sağlar. Premium, daha yüksek görüntü işleme özelliklerine ek olarak, birden çok bölgede tek bir kayıt defterini yönetmek için [coğrafi çoğaltma][container-registry-geo-replication] , resim etiketi imzalama için [içerik güveni](container-registry-content-trust.md) , özel [uç noktaları olan özel bağlantı](container-registry-private-link.md) kayıt defterine erişimi kısıtlamak gibi özellikler ekler. |

Temel, standart ve Premium katmanları, programlama özelliklerini de sağlar. Ayrıca, tamamen Azure tarafından yönetilen [görüntü depolamadan][container-registry-storage] de faydalanır. Daha yüksek düzeyde bir katman seçmek daha fazla performans ve ölçek sağlar. Birden çok hizmet katmanı ile, temel ile çalışmaya başlayabilir ve kayıt defteri kullanımınız arttıkça standart ve Premium 'a dönüştürebilirsiniz.

## <a name="service-tier-features-and-limits"></a>Hizmet katmanı özellikleri ve sınırları

Aşağıdaki tabloda temel, standart ve Premium hizmet katmanlarının özellikleri ve limitleri ayrıntılı olarak verilmiştir.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-tiers"></a>Katmanları değiştirme

Bir kayıt defterinin hizmet katmanını Azure CLı ile veya Azure portal değiştirebilirsiniz. Geçiş yaptığınız katman gerekli maksimum depolama kapasitesine sahip olduğu sürece katman arasında serbestçe geçiş yapabilirsiniz. 

### <a name="azure-cli"></a>Azure CLI

Azure CLı 'deki hizmet katmanları arasında gezinmek için [az ACR Update][az-acr-update] komutunu kullanın. Örneğin, Premium 'a geçmek için:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure portal

Azure portal kapsayıcı kayıt defterine **genel bakış** ' da **Güncelleştir**' i seçin ve ardından SKU açılır listesinden yeni bir **SKU** seçin.

![Azure portal içinde kapsayıcı kayıt defteri SKU 'SU güncelleştirme][update-registry-sku]

## <a name="pricing"></a>Fiyatlandırma

Azure Container Registry hizmet katmanlarındaki fiyatlandırma bilgileri için bkz. [Container Registry fiyatlandırması][container-registry-pricing].

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
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
