---
title: Çok kiracılı uygulamaları etkinleştirme - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins için çok kiracılı Azure Active Directory uygulamalarını yapılandırma.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 6e1321e01d8d12974a2704f4478b02a26c14142f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76264942"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Azure Digital Twins ile çok kiracılı uygulamaları etkinleştirme

Azure Digital Twins'i geliştiren çözüm geliştiriciler, birden çok müşteriyi tek bir hizmet veya çözümle desteklemek istediklerini görebilir. Aslında, *çok kiracılı* uygulamalar en yaygın Azure Digital Twins yapılandırmaları arasındadır.

Bu belge, bir Azure Dijital İkizler uygulamasının birkaç Azure Etkin Dizin kiracısını ve müşterisini destekleyecek şekilde nasıl yapılandırılabildiğini açıklar.

## <a name="multitenancy"></a>Çoklu oyuncu

*Çok kiracılı* kaynak, birden çok müşteriyi destekleyen tek bir sağlanan örnektir. Her müşterinin kendi bağımsız verileri ve ayrıcalıkları vardır. Her müşterinin deneyimi birbirlerinin deneyimlerinden izole edilmiştir, böylece uygulamanın "görünümü" farklıdır.

Çoklu kiralama hakkında daha fazla bilgi edinmek için [Azure'da Çok Kiracılı Uygulamalar'ı](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications)okuyun.

## <a name="problem-scenario"></a>Sorun senaryosu

Bu senaryoda, bir azure digital twins çözümü **(DEVELOPER)** oluşturan bir geliştirici yi ve bu çözümü kullanan bir müşteriyi düşünün (**CUSTOMER):**

- **DEVELOPER'ın** Azure Etkin Dizin kiracısı olan bir Azure aboneliği vardır.
- **DEVELOPER,** Azure aboneliklerine azure dijital ikizler örneği dağıtıyor. Azure Active Director, **DEVELOPER'ın**Azure Active Directory kiracısında otomatik olarak bir hizmet yöneticisi oluşturdu.
- **DEVELOPER'ın**Azure Active Directory kiracısı içindeki kullanıcılar, Azure Digital Twins hizmetinden [OAuth 2.0 belirteçleri](./security-authenticating-apis.md) edinebilir.
- **DEVELOPER** şimdi Azure Dijital İkizler Yönetimi API'leri ile doğrudan entegre olan bir mobil uygulama oluşturuyor.
- **GELİşTİrİcİ,** **MÜŞTERİ'nin** mobil uygulamanın kullanımına izin verir.
- **MÜŞTERİ,** **DEVELOPER'ın**uygulaması dahilinde Azure Dijital İkizler Yönetimi API'sini kullanma yetkisine verilmelidir.

Sorun:

- **MÜŞTERİ** **DEVELOPER'ın**uygulamasına giriş yaptığında, **uygulama, MÜŞTERİ**kullanıcılarının Azure Dijital İkizler Yönetimi API'leri ile kimlik doğrulaması için belirteçler elde edemez.
- Azure Active Directory'de Azure Digital Twins'in **MÜŞTERİ**dizininde tanınmadığını belirten bir özel durum yayımlanır.

## <a name="problem-solution"></a>Sorun çözümü

Önceki sorun senaryosunu çözmek için, **MÜŞTERİ'nin**Azure Etkin Dizin kiracısı içinde bir Azure Digital Twins hizmet yöneticisi oluşturmak için aşağıdaki işlemler gereklidir:

- **MÜŞTERİ'de** Azure Etkin Dizin kiracısı olan bir Azure aboneliği yoksa:

  - **MÜŞTERİ**'nin Azure Active Directory kiracı yöneticisi, [istediğiniz kadar öde Azure aboneliği](https://azure.microsoft.com/offers/ms-azr-0003p/)edinmelidir.
  - **MÜŞTERİ**'nin Azure Active Directory kiracı yöneticisi daha sonra [yeni abonelik ile kiracı bağlamak](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)gerekir.

- Azure [portalında,](https://portal.azure.com) **CUSTOMER**'ın Azure Active Directory kiracı yöneticisi aşağıdaki adımları atar:

  1. En üst Azure arama alanında **Abonelikleri** arayın. **Abonelikleri**seçin.
  1. **DEVELOPER'ın**uygulamasında kullanılmak üzere Azure Active Directory kiracısına sahip aboneliği seçin.

     [![Azure Active Directory abonelikleri](media/multitenant/ad-subscriptions.png)](media/multitenant/ad-subscriptions.png#lightbox)

  1. **Kaynak Sağlayıcılar'ı**seçin.
  1. **Microsoft.IoTSpaces**için arama yapın.
  1. **Kaydol**’u seçin.

     [![Azure Etkin Dizin kaynak sağlayıcıları](media/multitenant/ad-resource-providers.png)](media/multitenant/ad-resource-providers.png#lightbox)
  
## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins ile kullanıcı tanımlı işlevleri nasıl kullanacağıhakkında daha fazla bilgi edinmek için [Azure Digital Twins kullanıcı tanımlı işlevleri nasıl oluşturulur'](./how-to-user-defined-functions.md)okuyun.

- Rolü atamalarla uygulamayı daha fazla güvenli hale getirmek için rol tabanlı erişim denetimini nasıl kullanacağınızı öğrenmek için [Azure Digital Twins rol tabanlı erişim denetimini nasıl oluşturup yönetebilirsiniz'i](./security-create-manage-role-assignments.md)okuyun.
