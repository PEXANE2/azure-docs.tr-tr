---
title: Oturum açma sırasında kullanıcı adı araması - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory'de oturum açma sırasında ekrandaki mesajlaşmanın kullanıcı adı aramasını nasıl yansıttığı
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8b6a65a964016f702fcf75aa4cbdab33a952e3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74024245"
---
# <a name="home-realm-discovery-for-azure-active-directory-sign-in-pages"></a>Azure Active Directory oturum açma sayfaları için ana bölge keşfi

Yeni kimlik doğrulama yöntemlerine yer açmak ve kullanılabilirliği artırmak için Azure Active Directory (Azure AD) oturum açma davranışımızı değiştiriyoruz. Oturum açma sırasında Azure AD, kullanıcının nerede kimlik doğrulaması yapması gerektiğini belirler. Azure AD, oturum açma sayfasında girilen kullanıcı adı için kuruluş ve kullanıcı ayarlarını okuyarak akıllı kararlar verir. Bu, FIDO 2.0 gibi ek kimlik bilgilerini etkinleştiren parolasız bir geleceğe doğru atılmış bir adımdır.

## <a name="home-realm-discovery-behavior"></a>Ev alemi bulma davranışı

Tarihsel olarak, ana bölge keşfi oturum açma sırasında sağlanan etki alanı veya bazı eski uygulamalar için Bir Home Realm Discovery ilkesi tarafından yönetildi. Örneğin, bulma davranışımızda bir Azure Active Directory kullanıcısı kullanıcı adını yanlış yazabilir, ancak yine de kuruluşunun kimlik bilgisi toplama ekranına gelebilir. Bu, kullanıcı kuruluşun etki alanı adını "contoso.com" doğru şekilde verdiğinde oluşur. Bu davranış, tek bir kullanıcı için deneyimleri özelleştirmek için parçalı izin vermez.

Daha geniş bir kimlik bilgilerini desteklemek ve kullanılabilirliği artırmak için, oturum açma işlemi sırasında Azure Active Directory'nin kullanıcı adı arama davranışı artık güncelleştirildi. Yeni davranış, oturum açma sayfasında girilen kullanıcı adına göre kiracı ve kullanıcı düzeyi ayarlarını okuyarak akıllı kararlar verir. Bunu mümkün kılmak için Azure Active Directory, oturum açma sayfasında girilen kullanıcı adının belirtilen etki alanında bulununp var olmadığını veya kullanıcıyı kimlik bilgilerini sağlamak için yönlendirip yönlendirmediğini denetler.

Bu çalışmanın ek bir yararı hata iletisi geliştirilmiştir. Aşağıda, yalnızca Azure Etkin Dizin kullanıcılarını destekleyen bir uygulamada oturum açKen geliştirilmiş hata iletisine bazı örnekler verilmiştir.

- Kullanıcı adı yanlış yazılmış veya kullanıcı adı henüz Azure AD ile eşitlenmemiştir:
  
    ![kullanıcı adı yanlış yazılmış veya bulunamaz](./media/signin-realm-discovery/typo-username.png)
  
- Etki alanı adı yanlış yazılmış:
  
    ![alan adı yanlış yazılmış veya bulunamamıştır](./media/signin-realm-discovery/typo-domain.png)
  
- Kullanıcı bilinen bir tüketici etki alanı ile oturum açmaya çalışır:
  
    ![bilinen bir tüketici etki alanıyla oturum açma](./media/signin-realm-discovery/consumer-domain.png)
  
- Parola yanlış yazılmış, ancak kullanıcı adı doğrudur:  
  
    ![şifre iyi bir kullanıcı adı ile yanlış yazılır](./media/signin-realm-discovery/incorrect-password.png)
  
> [!IMPORTANT]
> Bu özellik, federasyonu zorlamak için eski etki alanı düzeyindeki Home Realm Discovery'ye güvenen federe etki alanları üzerinde etkili olabilir. Federe etki alanı desteğinin ne zaman eklenip ekleneceğine ilişkin güncelleştirmeler [için, Microsoft 365 hizmetleri için oturum açma sırasında Ana alan keşfine](https://azure.microsoft.com/updates/signin-hrd/)bakın. Bu arada, alan adı kullanıcıları şu anda kuruluşlarının etki alanı bitiş noktasına yönlendirdiği için, bazı kuruluşlar çalışanlarını Azure Etkin Dizini'nde bulunmayan ancak uygun etki alanı adını içeren bir kullanıcı adı ile oturum açmaları için eğitti. Yeni oturum açma davranışı buna izin vermez. Kullanıcıya kullanıcı adını düzeltmesi bildirilir ve Azure Active Directory'de bulunmayan bir kullanıcı adı ile oturum açmalarına izin verilmez.
>
> Sizin veya kuruluşunuz eski davranışa bağlı uygulamalara sahipse, kuruluş yöneticilerinin çalışan oturum açma ve kimlik doğrulama belgelerini güncelleştirmeleri ve çalışanları oturum açmak için Azure Etkin Dizin kullanıcı adlarını kullanmaları için eğitmek önemlidir.
  
Yeni davranışla ilgili endişeleriniz varsa, açıklamalarınızı bu makalenin **Geri Bildirim** bölümünde bırakın.  

## <a name="next-steps"></a>Sonraki adımlar

[Oturum açma markanızı özelleştirin](../fundamentals/add-custom-domain.md)
