---
title: Azure Depolama'da statik web sitesi barındırma
description: Modern Web uygulamalarını barındırmak için uygun maliyetli ve ölçeklenebilir bir çözüm sunan Azure Storage statik Web sitesi barındırma.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.author: normesta
ms.reviewer: dineshm
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 57ba59288cbf65c1ef588302965d480ee357ea4d
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82779986"
---
# <a name="static-website-hosting-in-azure-storage"></a>Azure Depolama'da statik web sitesi barındırma

*$Web*adlı bir depolama kapsayıcısından doğrudan statik IÇERIK (HTML, CSS, JavaScript ve resim dosyaları) hizmeti sağlayabilirsiniz. İçeriğinizi Azure Storage 'da barındırmak, [Azure işlevleri](/azure/azure-functions/functions-overview) ve diğer hizmet olarak platform (PaaS) hizmetlerini içeren sunucusuz mimariler kullanmanıza olanak sağlar.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

> [!NOTE]
> Siteniz sunucu tarafı koduna bağımlıysa bunun yerine [Azure App Service](/azure/app-service/overview) kullanın.

## <a name="setting-up-a-static-website"></a>Statik Web sitesi ayarlama

Statik Web sitesi barındırma, depolama hesabında etkinleştirmeniz gereken bir özelliktir.

Statik Web sitesi barındırmayı etkinleştirmek için, varsayılan dosyanızın adını seçin ve isteğe bağlı olarak özel bir 404 sayfasının yolunu belirtin. **$Web** adlı bir BLOB depolama kapsayıcısı hesapta zaten yoksa, sizin için bir tane oluşturulur. Sitenizin dosyalarını bu kapsayıcıya ekleyin.

