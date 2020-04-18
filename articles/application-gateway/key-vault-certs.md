---
title: Azure Anahtar Kasası sertifikaları ile TLS sonlandırma
description: HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Azure Uygulama Ağ Geçidi'ni Key Vault ile nasıl entegre edebileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 934cf854b0c526ed994c7dc91763f65de64fd14b
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617514"
---
# <a name="tls-termination-with-key-vault-certificates"></a>Key Vault sertifikaları ile TLS sonlandırma

[Azure Key Vault,](../key-vault/general/overview.md) sırları, anahtarları ve TLS/SSL sertifikalarını korumak için kullanabileceğiniz platform tarafından yönetilen gizli bir mağazadır. Azure Application Gateway, HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Key Vault ile tümleştirmeyi destekler. Bu destek, Uygulama Ağ Geçidi'nin v2 SKU'su ile sınırlıdır.

Key Vault entegrasyonu TLS sonlandırma için iki model sunar:

- Dinleyiciye bağlı TLS/SSL sertifikalarını açıkça sağlayabilirsiniz. Bu model, TLS/SSL sertifikalarını TLS sonlandırma için Uygulama Ağ Geçidi'ne geçirmenin geleneksel yoludur.
- HTTPS özellikli bir dinleyici oluşturduğunuzda, isteğe bağlı olarak varolan bir Key Vault sertifikasına veya gizliye başvuruda bulunabilirsiniz.

Key Vault ile Uygulama Ağ Geçidi entegrasyonu, şunları dahil olmak üzere birçok avantaj sunar:

- TLS/SSL sertifikaları doğrudan uygulama geliştirme ekibi tarafından işlenmediği için daha güçlü güvenlik. Tümleştirme, ayrı bir güvenlik ekibinin şunları yapmasına olanak tanır:
  * Uygulama ağ geçitlerini ayarlayın.
  * Uygulama ağ geçidi yaşam döngülerini kontrol edin.
  * Anahtar kasanızda depolanan sertifikalara erişmek için seçili uygulama ağ geçitlerine izin ver.
- Varolan sertifikaları anahtar kasanıza alma desteği. Veya güvenilen Key Vault ortaklarından herhangi biriyle yeni sertifikalar oluşturmak ve yönetmek için Key Vault API'lerini kullanın.
- Anahtar kasanızda depolanan sertifikaların otomatik olarak yenilenmesi desteği.

Application Gateway şu anda yalnızca yazılım onaylı sertifikaları destekler. Donanım güvenlik modülü (HSM) onaylı sertifikalar desteklenmez. Uygulama Ağ Geçidi, Anahtar Kasa sertifikalarını kullanacak şekilde yapılandırıldıktan sonra, örnekleri sertifikayı Key Vault'tan alır ve TLS sonlandırma için yerel olarak yükler. Örnekler ayrıca, varsa sertifikanın yenilenmiş bir sürümünü almak için 24 saatlik aralıklarla Key Vault'u da yoklar. Güncelleştirilmiş bir sertifika bulunursa, şu anda HTTPS dinleyicisiyle ilişkili TLS/SSL sertifikası otomatik olarak döndürülür.

> [!NOTE]
> Azure portalı yalnızca KeyVault Sertifikalarını destekler, sırları değil. Uygulama Ağ Geçidi hala KeyVault gelen başvuru sırlarını destekler, ancak powershell, CLI, API, ARM şablonları, vb gibi olmayan Portal kaynakları aracılığıyla. 

## <a name="how-integration-works"></a>Tümleştirme nasıl çalışır?

Key Vault ile Uygulama Ağ Geçidi tümleştirmesi üç adımlı yapılandırma işlemi gerektirir:

1. **Kullanıcı tarafından atanan yönetilen kimlik oluşturma**

   Application Gateway'in sizin adınıza Key Vault'tan sertifika almak için kullandığı, kullanıcı tarafından atanmış bir yönetilen kimlik oluşturur veya yeniden kullanırsınız. Daha fazla bilgi için azure [kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md) Bu adım, Azure Etkin Dizin kiracısında yeni bir kimlik oluşturur. Kimlik, kimliği oluşturmak için kullanılan abonelik tarafından güvenilir.

1. **Anahtar kasanızı yapılandırın**

   Daha sonra varolan bir sertifikayı içe aktarır veya anahtar kasanızda yeni bir sertifika oluşturursunuz. Sertifika, uygulama ağ geçidinde çalışan uygulamalar tarafından kullanılacaktır. Bu adımda, şifresiz, base-64 kodlu PFX dosyası olarak depolanan anahtar kasa sırrını da kullanabilirsiniz. Anahtar kasasındaki sertifika türü nesneleri yle kullanılabilen otomatik yenileme özelliği nedeniyle bir sertifika türü kullanmanızı öneririz. Bir sertifika veya sır oluşturduktan sonra, kimliğin gizliye *erişmesine* izin vermek için anahtar kasasında erişim ilkeleri tanımlarsınız.
   
   > [!NOTE]
   > Uygulama ağ geçidini bir ARM şablonu aracılığıyla, Azure CLI veya PowerShell'i kullanarak veya Azure portalından dağıtılan bir Azure Uygulaması aracılığıyla dağıtıysanız, temel 64 kodlu PFX dosyası olarak anahtar kasasında depolanan SSL sertifikası **parolasız olmalıdır.** Ayrıca, [dağıtım sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasasını Kullan'daki](../azure-resource-manager/templates/key-vault-parameter.md)adımları tamamlamanız gerekir. Özellikle . `enabledForTemplateDeployment` `true`

1. **Uygulama ağ geçidini yapılandırma**

   Önceki iki adımı tamamladıktan sonra, kullanıcı tarafından atanan yönetilen kimliği kullanmak için varolan bir uygulama ağ geçidi ayarlayabilir veya değiştirebilirsiniz. Ayrıca HTTP dinleyicisinin TLS/SSL sertifikasını Key Vault sertifikasının veya gizli kimliğinin tamamını uri'ye işaret etmek için yapılandırabilirsiniz.

   ![Anahtar kasa sertifikaları](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure PowerShell'i kullanarak TLS sonlandırmaişlemini Key Vault sertifikalarıyla yapılandırın](configure-keyvault-ps.md)
