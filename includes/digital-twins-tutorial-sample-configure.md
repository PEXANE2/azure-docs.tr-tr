---
author: baanders
description: Azure dijital TWINS öğreticileri için dosya ekleme-örnek projeyi yapılandırma
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 2a62cae025be05a8f5a6ef4407737ab8da36a951
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92210787"
---
## <a name="configure-the-sample-project"></a>Örnek projeyi yapılandırma

Ardından, Azure dijital TWINS örneğinizle etkileşime girebilen bir örnek istemci uygulaması ayarlayın.

Daha önce [*Azure dijital TWINS uçtan uca örnekleri*](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples) (ve henüz yapmadıysanız) için makinenizde daha önce indirdiğiniz dosyaya gidin.

Klasörün içindeyken _AdtSampleApp_adresine gidin. Visual Studio 2019 ' de _**AdtE2ESample. sln**_ 'yi açın. 

Visual Studio 'da, *Solution Explorer* _üzerinde sampleclientapp >serviceConfig.jsbir kopyasını oluşturmak için Çözüm Gezgini bölmesini kullanın **. Şablon** _ dosyası (kopyalamak ve yapıştırmak için sağ seçme menülerini kullanabilirsiniz). Kopyalama *serviceConfig.js*yeniden adlandırın. Bu, projeyi çalıştırmak için gerekli yapılandırma değişkenlerine sahip bir önceden ayarlanmış JSON dosyası işlevi görür.

Dosyayı Düzenle penceresinde açmak için *serviceConfig.js* seçin. ' İ `instanceUrl` Azure dijital TWINS örnek *ana bilgisayar* URL 'si ile değiştirin (aşağıda gösterildiği gibi *https://* önünde).

```json
{
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```



Dosyayı kaydedin ve kapatın. 

Sonra, *Sampleclientapp*' i oluştururken dosyayı çıkış dizinine kopyalamak üzere *serviceConfig.js* yapılandırın. Bunu yapmak için dosya * üzerindeserviceConfig.js* sağ seçin ve Özellikler ' i seçin *.* *Özellikler* denetçisinde, *Çıkış Dizinine Kopyala* özelliğinin değerini *daha yeniyse kopyala*olarak değiştirin.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Vurgulanan serviceConfig.jsiçeren Çözüm Gezgini bölmesini ve ' çıkış dizinine Kopyala ' özelliği olan Özellikler bölmesini &quot;daha yeniyse kopyala&quot; olarak gösteren Visual Studio penceresinden alıntı" border="false":::

Öğreticide kullanmaya devam etmek için _**AdtE2ESample**_ projesini Visual Studio 'da açık tutun.

