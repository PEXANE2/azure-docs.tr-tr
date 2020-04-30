---
title: include dosyası
description: include dosyası
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 39bff26baea622e6c0ed524ca68c3c8bae4e770d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76021256"
---
1. Bir Azure SignalR hizmeti kaynağı oluşturmak için öncelikle [Azure Portal](https://portal.azure.com)oturum açın. Sayfanın sol üst kısmında **+ kaynak oluştur**' u seçin. Market 'te **Ara** metin kutusuna **SignalR hizmeti**girin.

2. Sonuçlarda **SignalR hizmetini** seçin ve **Oluştur**' u seçin.

3. Yeni **SignalR** ayarları sayfasında, yeni SignalR kaynağınız için aşağıdaki ayarları ekleyin:

    | Adı | Önerilen değer | Açıklama |
    | ---- | ----------------- | ----------- |
    | Kaynak adı | *testsignalr* | SignalR kaynağı için kullanılacak benzersiz kaynak adını girin. Ad, 1 ile 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve kısa çizgi (`-`) karakteri içermelidir. Ad, tire karakteriyle başlayamaz veya bitmez ve ardışık çizgi karakterleri geçerli değildir.|
    | Abonelik | Aboneliğinizi seçin |  SignalR testi için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa, bu otomatik olarak seçilir ve **abonelik** açılan düzeyi gösterilmez.|
    | Kaynak grubu | *Signalrtestresources* adlı bir kaynak grubu oluşturun| SignalR kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğiniz birden çok kaynağı düzenlemek için yararlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/management/overview.md). |
    | Konum | *Doğu ABD* | SignalR kaynağınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. En iyi performans için kaynağınızı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturmanızı öneririz. |
    | Fiyatlandırma katmanı | *Ücretsiz* | Şu anda, **ücretsiz** ve **Standart** seçenekler kullanılabilir. |
    | Panoya sabitle | ✔ | Daha kolay bulabilmek için kaynağın panonuza sabitlendiği bu kutuyu seçin. |

4. **Oluştur**’u seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra **Ayarlar**altında **anahtarlar** ' ı seçin. Birincil anahtar için Bağlantı dizenizi kopyalayın. Bu dizeyi daha sonra uygulamanızı Azure SignalR hizmeti kaynağını kullanacak şekilde yapılandırmak için kullanacaksınız.

    Bağlantı dizesi aşağıdaki şekildedir:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
