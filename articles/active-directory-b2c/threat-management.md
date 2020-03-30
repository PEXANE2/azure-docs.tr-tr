---
title: Kaynaklara ve verilere yönelik tehditleri yönetme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de hizmet reddi saldırıları ve parola saldırıları için algılama ve azaltma teknikleri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a438363b054361420222804dffac7973470e82e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183610"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kaynaklara ve verilere yönelik tehditleri yönetme

Azure Active Directory B2C (Azure AD B2C), kaynaklarınıza ve verilerinize yönelik tehditlere karşı korumanıza yardımcı olabilecek yerleşik özelliklere sahiptir. Bu tehditler arasında hizmet reddi saldırıları ve parola saldırıları yer almaktadır. Hizmet reddi saldırıları, kaynakların amaçlanan kullanıcılar için kullanılamaz hale getirilebilir. Parola saldırıları kaynaklara yetkisiz erişimsağlar.

## <a name="denial-of-service-attacks"></a>Hizmet reddi saldırıları

Azure AD B2C, SYN çerezi kullanarak SYN sel saldırılarına karşı savunma eder. Azure AD B2C, hızlar ve bağlantılar için sınırlar kullanarak hizmet reddi saldırılarına karşı da koruma sağlar.

## <a name="password-attacks"></a>Parola saldırıları

Kullanıcılar tarafından ayarlanan parolaların oldukça karmaşık olması gerekir. Azure AD B2C, parola saldırıları için azaltma tekniklerine sahiptir. Azaltma kaba kuvvet parola saldırıları ve sözlük parola saldırıları algılama içerir. Azure AD B2C, çeşitli sinyaller kullanarak isteklerin bütünlüğünü analiz eder. Azure AD B2C, amaçlanan kullanıcıları bilgisayar korsanlarından ve botnetlerden akıllıca ayırt etmek için tasarlanmıştır.

Azure AD B2C, hesapları kilitlemek için gelişmiş bir strateji kullanır. Hesaplar isteğin IP'si ve girilen parolalara göre kilitlenir. Lokavtın süresi, saldırı olma olasılığına bağlı olarak da artar. Parola 10 kez başarısız olarak denendikten sonra (varsayılan deneme eşiği), bir dakikalık bir kilitleme oluşur. Hesap açıldıktan sonra bir sonraki oturum açma başarısız olduğunda (diğer bir deyişle, lokavt süresi sona erdikten sonra hesabın kilidi servis tarafından otomatik olarak açıldıktan sonra), bir dakikalık başka bir kilitleme oluşur ve her başarısız giriş için devam eder. Aynı parolayı tekrar tekrar girmek birden çok başarısız giriş sayılmaz.

İlk 10 lokavt süresi bir dakika uzunluğundadır. Sonraki 10 lokavt süresi biraz daha uzundur ve her 10 lokavt döneminden sonra süre artar. Hesap kilitli olmadığında başarılı bir oturum açtıktan sonra kilitleme sayacı sıfırlanır. Lokavt süreleri beş saate kadar sürebilir.

## <a name="manage-password-protection-settings"></a>Parola koruma ayarlarını yönetme

Kilitleme eşiği de dahil olmak üzere parola koruma ayarlarını yönetmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın
1. Azure AD B2C kiracınızı içeren dizin seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Güvenlik**altında, **Kimlik Doğrulama yöntemlerini (Önizleme)** seçin, ardından **Parola koruması'nı**seçin.
1. İstediğiniz parola koruma ayarlarını girin ve ardından **Kaydet'i**seçin.

    ![Azure REKLAM ayarlarında Azure portalı Parola koruma sayfası](./media/threat-management/portal-02-password-protection.png)
    <br />* **Parola koruma** ayarlarında kilitleme eşiğini 5 olarak ayarlama.*

## <a name="view-locked-out-accounts"></a>Kilitlenmiş hesapları görüntüleme

Kilitli hesaplar hakkında bilgi almak için Etkin Dizin [oturum açma faaliyet raporunu](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md)kontrol edebilirsiniz. **Durum**altında, **Hata'yı**seçin. **Oturum açma hata koduyla** başarısız oturum `50053` açma denemeleri kilitli bir hesabı gösterir:

![Azure AD oturum açma raporunun kilitli hesabı gösteren bölümü](./media/threat-management/portal-01-locked-account.png)

Azure Etkin Dizini'nde oturum açma faaliyet raporunu görüntüleme hakkında bilgi edinmek için [oturum açma etkinlik raporu hata kodlarına](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md)bakın.
