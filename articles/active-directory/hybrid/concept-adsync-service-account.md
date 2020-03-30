---
title: 'Azure AD Connect: ADSync hizmet hesabı | Microsoft Dokümanlar'
description: Bu konu, ADSync hizmet hesabını açıklar ve hesapla ilgili en iyi uygulamaları sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f228da5afc5998d8fa59ce2d720cec4c9f955b67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67478725"
---
# <a name="adsync-service-account"></a>AD Eşitleme hizmeti hesabı
Azure AD Connect, Active Directory ve Azure Active Directory arasında eşitleme düzenleyen bir şirket içi hizmet yükler.  Microsoft Azure AD Sync eşitleme hizmeti (ADSync), şirket içi ortamınızdaki bir sunucuda çalışır.  Hizmetkimlik bilgileri Express yüklemelerinde varsayılan olarak ayarlanır, ancak kuruluş güvenlik gereksinimlerinizi karşılayacak şekilde özelleştirilebilir.  Bu kimlik bilgileri şirket içi ormanlarınıza veya Azure Etkin Dizininize bağlanmak için kullanılmaz.

ADSync hizmet hesabını seçmek, Azure AD Connect'i yüklemeden önce karar vermek için önemli bir planlama kararıdır.  Yüklemeden sonra kimlik bilgilerini değiştirmeye yönelik herhangi bir girişim, hizmetin başlatılamamasına, eşitleme veritabanına erişimi kaybetmesine ve bağlı dizinlerinizle (Azure ve AD DS) kimlik doğrulamasına neden olur.  Özgün kimlik bilgileri geri yüklenene kadar eşitleme oluşmaz.

## <a name="the-default-adsync-service-account"></a>Varsayılan ADSync hizmet hesabı

Bir üye sunucuda çalıştırıldığında, AdSync hizmeti Sanal Hizmet Hesabı (VSA) bağlamında çalışır.  Ürün sınırlaması nedeniyle, etki alanı denetleyicisine yüklendiğinde özel bir hizmet hesabı oluşturulur.  Express ayarları hizmet hesabı kuruluş güvenlik gereksinimlerinizi karşılamıyorsa, Özelleştir seçeneğini seçerek Azure AD Connect'i dağıtın.  Ardından kuruluşunuzun gereksinimlerini karşılayan hizmet hesabı seçeneğini seçin.

>[!NOTE]
>Etki alanı denetleyicisi üzerine yüklendiğinde varsayılan hizmet hesabı Etki Alanı\AAD_InstallationIdentifier biçimindedir.  Bu hesabın parolası rasgele oluşturulur ve kurtarma ve parola döndürme için önemli zorluklar sunar.  Microsoft, bağımsız veya grup Yönetilen Hizmet Hesabı (sMSA / gMSA) kullanmak için bir etki alanı denetleyicisi üzerinde ilk yükleme sırasında hizmet hesabı özelleştirme önerir

|Azure AD Connect konumu|Oluşturulan hizmet hesabı|
|-----|-----|
|Üye Sunucu|NT SERVICE\ADSync|
|Etki Alanı Denetleyicisi|Etki alanı\AAD_74dc30c01e80 (nota bakınız)|

## <a name="custom-adsync-service-accounts"></a>Özel ADSync hizmet hesapları
Microsoft, ADSync hizmetini Sanal Hizmet Hesabı veya bağımsız veya grup Yönetilen Hizmet Hesabı bağlamında çalıştırmayı önerir.  Etki alanı yöneticiniz, belirli kuruluş güvenlik gereksinimlerinizi karşılamak üzere sağlanan bir hizmet hesabı oluşturmayı da seçebilir.   Yükleme sırasında kullanılan servis hesabını özelleştirmek için aşağıdaki Ekspres Ayarlar sayfasındaki Özelleştir seçeneğini belirleyin.   Aşağıdaki seçenekler mevcuttur:

- varsayılan hesap – Azure AD Connect, hizmet hesabını yukarıda açıklandığı şekilde sağlayacaktır
- yönetilen hizmet hesabı – yöneticiniz tarafından sağlanan bağımsız veya grup MSA kullanın
- etki alanı hesabı – yöneticiniz tarafından sağlanan bir etki alanı hizmeti hesabı kullanın

![](media/concept-adsync-service-account/adsync1.png)

![](media/concept-adsync-service-account/adsync2.png)

## <a name="diagnosing-adsync-service-account-changes"></a>ADSync hizmet hesabı değişikliklerini tanılama
Yüklemeden sonra ADSync hizmetinin kimlik bilgilerini değiştirmek, hizmetin başlatılamamasına, eşitleme veritabanına erişimi kaybetmesine ve bağlı dizinlerinizle (Azure ve AD DS) kimlik doğrulamasına neden olur.  Yeni ADSync hizmet hesabına veritabanı erişimi vermek bu sorundan kurtarmak için yeterli değildir. Özgün kimlik bilgileri geri yüklenene kadar eşitleme oluşmaz.

ADSync hizmeti başlatılamadığında olay günlüğüne bir hata düzeyi iletisi verir.  İletinin içeriği yerleşik veritabanının (localdb) veya tam SQL'in kullanılıp kullanılmadığına bağlı olarak değişir.  Aşağıda, mevcut olabilecek olay günlüğü girişlerinin örnekleri verilmiştir.

### <a name="example-1"></a>Örnek 1

AdSync hizmeti şifreleme anahtarları bulunamadı ve yeniden oluşturuldu.  Bu sorun düzeltilene kadar eşitleme oluşmaz.

Bu Sorunu Giderme AdSync hizmeti Oturum Açma kimlik bilgileri değiştirilirse Microsoft Azure AD Sync şifreleme anahtarlarına erişilemez hale gelir.  Kimlik bilgileri değiştirildiyse, Oturum Açma hesabını özgün olarak yapılandırılan değerine (ör. NT SERVICE\AdSync) ve hizmeti yeniden başlatın.  Bu, AdSync hizmetinin doğru çalışmasını hemen geri yüklenir.

Daha fazla bilgi için lütfen aşağıdaki [makaleye](https://go.microsoft.com/fwlink/?linkid=2086764) bakın.

### <a name="example-2"></a>Örnek 2

Yerel veritabanına (localdb) bağlantı kurulamadığından hizmet başlatılamadı.

Sorun giderme Bu Sorunu Microsoft Azure AD Eşitleme hizmeti, AdSync hizmeti Oturum Açma kimlik bilgileri değiştirilirse yerel veritabanı sağlayıcısına erişim iznini kaybeder.  Kimlik bilgileri değiştirildiyse, Oturum Açma hesabını orijinal olarak yapılandırılan değerine (ör. NT SERVICE\AdSync) ve hizmeti yeniden başlatın.  Bu, AdSync hizmetinin doğru çalışmasını hemen geri yüklenir.

Daha fazla bilgi için lütfen aşağıdaki [makaleye](https://go.microsoft.com/fwlink/?linkid=2086764) bakın.

Ek Ayrıntılar Aşağıdaki hata bilgileri sağlayıcı tarafından döndürüldü:
 

``` 
OriginalError=0x80004005 OLEDB Provider error(s): 
Description  = 'Login timeout expired'
Failure Code = 0x80004005
Minor Number = 0 
Description  = 'A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections. For more information see SQL Server Books Online.'
```
## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.