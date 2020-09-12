---
title: Azure AD Connect ve Federasyon | Microsoft Docs
description: Bu sayfa, Azure AD Connect kullanan AD FS işlemlerle ilgili tüm belgelerin merkezi konumudur.
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
ms.topic: how-to
ms.date: 10/09/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d61a3dd995efd1f433c2e862c4b7a59d31f79a3
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89660864"
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect ve federasyon
Azure Active Directory (Azure AD) Connect, Federasyon 'yi şirket içi Active Directory Federasyon Hizmetleri (AD FS) (AD FS) ve Azure AD ile yapılandırmanıza olanak tanır. Federasyon oturum açma sayesinde, kullanıcıların parolalarını yeniden girmeye gerek kalmadan, şirket içi parolalarla Azure AD tabanlı hizmetlerde oturum açmasını ve şirket ağı üzerinde oturum açmasını sağlayabilirsiniz. AD FS ile Federasyon seçeneğini kullanarak, yeni bir AD FS yüklemesi dağıtabilir veya var olan bir yüklemeyi Windows Server 2012 R2 grubunda belirtebilirsiniz.

Bu konu, Azure AD Connect için Federasyon ile ilgili işlevler hakkında bilgi için giriş. Tüm ilgili konuların bağlantılarını listeler. Azure AD Connect bağlantıları için bkz. Şirket [içi kimliklerinizi Azure Active Directory Ile tümleştirme](whatis-hybrid-identity.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: Federasyon konuları
| Konu | Ne kapsıyorsa ve ne zaman okunacaktır? |
|:--- |:--- |
| **Azure AD Connect Kullanıcı oturum açma seçenekleri** | |
| [Kullanıcı oturum açma seçeneklerini anlama](plan-connect-user-signin.md) |Çeşitli Kullanıcı oturum açma seçenekleri ve bunların Azure oturum açma kullanıcı deneyimini nasıl etkilediği hakkında bilgi edinin. |
| **Azure AD Connect kullanarak AD FS yüklemesi** | |
| [Önkoşullar](how-to-connect-install-custom.md#ad-fs-configuration-pre-requisites) |Azure AD Connect aracılığıyla başarılı bir AD FS yüklemesi için önkoşulları inceleyin. |
| [AD FS grubunu yapılandırma](how-to-connect-install-custom.md#configuring-federation-with-ad-fs) |Azure AD Connect kullanarak yeni bir AD FS grubu yükler. |
| [Alternatif oturum açma KIMLIĞINI kullanarak Azure AD ile federasyon oluşturma](how-to-connect-fed-management.md#alternateid) | Alternatif oturum açma KIMLIĞI kullanarak Federasyonu yapılandırma  |
| **AD FS yapılandırmasını değiştirme** | |
| [Güveni onarma](how-to-connect-fed-management.md#repairthetrust) |Şirket içi AD FS ve Microsoft 365/Azure arasındaki geçerli güveni onarın. |
| [Yeni bir AD FS sunucusu ekleme](how-to-connect-fed-management.md#addadfsserver) |İlk yüklemeden sonra ek bir AD FS sunucusu olan bir AD FS grubunu genişletin. |
| [Yeni bir AD FS WAP sunucusu ekleyin](how-to-connect-fed-management.md#addwapserver) |İlk yüklemeden sonra ek Web uygulaması ara sunucusu (WAP) sunucusuyla bir AD FS grubunu genişletin. |
| [Yeni bir Federasyon etki alanı Ekle](how-to-connect-fed-management.md#addfeddomain) |Azure AD ile Federasyon oluşturulacak başka bir etki alanı ekleyin. |
| [TLS/SSL sertifikasını güncelleştirme](how-to-connect-fed-ssl-update.md)| AD FS grubu için TLS/SSL sertifikasını güncelleştirin. |
| [Microsoft 365 ve Azure AD için Federasyon sertifikalarını yenileme](how-to-connect-fed-o365-certs.md)|Azure AD ile O365 sertifikanızı yenileyin.|
| **Diğer Federasyon yapılandırması** | |
| [Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme](how-to-connect-fed-single-adfs-multitenant-federation.md) | Birden çok Azure AD 'yi tek bir AD FS grubuyla federasyona| 
| [Özel bir şirket logosu/çizimi ekleme](how-to-connect-fed-management.md#customlogo) |AD FS oturum açma sayfasında gösterilen özel logoyu belirterek oturum açma deneyimini değiştirin. |
| [Oturum açma açıklaması ekle](how-to-connect-fed-management.md#addsignindescription) |AD FS oturum açma sayfasındaki oturum açma açıklamasını değiştirin. |
| [AD FS talep kurallarını değiştirme](how-to-connect-fed-management.md#modclaims) |Azure AD Connect eşitleme yapılandırmasına karşılık gelen AD FS talep kurallarını değiştirin veya ekleyin. |


## <a name="additional-resources"></a>Ek kaynaklar
* [Tek AD FS iki Azure AD Federasyonun](how-to-connect-fed-single-adfs-multitenant-federation.md)
* [Azure’da AD FS dağıtımı](/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs)
* [Azure 'da Azure 'da yüksek kullanılabilirliğe sahip çapraz coğrafi AD FS dağıtımı Traffic Manager](/windows-server/identity/ad-fs/deployment/active-directory-adfs-in-azure-with-azure-traffic-manager)