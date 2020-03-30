---
title: What If aracını kullanarak Koşullu Erişimi Sorun Giderme - Azure Etkin Dizini
description: Koşullu Erişim ilkelerinin hangileri ve neden uygulandığı nerede
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: troubleshooting
ms.date: 07/03/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 661afc08c76c6cde61b02a29a55b4a8bec932e21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73175829"
---
# <a name="troubleshooting-conditional-access-using-the-what-if-tool"></a>What If aracını kullanarak Koşullu Erişim Sorunu Giderme

Koşullu Erişim'deki [İyiLer aracı,](what-if-tool.md) bir ilkenin belirli bir durumda kullanıcıya neden uygulanıp uygulanmadığını veya bilinen bir durumda bir ilkenin uygulanıp uygulanmayacağını anlamaya çalışırken güçlüdür.

Ne Varsa aracı **Azure portalı** > **Azure Active Directory** > **Koşullu Erişim** > **bulunur si .**

![Koşullu Erişim Varsayılan durumda araç Ne olur](./media/troubleshoot-conditional-access-what-if/conditional-access-what-if-tool.png)

> [!NOTE]
> Ne Varsa aracı şu anda yalnızca rapor modunda ilkeleri değerlendirmez.

## <a name="gathering-information"></a>Bilgi toplama

What If aracının başlatılması için yalnızca bir **Kullanıcı** gerektirir. 

Aşağıdaki ek bilgiler isteğe bağlıdır, ancak belirli servis taleplerinin kapsamını daraltmaya yardımcı olur.

* Bulut uygulamaları veya eylemleri
* IP adresi 
* Ülke
* Cihaz platformu
* İstemci uygulamaları (önizleme)
* Aygıt durumu (önizleme) 
* Oturum açma riski

Bu bilgiler kullanıcıdan, aygıtlarından veya Azure AD oturum açma günlüğünden toplanabilir.

## <a name="generating-results"></a>Sonuç oluşturma

Önceki bölümde toplanan ölçütleri girdiniz ve bir sonuç listesi oluşturmak için **Ne Olur'u** seçin. 

Herhangi bir noktada, herhangi bir ölçüt girdisini temizlemek ve varsayılan duruma dönmek için **Sıfırla'yı** seçebilirsiniz.

## <a name="evaluating-results"></a>Sonuçları değerlendirme

### <a name="policies-that-will-apply"></a>Uygulanacak ilkeler

Bu liste, koşullar göz önüne alındığında hangi Koşullu Erişim ilkelerinin uygulanacağını gösterir. Liste, geçerli hibe ve oturum denetimlerini içerir. Örnekler arasında belirli bir uygulamaya erişmek için çok faktörlü kimlik doğrulaması gerektirilmesi verilebilir.

### <a name="policies-that-will-not-apply"></a>Geçerli olmayacak ilkeler

Bu liste, uygulanan koşullar uygulandığında uygulanmayacak Koşullu Erişim ilkelerini gösterir. Liste, herhangi bir ilkeyi ve neden geçerli olmadıklarını içerir. Örnekler arasında, bir ilkenin dışında tutulabilecek kullanıcılar ve gruplar verilebilir.

## <a name="use-case"></a>Kullanım örneği

Birçok kuruluş ağ konumlarına dayalı ilkeler oluşturur, güvenilen konumlara izin verilir ve erişimin gerçekleşmemesi gereken konumları engeller.

Bir yapılandırmanın uygun şekilde yapıldığını doğrulamak için, yönetici erişimi taklit etmek için, izin verilmesi gereken bir konumdan ve reddedilmesi gereken bir konumdan What If aracını kullanabilir.

![Engel erişimi ile sonuçları gösteren araç Ne Olur](./media/troubleshoot-conditional-access-what-if/conditional-access-what-if-results.png)

Bu durumda, Contoso'nun bu konumdan erişimi engellediği için kullanıcının Kuzey Kore seyahatinde herhangi bir bulut uygulamasına erişmeleri engellenir.

Bu sınama kapsamı daraltmak için diğer veri noktaları dahil etmek için genişletilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Koşullu Erişim Nedir?](overview.md)
* [Azure Active Directory Kimlik Koruması nedir?](../identity-protection/overview-v2.md)
* [Cihaz kimliği nedir?](../devices/overview.md)
* [Nasıl çalışır: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md)
