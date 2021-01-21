---
title: IoT için Azure Defender 'daki yenilikler
description: Bu makale, IoT için Defender 'ın en son sürümündeki yenilikleri öğrenmenizi sağlar.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/06/2021
ms.author: shhazam
ms.openlocfilehash: 72a6e50134647194679055a886b50b01f42e212d
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629988"
---
# <a name="whats-new"></a>Yenilikler

IoT 10,0 için Defender, güvenlik, yönetim ve kullanılabilirliği artıran Özellik geliştirmeleri sağlar.

## <a name="security"></a>Güvenlik

Bu sürümde sertifika ve parola kurtarma geliştirmeleri yapılmıştır.

### <a name="certificates"></a>Sertifikalar
  
Bu sürüm şunları yapmanızı sağlar:

- SSL sertifikalarını doğrudan sensörlerden ve şirket içi yönetim konsollarına yükleyin.
- Şirket içi yönetim konsolu ve bağlı sensörler arasında ve bir yönetim konsolu ve yüksek oranda kullanılabilir bir yönetim konsolu arasında doğrulama gerçekleştirin. Doğrulama, sona erme tarihlerini, kök CA özgünlük ve sertifika Iptal listelerini temel alır.  Doğrulama başarısız olursa, oturum devam etmez.

Yükseltmeler için:

- Yükseltme sırasında SSL sertifikası veya doğrulama işlevselliğinde değişiklik yapılmaz.
- Yükseltmeden sonra, algılayıcı ve şirket içi yönetim konsolu yönetici kullanıcıları, SSL sertifikalarını değiştirebilir veya sistem ayarları, SSL sertifikası penceresinden SSL sertifikası doğrulamasını etkinleştirebilir.  

Yeni yüklemeler için:

- İlk kez oturum açma sırasında, kullanıcıların bir SSL sertifikası (önerilen) veya yerel olarak oluşturulmuş otomatik olarak imzalanan bir sertifika kullanması gerekir (önerilmez)
- Sertifika doğrulama, yeni yüklemeler için varsayılan olarak açıktır.

### <a name="password-recovery"></a>Parola kurtarma
  
Algılayıcı ve şirket içi yönetim konsolu yönetici kullanıcıları artık IoT portalı için Azure Defender 'dan parolaları kurtarabilir. Daha önce parola kurtarma, destek ekibi tarafından gerektirilir.

## <a name="onboarding"></a>Ekleme

### <a name="on-premises-management-console---committed-devices"></a>Şirket içi yönetim konsolu ile kaydedilmiş cihazlar

Şirket içi yönetim konsolunda ilk oturum açma sonrasında, kullanıcılardan bir etkinleştirme dosyasını karşıya yüklemesi gerekir. Dosya, kuruluş ağında izlenecek toplam cihaz sayısını içerir. Bu sayı, işlenen cihaz sayısı olarak adlandırılır.
Kaydedilen cihazlar, etkinleştirme dosyasının oluşturulduğu IoT portalı için Azure Defender 'daki ekleme işlemi sırasında tanımlanmıştır.
Etkinleştirme dosyasını karşıya yüklemek için ilk kullanıcıların ve kullanıcıların yükseltilme yapması gerekir.
İlk etkinleştirmeden sonra, ağda algılanan cihazların sayısı, taahhüt edilen cihaz sayısını aşmış olabilir. Bu olay, örneğin, yönetim konsoluna daha fazla algılayıcı bağladığınızda meydana gelebilir. Algılanan cihaz sayısıyla kaydedilmiş cihazların sayısı arasında bir tutarsızlık varsa, yönetim konsolunda bir uyarı görüntülenir. Bu olay oluşursa, yeni bir etkinleştirme dosyası yüklemeniz gerekir.

### <a name="pricing-page-options"></a>Fiyatlandırma sayfası seçenekleri

Fiyatlandırma sayfası, IoT için Azure Defender 'a yeni abonelikler eklemenizi ve ağınızdaki kaydedilmiş cihazları tanımlamanızı sağlar.  
Ayrıca, fiyatlandırma sayfası artık bir algılayıcı ile ilişkili mevcut abonelikleri yönetmenize ve cihaz taahhüdünü güncelleştirmenize olanak tanır.

### <a name="view-and-manage-onboarded-sensors"></a>Eklendi sensörleri görüntüleme ve yönetme

Yeni bir site ve algılayıcı portalı sayfası şunları yapmanızı sağlar:

- Algılayıcı hakkında açıklayıcı bilgi ekleyin. Örneğin, sensörle ilişkili bir bölge veya serbest metin etiketleri.
- Algılayıcı bilgilerini görüntüleyin ve filtreleyin. Örneğin, bulut bağlı veya yerel olarak yönetilen sensörlerle ilgili ayrıntıları görüntüleyin veya belirli bir bölgedeki sensörlerle ilgili bilgileri görüntüleyin.  

## <a name="usability"></a>Kullanılabilirlik

### <a name="azure-sentinel-new-connector-page"></a>Azure Sentinel yeni bağlayıcı sayfası

Azure Sentinel 'te IoT veri bağlayıcı için Azure Defender sayfası yeniden tasarlanmıştır. Veri Bağlayıcısı artık IoT Hub 'Ları yerine aboneliklerine dayalıdır; Müşterilerin Azure Sentinel ile yapılandırma bağlantılarını daha iyi yönetmesine izin verme.

### <a name="azure-portal-permission-updates"></a>Azure portal izin güncelleştirmeleri  

Güvenlik okuyucusu ve güvenlik yöneticisi desteği eklenmiştir.

## <a name="other-updates"></a>Diğer güncelleştirmeler

### <a name="access-group---zone-permissions"></a>Erişim grubu-bölge izinleri
  
Şirket içi yönetim konsolu erişim grubu kuralları, belirli bir bölgeye erişim verme seçeneğini içermez. Siteleri, bölgeleri ve iş birimlerini kullanan kuralların tanımlanmasıyla ilgili hiçbir değişiklik yoktur.   Yükseltmenin ardından, belirli bölgelere erişime izin veren kuralların bulunduğu erişim grupları, tüm bölgeleri dahil olmak üzere üst sitesine erişime izin verecek şekilde değiştirilir.

### <a name="terminology-changes"></a>Terminoloji değişiklikleri

Varlık terimi, algılayıcı ve şirket içi yönetim konsolunda, raporlarda ve diğer çözüm arabirimlerinde cihaz olarak yeniden adlandırıldı.
Algılayıcı ve şirket içi yönetim konsolu uyarıları ' nda, bu olayı yönetme terimi düzeltme adımları olarak adlandırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

[IoT için Defender 'ı kullanmaya başlama](getting-started.md)
