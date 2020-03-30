---
title: Karma Kimlik gerekli bağlantı noktaları ve protokoller - Azure | Microsoft Dokümanlar
description: Bu sayfa, Azure AD Connect için açık olması gereken bağlantı noktaları için teknik bir başvuru sayfasıdır
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/04/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: da318840426d1c0b94eab06b89ff3152df9d26fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331101"
---
# <a name="hybrid-identity-required-ports-and-protocols"></a>Karma Kimlik için Gereken Bağlantı Noktaları ve Protokoller
Aşağıdaki belge, karma kimlik çözümünü uygulamak için gerekli bağlantı noktaları ve protokoller hakkında teknik bir başvurudur. Aşağıdaki çizimi kullanın ve ilgili tabloya bakın.

![Azure AD Connect nedir?](./media/reference-connect-ports/required3.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Tablo 1 - Azure AD Connect ve Şirket İçi AD
Bu tablo, Azure AD Connect sunucusu ile şirket içi AD arasındaki iletişim için gereken bağlantı noktalarını ve protokolleri açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |DNS hedef ormanı arar. |
| Kerberos |88 (TCP/UDP) |KERBEROS'un AD ormanına doğrulanması. |
| MS-RPC |135 (TCP) |Azure AD Connect sihirbazının ilk yapılandırması sırasında AD ormanına bağlandığında ve Parola eşitleme sırasında kullanılır. |
| LDAP |389 (TCP/UDP) |AD'den veri alma için kullanılır. Veriler Kerberos Sign & Seal ile şifrelenir. |
| SMB | 445 (TCP) |Seamless SSO tarafından AD ormanında bir bilgisayar hesabı oluşturmak için kullanılır. |
| LDAP/SSL |636 (TCP/UDP) |AD'den veri alma için kullanılır. Veri aktarımı imzalanır ve şifrelenir. Yalnızca TLS kullanıyorsanız kullanılır. |
| RPC |49152- 65535 (Rastgele yüksek RPC Bağlantı Noktası)(TCP) |Azure AD Connect'in ilk yapılandırması sırasında, AD ormanlarına bağlanırken ve Parola eşitleme sırasında kullanılır. Daha fazla bilgi için [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)ve [KB224196'ya](https://support.microsoft.com/kb/224196) bakın. |
|WinRM  | 5985 (TCP) |Yalnızca Azure AD Connect Sihirbazı tarafından gMSA ile AD FS yüklüyorsanız kullanılır|
|AD DS Web Hizmetleri | 9389 (TCP) |Yalnızca Azure AD Connect Sihirbazı tarafından gMSA ile AD FS yüklüyorsanız kullanılır |

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Tablo 2 - Azure AD Bağlantısı ve Azure AD
Bu tablo, Azure AD Connect sunucusu ile Azure AD BAĞLANTıSı sunucusu arasındaki iletişim için gereken bağlantı noktalarını ve protokolleri açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTP |80 (TCP) |TLS/SSL sertifikalarını doğrulamak için CRL'leri (Sertifika İptal Listeleri) indirmek için kullanılır. |
| HTTPS |443(TCP) |Azure AD ile eşitlemek için kullanılır. |

Güvenlik duvarınızda açmanız gereken URL'lerin ve IP adreslerinin listesi için [Office 365 URL'leri ve IP adres aralıklarına](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) ve [Sorun Giderme Azure AD Bağlantısı bağlantısına](tshoot-connect-connectivity.md#troubleshoot-connectivity-issues-in-the-installation-wizard)bakın.

## <a name="table-3---azure-ad-connect-and-ad-fs-federation-serverswap"></a>Tablo 3 - Azure AD Connect ve AD FS Federation Sunucuları/WAP
Bu tablo, Azure AD Connect sunucusu ile AD FS Federation/WAP sunucuları arasındaki iletişim için gereken bağlantı noktalarını ve protokolleri açıklar.  

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTP |80 (TCP) |TLS/SSL sertifikalarını doğrulamak için CRL'leri (Sertifika İptal Listeleri) indirmek için kullanılır. |
| HTTPS |443(TCP) |Azure AD ile eşitlemek için kullanılır. |
| WinRM |5985 |WinRM Dinleyici |

## <a name="table-4---wap-and-federation-servers"></a>Tablo 4 - WAP ve Federasyon Sunucuları
Bu tablo, Federasyon sunucuları ve WAP sunucuları arasındaki iletişim için gereken bağlantı noktalarını ve protokolleri açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP) |Kimlik doğrulama için kullanılır. |

