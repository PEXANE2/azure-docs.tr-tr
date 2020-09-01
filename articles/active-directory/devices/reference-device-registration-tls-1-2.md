---
title: TLS 1,2 zorlama-Azure Active Directory kayıt hizmeti
description: Azure AD cihaz kayıt hizmeti için TLS 1,0 ve 1,1 desteğini kaldırma
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: reference
ms.date: 07/10/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bb8c6c64e0a68f5176c4eb0c0177c5220394695
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268766"
---
# <a name="enforce-tls-12-for-the-azure-ad-registration-service"></a>Azure AD kayıt hizmeti için TLS 1,2 ' i zorla

Azure Active Directory (Azure AD) cihaz kayıt hizmeti cihaz kimliği ile cihazları buluta bağlamak için kullanılır. Azure AD cihaz kayıt hizmeti şu anda Azure ile iletişim için Aktarım Katmanı Güvenliği (TLS) 1,2 kullanımını desteklemektedir. Güvenlik ve en iyi sınıf şifrelemeyi sağlamak için Microsoft, TLS 1,0 ve 1,1 ' nin devre dışı bırakılmasını öneriyor. Bu belge, Azure AD cihaz kayıt 1,2 hizmeti ile iletişim kurmak için kullanılan makinelerin kayıt işleminin tamamlanmasını ve iletişim kurmasını sağlama hakkında bilgi sağlayacaktır.

TLS protokol sürümü 1,2, güvenli iletişim sağlamak için tasarlanan bir şifreleme protokolüdür. TLS protokolü birincil olarak gizlilik ve veri bütünlüğü sağlamaya yöneliktir. TLS, 1,2 sürümüne sahip çok sayıda yinelemeden [(dış bağlantı) RFC 5246](https://tools.ietf.org/html/rfc5246)' de tanımlanmıştır.

Bağlantıların geçerli analizi, az TLS 1,1 ve 1,0 kullanımını gösterir, ancak bu bilgileri, etkilenen istemcileri veya sunucuları, TLS 1,1 ve 1,0 uçlarını desteklemek için gereken şekilde güncelleştirebilmeniz için sağlıyoruz. Karma senaryolar veya Active Directory Federasyon Hizmetleri (AD FS) (AD FS) için herhangi bir şirket içi altyapı kullanıyorsanız, altyapının TLS 1,2 kullanan gelen ve giden bağlantıları destekleyebiliyorsanız emin olun.

## <a name="update-windows-servers"></a>Windows Server 'ı güncelleştirme

Azure AD cihaz kayıt hizmeti 'ni kullanan veya proxy olarak görev yapan Windows sunucularında, TLS 1,2 ' nin etkinleştirildiğinden emin olmak için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Kayıt defterini güncelleştirdikten sonra değişikliklerin etkili olması için Windows Server 'ı yeniden başlatmanız gerekir.

### <a name="enable-tls-12"></a>TLS 1.2’yi etkinleştirme

Aşağıdaki kayıt defteri dizelerinin gösterildiği gibi yapılandırıldığından emin olun:

- HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client
  - "DisabledByDefault" = DWORD: 00000000
  - "Etkin" = DWORD: 00000001
- HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ sunucu
  - "DisabledByDefault" = DWORD: 00000000
  - "Etkin" = DWORD: 00000001
- HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \. NETFramework\v4.0.30319
  - "Schusestrongşifre" = DWORD: 00000001

## <a name="update-non-windows-proxies"></a>Windows olmayan proxy 'leri Güncelleştir

Cihazlar ve Azure AD cihaz kayıt hizmeti arasında proxy görevi gören tüm makineler TLS 1,2 ' nin etkinleştirildiğinden emin olmalıdır. Destek sağlamak için satıcınızın kılavuzunu izleyin.

## <a name="update-ad-fs-servers"></a>AD FS sunucularını Güncelleştir

Azure AD cihaz kayıt hizmeti ile iletişim kurmak için kullanılan AD FS sunucularının TLS 1,2 ' nin etkin olduğundan emin olması gerekir. Bu yapılandırmayı etkinleştirme/doğrulama hakkında bilgi için bkz. [AD FS IÇIN SSL/TLS protokollerini ve şifre paketlerini yönetme](/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs) .

## <a name="client-updates"></a>İstemci güncelleştirmeleri

Tüm istemci-sunucu ve tarayıcı-sunucu birleşimlerinin Azure AD cihaz kayıt hizmeti ile bağlantı kurmak için TLS 1,2 kullanması gerektiğinden, bu cihazları güncelleştirmeniz gerekebilir.

Aşağıdaki istemcilerin TLS 1,2 destekleyemediği bilinmektedir. Kesintisiz erişim sağlamak için istemcilerinizi güncelleştirin.

- Android sürüm 4,3 ve öncesi
- Firefox sürüm 5,0 ve öncesi
- Windows 7 ve önceki sürümlerde Internet Explorer sürümleri 8-10
- Windows Phone 8,0 üzerinde Internet Explorer 10
- OS X 10.8.4 ve önceki sürümlerde Safari sürüm 6.0.4

## <a name="next-steps"></a>Sonraki adımlar

[TLS/SSL 'ye Genel Bakış (Schannel SSP)](/windows-server/security/tls/tls-ssl-schannel-ssp-overview)