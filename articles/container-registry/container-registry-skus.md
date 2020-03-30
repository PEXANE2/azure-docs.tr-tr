---
title: Hizmet katmanları ve SK'ler
description: Azure Konteyner Kayıt Defteri'nin Temel, Standart ve Premium hizmet katmanlarındaki (SK'ler) özellikler ve sınırlar hakkında bilgi edinin.
ms.topic: article
ms.date: 11/05/2019
ms.openlocfilehash: 1ebe5339b7523a4463dee45b126244d7ec5b2e4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74456277"
---
# <a name="azure-container-registry-skus"></a>Azure Konteyner Kayıt Defteri SK'ları

Azure Kapsayıcı Kayıt Defteri (ACR), SUS olarak bilinen birden çok hizmet katmanında kullanılabilir. Bu SK'ler, Azure'daki özel Docker kayıt defterinizin kapasite ve kullanım alışkanlıklarına hizalamak için öngörülebilir fiyatlandırma ve çeşitli seçenekler sunar.

| SKU | Açıklama |
| --- | ----------- |
| **Temel** | Azure Container Registry hakkında bilgi edinen geliştiriciler için düşük maliyetli bir giriş noktası. Temel kayıt defterleri Standart ve Premium (Azure Active Directory [kimlik doğrulama tümleştirmesi,](container-registry-authentication.md#individual-login-with-azure-ad) [görüntü silme][container-registry-delete]ve [webhooks][container-registry-webhook]gibi) ile aynı programlı özelliklere sahiptir. Ancak, dahil edilen depolama ve görüntü verimi daha düşük kullanım senaryoları için en uygundur. |
| **Standart** | Standart kayıt defterleri, artan depolama ve görüntü işleriyle Temel ile aynı yetenekleri sunar. Standart kayıt defterleri, çoğu üretim senaryosu gereksinimlerini karşılayabilir. |
| **Premium** | Premium kayıt defterleri, yüksek hacimli senaryolar sağlayarak en yüksek miktarda dahil edilen depolama ve eşzamanlı işlemleri sağlar. Premium, daha yüksek görüntü elde edilmesine ek olarak, birden çok bölgede tek bir kayıt defterini yönetmek için [coğrafi çoğaltma,][container-registry-geo-replication] resim etiketi imzalama için [içerik güveni,](container-registry-content-trust.md) güvenlik duvarları ve kayıt defterine erişimi kısıtlamak için [sanal ağlar (önizleme)](container-registry-vnet.md) gibi özellikler ekler. |

Temel, Standart ve Premium SK'lerin tümü aynı programlı özellikleri sağlar. Ayrıca tümü tamamen Azure tarafından yönetilen [görüntü depolamadan][container-registry-storage] da yararlanır. Daha üst düzey bir SKU seçmek daha fazla performans ve ölçek sağlar. Birden çok hizmet katmanıyla, Temel ile başlayıp kayıt defteri kullanımınız arttıkça Standart ve Premium'a dönüştürebilirsiniz.

## <a name="sku-features-and-limits"></a>SKU özellikleri ve sınırları

Aşağıdaki tabloda Temel, Standart ve Premium hizmet katmanlarının özellikleri ve sınırları ayrıntılı olarak anlatılır.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKS Değiştirme

Bir kayıt defterinin SKU'sını Azure CLI ile veya Azure portalında değiştirebilirsiniz. Geçiş yaptığınız SKU gerekli maksimum depolama kapasitesine sahip olduğu sürece SKU'lar arasında serbestçe dolaşabilirsiniz. 

### <a name="azure-cli"></a>Azure CLI

Azure CLI'deki SK'ler arasında ilerlemek için [az acr güncelleştirme][az-acr-update] komutunu kullanın. Örneğin, Premium'a geçmek için:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure portalında

Azure portalındaki konteyner kayıt **defterine Genel** Bakış'ta, **Güncelleştirme'yi**seçin ve ardından SKU açılır durumundan yeni bir **SKU** seçin.

![Azure portalında konteyner kayıt defteri SKU'su güncelleştirme][update-registry-sku]

## <a name="pricing"></a>Fiyatlandırma

Azure Konteyner Kayıt Defteri SK'lerinin her biri hakkındaki fiyatlandırma bilgileri [için, Konteyner Kayıt Defteri fiyatlandırması][container-registry-pricing]bölümüne bakın.

Veri aktarımları için fiyatlandırma hakkında ayrıntılı bilgi için Bant [Genişliği Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/bandwidth/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

**Azure Konteyner Kayıt Yol Haritası**

Hizmette gelecek özellikler hakkında bilgi edinmek için GitHub'daki [ACR Yol Haritası'nı][acr-roadmap] ziyaret edin.

**Azure Konteyner Kayıt Defteri KullanıcısıSes**

[ACR UserVoice'ta][container-registry-uservoice]yeni özellik önerileri gönderin ve oyverin.

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
