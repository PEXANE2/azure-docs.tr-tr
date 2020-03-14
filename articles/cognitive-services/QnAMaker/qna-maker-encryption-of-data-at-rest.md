---
title: Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: bd64321a6a7afbac61a63365c77a75120f837e83
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79372100"
---
# <a name="qna-maker-encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma

Soru-Cevap Oluşturma, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler, böylece kurumsal güvenlik ve uyumluluk hedeflerinizi karşılamanıza yardımcı olur.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Aboneliğiniz varsayılan olarak Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Ayrıca kendi anahtarlarınız ile aboneliğinizi yönetme seçeneği de vardır. Müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Soru-Cevap Oluşturma Azure Search 'ten CMK desteği kullanır. [Azure Key Vault kullanarak CMK Azure Search](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)oluşturmanız gerekir. Bu Azure örneği, CMK 'ın etkinleştirilmesini sağlamak için Soru-Cevap Oluşturma Hizmeti ile ilişkilendirilmelidir.

> [!IMPORTANT]
> Azure Search hizmeti kaynağınız 2019 Ocak 'tan sonra oluşturulmuş olmalı ve ücretsiz (paylaşılan) katmanda olamaz. Azure portal müşteri tarafından yönetilen anahtarları yapılandırma desteği yoktur.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

Soru-Cevap Oluşturma hizmeti Azure Search hizmetinden CMK kullanır. CMKs 'i etkinleştirmek için şu adımları izleyin:

1. Yeni bir Azure Search örneği oluşturun ve [Azure bilişsel arama için müşteri tarafından yönetilen anahtar önkoşulları](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys#prerequisites)bölümünde bahsedilen önkoşulları etkinleştirin.

   ![Şifreleme ayarlarını görüntüle](../media/cognitive-services-encryption/qna-encryption-1.png)

2. Bir Soru-Cevap Oluşturma kaynağı oluşturduğunuzda, otomatik olarak bir Azure Search örneğiyle ilişkilendirilir. Bu, CMK ile kullanılamaz. CMK 'yi kullanmak için, adım 1 ' de oluşturulan yeni oluşturulan Azure Search örneğini ilişkilendirmeniz gerekir. Özellikle, Soru-Cevap Oluşturma kaynağınızın `AzureSearchAdminKey` ve `AzureSearchName` güncelleştirmeniz gerekir.

   ![Şifreleme ayarlarını görüntüle](../media/cognitive-services-encryption/qna-encryption-2.png)

3. Sonra, yeni bir uygulama ayarı oluşturun:
   * **Ad**: bunu `CustomerManagedEncryptionKeyUrl` ayarla
   * **Değer**: Azure Search örneğinizi oluştururken 1. adımda aldığınız değerdir.

   ![Şifreleme ayarlarını görüntüle](../media/cognitive-services-encryption/qna-encryption-3.png)

4. İşiniz bittiğinde çalışma zamanını yeniden başlatın. Artık Soru-Cevap Oluşturma hizmetiniz CMK-etkinleştirilmiştir.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar tüm Azure Search bölgelerde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault 'de CMKs kullanarak Azure Search şifreleme](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)
* [Bekleme sırasında veri şifrelemesi](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
