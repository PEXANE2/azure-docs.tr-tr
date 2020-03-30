---
title: Gömme İnternet Çözümleyici İstemci | Microsoft Dokümanlar
description: Bu makalede, internet çözümleyicijavascript istemcisini uygulamanıza nasıl gömeriz öğrenin.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: quickstart
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: f9ecb8d731945847160b49c68c554fafdd7285d9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74896377"
---
# <a name="embed-the-internet-analyzer-client"></a>Internet Analyzer istemcisini gömme

Bu makalede, uygulamanıza JavaScript istemcisini nasıl gömersiniz gösterilmektedir. Bu istemcinin yüklenmesi testleri çalıştırmak ve karne analizi almak için gereklidir. **Profile özel JavaScript istemcisi, ilk test yapılandırıldıktan sonra sağlanır.** Buradan, yeni bir komut dosyası katıştırmak zorunda kalmadan bu profile testler eklemeye veya kaldırmaya devam edebilirsiniz. Internet Analyzer hakkında daha fazla bilgi için [genel bakışa](internet-analyzer-overview.md)bakın. 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Internet Analyzer'ın düzgün çalışması için Azure ve diğer Microsoft hizmetlerine erişim gerektirir. İstemciyi gömmeden önce, lütfen ağ erişimine `fpc.msedge.net` ve önceden yapılandırılmış uç nokta URL'lerine [(CLI](internet-analyzer-cli.md)aracılığıyla görünür) izin verin.

## <a name="find-the-client-script-url"></a>İstemci komut dosyası URL'sini bulma

Komut dosyası URL'si, bir test yapıldıktan sonra Azure portalı veya Azure CLI aracılığıyla bulunabilir. Daha fazla bilgi için [bkz.](internet-analyzer-create-test-portal.md)

1. Seçenek Azure portalında, Azure Internet Analyzer için önizleme portalı sayfasını açmak için [bu bağlantıyı](https://aka.ms/InternetAnalyzerPreviewPortal) kullanın. **Ayarlar > Yapılandırma'ya**giderek komut dosyası URL'sini görmek için Internet Analyzer profilinize gidin.

2. Seçenek Azure CLI'yi kullanarak `scriptFileUri` Özelliği denetleyin.
```azurecli-interactive
    az extension add --name internet-analyzer    
    az internet-analyzer test list --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile"
```

## <a name="client-details"></a>İstemci ayrıntıları

Komut dosyası profiliniz ve testler için özel olarak oluşturulur. Yüklendikten sonra komut dosyası 2 saniyelik bir gecikmeyle yürütülür. Önce, testlerinizde yapılandırılan uç noktaların listesini almak için Internet Analyzer hizmetiyle bağlantı kurun. Daha sonra ölçümleri çalıştırır ve zamanlanmış sonuçları Internet Analyzer hizmetine geri yükler.

## <a name="client-examples"></a>İstemci örnekleri

Bu örnekler, istemci JavaScript'i web sayfanıza veya uygulamanıza yerleştirmek için birkaç temel yöntem gösterir. Komut `0bfcb32638b44927935b9df86dcfe397` dosyası URL'si için örnek profil kimliği olarak kullanırız.

### <a name="run-on-page-load"></a>Sayfa yükünde çalıştır
En basit yöntem, meta etiket bloğunun içindeki komut dosyası etiketini kullanmaktır. Bu etiket, sayfa başına bir kez komut dosyası yürütülür.

```html
<html>
<meta>
    <script src="//fpc.msedge.net/client/v2/0bfcb32638b44927935b9df86dcfe397/ab.min.js"></script>
</meta>
<body></body>
</html>
```

## <a name="next-steps"></a>Sonraki adımlar

Internet [Analyzer SSS'yi](internet-analyzer-faq.md) okuyun
