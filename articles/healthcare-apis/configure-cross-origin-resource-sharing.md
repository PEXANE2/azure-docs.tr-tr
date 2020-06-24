---
title: Azure API 'de FHıR için çıkış noktaları arası kaynak paylaşımını yapılandırma
description: Bu makalede, FHıR için Azure API 'de, çıkış noktaları arası kaynak paylaşımının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.author: matjazl
ms.date: 3/11/2019
ms.topic: reference
ms.service: healthcare-apis
ms.subservice: fhir
ms.openlocfilehash: dc4c034b0821f1fe5ecb940591fca77d61edc3ce
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "84871753"
---
# <a name="configure-cross-origin-resource-sharing-in-azure-api-for-fhir"></a>Azure API 'de FHıR için çıkış noktaları arası kaynak paylaşımını yapılandırma

Hızlı sağlık ile birlikte çalışabilirlik kaynakları (FHıR) için Azure API, [çıkış noktaları arası kaynak paylaşımını (CORS)](https://wikipedia.org/wiki/Cross-Origin_Resource_Sharing)destekler. CORS, bir etki alanındaki (Origin) uygulamaların, etki alanları arası istek olarak bilinen farklı bir etki alanındaki kaynaklara erişebilmesi için ayarları yapılandırmanıza olanak tanır.

CORS genellikle farklı bir etki alanına yeniden bir API çağrısı gereken tek sayfalı bir uygulamada kullanılır.

FHıR için Azure API 'sinde bir CORS ayarı yapılandırmak için aşağıdaki ayarları belirtin:

- **Çıkış (erişim-denetim-izin-kaynak)**. FHıR için Azure API 'sine çapraz kaynak istekleri yapmasına izin verilen etki alanlarının listesi. Her etki alanının (Origin) ayrı bir satıra girilmesi gerekir. Herhangi bir etki alanından çağrıya izin vermek için bir yıldız işareti (*) girebilirsiniz, ancak bu, bir güvenlik riski olduğundan bunu önermiyoruz.

- **Üst bilgiler (erişim-denetim-izin-üstbilgiler)**. Kaynak isteğin içereceği üst bilgilerin bir listesi. Tüm üstbilgilere izin vermek için yıldız işareti (*) girin.

- **Yöntemler (erişim-denetim-Izin verme-Yöntemler)**. Bir API çağrısında izin verilen Yöntemler (PUT, GET, POST, vb.). Tüm yöntemler için **Tümünü Seç ' i** seçin.

- **Maksimum yaş (erişim-denetim-en yüksek yaş)**. Erişim-denetim-Izin-üst bilgiler ve erişim-denetim-Izin verme yöntemleriyle ilgili ön kontrol isteği sonuçlarını önbelleğe almak için saniye cinsinden değer.

- **Kimlik bilgilerine Izin ver (erişim-denetim-izin-kimlik bilgileri)**. CORS istekleri normalde [siteler arası istek sahteciliği (CSRF)](https://en.wikipedia.org/wiki/Cross-site_request_forgery) saldırılarını önlemeye yönelik tanımlama bilgileri içermez. Bu ayarı seçerseniz, istek tanımlama bilgileri gibi kimlik bilgilerini dahil etmek için de yapılabilir. Zaten bir yıldız işareti (*) ile çıkış ayarladıysanız bu ayarı yapılandıramazsınız.

![Çıkış noktaları arası kaynak paylaşımı (CORS) ayarları](media/cors/cors.png)

>[!NOTE]
>Farklı etki alanı kaynakları için farklı ayarlar belirtemezsiniz. Tüm ayarlar (**üstbilgiler**, **Yöntemler**, **Maksimum yaş**ve **izin verilen kimlik bilgileri**), kaynaklar ayarında belirtilen tüm kaynaklar için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'de çapraz kaynak paylaşımını yapılandırmayı öğrendiniz. Daha sonra FHIR için tam olarak yönetilen bir Azure API dağıtımı:
 
>[!div class="nextstepaction"]
>[FHIR için Azure API'sini dağıtma](fhir-paas-portal-quickstart.md)