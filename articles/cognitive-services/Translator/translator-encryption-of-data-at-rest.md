---
title: Bekleyen verilerin Translator şifrelemesi
titleSuffix: Azure Cognitive Services
description: Bekleyen verilerin Translator şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 44bb223dbc944be1b8769aa2572f1b88b916528b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79372243"
---
# <a name="translator-encryption-of-data-at-rest"></a>Bekleyen verilerin Translator şifrelemesi

Çevirmen, verilerinizi otomatik olarak şifreler, bu, bulutta kalıcı olduğunda, kuruluş güvenlik ve uyumluluk hedeflerinizi karşılamanıza yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel hizmetler şifreleme hakkında

[Fıps 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifrelemesi kullanılarak veriler şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetilip yönetilmediğini belirtir. Verileriniz varsayılan olarak güvenlidir ve şifreleme avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Aboneliğiniz varsayılan olarak Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Müşteri tarafından yönetilen anahtarları destekleyen bir Fiyatlandırma Katmanı kullanıyorsanız, aşağıdaki görüntüde gösterildiği gibi [Azure Portal](https://portal.azure.com) **şifreleme** bölümünde kaynağınız için şifreleme ayarlarını görebilirsiniz.

![Şifreleme ayarlarını görüntüle](../media/cognitive-services-encryption/encryptionblade.png)

Yalnızca Microsoft tarafından yönetilen şifreleme anahtarlarını destekleyen abonelikler için **şifreleme** bölümüne sahip olursunuz.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Key Vault ile müşteri tarafından yönetilen anahtarlar

Ayrıca kendi anahtarlarınız ile aboneliğinizi yönetme seçeneği de vardır. Kendi anahtarını getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, çevirmen hizmeti için tüm fiyatlandırma katmanlarında kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma olanağı istemek için [Translator müşteri tarafından yönetilen anahtar Isteği formunu](https://aka.ms/cogsvc-cmk) doldurun ve gönderebilirsiniz. Bu işlem, isteğinizin durumunu öğrenmek için yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. Translator hizmeti ile CMK kullanmaya onaylandıktan sonra yeni bir çevirmen kaynağı oluşturmanız gerekir. Çevirmen kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault kullanabilirsiniz.

Çevirmen için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere aşağıdaki adımları izleyin:

1. Yeni bölgesel Translator Metin Çevirisi veya bölgesel bilişsel hizmetler kaynağını oluşturun. Bu, genel bir kaynakla çalışmaz.
2. Azure portal yönetilen kimliği etkinleştirdi ve müşteri tarafından yönetilen anahtar bilgilerinizi ekleyin.
3. Özel çevirmende yeni bir çalışma alanı oluşturun ve bu abonelik bilgilerini ilişkilendirin.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

### <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

Müşteri tarafından yönetilen anahtarlarınızın depolanması için Azure Key Vault kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Bilişsel hizmetler kaynağı ve Anahtar Kasası aynı bölgede ve aynı Azure Active Directory (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

Yeni bir bilişsel hizmetler kaynağı, her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Kaynağın oluşturulduğu sırada müşteri tarafından yönetilen anahtarların etkinleştirilmesi mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Key Vault depolanır ve anahtar kasasının bilişsel hizmetler kaynağıyla ilişkili yönetilen kimliğe anahtar izinleri veren erişim ilkeleriyle sağlanması gerekir. Yönetilen kimlik, kaynak oluşturulduğu anda kullanılabilir.

Bilişsel hizmetler şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarların nasıl kullanılacağını öğrenmek için bkz.:

- [Bilişsel hizmetler şifrelemesi için Key Vault Azure portal ile müşteri tarafından yönetilen anahtarları yapılandırın](../Encryption/cognitive-services-encryption-keys-portal.md)

Müşteri tarafından yönetilen anahtarların etkinleştirilmesi, bir Azure AD 'nin bir özelliği olan bir sistem tarafından atanmış yönetilen kimliği de etkinleştirir. Sistem tarafından atanan yönetilen kimlik etkinleştirildikten sonra, bu kaynak Azure Active Directory kaydedilir. Kaydolduktan sonra yönetilen kimliğe, müşteri tarafından yönetilen anahtar kurulumu sırasında seçilen Key Vault erişim verilir. [Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)hakkında daha fazla bilgi edinebilirsiniz.

> [!IMPORTANT]
> Sistem tarafından atanan yönetilen kimlikleri devre dışı bırakırsanız, anahtar kasasına erişim kaldırılır ve müşteri anahtarlarıyla şifrelenen verilere artık erişilemeyecektir. Bu verilere bağımlı olan özellikler çalışmayı durdurur. Dağıttığınız tüm modeller de dağıtılmayacak. Karşıya yüklenen tüm veriler özel çevirmenden silinecek. Yönetilen kimlikler yeniden etkinleştirilmişse, modeli sizin için otomatik olarak yeniden dağıtmayacak.

> [!IMPORTANT]
> Yönetilen kimlikler Şu anda çapraz dizin senaryolarını desteklemez. Azure portal müşteri tarafından yönetilen anahtarları yapılandırırken, bir yönetilen kimlik, kapakların altında otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya kaynağı bir Azure AD dizininden diğerine taşırsanız, kaynakla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. SSS 'de **Azure AD dizinleri arasında bir abonelik aktarma** [ve Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde depola

Müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak üzere bir Azure Key Vault kullanmanız gerekir. Anahtar kasasında hem **geçici silme** hem de **Temizleme** özelliklerini etkinleştirmeniz gerekir.

Bilişsel hizmetler şifrelemesiyle yalnızca 2048 boyutundaki RSA anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) **Key Vault anahtarlar** .

> [!NOTE]
> Tüm Anahtar Kasası silinirse, verileriniz artık gösterilmeyecektir ve tüm modelleriniz dağıtılmayacaktır. Karşıya yüklenen tüm veriler özel çevirmenden silinecek. 

### <a name="revoke-access-to-customer-managed-keys"></a>Müşterinin yönettiği anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLı kullanın. Daha fazla bilgi için bkz. PowerShell veya [Azure Key Vault clı](https://docs.microsoft.com/cli/azure/keyvault) [Azure Key Vault](https://docs.microsoft.com/powershell/module/az.keyvault//) . Erişimi iptal etmek bilişsel hizmetler kaynağındaki tüm verilere erişimi etkin bir şekilde engeller ve şifreleme anahtarına bilişsel hizmetler tarafından erişilemediğinden modelleriniz dağıtılmayacak. Karşıya yüklenen tüm veriler özel çevirmenden de silinecek.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Key Vault hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
