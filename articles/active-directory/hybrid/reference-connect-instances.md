---
title: 'Azure AD Connect: hizmet örneklerini Eşitle | Microsoft Docs'
description: Bu sayfa, Azure AD örnekleri için özel hususlar belgeler.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c342eac5460d8d52422b0497b1283f367660eb3c
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "66298826"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: örneklerle ilgili özel konular
Azure AD Connect en yaygın olarak, Azure AD ve Office 365 'nin dünya çapındaki örneğiyle kullanılır. Ancak başka örnekler de vardır ve bunlar, URL 'Ler ve diğer özel hususlar için farklı gereksinimlere sahiptir.

## <a name="microsoft-cloud-germany"></a>Almanya Microsoft Bulut
[Almanya Microsoft bulut](https://www.microsoft.de/cloud-deutschland) , bir Almanya veri emanetçisi tarafından çalıştırılan bir sogeign bulutu.

| Proxy sunucuda açılacak URL 'Ler |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Sertifika Iptal listeleri |

Azure AD kiracınızda oturum açtığınızda, onmicrosoft.de etki alanında bir hesap kullanmanız gerekir.

Microsoft Bulut Almanya 'da Şu anda mevcut olmayan özellikler:

* **Parola geri yazma** , Azure AD Connect sürüm 1.1.570.0 ve sonrasında önizleme için kullanılabilir.
* Diğer Azure AD Premium hizmetleri kullanılamaz.

## <a name="microsoft-azure-government"></a>Microsoft Azure Kamu
[Microsoft Azure Kamu Bulutu](https://azure.microsoft.com/features/gov/) ABD hükümeti için bir bulutdır.

Bu bulut, DirSync 'in daha eski sürümleri tarafından desteklenmektedir. Derleme 1.1.180 Azure AD Connect, bulutun yeni nesli desteklenir. Bu nesil yalnızca ABD tabanlı uç noktaları kullanıyor ve proxy sunucunuzda açılacak farklı bir URL listesine sahip.

| Proxy sunucuda açılacak URL 'Ler |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (otomatik Azure Kamu kiracısı algılaması için gereklidir) |
| \*. gov.us.microsoftonline.com |
| + Sertifika Iptal listeleri |

> [!NOTE]
> Azure AD Connect sürüm 1.1.647.0 itibariyle, kayıt defterindeki AzureInstance değerini ayarlamak, ara sunucunuzda *. windows.net açık olması için artık gerekli değildir. Ancak, Azure AD Connect sunucusundan Internet bağlantısına izin vermediği müşteriler için aşağıdaki el ile yapılandırma kullanılabilir.

### <a name="manual-configuration"></a>El ile yapılandırma

Azure AD Connect Azure Kamu eşitleme uç noktalarını kullandığından emin olmak için aşağıdaki el ile yapılandırma adımları kullanılır.

1. Azure AD Connect yüklemesini başlatın.
2. EULA 'Yı kabul etmeniz beklenen ilk sayfayı gördüğünüzde, devam etmeyin ancak Yükleme Sihirbazı 'nı çalışır durumda bırakın.
3. Regedit 'i başlatın ve kayıt defteri anahtarını `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` değerine `4`değiştirin.
4. Azure AD Connect yükleme sihirbazına dönün, EULA 'yı kabul edin ve devam edin. Yükleme sırasında, **özel yapılandırma** yükleme yolunu (hızlı yükleme değil) kullandığınızdan emin olun, ardından yüklemeye her zamanki gibi devam edin.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
