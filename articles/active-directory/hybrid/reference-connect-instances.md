---
title: 'Azure AD Connect: Hizmet örneklerini eşitleme | Microsoft Dokümanlar'
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66298826"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Örneğin özel hususlar
Azure AD Connect en yaygın olarak azure AD ve Office 365'in dünya çapındaki örneğiyle kullanılır. Ama diğer örnekleri de vardır ve bu URL'ler ve diğer özel hususlar için farklı gereksinimleri vardır.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Almanya
[Microsoft Cloud Germany,](https://www.microsoft.de/cloud-deutschland) bir Alman veri mütevellisi tarafından işletilen egemen bir bulutdur.

| URL'ler proxy sunucusunda açılacak |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| +Sertifika İptal Listeleri |

Azure AD kiracınızda oturum açarak oturum açbaktığınızda, onmicrosoft.de etki alanında bir hesap kullanmanız gerekir.

Şu anda Microsoft Cloud Almanya'da bulunmayan özellikler:

* **Parola geri yazımı,** Azure AD Connect sürümü 1.1.570.0 ve sonrasında önizleme için kullanılabilir.
* Diğer Azure AD Premium hizmetleri kullanılamıyor.

## <a name="microsoft-azure-government"></a>Microsoft Azure Yönetimi
[Microsoft Azure Devlet bulutu,](https://azure.microsoft.com/features/gov/) ABD hükümeti için bir bulut.

Bu bulut DirSync önceki sürümleri tarafından desteklenmiştir. Azure AD Connect'in 1.1.180'lik yapısından bulutun yeni nesli desteklenir. Bu nesil yalnızca ABD tabanlı uç noktaları kullanıyor ve proxy sunucunuzda açılacak farklı bir URL listesine sahip.

| URL'ler proxy sunucusunda açılacak |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*.windows.net (Otomatik Azure Kamu kiracı algılaması için gereklidir) |
| \*.gov.us.microsoftonline.com |
| +Sertifika İptal Listeleri |

> [!NOTE]
> Azure AD Connect sürüm 1.1.647.0 itibariyle, proxy sunucunuzda *.windows.net açık olması koşuluyla, kayıt defterinde AzureInstance değerini ayarlamak artık gerekli değildir. Ancak, Azure AD Connect sunucularından Internet bağlantısına izin vermemektedir(

### <a name="manual-configuration"></a>Manuel Yapılandırma

Azure AD Connect'in Azure Kamu senkronizasyon uç noktalarını kullandığından emin olmak için aşağıdaki el ile yapılandırma adımları kullanılır.

1. Azure AD Connect yüklemesini başlatın.
2. EULA'yı kabul etmeniz gereken ilk sayfayı gördüğünüzde, devam etmeyin ancak yükleme sihirbazını çalışır durumda bırakın.
3. Regedit'i başlatın ve `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` kayıt `4`defteri anahtarını değerle değiştirin.
4. Azure AD Connect yükleme sihirbazına geri dön, EULA'yı kabul et ve devam et. Yükleme sırasında, **özel yapılandırma** yükleme yolunu (Express yüklemesi değil) kullandığınızdan emin olun, ardından yüklemeye her zamanki gibi devam edin.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
