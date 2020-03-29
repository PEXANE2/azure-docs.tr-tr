---
title: Azure MFA NPS uzantısını yapılandırın - Azure Etkin Dizini
description: NPS uzantısını yükledikten sonra, IP beyaz liste ve UPN değiştirme gibi gelişmiş yapılandırma için bu adımları kullanın.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ea5b4f52fc161cb8359ef56e76e0607459d6280
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848366"
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Multi-Factor Authentication NPS uzantısı için gelişmiş yapılandırma seçenekleri

Ağ İlkesi Sunucusu (NPS) uzantısı, bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama özelliklerinizi şirket içi altyapınıza genişletir. Bu makalede, uzantı zaten yüklü olduğunu varsayar ve şimdi nasıl ihtiyaçlarınız için uzantısı özelleştirmek için bilmek istiyorum. 

## <a name="alternate-login-id"></a>Alternatif giriş kimliği

NPS uzantısı hem şirket içi hem de bulut dizinlerinize bağlandığından, şirket içi kullanıcı ana adlarınızın (UPN' ler) buluttaki adlarla eşleşmediği bir sorunla karşılaşabilirsiniz. Bu sorunu çözmek için alternatif giriş eklerini kullanın. 

NPS uzantısı içinde, Azure Çok Faktörlü Kimlik Doğrulaması için UPN yerine kullanılacak bir Etkin Dizin özniteliği atayabilirsiniz. Bu, şirket içi UPN'lerinizi değiştirmeden şirket içi kaynaklarınızı iki aşamalı doğrulamayla korumanızı sağlar. 

Alternatif giriş bağlantı larını yapılandırmak için `HKLM\SOFTWARE\Microsoft\AzureMfa` aşağıdaki kayıt defteri değerlerine gidin ve edin:

| Adı | Tür | Varsayılan değer | Açıklama |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | string | Boş | UPN yerine kullanmak istediğiniz Active Directory özniteliğinin adını belirleyin. Bu öznitelik AlternateLoginId özniteliği olarak kullanılır. Bu kayıt defteri değeri geçerli bir [Active Directory özniteliği](https://msdn.microsoft.com/library/ms675090.aspx) (örneğin, posta veya displayName) olarak ayarlanmışsa, özniteliğin değeri kimlik doğrulaması için kullanıcının UPN yerine kullanılır. Bu kayıt defteri değeri boşsa veya yapılandırılmamışsa, AlternateLoginId devre dışı bırakılır ve kullanıcının UPN'si kimlik doğrulaması için kullanılır. |
| LDAP_FORCE_GLOBAL_CATALOG | boole | False | AlternateLoginId'i ararken LDAP aramaları için Global Catalog'un kullanımını zorlamak için bu bayrağı kullanın. Etki alanı denetleyicisini Global Katalog olarak yapılandırın, Global Catalog'a AlternateLoginId özniteliğini ekleyin ve bu bayrağı etkinleştirin. <br><br> LDAP_LOOKUP_FORESTS yapılandırılırsa (boş değilse), bu bayrak, kayıt defteri ayarının değerine bakılmaksızın **gerçek olarak zorlanır.** Bu durumda, NPS uzantısı, Küresel Kataloğun her orman için AlternateLoginId özniteliği ile yapılandırılmasını gerektirir. |
| LDAP_LOOKUP_FORESTS | string | Boş | Aramak için ormanların yarı-kolon ayrılmış listesini sağlayın. Örneğin, *contoso.com;foobar.com*. Bu kayıt defteri değeri yapılandırılırsa, NPS uzantısı tüm ormanları listelendikleri sırada yinelemeli olarak arar ve ilk başarılı AlternativeLoginId değerini döndürür. Bu kayıt defteri değeri yapılandırılmamışsa, AlternativeLoginId araması geçerli etki alanıyla sınırlıdır.|

Alternatif giriş kimlikleriyle ilgili sorunları gidermek [için, Alternatif giriş kimliği hataları](howto-mfa-nps-extension-errors.md#alternate-login-id-errors)için önerilen adımları kullanın.

## <a name="ip-exceptions"></a>IP özel durumları

Yük dengeleyicileri iş yüklerini göndermeden önce hangi sunucuların çalıştığını doğrularsa, sunucu kullanılabilirliğini izlemeniz gerekiyorsa, bu denetimlerin doğrulama istekleri tarafından engellenmesini istemezsiniz. Bunun yerine, hizmet hesapları tarafından kullanıldığını bildiğiniz IP adreslerinin bir listesini oluşturun ve bu liste için Çok Faktörlü Kimlik Doğrulama gereksinimlerini devre dışı kılabilir.

IP izin verilen bir listeyi `HKLM\SOFTWARE\Microsoft\AzureMfa` yapılandırmak için aşağıdaki kayıt defteri değerine gidin ve yapılandırıldı:

| Adı | Tür | Varsayılan değer | Açıklama |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | string | Boş | IP adreslerinin yarı nokta noktalı ayrılmış bir listesini sağlayın. NAS/VPN sunucusu gibi hizmet isteklerinin kaynaklandığı makinelerin IP adreslerini ekleyin. IP aralıkları ve alt ağları desteklenmez. <br><br> Örneğin, *10.0.0.1;10.0.0.2;10.0.0.3*.

> [!NOTE]
> Bu kayıt defteri anahtarı yükleyici tarafından varsayılan olarak oluşturulmaz ve hizmet yeniden başlatıldığında AuthZOptCh günlüğünde bir hata belirir. Günlükteki bu hata yoksayılabilir, ancak bu kayıt defteri anahtarı oluşturulur ve gerekli değilse boş bırakılırsa hata iletisi dönmez.

Bir `IP_WHITELIST`istek, iki aşamalı doğrulamada bulunan bir IP adresinden geldiğinde atlanır. IP listesi, RADIUS isteğinin *ratNASIPAddress* özniteliğiiçinde sağlanan IP adresiyle karşılaştırılır. Bir RADIUS isteği ratNASIPAddress özniteliği olmadan gelirse, aşağıdaki uyarı günlüğe kaydedilir: "P_WHITE_LIST_WARNING::IP Beyaz Liste kaynak IP NasIpAddress özniteliği radius isteği eksik olduğu gibi göz ardı ediliyor."

## <a name="next-steps"></a>Sonraki adımlar

[Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md)
