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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76021256"
---
1. Bir Azure Sinyal Hizmeti kaynağı oluşturmak için önce [Azure portalında](https://portal.azure.com)oturum açın. Sayfanın sol üst tarafında + **Kaynak oluştur'u**seçin. Pazar Ara metin **kutusuna** **SignalR Hizmeti**girin.

2. Sonuçlarda **SignalR Hizmeti'ni** seçin ve **Oluştur'u**seçin.

3. Yeni SignalR ayarları sayfasında, yeni **SignalR** kaynağınız için aşağıdaki ayarları ekleyin:

    | Adı | Önerilen değer | Açıklama |
    | ---- | ----------------- | ----------- |
    | Kaynak adı | *testsignalr* | SignalR kaynağı için kullanılacak benzersiz kaynak adını girin. Ad 1 ile 63 karakterarasında bir dize olmalı ve yalnızca sayılar,`-`harfler ve tire ( ) karakteri içermelidir. Ad tire karakteriyle başlayamaz veya sonlanamaz ve ardışık tire karakterleri geçerli değildir.|
    | Abonelik | Aboneliğinizi seçin |  SignalR testi için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa, otomatik olarak seçilir ve **Abonelik** açılır açılır ifadesi görüntülenmez.|
    | Kaynak grubu | *SignalRTestResources* adında bir kaynak grubu oluşturma| SignalR kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğin birden çok kaynağı düzenlemek için yararlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/management/overview.md). |
    | Konum | *Doğu ABD* | SignalR kaynağınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. En iyi performans için kaynağınızı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturmanızı öneririz. |
    | Fiyatlandırma katmanı | *Ücretsiz* | Şu **anda, Ücretsiz** ve **Standart** seçenekler mevcuttur. |
    | Panoya sabitle | ✔ | Kaynağın panonuza sabitlenebilmeleri için bu kutuyu seçin, böylece daha kolay bulunun. |

4. **Oluştur'u**seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra **AYARLAR**altında **Tuşları** seçin. Birincil anahtar için bağlantı dizenizi kopyalayın. Bu dizeyi daha sonra uygulamanızı Azure Sinyal Hizmeti kaynağını kullanacak şekilde yapılandırmak için kullanırsınız.

    Bağlantı dizesi aşağıdaki şekildedir:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
