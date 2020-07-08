---
title: 'Azure AD Connect: ADSync hizmet hesabı | Microsoft Docs'
description: Bu konu, ADSync hizmet hesabını açıklar ve firmayla ilgili en iyi yöntemleri sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9614def5310bdc6fa8c6f37d7cdcc0a5f081a96
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85360308"
---
# <a name="adsync-service-account"></a>AD Eşitleme hizmeti hesabı
Azure AD Connect, Active Directory ve Azure Active Directory arasındaki eşitlemeyi düzenleyen bir şirket içi hizmeti yüklüyor.  Microsoft Azure AD eşitleme eşitleme hizmeti (ADSync), şirket içi ortamınızdaki bir sunucuda çalışır.  Hizmetin kimlik bilgileri hızlı yüklemelerde varsayılan olarak ayarlanır ancak kuruluş güvenlik gereksinimlerinizi karşılayacak şekilde özelleştirilebilir.  Bu kimlik bilgileri, şirket içi ormanlarınız veya Azure Active Directory bağlanmak için kullanılmaz.

ADSync hizmeti hesabını seçmek, Azure AD Connect yüklemeden önce yapmanız için önemli bir planlama karardır.  Yüklemeden sonra kimlik bilgilerini değiştirme girişimleri, hizmetin başlamalamaması, eşitleme veritabanına erişiminin kaybedilmesi ve bağlı dizinlerinizde kimlik doğrulaması başarısız olur (Azure ve AD DS).  Özgün kimlik bilgileri geri yüklenene kadar hiçbir eşitleme gerçekleşmeyecektir.

## <a name="the-default-adsync-service-account"></a>Varsayılan ADSync hizmet hesabı

Bir üye sunucuda çalıştırıldığında, AdSync hizmeti bir sanal hizmet hesabı (VSA) bağlamında çalışır.  Bir ürün sınırlaması nedeniyle, bir etki alanı denetleyicisine yüklendiğinde özel bir hizmet hesabı oluşturulur.  Hızlı ayarlar hizmet hesabı kurumsal güvenlik gereksinimlerinizi karşılamıyorsa, özelleştirme seçeneğini belirleyerek Azure AD Connect dağıtın.  Ardından, kuruluşunuzun gereksinimlerini karşılayan hizmet hesabı seçeneğini belirleyin.

>[!NOTE]
>Bir etki alanı denetleyicisine yüklendiğinde varsayılan hizmet hesabı, etki alanı \ AAD_InstallationIdentifier formundadır.  Bu hesabın parolası rastgele oluşturulmuştur ve kurtarma ve parola dönüşüyle ilgili önemli zorluk gösterir.  Microsoft, bir etki alanı denetleyicisinde ilk yükleme sırasında tek başına veya grup tarafından yönetilen hizmet hesabı (sMSA/gMSA) kullanmak için hizmet hesabını özelleştirmeyi önerir

|Azure AD Connect konumu|Hizmet hesabı oluşturuldu|
|-----|-----|
|Üye sunucu|NT SERVICE\ADSync|
|Etki Alanı Denetleyicisi|Etki alanı \ AAD_74dc30c01e80 (nota bakın)|

## <a name="custom-adsync-service-accounts"></a>Özel ADSync hizmeti hesapları
Microsoft, ADSync hizmetini bir sanal hizmet hesabı ya da tek başına veya grup tarafından yönetilen hizmet hesabı bağlamında çalıştırmayı önerir.  Etki alanı yöneticiniz, belirli kurumsal güvenlik gereksinimlerinizi karşılamak için sağlanan bir hizmet hesabı oluşturmayı da seçebilir.   Yükleme sırasında kullanılan hizmet hesabını özelleştirmek için aşağıdaki hızlı ayarlar sayfasında Özelleştir seçeneğini belirleyin.   Aşağıdaki seçenekler mevcuttur:

- Varsayılan hesap – Azure AD Connect, yukarıda açıklandığı gibi hizmet hesabı sağlayacak
- yönetilen hizmet hesabı – yöneticiniz tarafından sağlanan tek başına veya grup MSA kullanın
- etki alanı hesabı – yöneticiniz tarafından sağlanan bir etki alanı hizmet hesabı kullanın

![](media/concept-adsync-service-account/adsync1.png)

![](media/concept-adsync-service-account/adsync2.png)

## <a name="diagnosing-adsync-service-account-changes"></a>ADSync hizmeti hesap değişikliklerini tanılama
Yükleme sonrasında ADSync hizmeti için kimlik bilgilerini değiştirmek, hizmetin başlamamasına, eşitleme veritabanına erişimi kaybetmesini ve bağlı dizinlerinizde (Azure ve AD DS) kimlik doğrulaması başarısız olmasına neden olur.  Yeni ADSync hizmet hesabına veritabanı erişimi verilmesi bu sorundan kurtulmak için yeterli değil. Özgün kimlik bilgileri geri yüklenene kadar hiçbir eşitleme gerçekleşmeyecektir.

ADSync hizmeti, başlayamayacak olan olay günlüğüne hata düzeyi iletisi yayımlayacak.  İletinin içeriği, yerleşik veritabanının (LocalDB) veya tam SQL 'in kullanımda olmasına bağlı olarak değişir.  Aşağıda, mevcut olabilecek olay günlüğü girişlerinin örnekleri verilmiştir.

### <a name="example-1"></a>Örnek 1

AdSync hizmeti şifreleme anahtarları bulunamadı ve yeniden oluşturuldu.  Bu sorun düzeltilene kadar eşitleme gerçekleşmeyecektir.

Bu sorun giderilir Microsoft Azure AD Eşitleme şifreleme anahtarları, AdSync hizmeti oturum açma kimlik bilgileri değiştirilirse erişilemez duruma gelir.  Kimlik bilgileri değiştirilmişse, oturum açma hesabını ilk yapılandırılmış değerine (örn.) yeniden değiştirmek için hizmetler uygulamasını kullanın. NT SERVICE\AdSync) ve hizmeti yeniden başlatın.  Bu, AdSync hizmetinin doğru işlemini hemen geri yükler.

Daha fazla bilgi için lütfen aşağıdaki [makaleye](https://go.microsoft.com/fwlink/?linkid=2086764) bakın.

### <a name="example-2"></a>Örnek 2

Yerel veritabanı (LocalDB) bağlantısı kurulamadığından hizmet başlatılamadı.

Bu sorun giderilir Microsoft Azure AD eşitleme hizmeti, AdSync hizmeti oturum açma kimlik bilgileri değiştirilirse yerel veritabanı sağlayıcısına erişim iznini kaybedecektir.  Kimlik bilgileri değiştirilmişse, oturum açma hesabını ilk yapılandırılmış değerine (örn.) yeniden değiştirmek için hizmetler uygulamasını kullanın. NT SERVICE\AdSync) ve hizmeti yeniden başlatın.  Bu, AdSync hizmetinin doğru işlemini hemen geri yükler.

Daha fazla bilgi için lütfen aşağıdaki [makaleye](https://go.microsoft.com/fwlink/?linkid=2086764) bakın.

Ek ayrıntılar sağlayıcı tarafından aşağıdaki hata bilgileri döndürüldü:
 

``` 
OriginalError=0x80004005 OLEDB Provider error(s): 
Description  = 'Login timeout expired'
Failure Code = 0x80004005
Minor Number = 0 
Description  = 'A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online.'
```
## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
