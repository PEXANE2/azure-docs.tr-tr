---
title: include dosyası
description: Gizli istemci senaryosu giriş sayfaları için dosya ekleme (Daemon, Web uygulaması, Web API)
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
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773378"
---
## <a name="register-secrets-or-certificates"></a>Gizli dizileri veya sertifikaları Kaydet

Tüm gizli istemci uygulamaları için bir gizli dizi veya sertifika kaydetmeniz gerekir. [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) etkileşimli deneyim aracılığıyla veya komut satırı araçlarını (PowerShell gibi) kullanarak uygulama gizli dizilerini kaydedebilirsiniz.

### <a name="register-client-secrets-by-using-the-application-registration-portal"></a>Uygulama kayıt portalını kullanarak istemci gizli dizilerini kaydetme

İstemci kimlik bilgilerinin yönetimi, bir uygulama için **sertifikalar & gizli** diziler sayfasında gerçekleşir:

![Sertifikalar & gizlilikler sayfası](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets.png)

- Gizli istemci uygulamasının kaydı sırasında, uygulama gizli anahtarı (istemci gizli dizisi olarak da adlandırılır) Azure AD tarafından oluşturulur. **Yeni istemci parolası**' nı seçtiğinizde bu oluşturma gerçekleşir. Bu noktada, **Kaydet**' i seçmeden önce uygulamanızda kullanılması için gizli dizeyi panoya kopyalamanız gerekir. Bu dize daha fazla sunulmaz.
- Uygulama kaydı sırasında sertifikayı karşıya yüklemek için **sertifikayı karşıya yükle** düğmesini kullanırsınız. Azure AD yalnızca uygulamada doğrudan kayıtlı olan sertifikaları destekler ve sertifika zincirlerini takip etmeyin.

Ayrıntılar için bkz [. hızlı başlangıç: Web API 'lerine erişmek için istemci uygulaması yapılandırma | Uygulamanıza kimlik bilgileri ekleyin](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application).



### <a name="register-client-secrets-by-using-powershell"></a>İstemci gizli dizilerini PowerShell kullanarak kaydetme

Alternatif olarak, komut satırı araçlarını kullanarak uygulamanızı Azure AD 'ye kaydedebilirsiniz. [Active-Directory-dotnetcore-Daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) örneği, BIR Azure AD uygulaması ile uygulama gizli dizisi veya sertifikasının nasıl kaydedileceği gösterilmektedir:

- Uygulama gizliliğini kaydetme hakkında ayrıntılı bilgi için bkz. [Appcreationscripts/configure. ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190).
- Bir uygulama ile bir sertifikayı kaydetme hakkında ayrıntılı bilgi için bkz. [AppCreationScripts-withCert/configure. ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178).