Adım adım yönergeler için bkz. [Azure depolama 'da statik bir Web sitesi barındırma](storage-blob-static-website-how-to.md).

![Azure depolama statik Web siteleri ölçümleri ölçümü](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

**$Web** kapsayıcısındaki dosyalar, anonim erişim istekleri aracılığıyla sunulan ve yalnızca okuma işlemleri aracılığıyla erişilebilen büyük/küçük harfe duyarlıdır.

## <a name="uploading-content"></a>İçerik karşıya yükleniyor

**$Web** kapsayıcısına içerik yüklemek için bu araçlardan herhangi birini kullanabilirsiniz:

> [!div class="checklist"]
> * [Azure CLI](storage-blob-static-website-how-to.md?tabs=azure-cli)
> * [Azure PowerShell modülü](storage-blob-static-website-how-to.md?tabs=azure-powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Visual Studio Code uzantısı](/azure/javascript/tutorial-vscode-static-website-node-01)

## <a name="viewing-content"></a>İçeriği görüntüleme

Kullanıcılar Web sitesinin genel URL 'sini kullanarak bir tarayıcıdan site içeriğini görüntüleyebilir. Azure portal, Azure CLı veya PowerShell kullanarak URL 'YI bulabilirsiniz. Bu tabloyu kılavuz olarak kullanın.

|Araç| Rehber |
|----|----|
|**Azure portal** | [Azure portal kullanarak Web sitesi URL 'sini bulma](storage-blob-static-website-how-to.md#portal-find-url) |
|**Azure CLI** | [Azure CLı kullanarak Web sitesi URL 'sini bulma](storage-blob-static-website-how-to.md#cli-find-url) |
|**Azure PowerShell modülü** | [PowerShell kullanarak Web sitesi URL 'sini bulma](storage-blob-static-website-how-to.md#powershell-find-url) |

Sitenizin URL 'SI bölgesel bir kod içerir. Örneğin, URL `https://contosoblobaccount.z22.web.core.windows.net/` bölgesel kod `z22`içerir.

Bu kodun URL 'de kalması gerekir, ancak yalnızca iç kullanım içindir ve bu kodu başka bir şekilde kullanmak zorunda kalmazsınız.

Statik Web sitesi barındırmayı etkinleştirdiğinizde belirttiğiniz dizin belgesi, kullanıcılar siteyi açtıklarında ve belirli bir dosya belirtmezseniz görüntülenir (örneğin: `https://contosoblobaccount.z22.web.core.windows.net`).  

Sunucu bir 404 hatası döndürürse ve Web sitesini etkinleştirdiğinizde bir hata belgesi belirtmediğinde, kullanıcıya varsayılan bir 404 sayfası döndürülür.

> [!NOTE]
> [CORS](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) , statik Web sitesinde desteklenmez.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Web kapsayıcısının genel erişim düzeyini ayarlamanın etkisi

**$Web** kapsayıcısının genel erişim düzeyini değiştirebilirsiniz, ancak bu dosyalar Anonim erişim istekleri aracılığıyla sunulduğundan birincil statik Web sitesi uç noktası üzerinde hiçbir etkisi yoktur. Bu, tüm dosyalara genel (salt okuma) erişimi anlamına gelir.

Aşağıdaki ekran görüntüsünde Azure portal ortak erişim düzeyi ayarı gösterilmektedir:

![Portalda genel erişim düzeyinin nasıl ayarlanacağını gösteren ekran görüntüsü](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Birincil statik Web sitesi uç noktası etkilenmediğinden, genel erişim düzeyinde yapılan bir değişiklik birincil blob hizmeti uç noktasını etkiler.

Örneğin, **$Web** kapsayıcısının genel erişim düzeyini ( **Anonim erişim olmadan)** **BLOB 'a (yalnızca blob 'lar için anonim okuma erişimi**) değiştirirseniz, birincil statik Web sitesi uç noktasına `https://contosoblobaccount.z22.web.core.windows.net/index.html` genel erişim düzeyi değişmez.

Bununla birlikte, birincil blob hizmeti uç noktasına `https://contosoblobaccount.blob.core.windows.net/$web/index.html` genel erişim, Private iken public olarak değişir. Artık kullanıcılar bu iki uç noktanın birini kullanarak bu dosyayı açabilir.

## <a name="mapping-a-custom-domain-to-a-static-website-url"></a>Özel bir etki alanını statik bir Web sitesi URL 'siyle eşleme

Statik Web sitenizi özel bir etki alanı aracılığıyla kullanılabilir hale getirebilirsiniz. 

Azure Storage tarafından yerel olarak desteklendiğinden, özel etki alanınız için HTTP erişimini etkinleştirmek daha kolay olur. HTTPS 'yi etkinleştirmek için, Azure Storage özel etki alanları ile HTTPS 'yi henüz yerel olarak desteklemediğinden Azure CDN kullanmanız gerekir. adım adım yönergeler için bkz. [özel bir etki alanını Azure Blob depolama uç noktasına eşleme](storage-custom-domain-name.md) .

Depolama hesabı HTTPS üzerinden [Güvenli aktarım gerektirecek](../common/storage-require-secure-transfer.md) şekilde yapılandırıldıysa, kullanıcıların HTTPS uç noktasını kullanması gerekir. 

> [!TIP]
> Etki alanınızı Azure 'da barındırmayı düşünün. Daha fazla bilgi için bkz. [Azure DNS etki alanınızı barındırma](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="adding-http-headers"></a>HTTP üstbilgileri ekleme

Üstbilgileri statik Web sitesi özelliğinin bir parçası olarak yapılandırmanın bir yolu yoktur. Ancak üst bilgileri eklemek ve başlık değerlerini eklemek (veya üzerine yazmak) için Azure CDN kullanabilirsiniz. [Azure CDN Için standart kurallar altyapısı başvurusuna](https://docs.microsoft.com/azure/cdn/cdn-standard-rules-engine-reference)bakın.

Önbelleğe almayı denetlemek için üst bilgileri kullanmak istiyorsanız, bkz. önbelleğe alma [kurallarıyla denetim Azure CDN önbelleğe alma davranışı](https://docs.microsoft.com/azure/cdn/cdn-caching-rules).

## <a name="pricing"></a>Fiyatlandırma

Statik Web sitesi barındırmayı ücretsiz olarak etkinleştirebilirsiniz. Yalnızca sitenizin kullandığı BLOB depolama alanı ve operasyon maliyetlerine göre faturalandırılırsınız. Azure Blob depolama fiyatları hakkında daha fazla bilgi için [Azure Blob depolama fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/blobs/)göz atın.

## <a name="metrics"></a>Ölçümler

Statik Web sitesi sayfalarında ölçümleri etkinleştirebilirsiniz. Ölçümleri etkinleştirdikten sonra, **$Web** kapsayıcısındaki dosyalardaki trafik istatistikleri ölçüm panosunda raporlanır.

Statik Web sitesi sayfalarınızda ölçümleri etkinleştirmek için bkz. [statik Web sitesi sayfalarında ölçümleri etkinleştirme](storage-blob-static-website-how-to.md#metrics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure depolama 'da statik bir Web sitesi barındırma](storage-blob-static-website-how-to.md)
* [Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)
* [Azure İşlevleri](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [İlk sunucusuz Web uygulamanızı oluşturma](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Öğretici: Azure DNS’te etki alanınızı barındırma](../../dns/dns-delegate-domain-azure-dns.md)
