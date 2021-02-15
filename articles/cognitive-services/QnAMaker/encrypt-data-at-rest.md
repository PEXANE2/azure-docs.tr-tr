---
title: Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Microsoft, Microsoft tarafından yönetilen şifreleme anahtarları sunar ve ayrıca bilişsel hizmetler aboneliklerinizi, müşteri tarafından yönetilen anahtarlar (CMK) olarak adlandırılan kendi Anahtarlarınıza göre yönetmenizi sağlar. Bu makalede, Soru-Cevap Oluşturma için bekleyen veri şifrelemesi ve CMK 'nin nasıl etkinleştirileceği ve yönetileceği ele alınmaktadır.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.author: egeaney
ms.openlocfilehash: 19dc0f3a676d5373b28e4b7055050477c426f847
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100524534"
---
# <a name="qna-maker-encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi Soru-Cevap Oluşturma

Soru-Cevap Oluşturma, bulutta kalıcı olduğunda verilerinizi otomatik olarak şifreler, böylece kurumsal güvenlik ve uyumluluk hedeflerinizi karşılamanıza yardımcı olur.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Aboneliğiniz varsayılan olarak Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Aboneliğinizi, müşteri tarafından yönetilen anahtarlar (CMK) adlı kendi anahtarlarınız ile yönetme seçeneği de vardır. CMK, erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz. Aboneliğiniz için CMK yapılandırıldıysa, ikinci bir koruma katmanı sunan ve Azure Key Vault şifreleme anahtarını denetlemenizi sağlayan çift şifreleme sağlanır.

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Soru-Cevap Oluşturma Azure Search 'ten CMK desteği kullanır. [Azure Search Azure Key Vault kullanarak CMK](../../search/search-security-manage-encryption-keys.md)'ı yapılandırın. Bu Azure örneği, CMK 'ın etkinleştirilmesini sağlamak için Soru-Cevap Oluşturma Hizmeti ile ilişkilendirilmelidir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Soru-Cevap Oluşturma [Azure Search 'Ten CMK desteğini](../../search/search-security-manage-encryption-keys.md)kullanır ve Azure Search dizininde depolanan verileri şifrelemek için belirtilen CMK 'yi otomatik olarak ilişkilendirir.

---

> [!IMPORTANT]
> Azure Search hizmeti kaynağınız 2019 Ocak 'tan sonra oluşturulmuş olmalı ve ücretsiz (paylaşılan) katmanda olamaz. Azure portal müşteri tarafından yönetilen anahtarları yapılandırma desteği yoktur.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

Soru-Cevap Oluşturma hizmeti Azure Search hizmetinden CMK kullanır. CMKs 'i etkinleştirmek için şu adımları izleyin:

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

1. Yeni bir Azure Search örneği oluşturun ve [Azure bilişsel arama için müşteri tarafından yönetilen anahtar önkoşulları](../../search/search-security-manage-encryption-keys.md#prerequisites)bölümünde bahsedilen önkoşulları etkinleştirin.

   ![Şifreleme ayarlarını görüntüleyin 1](../media/cognitive-services-encryption/qna-encryption-1.png)

2. Bir Soru-Cevap Oluşturma kaynağı oluşturduğunuzda, otomatik olarak bir Azure Search örneğiyle ilişkilendirilir. Bu örnek CMK ile kullanılamaz. CMK 'yi kullanmak için, adım 1 ' de oluşturulan yeni oluşturulan Azure Search örneğini ilişkilendirmeniz gerekir. Özellikle, `AzureSearchAdminKey` soru-cevap oluşturma kaynağınız içinde ve ' ı güncelleştirmeniz gerekir `AzureSearchName` .

   ![Şifreleme ayarlarını görüntüle 2](../media/cognitive-services-encryption/qna-encryption-2.png)

3. Sonra, yeni bir uygulama ayarı oluşturun:
   * **Ad**: ayarla `CustomerManagedEncryptionKeyUrl`
   * **Değer**: Azure Search örneğinizi oluştururken 1. adımda aldığınız değeri kullanın.

   ![Şifreleme ayarlarını görüntüleme 3](../media/cognitive-services-encryption/qna-encryption-3.png)

4. İşiniz bittiğinde çalışma zamanını yeniden başlatın. Artık Soru-Cevap Oluşturma hizmetiniz CMK-etkinleştirilmiştir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

1.  Soru-Cevap Oluşturma yönetilen (Önizleme) hizmetinizin **şifreleme** sekmesine gidin.
2.  **Müşteri tarafından yönetilen anahtarlar** seçeneğini belirleyin. [Müşteri tarafından yönetilen anahtarlarınızın](../../storage/common/customer-managed-keys-configure-key-vault.md?tabs=portal) ayrıntılarını sağlayın ve **Kaydet**' e tıklayın.

     :::image type="content" source="../media/cognitive-services-encryption/qnamaker-v2-encryption-cmk.png" alt-text="Soru-Cevap Oluşturma yönetilen (Önizleme) CMK ayarı" lightbox="../media/cognitive-services-encryption/qnamaker-v2-encryption-cmk.png":::

3.  Başarılı bir kaydetme işlemi sırasında CMK, Azure Search dizininde depolanan verileri şifrelemek için kullanılacaktır.

> [!IMPORTANT]
> Herhangi bir bilgi tabanı oluşturulmadan önce CMK 'nizi yeni bir Azure Bilişsel Arama hizmetinde ayarlamanız önerilir. CMK 'yi, mevcut bilgi temellerine sahip bir Soru-Cevap Oluşturma hizmetinde ayarlarsanız, bunlara erişiminizi kaybedebilirsiniz. Azure bilişsel arama 'da [şifrelenmiş içerikle çalışma](../../search/search-security-manage-encryption-keys.md#work-with-encrypted-content) hakkında daha fazla bilgi edinin.

> [!NOTE]
> Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için bilişsel [hizmetler Customer-Managed anahtar Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve iletin.

---

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Müşteri tarafından yönetilen anahtarlar tüm Azure Search bölgelerde kullanılabilir.

## <a name="encryption-of-data-in-transit"></a>Yoldaki verilerin şifrelenmesi

Soru-Cevap Oluşturma Portal kullanıcının tarayıcısında çalışır. Her eylem, ilgili bilişsel hizmet API 'sine doğrudan çağrı tetikler. Bu nedenle Soru-Cevap Oluşturma iletim verileri için uyumludur.
Ancak, Soru-Cevap Oluşturma Portal hizmeti Batı ABD 'de barındırıldığından, bu, ABD dışı müşteriler için de ideal değildir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault 'de CMKs kullanarak Azure Search şifreleme](../../search/search-security-manage-encryption-keys.md)
* [Bekleme sırasında veri şifrelemesi](../../security/fundamentals/encryption-atrest.md)
* [Azure Key Vault hakkında daha fazla bilgi edinin](../../key-vault/general/overview.md)