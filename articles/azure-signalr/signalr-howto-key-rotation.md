---
title: Azure SignalR Hizmeti için erişim anahtarını döndürme
description: Müşterinin erişim anahtarlarını neden rutin olarak döndürmesi gerektiği ve Azure portalı GUI ve Azure CLI ile nasıl yapacağına genel bir bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 133edc64ac2f858a397a4a184c24497dae8af333
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67565736"
---
# <a name="how-to-rotate-access-key-for-azure-signalr-service"></a>Azure SignalR Hizmeti için erişim anahtarını döndürme

Her Azure Sinyal Hizmeti örneğinin Birincil ve İkincil anahtarlar olarak adlandırılan bir çift erişim anahtarı vardır. Hizmete isteklerde bulunulduğunda SignalR istemcilerinin kimliğini doğrulamak için kullanılırlar. Anahtarlar, örnek bitiş noktası url'si ile ilişkilidir. Anahtarlarınızı güvende tutun ve düzenli olarak döndürün. Size iki erişim anahtarı sağlanır, böylece diğerini yenilerken bir tuşu kullanarak bağlantıları koruyabilirsiniz.

## <a name="why-rotate-access-keys"></a>Erişim anahtarlarını neden döndürün?

Güvenlik nedenleriyle ve uyumluluk gereksinimleri için erişim anahtarlarınızı düzenli olarak döndürün.

## <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluşturma

1. [Azure portalına](https://portal.azure.com/)gidin ve kimlik bilgilerinizle oturum açın.

1. Yeniden oluşturmak istediğiniz anahtarlarla Azure Sinyal Hizmeti örneğinde **Keys** bölümünü bulun.

1. Gezinti menüsünde **Keys'i** seçin.

1. **Birincil Anahtarı Yeniden Oluştur'u** seçin veya **İkincil Anahtarı Yeniden Oluşturun.**

   Yeni bir anahtar ve karşılık gelen bağlantı dizesi oluşturulur ve görüntülenir.

   ![Anahtarları Yeniden Oluştur](media/signalr-howto-key-rotation/regenerate-keys.png)

Ayrıca [Azure CLI'yi](/cli/azure/signalr/key?view=azure-cli-latest#az-signalr-key-renew)kullanarak tuşları yeniden oluşturabilirsiniz.

## <a name="update-configurations-with-new-connection-strings"></a>Yeni bağlantı dizeleri ile yapılandırmaları güncelleştirme

1. Yeni oluşturulan bağlantı dizesini kopyalayın.

1. Yeni bağlantı dizesini kullanmak için tüm yapılandırmaları güncelleştirin.

1. Uygulamayı gerektiği gibi yeniden başlatın.

## <a name="forced-access-key-regeneration"></a>Zorunlu erişim anahtar rejenerasyonu

Azure SignalR Hizmeti, belirli durumlarda zorunlu erişim anahtarı rejenerasyonu uygulayabilir. Hizmet, e-posta ve portal bildirimi yoluyla müşterileri bildirir. Bu iletişimi alırsanız veya bir erişim anahtarı nedeniyle hizmet hatasıyla karşılaşırsanız, bu kılavuzdaki yönergeleri izleyerek tuşları döndürün.

## <a name="next-steps"></a>Sonraki adımlar

Erişim anahtarlarınızı iyi bir güvenlik uygulaması olarak düzenli olarak döndürün.

Bu kılavuzda, erişim anahtarlarını nasıl yeniden oluşturacağınızı öğrendiniz. OAuth veya Azure İşlevleri ile kimlik doğrulama yla ilgili sonraki öğreticilere devam edin.

> [!div class="nextstepaction"]
> [ASP.NET temel kimlikle tümleştirme](./signalr-concept-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Kimlik doğrulaması yla sunucusuz gerçek zamanlı bir uygulama oluşturun](./signalr-tutorial-authenticate-azure-functions.md)