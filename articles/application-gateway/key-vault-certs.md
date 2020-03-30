---
title: Azure Anahtar Kasası sertifikaları ile SSL sonlandırma
description: HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Azure Uygulama Ağ Geçidi'ni Key Vault ile nasıl entegre edebileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 5633dd7b72f4de22cd34b7d093e8ec4d9cb411f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137693"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>Key Vault sertifikaları ile SSL sonlandırma

[Azure Key Vault,](../key-vault/key-vault-overview.md) sırları, anahtarları ve SSL sertifikalarını korumak için kullanabileceğiniz platform tarafından yönetilen gizli bir mağazadır. Azure Application Gateway, HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Key Vault ile tümleştirmeyi destekler. Bu destek, Uygulama Ağ Geçidi'nin v2 SKU'su ile sınırlıdır.

Key Vault entegrasyonu SSL sonlandırma için iki model sunar:

- Dinleyiciye bağlı SSL sertifikalarını açıkça sağlayabilirsiniz. Bu model, SSL sonlandırma için Uygulama Ağ Geçidi'ne SSL sertifikaları geçmek için geleneksel bir yoldur.
- HTTPS özellikli bir dinleyici oluşturduğunuzda, isteğe bağlı olarak varolan bir Key Vault sertifikasına veya gizliye başvuruda bulunabilirsiniz.

Key Vault ile Uygulama Ağ Geçidi entegrasyonu, şunları dahil olmak üzere birçok avantaj sunar:

- SSL sertifikaları doğrudan uygulama geliştirme ekibi tarafından işlenmediği için daha güçlü güvenlik. Tümleştirme, ayrı bir güvenlik ekibinin şunları yapmasına olanak tanır:
  * Uygulama ağ geçitlerini ayarlayın.
  * Uygulama ağ geçidi yaşam döngülerini kontrol edin.
  * Anahtar kasanızda depolanan sertifikalara erişmek için seçili uygulama ağ geçitlerine izin ver.
- Varolan sertifikaları anahtar kasanıza alma desteği. Veya güvenilen Key Vault ortaklarından herhangi biriyle yeni sertifikalar oluşturmak ve yönetmek için Key Vault API'lerini kullanın.
- Anahtar kasanızda depolanan sertifikaların otomatik olarak yenilenmesi desteği.

Application Gateway şu anda yalnızca yazılım onaylı sertifikaları destekler. Donanım güvenlik modülü (HSM) onaylı sertifikalar desteklenmez. Uygulama Ağ Geçidi, Anahtar Kasa sertifikalarını kullanacak şekilde yapılandırıldıktan sonra, örnekleri sertifikayı Key Vault'tan alır ve SSL sonlandırma için yerel olarak yükler. Örnekler ayrıca, varsa sertifikanın yenilenmiş bir sürümünü almak için 24 saatlik aralıklarla Key Vault'u da yoklar. Güncelleştirilmiş bir sertifika bulunursa, şu anda HTTPS dinleyicisiyle ilişkili SSL sertifikası otomatik olarak döndürülür.

> [!NOTE]
> Azure portalı yalnızca KeyVault Sertifikalarını destekler, sırları değil. Uygulama Ağ Geçidi hala KeyVault gelen başvuru sırlarını destekler, ancak powershell, CLI, API, ARM şablonları, vb gibi olmayan Portal kaynakları aracılığıyla. 

## <a name="how-integration-works"></a>Tümleştirme nasıl çalışır?

Key Vault ile Uygulama Ağ Geçidi tümleştirmesi üç adımlı yapılandırma işlemi gerektirir:

1. **Kullanıcı tarafından atanan yönetilen kimlik oluşturma**

   Application Gateway'in sizin adınıza Key Vault'tan sertifika almak için kullandığı, kullanıcı tarafından atanmış bir yönetilen kimlik oluşturur veya yeniden kullanırsınız. Daha fazla bilgi için azure [kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md) Bu adım, Azure Etkin Dizin kiracısında yeni bir kimlik oluşturur. Kimlik, kimliği oluşturmak için kullanılan abonelik tarafından güvenilir.

1. **Anahtar kasanızı yapılandırın**

   Daha sonra varolan bir sertifikayı içe aktarır veya anahtar kasanızda yeni bir sertifika oluşturursunuz. Sertifika, uygulama ağ geçidinde çalışan uygulamalar tarafından kullanılacaktır. Bu adımda, 64 kodlu PFX dosyası, parolasız olarak depolanan anahtar kasa sırrını da kullanabilirsiniz. Anahtar kasasındaki sertifika türü nesneleri yle kullanılabilen otomatik yenileme özelliği nedeniyle bir sertifika türü kullanmanızı öneririz. Bir sertifika veya sır oluşturduktan sonra, kimliğin gizliye *erişmesine* izin vermek için anahtar kasasında erişim ilkeleri tanımlarsınız.

1. **Uygulama ağ geçidini yapılandırma**

   Önceki iki adımı tamamladıktan sonra, kullanıcı tarafından atanan yönetilen kimliği kullanmak için varolan bir uygulama ağ geçidi ayarlayabilir veya değiştirebilirsiniz. Ayrıca, HTTP dinleyicisinin SSL sertifikasını Key Vault sertifikasının veya gizli kimliğin uri'sinin tamamını işaret etmek üzere yapılandırabilirsiniz.

   ![Anahtar kasa sertifikaları](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure PowerShell'i kullanarak SSL sonlandırmaişlemini Key Vault sertifikalarıyla yapılandırın](configure-keyvault-ps.md)
