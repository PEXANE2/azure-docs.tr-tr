---
title: QnA Maker veri şifrelemesi
titleSuffix: Azure Cognitive Services
description: QnA Maker veri şifreleme sada.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: bd64321a6a7afbac61a63365c77a75120f837e83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372100"
---
# <a name="qna-maker-encryption-of-data-at-rest"></a>QnA Maker veri şifrelemesi

QnA Maker, verilerinizi bulutta kalıcı olduğunda otomatik olarak şifreler ve kuruluş güvenliği ve uyumluluk hedeflerinize uymanıza yardımcı olur.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Varsayılan olarak, aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Aboneliğinizi kendi anahtarlarınızla yönetme seçeneği de vardır. Müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimleri oluşturmak, döndürmek, devre dışı etmek ve iptal etmek için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

QnA Maker, Azure aramasından CMK desteği kullanır. Azure Anahtar [Kasası'nı kullanarak Azure Arama'da CMK](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)oluşturmanız gerekir. Bu Azure örneği, CMK'yı etkin kılmak için QnA Maker hizmetiyle ilişkilendirilmelidir.

> [!IMPORTANT]
> Azure Arama hizmet kaynağınız Ocak 2019'dan sonra oluşturulmuş olmalıdır ve ücretsiz (paylaşılan) katmanda olamaz. Azure portalında müşteri tarafından yönetilen anahtarları yapılandırmak için destek yoktur.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme

QnA Maker hizmeti, Azure Arama hizmetinden CMK kullanır. CMK'ları etkinleştirmek için aşağıdaki adımları izleyin:

1. Yeni bir Azure Arama örneği oluşturun ve [Azure Bilişsel Arama için müşteri tarafından yönetilen temel ön koşullarda](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys#prerequisites)belirtilen ön koşulları etkinleştirin.

   ![Şifreleme ayarlarını görüntüleme](../media/cognitive-services-encryption/qna-encryption-1.png)

2. Bir QnA Maker kaynağı oluşturduğunuzda, bu kaynak otomatik olarak bir Azure Arama örneğiyle ilişkilendirilsin. Bu CMK ile kullanılamaz. CMK'yı kullanmak için, adım 1'de oluşturulan yeni oluşturulmuş Azure Arama örneğini ilişkilendirmeniz gerekir. Özellikle, `AzureSearchAdminKey` ve `AzureSearchName` QnA Maker kaynak güncellemeniz gerekir.

   ![Şifreleme ayarlarını görüntüleme](../media/cognitive-services-encryption/qna-encryption-2.png)

3. Ardından, yeni bir uygulama ayarı oluşturun:
   * **Adı**: Bu ayarlayın`CustomerManagedEncryptionKeyUrl`
   * **Değer**: Azure Arama örneğini oluştururken Adım 1'de aldığınız değerdir.

   ![Şifreleme ayarlarını görüntüleme](../media/cognitive-services-encryption/qna-encryption-3.png)

4. Tamamlandığında, çalışma süresini yeniden başlatın. QnA Maker hizmetiniz cmk özellikli.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar tüm Azure Arama bölgelerinde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Anahtar Kasasında CMK'ları kullanarak Azure Arama'da Şifreleme](https://docs.microsoft.com/azure/search/search-security-manage-encryption-keys)
* [Bekleme sırasında veri şifrelemesi](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)
* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
