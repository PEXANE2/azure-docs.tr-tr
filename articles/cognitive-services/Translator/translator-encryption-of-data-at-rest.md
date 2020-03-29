---
title: Veri şifrelemesi
titleSuffix: Azure Cognitive Services
description: Veri şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 44bb223dbc944be1b8769aa2572f1b88b916528b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372243"
---
# <a name="translator-encryption-of-data-at-rest"></a>Veri şifrelemesi

Çevirmen, bulutta kalıcı olduğunda özel çeviri modelleri oluşturmak için yüklediğiniz verilerinizi otomatik olarak şifreler ve kuruluş güvenliği ve uyumluluk hedeflerinize uymanıza yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel Hizmetler şifrelemesi hakkında

Veriler [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifreleme kullanılarak şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetildiği anlamına gelir. Verileriniz varsayılan olarak güvenlidir ve şifrelemeden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Varsayılan olarak, aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Müşteri tarafından yönetilen anahtarları destekleyen bir fiyatlandırma katmanı kullanıyorsanız, aşağıdaki resimde gösterildiği gibi [Azure portalının](https://portal.azure.com) **Şifreleme** bölümünde kaynağınızın şifreleme ayarlarını görebilirsiniz.

![Şifreleme ayarlarını görüntüleme](../media/cognitive-services-encryption/encryptionblade.png)

Yalnızca Microsoft tarafından yönetilen şifreleme anahtarlarını destekleyen abonelikler için **Şifreleme** bölümünüz olmaz.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarlar

Aboneliğinizi kendi anahtarlarınızla yönetme seçeneği de vardır. Kendi anahtarınızı getir (BYOK) olarak da bilinen müşteri tarafından yönetilen anahtarlar (CMK), erişim denetimleri oluşturmak, döndürmek, devre dışı etmek ve iptal etmek için daha fazla esneklik sunar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Çevirmen hizmetinin tüm fiyatlandırma katmanları için kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanma, çevirmen [müşteri tarafından yönetilen anahtar istek formunu](https://aka.ms/cogsvc-cmk) doldurma ve gönderme olanağı talep etmek için, isteğinizin durumunu duymak yaklaşık 3-5 iş günü sürer. İsteğe bağlı olarak, bir kuyruğa yerleştirilebilir ve alan kullanılabilir hale geldikçe onaylanabilirsiniz. CMK'yı Çevirmen hizmetiyle kullanmak üzere onaylandıktan sonra yeni bir Çevirmen kaynağı oluşturmanız gerekir. Çevirmen kaynağınız oluşturulduktan sonra, yönetilen kimliğinizi ayarlamak için Azure Key Vault'u kullanabilirsiniz.

Çevirmen için müşteri tarafından yönetilen anahtarları etkinleştirmek için aşağıdaki adımları izleyin:

1. Yeni bölgesel Çevirmen Metninizi veya bölgesel Bilişsel Hizmetler kaynağınızı oluşturun. Bu, genel bir kaynakla çalışmaz.
2. Azure portalında Yönetilen Kimliği etkinleştirin ve müşteri tarafından yönetilen anahtar bilgilerinizi ekleyin.
3. Özel Çevirmen'de yeni bir çalışma alanı oluşturun ve bu abonelik bilgilerini ilişkilendirin.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

### <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme

Müşteri tarafından yönetilen anahtarlarınızı depolamak için Azure Key Vault'u kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Bilişsel Hizmetler kaynağı ve anahtar kasası aynı bölgede ve aynı Azure Etkin Dizini (Azure AD) kiracısında olmalıdır, ancak farklı aboneliklerde olabilir. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)

Yeni bir Bilişsel Hizmetler kaynağı her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Kaynağın oluşturulduğu anda müşteri tarafından yönetilen anahtarları etkinleştirmek mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Anahtar Kasası'nda depolanır ve anahtar kasası, Bilişsel Hizmetler kaynağıyla ilişkili yönetilen kimliğe önemli izinler veren erişim ilkeleriyle birlikte sağlanmalıdır. Yönetilen kimlik, kaynak oluşturulur oluşturulmaz kullanılabilir.

Bilişsel Hizmetler şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları nasıl kullanacağınızı öğrenmek için bkz:

- [Azure portalından Bilişsel Hizmetler şifrelemesi için Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandırın](../Encryption/cognitive-services-encryption-keys-portal.md)

Müşteri yönetilen anahtarları etkinleştirmek, Azure AD özelliği olan yönetilen kimlik atanan bir sisteme de olanak sağlar. Yönetilen kimlik atanan sistem etkinleştirildiğinde, bu kaynak Azure Etkin Dizini'ne kaydedilir. Kaydedildikten sonra, yönetilen kimlik müşteri tarafından yönetilen anahtar kurulumu sırasında seçilen Key Vault'a erişim hakkı verilir. [Yönetilen Kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)hakkında daha fazla bilgi edinebilirsiniz.

> [!IMPORTANT]
> Yönetilen kimlikler atanan sistemi devre dışı bduyarsanız, anahtar kasasına erişim kaldırılır ve müşteri anahtarlarıyla şifrelenmiş verilere artık erişilemez. Bu verilere bağlı tüm özellikler çalışmayı durdurur. Dağıttığınız tüm modeller de dağıtılmaz. Yüklenen tüm veriler Custom Translator'dan silinecektir. Yönetilen kimlikler yeniden etkinleştirilirse, modeli sizin için otomatik olarak yeniden dağıtmayız.

> [!IMPORTANT]
> Yönetilen kimlikler şu anda çapraz dizin senaryolarını desteklemiyor. Azure portalında müşteri tarafından yönetilen anahtarları yapılandırdığınızda, yönetilen bir kimlik kapakların altına otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya kaynağı bir Azure REKLAM dizininden diğerine taşırsanız, kaynakla ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, bu nedenle müşteri tarafından yönetilen anahtarlar artık çalışmayabilir. Daha fazla bilgi için **bkz.** [FAQs and known issues with managed identities for Azure resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories)  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası'nda saklayın

Müşteri tarafından yönetilen anahtarları etkinleştirmek için, anahtarlarınızı depolamak için bir Azure Anahtar Kasası kullanmanız gerekir. Hem **Yumuşak Silme** **hem** de Temizleme özelliklerini anahtar kasasında etkinleştirmelisiniz.

Yalnızca 2048 boyutunda RSA anahtarları Bilişsel Hizmetler şifrelemesi ile desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys)Key Vault **tuşlarına** bakın.

> [!NOTE]
> Anahtar kasasının tamamı silinirse, verileriniz artık görüntülenmez ve tüm modelleriniz dağıtılmaz. Yüklenen tüm veriler Custom Translator'dan silinecektir. 

### <a name="revoke-access-to-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLI'yi kullanın. Daha fazla bilgi için Azure [Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault//) veya [Azure Key Vault CLI'ye](https://docs.microsoft.com/cli/azure/keyvault)bakın. Erişimin iptali, Bilişsel Hizmetler kaynağındaki tüm verilere erişimi etkili bir şekilde engeller ve şifreleme anahtarına Bilişsel Hizmetler tarafından erişilemeden modelleriniz dağıtılamaz. Yüklenen tüm veriler de Custom Translator'dan silinecektir.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Anahtar Kasası hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
