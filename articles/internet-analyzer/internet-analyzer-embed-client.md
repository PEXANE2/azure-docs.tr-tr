---
title: Internet çözümleyici Istemcisini Ekle | Microsoft Docs
description: Bu makalede, uygulamanıza Internet Çözümleyicisi JavaScript istemcisini nasıl katıştıracağınızı öğrenin.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: how-to
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 0d4b27b85ac7bc61e14a79f29e4e26ec4973ced1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84744060"
---
# <a name="embed-the-internet-analyzer-client"></a>Internet çözümleyici istemcisini katıştırma

Bu makalede JavaScript istemcisini uygulamanıza nasıl katıştırabileceğinizi gösterir. Bu istemcinin yüklenmesi, testleri çalıştırmak ve karne analizlerini almak için gereklidir. **Profile özgü JavaScript istemcisi, ilk test yapılandırıldıktan sonra sağlanır.** Buradan, yeni bir komut dosyası eklemeye gerek kalmadan bu profile test ekleme veya kaldırma işlemine devam edebilirsiniz. Internet çözümleyici hakkında daha fazla bilgi için bkz. [genel bakış](internet-analyzer-overview.md). 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Internet Çözümleyicisi 'nin doğru çalışması için Azure 'a ve diğer Microsoft hizmetlerine erişimi olması gerekir. `fpc.msedge.net`İstemciyi gömmeden önce lütfen ve önceden yapılandırılmış tüm uç nokta URL 'lerine ( [CLI](internet-analyzer-cli.md)aracılığıyla görünür) ağ erişimine izin verin.

## <a name="find-the-client-script-url"></a>İstemci komut dosyası URL 'sini bulma

Betik URL 'SI, bir test yapılandırıldıktan sonra Azure portal veya Azure CLı aracılığıyla bulunabilir. Daha fazla bilgi için bkz. [Internet çözümleyici kaynağı oluşturma](internet-analyzer-create-test-portal.md).

1. Seçenek Azure portal, [Bu bağlantıyı](https://aka.ms/InternetAnalyzerPreviewPortal) kullanarak Azure Internet Çözümleyicisi için Önizleme portalı sayfasını açın. **Ayarlar > yapılandırması**' na gıderek betik URL 'sini görmek Için Internet Çözümleyicisi profilinize gidin.

2. Seçenek Azure CLı 'yı kullanarak `scriptFileUri` özelliği denetleyin.
```azurecli-interactive
    az extension add --name internet-analyzer    
    az internet-analyzer test list --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile"
```

## <a name="client-details"></a>İstemci ayrıntıları

Betik, özel olarak profil ve testleriniz için oluşturulur. Yüklendikten sonra betik 2 saniyelik bir gecikmeyle yürütülür. İlk olarak, testlerinizde yapılandırılan uç noktaların listesini getirmek için Internet Çözümleyicisi hizmetiyle iletişim kurar. Daha sonra ölçümleri çalıştırır ve zamanlanan sonuçları Internet çözümleyici hizmetine geri yükler.

## <a name="client-examples"></a>İstemci örnekleri

Bu örneklerde, istemci JavaScript 'ı Web sayfanıza veya uygulamanıza eklemek için birkaç temel yöntem gösterilmektedir. `0bfcb32638b44927935b9df86dcfe397`Betik URL 'si için örnek bir PROFIL kimliği olarak kullanıyoruz.

### <a name="run-on-page-load"></a>Sayfa yükleme sırasında Çalıştır
En basit yöntem, meta etiketi bloğunun içindeki komut dosyası etiketini kullanmaktır. Bu etiket, sayfa yüküne göre betiği çalıştırır.

```html
<html>
<meta>
    <script src="//fpc.msedge.net/client/v2/0bfcb32638b44927935b9df86dcfe397/ab.min.js"></script>
</meta>
<body></body>
</html>
```

## <a name="next-steps"></a>Sonraki adımlar

[Internet ÇÖZÜMLEYICISI SSS](internet-analyzer-faq.md) makalesini okuyun
