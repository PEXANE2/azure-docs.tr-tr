---
title: FHıR için Azure API 'SI için müşteri tarafından yönetilen anahtarları yapılandırma
description: Azure API 'de, Cosmos DB aracılığıyla Azure API 'de desteklenen kendi anahtar özelliğini getirin
services: healthcare-apis
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 09/28/2020
ms.author: ginle
ms.openlocfilehash: ae78aa80594e46b02d77adcafed961e801780d4f
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430268"
---
# <a name="configure-customer-managed-keys-at-rest"></a>Bekleyen müşteri tarafından yönetilen anahtarları yapılandırma

FHıR hesabı için yeni bir Azure API 'SI oluşturduğunuzda verileriniz varsayılan olarak Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Şimdi, seçtiğiniz ve yönettiğiniz kendi anahtarınızı kullanarak veriler için ikinci bir şifreleme katmanı ekleyebilirsiniz.

Azure 'da bu, genellikle müşterinin Azure Key Vault bir şifreleme anahtarı kullanılarak gerçekleştirilir. Azure SQL, Azure depolama ve Cosmos DB bu özelliği bugün sağlayan bazı örneklerdir. FHıR için Azure API Cosmos DB bu destekten yararlanır. Bir hesap oluşturduğunuzda, bir Azure Key Vault anahtar URI 'SI belirtme seçeneğine sahip olursunuz. Bu anahtar, DB hesabı sağlandığında Cosmos DB geçirilecek. Bir FHıR isteği yapıldığında, Cosmos DB anahtarınızı getirir ve verileri şifrelemek/şifresini çözmek için kullanır. Başlamak için aşağıdaki bağlantılara başvurabilirsiniz:

- [Azure aboneliğiniz için Azure Cosmos DB kaynak sağlayıcısını kaydetme](../cosmos-db/how-to-setup-cmk.md#register-resource-provider) 
- [Azure Key Vault örneğinizi yapılandırma](../cosmos-db/how-to-setup-cmk.md#configure-your-azure-key-vault-instance)
- [Azure Key Vault örneğine bir erişim ilkesi ekleme](../cosmos-db/how-to-setup-cmk.md#add-an-access-policy-to-your-azure-key-vault-instance)
- [Azure Key Vault bir anahtar oluşturun](../cosmos-db/how-to-setup-cmk.md#generate-a-key-in-azure-key-vault)

## <a name="specify-the-azure-key-vault-key"></a>Azure Key Vault anahtarını belirtin

Azure portal için Azure API 'nizi oluştururken, "ek ayarlar" sekmesinde "veritabanı ayarları" altında "veri şifreleme" yapılandırma seçeneğini görebilirsiniz. Hizmet tarafından yönetilen anahtar seçeneği varsayılan olarak seçilir. 

KeyPicker adresinden anahtarınızı seçebilirsiniz:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-keypicker.png" alt-text="KeyPicker":::

İsterseniz, "müşteri tarafından yönetilen anahtar" seçeneğini belirleyerek Azure Key Vault anahtarınızı burada belirtebilirsiniz. Anahtar URI 'sini buraya girebilirsiniz:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-create.png" alt-text="FHıR için Azure API oluşturma":::

Mevcut FHıR hesaplarında, anahtar şifreleme seçimini (hizmet veya müşteri tarafından yönetilen anahtar) aşağıda gösterildiği gibi "veritabanı" dikey penceresinde görüntüleyebilirsiniz. Yapılandırma seçeneği seçildikten sonra değiştirilemez. Ancak anahtarınızı değiştirebilir ve güncelleştirebilirsiniz.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-database.png" alt-text="Veritabanı":::

Ayrıca, belirtilen anahtarın yeni bir sürümünü oluşturabilirsiniz, bundan sonra verilerinizin hizmet kesintisi olmadan yeni sürümle şifrelenmesini sağlayabilirsiniz. Verilere erişimi kaldırmak için anahtara erişimi de kaldırabilirsiniz. Anahtar devre dışı bırakıldığında, sorgular bir hataya neden olur. Anahtar yeniden etkinleştirilmişse sorgular yeniden başarılı olur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, müşteri tarafından yönetilen anahtarların bekleyen olarak nasıl yapılandırılacağını öğrendiniz. Daha sonra, Azure Cosmos DB SSS bölümüne bakabilirsiniz: 
 
>[!div class="nextstepaction"]
>[Cosmos DB: CMK 'yi kurma](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#frequently-asked-questions)