## <a name="table-5---wap-and-users"></a>Tablo 5 - WAP ve Kullanıcılar
Bu tablo, kullanıcılar ve WAP sunucuları arasındaki iletişim için gerekli olan bağlantı noktalarını ve protokolleri açıklar.

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP) |Aygıt kimlik doğrulaması için kullanılır. |
| TCP |49443 (TCP) |Sertifika kimlik doğrulaması için kullanılır. |

## <a name="table-6a--6b---pass-through-authentication-with-single-sign-on-sso-and-password-hash-sync-with-single-sign-on-sso"></a>Tablo 6a & 6b - Tek Oturum Açma (SSO) ve Tek Oturum Açmalı Şifre Hash Sync (SSO) ile Geçiş Kimlik Doğrulama
Aşağıdaki tablolar, Azure AD Connect ve Azure AD arasındaki iletişim için gereken bağlantı noktalarını ve protokolleri açıklar.

### <a name="table-6a---pass-through-authentication-with-sso"></a>Tablo 6a - SSO ile Geçiş Kimlik Doğrulaması
|Protokol|Bağlantı Noktası Numarası|Açıklama
| --- | --- | ---
|HTTP|80|SSL gibi güvenlik doğrulaması için giden HTTP trafiğini etkinleştirin. Ayrıca bağlayıcı otomatik güncelleştirme özelliğinin düzgün çalışması için de gereklidir.
|HTTPS|443| Özelliği etkinleştirme ve devre dışı bırakma, bağlayıcıları kaydetme, bağlayıcı güncelleştirmelerini indirme ve tüm kullanıcı oturum açma isteklerini işleme gibi işlemler için giden HTTPS trafiğini etkinleştirin.

Buna ek olarak, Azure AD Connect'in [Azure veri merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)doğrudan IP bağlantısı yapabilmesi gerekir.

### <a name="table-6b---password-hash-sync-with-sso"></a>Tablo 6b - SSO ile Şifre Karma Senkronizasyonu

|Protokol|Bağlantı Noktası Numarası|Açıklama
| --- | --- | ---
|HTTPS|443| SSO kaydını etkinleştirin (yalnızca SSO kayıt işlemi için gereklidir).

Buna ek olarak, Azure AD Connect'in [Azure veri merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)doğrudan IP bağlantısı yapabilmesi gerekir. Yine, bu sadece SSO kayıt işlemi için gereklidir.

## <a name="table-7a--7b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablo 7a & 7b - Azure AD(AD FS/Sync) ve Azure AD için Sistem Durumu aracısı
Aşağıdaki tablolar, Azure AD Connect Sistem durumu aracıları ile Azure AD AD arasındaki iletişim için gereken uç noktaları, bağlantı noktalarını ve protokolleri açıklar

### <a name="table-7a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Tablo 7a - Azure AD Connect Health aracısı (AD FS/Sync) ve Azure AD için bağlantı noktaları ve Protokoller
Bu tablo, Azure AD Connect Sistem Durumu aracıları ile Azure AD AD arasındaki iletişim için gereken aşağıdaki giden bağlantı noktalarını ve protokolleri açıklar.  

| Protokol | Bağlantı Noktaları | Açıklama |
| --- | --- | --- |
| HTTPS |443(TCP) |Giden |
| Azure Service Bus |5671 (TCP) |Giden |

Azure Servis Veri Servisi bağlantı noktası 5671 artık aracının en son sürümü için gerekli değildir. En son Azure AD Connect Health aracısı sürümü yalnızca bağlantı noktası 443'e ihtiyaç dokunur.

### <a name="7b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>7b - Azure AD Connect Health aracısı (AD FS/Sync) ve Azure AD için bitiş noktaları
Bitiş noktaları listesi için [Azure AD Connect Health aracısı için Gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements)bakın.

