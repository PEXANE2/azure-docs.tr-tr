---
title: Azure Güvenlik Merkezi 'ne gelen önemli değişiklikler
description: Azure Güvenlik Merkezi 'Nde, planlamanız gerekebilecek ve planlamanız gerekebilen değişiklikler
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2020
ms.author: memildin
ms.openlocfilehash: ba0c68589f7ea4b32970e3ad81fea5175b998dc7
ms.sourcegitcommit: 3e8058f0c075f8ce34a6da8db92ae006cc64151a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92629162"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

### <a name="recommendations-related-to-azure-security-benchmark-to-be-added-preview"></a>Eklenecek Azure Güvenlik kıyaslaması ile ilgili öneriler (Önizleme)

| Görünüş | Ayrıntılar |
|---------|---------|
|Duyuru tarihi | 26 Ekim 2020  |
|Bu değişikliğin tarihi  |  2020 Kasım |
|Etki     | Potansiyel olarak gözden geçirilecek daha fazla öneri.<br>Güvenli Puanlama üzerinde anında etki değildir-önizleme önerileri, güvenli puanınızı etkilemez.<br>Kaynaklarınızın sağlık durumu üzerinde acil bir etkisi yoktur-önizleme önerileri bir kaynağı "sağlıksız" olarak işlemez.|
|  |  |

Azure Güvenlik kıyaslaması, yaygın uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. [Azure Güvenlik Karşılaştırması hakkında daha fazla bilgi edinin](../security/benchmarks/introduction.md).

Aşağıdaki 29 yeni öneriler, kıyaslama kapsamını artırmak için Güvenlik Merkezi 'ne eklenecektir.

Önizleme önerileri bir kaynağı sağlıksız bir şekilde işlemez ve güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunmak için bunları mümkün olduğunda düzeltin. [Azure Güvenlik Merkezi 'ndeki önerileri düzeltin](security-center-remediate-recommendations.md)bölümünde bu önerilere yanıt verme hakkında daha fazla bilgi edinin.

- Sanal makineler için Azure Backup etkinleştirilmelidir
- SQL sunucuları için denetim bekletme en az 90 gün olarak ayarlanmalıdır
- Tanılama günlükleri App Service ' de etkinleştirilmelidir 
- MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- API uygulamanızda FTPS gerekli olmalıdır
- İşlev uygulamanızda FTPS gerekli olmalıdır
- Web uygulamanızda FTPS gerekli olmalıdır
- MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- Java, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- Java, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- Java, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- Yönetilen kimlik API uygulamanızda kullanılmalıdır
- Yönetilen kimlik, işlev uygulamanızda kullanılmalıdır
- Yönetilen kimliğin Web uygulamanızda kullanılması gerekir
- PHP, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- PHP, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- MariaDB sunucuları için özel uç nokta etkinleştirilmelidir
- MySQL sunucuları için özel uç nokta etkinleştirilmelidir
- PostgreSQL sunucuları için özel uç nokta etkinleştirilmelidir
- Python, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- Python, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- Python, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- TLS, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- TLS, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- TLS, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir
- Web uygulamalarının tüm gelen istekler için bir SSL sertifikası istemesi gerekir

İlgili bağlantılar:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [Azure API Apps hakkında daha fazla bilgi](../app-service/app-service-web-tutorial-rest-api.md)
- [Azure işlev uygulamaları hakkında daha fazla bilgi edinin](../azure-functions/functions-overview.md)
- [Azure Web Apps hakkında daha fazla bilgi](../app-service/overview.md)
- [MariaDB için Azure veritabanı hakkında daha fazla bilgi edinin](../mariadb/overview.md)
- [MySQL için Azure veritabanı hakkında daha fazla bilgi edinin](../mysql/overview.md)
- [PostgreSQL için Azure veritabanı hakkında daha fazla bilgi edinin](../postgresql/overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).