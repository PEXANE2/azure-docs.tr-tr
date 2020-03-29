---
title: include dosyası
description: gizli istemci senaryo açılış sayfaları (daemon, web uygulaması, web API) için dosya dahil
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: a5d34ac7eea50b67bd679d8cb8ddecf7ca277abd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773378"
---
## <a name="register-secrets-or-certificates"></a>Sırları veya sertifikaları kaydetme

Herhangi bir gizli istemci başvurusu gelince, bir gizli veya sertifika kayıt gerekir. Uygulama sırlarınızı [Azure portalındaki](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) etkileşimli deneyimle veya komut satırı araçlarını (PowerShell gibi) kullanarak kaydedebilirsiniz.

### <a name="register-client-secrets-by-using-the-application-registration-portal"></a>Başvuru kayıt portalını kullanarak istemci sırlarını kaydetme

İstemci kimlik bilgilerinin yönetimi, bir uygulama için **Sertifikalar & sırları** sayfasında gerçekleşir:

![Sertifikalar & sırlar sayfası](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets.png)

- Uygulama sırrı (istemci sırrı olarak da adlandırılır), gizli istemci uygulamasının kaydı sırasında Azure AD tarafından oluşturulur. Bu **nesil, Yeni istemci sırrını**seçtiğinizde gerçekleşir. Bu noktada, **Kaydet'i**seçmeden önce uygulamanızda kullanılmak üzere gizli dizeyi panoya kopyalamanız gerekir. Bu dize artık sunulmayacak.
- Başvuru kaydı sırasında, sertifikayı yüklemek için **Sertifika** yükle düğmesini kullanırsınız. Azure AD, yalnızca uygulamada doğrudan kayıtlı olan ve sertifika zincirlerini izlemeyen sertifikaları destekler.

Ayrıntılar için [Quickstart: Web API'lerine erişmek için istemci uygulamasını yapılandırın | Uygulamanıza kimlik bilgileri ekleyin.](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application)



### <a name="register-client-secrets-by-using-powershell"></a>PowerShell'i kullanarak istemci sırlarını kaydetme

Alternatif olarak, komut satırı araçlarını kullanarak uygulamanızı Azure AD'ye kaydedebilirsiniz. [Active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) örneği, bir uygulama sırrının veya sertifikanın Azure AD uygulamasıyla nasıl kaydedilebildiğini gösterir:

- Bir uygulama sırrının nasıl kaydedileceklerine ilişkin ayrıntılar için [AppCreationScripts/Configure.ps1 'e](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190)bakın.
- Bir sertifikanın bir uygulamaya nasıl kaydedilenhakkında daha fazla bilgi için [AppCreationScripts-withCert/Configure.ps1'e](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178)bakın.
