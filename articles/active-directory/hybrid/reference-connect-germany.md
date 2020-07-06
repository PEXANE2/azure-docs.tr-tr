---
title: Microsoft Bulut Almanya’da Azure AD Connect
description: Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz.
keywords: Azure AD Connect’e giriş, Azure AD Connect’e genel bakış, Azure AD Connect nedir, active directory yükleme, Almanya, Black Forest
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 62f0d82a543c0ae4e629eda3bca18b0a06322f2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "60381250"
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Microsoft Bulut Almanya’da Azure AD Connect - Genel Önizleme
## <a name="introduction"></a>Giriş
Azure AD Connect, şirket içi Active Directory’niz ile Azure Active Directory arasında eşitleme sağlar.
Şu anda [Microsoft Bulut Almanya](https://azure.microsoft.com/global-infrastructure/germany/
)’daki senaryoların çoğunun operatör tarafından yürütülmesi gerekmektedir. Almanya Microsoft Bulut kullanırken aşağıdaki bilgilere dikkat etmeniz gerekir:

* Eşitlemenin başarıyla gerçekleştirilebilmesi için aşağıdaki URL'lerin bir ara sunucuda açılması gerekir:
  
  * *. microsoftonline.de
  * *.windows.net
  * * Sertifika İptal Listeleri
* Azure AD dizininizde oturum açarken, onmicrosoft.de etki alanında yer alan bir hesap kullanmanız gerekir.

 
## <a name="download"></a>İndir
Azure AD Connect’i portalın içerisindeki Azure AD Connect dikey penceresinden indirebilirsiniz.  Azure AD Connect dikey penceresini bulmak için aşağıdaki yönergeleri kullanın.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect Dikey Penceresi
Azure portal oturum açtıktan sonra:

1. Gözat’a gidin
2. Azure Active Directory'yi seçin
3. Azure AD Connect'i seçin

Şu ayrıntıları görürsünüz:

![Azure AD Connect Dikey Penceresi](./media/reference-connect-germany/germany1.png)

Aşağıdaki tabloda, dikey pencerede gösterilen özellikler açıklanmaktadır.

| Başlık | Açıklama |
| --- | --- |
| EŞİTLEME DURUMU |Eşitlemenin etkin mi yoksa devre dışı mı olduğunu gösterir. |
| SON EŞİTLEME |Başarılı bir eşitlemenin tamamlandığı en son zaman. |
| FEDERASYON ETKİ ALANLARI |Şu anda yapılandırılmış durumda olan federasyon etki alanlarının sayısını gösterir. |

## <a name="installation"></a>Yükleme
Azure AD Connect'i yüklemek için [buradaki](how-to-connect-install-roadmap.md) belgeleri kullanabilirsiniz.

## <a name="advanced-features-and-additional-information"></a>Gelişmiş özellikler ve Ek Bilgiler
Özel ayarlar ve gelişmiş yapılandırma hakkında daha fazla bilgi için, Şirket [içi kimliklerinizi Azure Active Directory tümleştirme](whatis-hybrid-identity.md)konusuna gidin. Bu sayfada, bilgiler ve ek rehberlik sağlayan diğer sayfalara bağlantılar bulabilirsiniz.

