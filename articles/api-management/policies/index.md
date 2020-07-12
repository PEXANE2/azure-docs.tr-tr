---
title: Azure API Management ilke örnekleri | Microsoft Docs
description: Azure API Management'ta kullanılabilecek ilkeler hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 10/31/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 252e7b8d13cf53a98ea1f25b207740c0e623f92c
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86242976"
---
# <a name="api-management-policy-samples"></a>API Management ilke örnekleri

[İlkeler](../api-management-howto-policies.md), yayımcının API’nin davranışını yapılandırma yoluyla değiştirmesini sağlayan güçlü sistem özellikleridir. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. Aşağıdaki tabloda örneklerin bağlantıları yer alır ve her örnek için kısa bir açıklama verilir.

|                                                                                                                                                                      |                                                                                                                                                                                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Gelen ilkeleri**                                                                                                                                                 |                                                                                                                                                                                                                             |
| [Arka uç API'sinin düzgün URL'ler oluşturmasını sağlamak için bir İletildi üst bilgisi ekleme](./set-header-to-enable-backend-to-construct-urls.md) | Arka uç API'sinin düzgün URL'ler oluşturmasını sağlamak için gelen isteğe İletildi üst bilgisi ekleme işlemini gösterir.                                                                                                        |
| [Bağıntı kimliği içeren bir üst bilgi ekleme](./add-correlation-id.md)                                                             | Gelen isteğe bağıntı kimliği içeren bir üst bilgi ekleme işlemini gösterir.                                                                                                                                        |
| [Arka uç hizmetine özellikler ekleme ve yanıtı önbelleğe alma](./cache-response.md)                                             | Arka uç hizmetine özellikler ekleme işlemini gösterir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin.                                                                    |
| [JWT talepleri temelinde erişim yetkisi verme](./authorize-request-based-on-jwt-claims.md)                                              | JWT talepleri temelinde bir API'deki belirli HTTP yöntemlerine erişim yetkisi verme işlemini gösterir.                                                                                                                                       |
| [Dış yetkilendirici kullanarak istekleri yetkilendirme](./authorize-request-using-external-authorizer.md)                                                   | API erişiminin güvenliğini sağlamak için dış yetkilendiricinin nasıl kullanılacağını gösterir.                                                                                                                                                               |
| [Google OAuth belirtecini kullanarak erişim yetkisi verme](./use-google-as-oauth-token-provider.md)                                            | OAuth belirteç sağlayıcısı olarak Google'ı kullanıp uç noktalarınıza erişim yetkisi verme işlemini gösterir.                                                                                                                                    |
| [Application Gateway kullanırken IP Adreslerini filtreleme](./filter-ip-addresses-when-using-appgw.md) | API Management örneğine Application Gateway üzerinden erişirken ilkelerde IP'nin nasıl filtreleneceğini gösterir
| [Paylaşılan Erişim İmzası oluşturma ve Azure depolamaya isteği iletme](./generate-shared-access-signature.md)                  | İfadeleri kullanarak [Paylaşılan Erişim İmzası](../../storage/common/storage-sas-overview.md) oluşturma ve rewrite-uri ilkesiyle Azure depolamaya isteği iletme işlemlerini gösterir. |
| [AAD'den OAuth2 erişim belirtecini alma ve bunu arka uca iletme](./use-oauth2-for-authorization.md)                             | Ağ geçidiyle arka uç arasındaki yetkilendirme için OAuth2 kullanma örneği sağlar. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir.                                                    |
| [İstek gönderme ilkesini kullanarak SAP ağ geçidinden X-CSRF belirtecini alma](./get-x-csrf-token-from-sap-gateway.md)                           | Birçok API tarafından kullanılan X-CSRF deseninin nasıl uygulandığını gösterir. Bu örnek SAP Ağ Geçidi'ne özgüdür.                                                                                                                           |
| [İsteği, gövdesinin boyutu temelinde yönlendirme](./route-requests-based-on-size.md)                                            | İstekleri gövdelerinin boyutu temelinde yönlendirme işlemini gösterir.                                                                                                                                                       |
| [Arka uç hizmetine istek bağlamı bilgilerini gönderme](./send-request-context-info-to-backend-service.md)                    | Günlüğe kaydedilmesi veya işlenmesi için arka uç hizmetine bazı bağlam bilgilerinin nasıl gönderileceğini gösterir.                                                                                                                                |
| [Yanıt önbellek süresini ayarlama](./set-cache-duration.md)                                                                          | Arka uç tarafından gönderilen Cache-Control üst bilgisindeki maxAge değerini kullanarak yanıt önbellek süresini ayarlama işlemini gösterir.                                                                                                             |
| **Giden ilkeleri**                                                                                                                                                |                                                                                                                                                                                                                             |
| [Yanıt içeriğini filtreleme](./filter-response-content.md)                                                                         | İstekle ilişkilendirilmiş ürün temelinde yanıt yükünden veri öğelerinin nasıl filtreleneceğini gösterir.                                                                                                        |
| **Hata durum ilkeleri**                                                                                                                                                |                                                                                                                                                                                                                             |
| [Hataları Stackify'de günlüğe kaydetme](./log-errors-to-stackify.md)                                                                           | Hataları günlüğe kaydedilmek üzere Stackify'ye göndermek üzere bir hata günlüğü ilkesi ekleme işlemini gösterir.                                                                                                                                            |
