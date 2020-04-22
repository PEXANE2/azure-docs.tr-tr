---
title: Azure AD Connect "Yapılandırma" | Microsoft Dokümanlar
description: Bu makalede, Azure AD Connect özelliği ile PTA'nın nasıl devre dışı kıldığı açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 04/20/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa1046dc64fed3edb6c9d04f76a96f488769ff42
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726806"
---
# <a name="disable-pta-when-using-azure-ad-connect-do-not-configure"></a>Azure AD Connect "Yapılandırma" kullanırken PTA'yı devre dışı

Azure AD Connect ile Geçiş Kimlik Doğrulaması kullanıyorsanız ve "Yapılandırmayın" olarak ayarlanmışsa, devre dışı bırakabilirsiniz. PTA'nın devre dışı bırakılması aşağıdaki cmdlets kullanılarak yapılabilir. 

## <a name="prerequisites"></a>Ön koşullar
Aşağıdaki ön koşullar gereklidir:
- PTA aracısını yüklü olan tüm windows makinesi. 
- Aracı, sürüm 1.5.1742.0 veya sonraki sürümde olmalıdır. 
- PowerShell cmdlets'i Çalıştırmak için Bir Azure global yönetici hesabı PTA'yı devre dışı bırakırsak.

>[!NOTE]
> Aracınız daha yaşlıysa, bu işlemi tamamlamak için gereken cmdletler olmayabilir. Azure Portal'dan yeni bir aracıyı herhangi bir windows makinesine yükleyebilir ve yönetici kimlik bilgilerini sağlayabilirsiniz. (Aracının yüklenmesi buluttaki PTA durumunu etkilemez)

> [!IMPORTANT]
> Azure Devlet bulutu kullanıyorsanız, aşağıdaki değere sahip ENVIRONMENTNAME parametresini geçmeniz gerekir. 
>
>| Ortam Adı | Bulut |
>| - | - |
>| AzureUSGovernment | US Gov|


## <a name="to-disable-pta"></a>PTA'yı devre dışı kayırmak için
PowerShell oturumu içinden, PTA'yı devre dışı kullanabilirsiniz:
1. PS C:\Program Files\Microsoft Azure AD Connect Kimlik Doğrulama Aracısı>`Import-Module .\Modules\PassthroughAuthPSModule`
2. `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth` veya `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth -EnvironmentName <identifier>`
3. `Disable-PassthroughAuthentication  -Feature PassthroughAuth` veya `Disable-PassthroughAuthentication -Feature PassthroughAuth -EnvironmentName <identifier>`

## <a name="if-you-dont-have-access-to-an-agent"></a>Bir temsilciye erişiminiz yoksa

Bir aracı makineniz yoksa, aracı yüklemek için aşağıdaki komutu kullanabilirsiniz.

1. portal.azure.com en son Auth Agent indirin.
2. Özelliği yükleyin: `.\AADConnectAuthAgentSetup.exe` veya`.\AADConnectAuthAgentSetup.exe ENVIRONMENTNAME=<identifier>`


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory Geçişli Kimlik Doğrulaması ile kullanıcı oturumu açma](how-to-connect-pta.md)