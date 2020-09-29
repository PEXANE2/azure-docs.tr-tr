---
title: Key Vault için Azure Defender-avantajlar ve Özellikler
description: Key Vault için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: 6649a8d470a75922aac423bf0b411163bdd79f71
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91449091"
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


> [!TIP]
> [Azure Güvenlik Merkezi 'nde Azure Key Vault tehdit algılamayı doğrulama](https://techcommunity.microsoft.com/t5/azure-security-center/validating-azure-key-vault-threat-detection-in-azure-security/ba-p/1220336)konusundaki yönergeleri Izleyerek Azure Defender 'ın Key Vault uyarıları benzetimi yapabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Key Vault için Azure Defender hakkında bilgi edindiniz.

İlgili malzemeler için aşağıdaki makalelere bakın: 

- [Güvenlik uyarılarını Key Vault](alerts-reference.md#alerts-azurekv)--tüm Azure Güvenlik Merkezi uyarıları için başvuru tablosunun Key Vault bölümü
- [Uyarıları SıEM 'ye aktarma](continuous-export.md)
- [Azure Defender 'dan uyarıları gösterme](alerts-suppression-rules.md)