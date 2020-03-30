---
title: Azure AD Connect ve federasyon | Microsoft Dokümanlar
description: Bu sayfa, Azure AD Connect'i kullanan AD FS işlemleriyle ilgili tüm belgelerin merkezi konumudur.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0518c58abf156c718ee083ffadb0ef8e0a590252
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331534"
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect ve federasyon
Azure Active Directory (Azure AD) Connect, federasyonları şirket içi Active Directory Federation Services (AD FS) ve Azure AD ile yapılandırmanıza olanak tanır. Federasyon oturum açma ile kullanıcıların şirket içi parolalarıyla azure AD tabanlı hizmetlerde oturum açmalarını ve şirket ağında parolalarını yeniden girmelerine gerek kalmadan oturum açmalarını sağlayabilirsiniz. AD FS ile federasyon seçeneğini kullanarak, yeni bir AD FS yüklemesi dağıtabilir veya Windows Server 2012 R2 çiftliğinde varolan bir yüklemeyi belirtebilirsiniz.

Bu konu, Azure AD Connect için federasyonla ilgili işlevler hakkında bilgi edinme ye ev sahipliği yapmaktadır. İlgili tüm konulara bağlantılar listeler. Azure AD Connect'e bağlantılar için [bkz.](whatis-hybrid-identity.md)

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: federasyon konuları
| Konu başlığı | Neleri kapsıyor ve ne zaman okunacak |
|:--- |:--- |
| **Azure AD Connect kullanıcı oturum açma seçenekleri** | |
| [Kullanıcı oturum açma seçeneklerini anlama](plan-connect-user-signin.md) |Çeşitli kullanıcı oturum açma seçenekleri ve bunların Azure oturum açma kullanıcı deneyimini nasıl etkilediği hakkında bilgi edinin. |
| **Azure AD Connect'i kullanarak AD FS'yi yükleme** | |
| [Ön koşullar](how-to-connect-install-custom.md#ad-fs-configuration-pre-requisites) |Azure AD Connect üzerinden başarılı bir AD FS yüklemesi için ön koşullara bakın. |
| [AD FS çiftliğini yapılandırma](how-to-connect-install-custom.md#configuring-federation-with-ad-fs) |Azure AD Connect'i kullanarak yeni bir AD FS çiftliği yükleyin. |
| [Alternatif giriş kimliği kullanarak Azure AD ile fetorat](how-to-connect-fed-management.md#alternateid) | Alternatif oturum açma kimliğini kullanarak federasyonu yapılandırma  |
| **AD FS yapılandırmasını değiştirme** | |
| [Güveni onarın](how-to-connect-fed-management.md#repairthetrust) |Şirket içi AD FS ve Office 365/Azure arasındaki geçerli güveni onarın. |
| [Yeni bir AD FS sunucusu ekleme](how-to-connect-fed-management.md#addadfsserver) |İlk yüklemeden sonra ek bir AD FS sunucusuyla bir AD FS çiftliğini genişletin. |
| [Yeni bir AD FS WAP sunucusu ekleme](how-to-connect-fed-management.md#addwapserver) |İlk yüklemeden sonra ek bir Web Application Proxy (WAP) sunucusuyla bir AD FS çiftliğini genişletin. |
| [Yeni bir federe etki alanı ekleme](how-to-connect-fed-management.md#addfeddomain) |Azure AD ile federe olacak başka bir etki alanı ekleyin. |
| [TLS/SSL sertifikasını güncelleştirin](how-to-connect-fed-ssl-update.md)| Bir AD FS çiftliği için TLS/SSL sertifikasını güncelleştirin. |
| [Office 365 ve Azure AD için federasyon sertifikalarını yenileme](how-to-connect-fed-o365-certs.md)|Azure AD ile O365 sertifikanızı yenileyin.|
| **Diğer federasyon yapılandırması** | |
| [Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme](how-to-connect-fed-single-adfs-multitenant-federation.md) | Tek AD FS çiftliğiile birden fazla Azure REKLAM'ı fesuye| 
| [Özel şirket logosu/çizimi ekleme](how-to-connect-fed-management.md#customlogo) |AD FS oturum açma sayfasında gösterilen özel logoyu belirterek oturum açma deneyimini değiştirin. |
| [Oturum açma açıklaması ekleme](how-to-connect-fed-management.md#addsignindescription) |AD FS oturum açma sayfasındaoturum açma açıklamasını değiştirin. |
| [AD FS talep kurallarını değiştirme](how-to-connect-fed-management.md#modclaims) |AD FS'de Azure AD Connect eşitleme yapılandırmasına karşılık gelen talep kurallarını değiştirin veya ekleyin. |


## <a name="additional-resources"></a>Ek kaynaklar
* [Tek AD FS ile iki Azure REKLAM'ı fecile değerlendirme](how-to-connect-fed-single-adfs-multitenant-federation.md)
* [Azure’da AD FS dağıtımı](how-to-connect-fed-azure-adfs.md)
* [Azure Trafik Yöneticisi ile Azure'da yüksek kullanılabilirlik liyakat çapraz coğrafi AD FS dağıtımı](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
