---
title: Azure Active Directory B2C kaynakları ve verileri tehditlere karşı yönetme
description: Azure Active Directory B2C hizmet reddi saldırıları ve parola saldırıları için algılama ve risk azaltma teknikleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 281bd73671352e1e525e11a7bfde1882d3ef8864
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065413"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Azure Active Directory B2C kaynakları ve verileri tehditlere karşı yönetme

Azure Active Directory B2C (Azure AD B2C), kaynaklarınız ve verileriniz için tehditlere karşı korumanıza yardımcı olabilecek yerleşik özelliklere sahiptir. Bu tehditler, hizmet reddi saldırıları ve parola saldırıları içerir. Hizmet reddi saldırıları, kaynakları amaçlanan kullanıcılar için kullanılamaz hale getirir. Parola saldırıları kaynaklara yetkisiz erişime yol açabilir.

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırıları

SYN tanımlama bilgilerini kullanarak SYN savunma saldırılarına karşı savunma Azure AD B2C. Azure AD B2C Ayrıca ücretler ve bağlantılar için sınırları kullanarak hizmet reddi saldırılarına karşı koruma sağlar.

## <a name="password-attacks"></a>Parola saldırıları

Kullanıcılar tarafından ayarlanan parolaların makul bir şekilde karmaşık olması gerekir. Azure AD B2C, parola saldırıları için yerinde risk azaltma tekniklerini içerir. Hafifletme, deneme yanılma, parola saldırılarını ve sözlük parola saldırılarını algılamayı içerir. Çeşitli sinyalleri kullanarak isteklerin bütünlüğünü analiz Azure AD B2C. Azure AD B2C, Kullanıcı korsanlarından ve botlardan hedeflenen kullanıcıları akıllıca ayırt etmek için tasarlanmıştır.

Azure AD B2C, hesapları kilitlemek için gelişmiş bir strateji kullanır. Hesaplar, isteğin IP 'si ve girilen parolalar temelinde kilitlenir. Kilitleme süresi de bir saldırı olma olasılığını temel alarak artar. Parolanın süresi 10 kez denendikten sonra (varsayılan deneme eşiği), tek dakikalık bir kilitleme meydana gelir. Hesabın kilidi açıldıktan sonra bir sonraki oturum açma işlemi başarısız olursa (yani, kilitleme süresi dolduktan sonra hesap hizmet tarafından otomatik olarak açıldıktan sonra), bir dakikalık kilit oluşur ve her başarısız oturum açma işlemi için devam eder. Aynı parolanın tekrar tekrar girilmesi birden çok başarısız oturum açma sayısı olarak sayılır.

İlk 10 kilitleme dönemi bir dakika uzunluğundadır. Sonraki 10 kilitleme dönemi biraz daha uzundur ve her 10 kilitleme süresinden sonra süre içinde artar. Hesap kilitlenmediğinde, başarılı bir oturum açma işleminden sonra kilitleme sayacı sıfıra sıfırlanır. Kilitleme süreleri en fazla beş saate kadar sürebilir.

## <a name="manage-password-protection-settings"></a>Parola koruma ayarlarını yönetme

Kilitleme eşiği dahil parola koruma ayarlarını yönetmek için:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Portalın sağ üst menüsünde **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı seçin.
1. Sol taraftaki menüden **Azure Active Directory** ' yi seçin (veya portalın sol üst kısmındaki **tüm hizmetler** ' i seçin ve *Azure Active Directory*' yı arayıp seçin.
1. **Güvenlik**altında **kimlik doğrulama yöntemleri**' ni seçin ve **parola koruması**' nı seçin.
1. İstediğiniz parola koruma ayarlarınızı girip **Kaydet**' i seçin.

    ![Azure AD ayarlarındaki Azure portal parola koruma sayfası](media/active-directory-b2c-reference-threat-management/portal-02-password-protection.png)
    <br />***Parola koruma** ayarlarında kilitleme eşiğini 5 olarak ayarlama*.

## <a name="view-locked-out-accounts"></a>Kilitli hesapları görüntüleme

Kilitli hesaplar hakkında bilgi edinmek için Active Directory [oturum açma etkinliği raporunu](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md)kontrol edebilirsiniz. **Durum**' un altında, **hata**' ı seçin. Kilitli bir hesabı `50053` göstermek için **oturum açma hata kodu** ile oturum açma girişimleri başarısız oldu:

![Kilitli hesabı gösteren Azure AD oturum açma raporunun bölümü](media/active-directory-b2c-reference-threat-management/portal-01-locked-account.png)

Azure Active Directory oturum açma etkinliği raporunu görüntüleme hakkında bilgi edinmek için bkz. [oturum açma etkinlik raporu hata kodları](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md).
