---
title: Azure AD Etki Alanı Hizmetlerinde güvenli LDAP sorun giderme | Microsoft Dokümanlar
description: Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanı için güvenli LDAP (LDAPS) sorun giderme öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: 22d1b6e2344256b52cfdbc48720a680a770a4216
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132161"
---
# <a name="troubleshoot-secure-ldap-connectivity-issues-to-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanında güvenli LDAP bağlantı sorunlarını giderme

Azure Active Directory Domain Services (Azure AD DS) ile iletişim kurmak için hafif dizin erişim protokolü (LDAP) kullanan uygulamalar ve hizmetler [güvenli LDAP kullanacak şekilde yapılandırılabilir.](tutorial-configure-ldaps.md) Güvenli LDAP'nin düzgün çalışması için uygun bir sertifika ve gerekli ağ bağlantı noktalarının açık olması gerekir.

Bu makale, Azure AD DS'de güvenli LDAP erişimiyle ilgili sorunları gidermenize yardımcı olur.

## <a name="common-connection-issues"></a>Sık karşılaşılan bağlantı sorunları

Güvenli LDAP kullanarak Azure AD DS yönetilen bir etki alanına bağlanmada sorun yaşıyorsanız, aşağıdaki sorun giderme adımlarını gözden geçirin. Her sorun giderme adımından sonra, Azure AD DS yönetilen etki alanına yeniden bağlanmayı deneyin:

* Güvenli LDAP sertifikasının veren zinciri istemciye güvenilmelidir. Güveni oluşturmak için istemcideki güvenilen kök sertifika deposuna Root sertifika yetkilisini (CA) ekleyebilirsiniz.
    * [Sertifikayı dışa aktarDığınızdan ve istemci bilgisayarlara uyguladığınıza][client-cert]emin olun.
* Yönetilen etki alanınızın güvenli LDAP sertifikasının *Özne'de* DNS adı veya *Konu Alternatif Adları* özniteliği olduğunu doğrulayın.
    * Güvenli [LDAP sertifika gereksinimlerini][certs-prereqs] gözden geçirin ve gerekirse yedek sertifika oluşturun.
* *LDP.exe* gibi LDAP istemcisinin IP adresini değil, DNS adını kullanarak güvenli LDAP bitiş noktasına bağladığını doğrulayın.
    * Azure AD DS yönetilen etki alanına uygulanan sertifika, hizmetin IP adreslerini değil, yalnızca DNS adlarını içerir.
* LDAP istemcisinin bağlandığı DNS adını kontrol edin. Azure AD DS yönetilen etki alanında güvenli LDAP için genel IP adresine çözüm lenmelidir.
    * DNS adı iç IP adresine çözülürse, harici IP adresine çözüm bulmak için DNS kaydını güncelleştirin.
* Dış bağlantı için ağ güvenlik grubu, Internet'ten TCP bağlantı noktası 636'ya olan trafiğini sağlayan bir kural içermelidir.
    * Azure AD DS yönetilen etki alanına doğrudan sanal ağa bağlı ancak dış bağlantılara bağlı kaynaklardan güvenli LDAP kullanarak bağlanabiliyorsanız, [güvenli LDAP trafiğine izin vermek için bir ağ güvenlik grubu kuralı oluşturduğunuzdan][ldaps-nsg]emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorunlarınız hala varsa, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-ldaps]: tutorial-configure-ldaps.md
[certs-prereqs]: tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap
[client-cert]: tutorial-configure-ldaps.md#export-a-certificate-for-client-computers
[ldaps-nsg]: tutorial-configure-ldaps.md#lock-down-secure-ldap-access-over-the-internet
