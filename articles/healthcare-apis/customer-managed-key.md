---
title: FHıR için Azure API 'SI için müşteri tarafından yönetilen anahtarları yapılandırma
description: Azure API 'de, Cosmos DB aracılığıyla Azure API 'de desteklenen kendi anahtar özelliğini getirin
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 09/28/2020
ms.author: matjazl
ms.openlocfilehash: 535bb5d21beafaabb50769a6c03478dbd1f942d4
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92051358"
---
# <a name="configure-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları yapılandırma

FHıR hesabı için yeni bir Azure API 'SI oluşturduğunuzda verileriniz varsayılan olarak Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Şimdi, seçtiğiniz ve yönettiğiniz kendi anahtarınızı kullanarak veriler için ikinci bir şifreleme katmanı ekleyebilirsiniz.

Azure 'da bu, genellikle müşterinin Azure Key Vault (AKV) bir şifreleme anahtarı kullanılarak gerçekleştirilir. Azure SQL, Azure depolama ve Cosmos DB bu özelliği bugün sağlayan bazı örneklerdir. FHıR için Azure API Cosmos DB bu destekten yararlanır. Bir hesap oluşturduğunuzda, bir AKV anahtar URI 'SI belirtme seçeneğine sahip olursunuz. Bu anahtarı, DB hesabı sağlandığında Cosmos DB geçiyoruz. Bir FHıR isteği yapıldığında, Cosmos DB anahtarınızı getirir ve verileri şifrelemek/şifresini çözmek için kullanır. Başlamak için aşağıdaki bağlantılara başvurabilirsiniz:

- [Azure aboneliğiniz için Azure Cosmos DB kaynak sağlayıcısını kaydetme](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#register-resource-provider) 
- [AKV örneğinizi yapılandırma](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#configure-your-azure-key-vault-instance)
-  [AKV örneğinize erişim ilkesi ekleme](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#add-an-access-policy-to-your-azure-key-vault-instance)
- [AKV 'de bir anahtar oluşturma](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#generate-a-key-in-azure-key-vault)

Azure portal 'de FHıR hesabı için Azure API 'nizi oluşturduktan sonra, "ek ayarlar" sekmesinde "veritabanı ayarları" altında "veri şifreleme" yapılandırma seçeneğini görebilirsiniz. Hizmet tarafından yönetilen anahtar seçeneği varsayılan olarak seçilir. "Müşteri tarafından yönetilen anahtar" seçeneğini belirleyerek AKV anahtarınızı buradan belirtebilirsiniz. Kopyalanmış anahtar URI 'sini buraya girebilirsiniz.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-create.png" alt-text="FHıR için Azure API oluşturma":::

Ya da KeyPicker adresinden anahtarınızı seçebilirsiniz:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-keypicker.png" alt-text="FHıR için Azure API oluşturma" dikey penceresinde görüntüleyebilirsiniz. Yapılandırma seçeneği seçildikten sonra değiştirilemez. Ancak anahtarınızı değiştirebilir ve güncelleştirebilirsiniz.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-database.png" alt-text="FHıR için Azure API oluşturma":::

Ayrıca, belirtilen anahtarın yeni bir sürümünü oluşturabilirsiniz, bundan sonra verilerinizin hizmet kesintisi olmadan yeni sürümle şifrelenmesini sağlayabilirsiniz. Verilere erişimi kaldırmak için anahtara erişimi de kaldırabilirsiniz.
