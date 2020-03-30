---
title: Azure Depolama'da statik web sitesi barındırma
description: Azure Depolama statik web sitesi barındırma, modern web uygulamaları barındırma için uygun maliyetli, ölçeklenebilir bir çözüm sağlar.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.author: normesta
ms.reviewer: dineshm
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 848fd89444281d82d6d0d1bfc4df15d499c09ee0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370500"
---
# <a name="static-website-hosting-in-azure-storage"></a>Azure Depolama'da statik web sitesi barındırma

Statik içeriği (HTML, CSS, JavaScript ve resim dosyaları) doğrudan *$web*adlı bir depolama kapsayıcısından servis edebilirsiniz. İçeriğinizi Azure Depolama'da barındırmak, [Azure İşlevlerini](/azure/azure-functions/functions-overview) ve diğer Platform'u hizmet (PaaS) hizmetleri olarak içeren sunucusuz mimarileri kullanmanıza olanak tanır.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

> [!NOTE]
> Siteniz sunucu tarafındaki koda bağlıysa, bunun yerine [Azure Uygulama Hizmeti'ni](/azure/app-service/overview) kullanın.

## <a name="setting-up-a-static-website"></a>Statik bir web sitesi kurma

Statik web sitesi barındırma depolama hesabında etkinleştirmek zorunda bir özelliktir.

Statik web sitesi barındırmayı etkinleştirmek için varsayılan dosyanızın adını seçin ve ardından isteğe bağlı olarak özel bir 404 sayfasına giden bir yol sağlayın. hesapta **$web** adında bir blob depolama kapsayıcısı yoksa, sizin için bir tane oluşturulur. Sitenizin dosyalarını bu kapsayıcıya ekleyin.

Adım adım kılavuzluk için Azure [Depolama'da statik bir web sitesi barındırma 'na](storage-blob-static-website-how-to.md)bakın.

![Azure Depolama statik web siteleri ölçümleri metrik](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

**$web** kapsayıcıdaki dosyalar büyük/küçük harf duyarlıdır, anonim erişim istekleri yle sunulur ve yalnızca okuma işlemleri yoluyla kullanılabilir.

## <a name="uploading-content"></a>İçerik yükleme

$web **kapsayıcısına** içerik yüklemek için aşağıdaki araçlardan herhangi birini kullanabilirsiniz:

> [!div class="checklist"]
> * [Azure CLI](storage-blob-static-website-how-to.md#cli)
> * [Azure PowerShell modülü](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Ardışık Hatları](https://azure.microsoft.com/services/devops/pipelines/)
> * [Visual Studio Code uzantısı](/azure/javascript/tutorial-vscode-static-website-node-01)

## <a name="viewing-content"></a>İçeriği görüntüleme

Kullanıcılar, web sitesinin genel URL'sini kullanarak site içeriğini bir tarayıcıdan görüntüleyebilir. URL'yi Azure portalını, Azure CLI'yi veya PowerShell'i kullanarak bulabilirsiniz. Bu tabloyu kılavuz olarak kullanın.

|Araç| Rehber |
|----|----|
|**Azure portalında** | [Azure portalını kullanarak web sitesinin URL'sini bulun](storage-blob-static-website-how-to.md#portal-find-url) |
|**Azure CLI** | [Azure CLI'yi kullanarak web sitesinin URL'sini bulun](storage-blob-static-website-how-to.md#cli-find-url) |
|**Azure PowerShell modülü** | [PowerShell'i kullanarak web sitesinin URL'sini bulun](storage-blob-static-website-how-to.md#powershell-find-url) |

Sitenizin URL'si bölgesel bir kod içerir. Örneğin URL `https://contosoblobaccount.z22.web.core.windows.net/` bölgesel kod `z22`içerir.

Bu kodun URL'de kalması gerekirken, yalnızca dahili kullanım içindir ve bu kodu başka bir şekilde kullanmanız gerekmez.

Statik web sitesi barındırmayı etkinleştirdiğinizde belirttiğiniz dizin belgesi, kullanıcılar siteyi açtığında görünür ve belirli `https://contosoblobaccount.z22.web.core.windows.net`bir dosya belirtmez (Örneğin: ).  

Sunucu bir 404 hatası döndürürve web sitesini etkinleştirdiğinizde bir hata belgesi belirtmediyseniz, varsayılan 404 sayfası kullanıcıya döndürülür.

> [!NOTE]
> [CORS](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) statik web sitesi ile desteklenmez.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Web kapsayıcısının genel erişim düzeyinin ayarlanmasının etkisi

**$web** kapsayıcının genel erişim düzeyini değiştirebilirsiniz, ancak bu dosyaların anonim erişim istekleri yoluyla sunulduğundan, birincil statik web sitesi bitiş noktası üzerinde hiçbir etkisi yoktur. Bu, tüm dosyalara herkese açık (salt okunur) erişim anlamına gelir.

Aşağıdaki ekran görüntüsü, Azure portalındaki genel erişim düzeyi ayarını gösterir:

![Portalda genel erişim düzeyinin nasıl ayarlanını gösteren ekran görüntüsü](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Birincil statik web sitesi bitiş noktası etkilenmezken, genel erişim düzeyinde yapılan bir değişiklik birincil blob hizmeti bitiş noktasını etkiler.

Örneğin, **$web** kapsayıcısının genel erişim düzeyini **Özel'den (anonim erişim yok)** **Blob'a (yalnızca bloblar için anonim okuma erişimi)** değiştirirseniz, birincil statik web sitesi bitiş noktasına `https://contosoblobaccount.z22.web.core.windows.net/index.html` genel erişim düzeyi değişmez.

Ancak, birincil blob hizmeti bitiş noktasına `https://contosoblobaccount.blob.core.windows.net/$web/index.html` genel erişim özelden genele değişir. Artık kullanıcılar bu iki uç noktadan birini kullanarak bu dosyayı açabilir.

## <a name="mapping-a-custom-domain-to-a-static-website-url"></a>Statik bir web sitesi URL'sine özel bir etki alanı eşleme

Statik web sitenizi özel bir etki alanı üzerinden kullanılabilir hale getirebilirsiniz. 

Azure Depolama yerel olarak desteklediğinden, özel etki alanınız için HTTP erişimini etkinleştirmek daha kolaydır. HTTPS'yi etkinleştirmek için Azure CDN kullanmanız gerekir, çünkü Azure Depolama henüz özel etki alanlarıyla HTTPS'yi yerel olarak desteklemez. Bkz. Adım adım kılavuzluk için [özel bir etki alanını Azure Blob Depolama bitiş noktasına eşleyin.](storage-custom-domain-name.md)

Depolama hesabı HTTPS üzerinden [güvenli aktarım gerektirecek](../common/storage-require-secure-transfer.md) şekilde yapılandırılırsa, kullanıcıların HTTPS bitiş noktasını kullanması gerekir. 

> [!TIP]
> Etki alanınızı Azure'da barındırmayı düşünün. Daha fazla bilgi için Azure [DNS'de etki alanınızı barındır'a](../../dns/dns-delegate-domain-azure-dns.md)bakın.

## <a name="adding-http-headers"></a>HTTP üstbilgi ekleme

Statik web sitesi özelliğinin bir parçası olarak üstleri yapılandırmanın bir yolu yoktur. Ancak, üstbilgi eklemek ve üstbilgi değerlerini eklemek (veya üzerine yazmak) için Azure CDN'yi kullanabilirsiniz. [Azure CDN için Standart kurallar motoru başvurusuna](https://docs.microsoft.com/azure/cdn/cdn-standard-rules-engine-reference)bakın.

Önbelleğe alma denetimi için üstbilgi kullanmak istiyorsanız, [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını](https://docs.microsoft.com/azure/cdn/cdn-caching-rules)denetle'ye bakın.

## <a name="pricing"></a>Fiyatlandırma

Statik web sitesi barındırma ücretsiz etkinleştirebilirsiniz. Yalnızca sitenizin kullandığı blob depolama alanı ve işletme maliyetleri için faturalandırılırsınız. Azure Blob Depolama fiyatları hakkında daha fazla bilgi için [Azure Blob Depolama Fiyatlandırma Sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/)göz atın.

## <a name="metrics"></a>Ölçümler

Statik web sitesi sayfalarında ölçümleri etkinleştirebilirsiniz. Ölçümleri etkinleştirdikten sonra, **$web** kapsayıcıdaki dosyalara ilişkin trafik istatistikleri ölçümler panosunda bildirilir.

Statik web sitesi sayfalarınızdaki ölçümleri etkinleştirmek için statik [web sitesi sayfalarındaki ölçümleri etkinleştir metotları etkinleştirme'ye](storage-blob-static-website-how-to.md#metrics)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama'da statik bir web sitesi barındırma](storage-blob-static-website-how-to.md)
* [Azure Blob Depolama bitiş noktasıyla özel bir etki alanı haritası](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure/azure-functions/functions-overview)
* [Azure Uygulama Hizmeti](/azure/app-service/overview)
* [İlk sunucusuz web uygulamanızı oluşturun](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Öğretici: Azure DNS’te etki alanınızı barındırma](../../dns/dns-delegate-domain-azure-dns.md)
