---
title: Azure AD Domain Services 'de Güvenli LDAP (LDAPS) sorunlarını giderme | Microsoft Docs
description: Azure AD Domain Services yönetilen bir etki alanı için Güvenli LDAP (LDAPS) sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 285af0e5e5d5ab03027fc29064a5f3623ed10e2f
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617057"
---
# <a name="troubleshoot-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen bir etki alanı için Güvenli LDAP (LDAPS) sorunlarını giderme

## <a name="connection-issues"></a>Bağlantı sorunları
Güvenli LDAP kullanarak yönetilen etki alanına bağlanmada sorun yaşıyorsanız:

* Güvenli LDAP sertifikasının veren zincirinin istemcide güvenilir olması gerekir. Güveni oluşturmak için, kök sertifika yetkilisini istemcideki güvenilen kök sertifika deposuna eklemeyi tercih edebilirsiniz.
* LDAP istemcisinin (örneğin, Ldp. exe), IP adresi değil, DNS adı kullanarak Güvenli LDAP uç noktasına bağlandığını doğrulayın.
* LDAP istemcisinin bağlandığı DNS adını denetleyin. Yönetilen etki alanında güvenli LDAP için genel IP adresine çözümlenmelidir.
* Yönetilen etki alanınız için Güvenli LDAP sertifikasının konu veya konu alternatif adları özniteliğinde DNS adına sahip olduğunu doğrulayın.
* Sanal ağın NSG ayarları, bağlantı noktası 636 ' e internet 'ten trafiğe izin vermelidir. Bu adım yalnızca internet üzerinden güvenli LDAP erişimini etkinleştirdiyseniz geçerlidir.


## <a name="need-help"></a>Yardım mı gerekiyor?
Güvenli LDAP kullanarak yönetilen etki alanına bağlanma konusunda hala sorun yaşıyorsanız yardım için [ürün ekibine başvurun](contact-us.md) . Sorunu tanılamaya daha iyi yardımcı olması için aşağıdaki bilgileri ekleyin:
* Ldp. exe ' nin bağlantı kuran ve başarısız olduğu bir ekran görüntüsü.
* Azure AD kiracı KIMLIĞINIZ ve yönetilen etki alanının DNS etki alanı adı.
* Farklı bağlamaya çalıştığınız Kullanıcı adı.


## <a name="related-content"></a>İlgili içerik
* [Azure AD Domain Services-Başlarken Kılavuzu](tutorial-create-instance.md)
* [Azure AD Domain Services etki alanını yönetme](tutorial-create-management-vm.md)
* [LDAP sorgusu temelleri](https://technet.microsoft.com/library/aa996205.aspx)
* [Azure AD Domain Services için grup ilkesi yönetme](manage-group-policy.md)
* [Ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)
