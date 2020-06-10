---
author: baanders
description: Azure dijital TWINS öğreticileri için dosya ekleme-örnek projeyi yapılandırma
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: f7b3bf347b15ebad2403f3aa64c0f3fd6c4be052
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84613539"
---
## <a name="configure-the-sample-project"></a>Örnek projeyi yapılandırma

Ardından, Azure dijital TWINS örneğinizle etkileşime girebilen bir örnek istemci uygulaması ayarlayın. Örnek projeyi henüz indirmediyseniz [Azure dijital TWINS örnekleri deposunu BIR ZIP dosyası olarak indirerek](https://github.com/Azure-Samples/digital-twins-samples/archive/master.zip)bunu hemen alın. 

Makinenizde indirilen dosyaya gidin ve dosyayı ayıklayın.

Sıkıştırılmış klasörün içinden bir kez, _dijital-TWINS-örnekler-Master/AdtSampleApp/_ dizinine gidin. Visual Studio 2019 ' de _**AdtE2ESample. sln**_ 'yi açın. 

Visual Studio 'da, _Sampleclientapp > **ServiceConfig. JSON. Template** _ dosyasının bir kopyasını oluşturmak için *Çözüm Gezgini* bölmesini kullanın (kopyalamak ve yapıştırmak için sağ seçme menülerini kullanabilirsiniz). Copy *ServiceConfig. JSON*dosyasını yeniden adlandırın. Bu, projeyi çalıştırmak için gerekli yapılandırma değişkenlerine sahip bir önceden ayarlanmış JSON dosyası işlevi görür.

Yeni dosyayı, Düzen penceresinde açmak için seçin. ' I `tenantId` *Dizin Kimliğinizle*, `clientId` *uygulama Kimliğinizle*ve `instanceUrl` Azure dijital TWINS örnek *ana bilgisayar adı* URL 'si ile değiştirin (aşağıda gösterildiği gibi *https://* önünde).

```json
{
  "tenantId": "<your-directory-ID>",
  "clientId": "<your-application-ID>",
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```

Dosyayı kaydedin ve kapatın. 

Ardından, *Sampleclientapp*oluştururken çıkış dizinine kopyalanacak *ServiceConfig. JSON* dosyasını yapılandırın. Bunu yapmak için *ServiceConfig. JSON* dosyasını sağ seçin ve Özellikler ' i seçin *.* *Özellikler* denetçisinde, *Çıkış Dizinine Kopyala* özelliğinin değerini *daha yeniyse kopyala*olarak değiştirin.

:::image type="content" source="../articles/digital-twins/media/include-tutorial/copy-config.png" alt-text="ServiceConfig. JSON ile Çözüm Gezgini bölmesini gösteren ve ' çıkış dizinine Kopyala ' özelliği olan Özellikler bölmesinin ' daha yeniyse kopyala ' olarak ayarlandığı Visual Studio penceresinden alıntı" border="false":::

Öğreticide kullanmaya devam etmek için _**AdtE2ESample**_ projesini Visual Studio 'da açık tutun.

