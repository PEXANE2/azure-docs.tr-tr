---
title: Key Vault için Azure Defender-avantajlar ve Özellikler
description: Key Vault için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: conceptual
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: 06818e443568918e2ee87bbfbec81836ea85648b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941842"
---
# <a name="introduction-to-azure-defender-for-key-vault"></a>Key Vault için Azure Defender 'a giriş

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

Azure yerel için **Key Vault Için Azure Defender** 'ı etkinleştirin ve Azure Key Vault için Gelişmiş tehdit koruması, ek bir güvenlik zekası katmanı sağlar. 

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|**Key Vault Için Azure Defender** , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-key-vault"></a>Key Vault için Azure Defender 'ın avantajları nelerdir?

Azure Defender, Key Vault hesaplarına erişme veya yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmadan ve üçüncü taraf güvenlik izleme sistemlerini yönetmeye gerek kalmadan tehditleri ele almanıza olanak sağlar.  

Anormal etkinlikler gerçekleştiğinde, Azure Defender uyarıları gösterir ve isteğe bağlı olarak bunları kuruluşunuzun ilgili üyelerine e-posta ile gönderir. Bu uyarılar, şüpheli etkinliğin ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir. 

## <a name="azure-defender-for-key-vault-alerts"></a>Key Vault uyarılar için Azure Defender
Key Vault için Azure Defender 'dan bir uyarı aldığınızda, [Key Vault Için Azure Defender 'A yanıt verme](defender-for-key-vault-usage.md)bölümünde açıklandığı gibi uyarıyı araştırıp yanıtlamanız önerilir. Key Vault için Azure Defender, uygulamaları ve kimlik bilgilerini korur. bu nedenle, uyarıyı tetikleyen uygulama veya Kullanıcı hakkında bilgi sahibi olsanız bile, her uyarının çevresindeki durumu kontrol etmeniz önemlidir.

Uyarılar Key Vault **güvenlik** sayfasında, Azure Defender panosunda ve Güvenlik Merkezi 'nin Uyarılar sayfasında görüntülenir.

:::image type="content" source="./media/defender-for-key-vault-intro/key-vault-security-page.png" alt-text="Azure Key Vault güvenlik sayfası":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Key Vault için Azure Defender hakkında bilgi edindiniz.

İlgili malzemeler için aşağıdaki makalelere bakın: 

- [Güvenlik uyarılarını Key Vault](alerts-reference.md#alerts-azurekv)--tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosunun Key Vault bölümü
- [Uyarıları SıEM 'ye aktarma](continuous-export.md)
- [Azure Defender 'dan uyarıları gösterme](alerts-suppression-rules.md)