---
author: baanders
description: Azure dijital TWINS öğreticileri için dosya ekleme-örnek projeyi yapılandırma
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: f5b332415f1abf7d4c1002bdd4f3bfcef12f1267
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96023285"
---
## <a name="configure-the-sample-project"></a>Örnek projeyi yapılandırma

Ardından, Azure dijital TWINS örneğinizle etkileşime girebilen bir örnek istemci uygulaması ayarlayın.

Daha önce [*Azure dijital TWINS uçtan uca örnekleri*](/samples/azure-samples/digital-twins-samples/digital-twins-samples) (ve henüz yapmadıysanız) için makinenizde daha önce indirdiğiniz dosyaya gidin.

Klasörün içindeyken _AdtSampleApp_ adresine gidin. Visual Studio 2019 ' de _**AdtE2ESample. sln**_ 'yi açın. 

Visual Studio 'da, dosyayı Düzenle penceresinde açmak için dosya _üzerinde Sampleclientapp > **appsettings.js**_ seçin. Bu, projeyi çalıştırmak için gerekli yapılandırma değişkenlerine sahip bir önceden ayarlanmış JSON dosyası işlevi görür.

Dosya gövdesinde, öğesini `instanceUrl` Azure Digital TWINS örnek *ana bilgisayar adı* URL 'siyle değiştirin (aşağıda gösterildiği gibi *https://* önünde).

```json
{
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```

Dosyayı kaydedin ve kapatın. 

Sonra, *Sampleclientapp*' i oluştururken dosyayı çıkış dizinine kopyalamak üzere *appsettings.js* yapılandırın. Bunu yapmak için dosya *üzerindeappsettings.js* sağ seçin ve Özellikler ' i seçin *.* *Özellikler* denetçisinde, *Çıkış Dizinine Kopyala* özelliğinin değerini *daha yeniyse kopyala* olarak değiştirin.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Vurgulanan appsettings.jsiçeren Çözüm Gezgini bölmesini ve ' çıkış dizinine Kopyala ' özelliği olan Özellikler bölmesini &quot;daha yeniyse kopyala&quot; olarak gösteren Visual Studio penceresinden alıntı" border="false" lightbox="../articles/digital-twins/media/includes/copy-config.png":::

Öğreticide kullanmaya devam etmek için _**AdtE2ESample**_ projesini Visual Studio 'da açık tutun.

