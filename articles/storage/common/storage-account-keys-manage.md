---
title: Hesap erişim anahtarlarını yönetme
titleSuffix: Azure Storage
description: Depolama hesabı erişim anahtarlarınızı nasıl görüntüleyeceknizi, yöneteceklerinizi ve döndüreceklerinizi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/18/2019
ms.author: tamram
ms.openlocfilehash: 13adf6de420b54299d04a226dab81e75cbb9fef2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75975779"
---
# <a name="manage-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını yönetme

Bir depolama hesabı oluşturduğunuzda, Azure iki 512 bit depolama hesabı erişim anahtarı oluşturur. Bu anahtarlar, Paylaşılan Anahtar yetkilendirmesi aracılığıyla depolama hesabınızdaki verilere erişim yetkisi vermek için kullanılabilir.

Microsoft, erişim anahtarlarınızı yönetmek için Azure Key Vault'u kullanmanızı ve anahtarlarınızı düzenli olarak döndürmenizi ve yeniden oluşturmanızı önerir. Azure Key Vault'u kullanmak, uygulamalarınızı kesintiye uğramadan anahtarlarınızı döndürmenizi kolaylaştırır. Ayrıca anahtarlarınızı el ile de döndürebilirsiniz.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-access-keys-and-connection-string"></a>Erişim anahtarlarını ve bağlantı dizelerini görüntüleme

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

## <a name="use-azure-key-vault-to-manage-your-access-keys"></a>Erişim anahtarlarınızı yönetmek için Azure Anahtar Kasası'nı kullanın

Microsoft, erişim anahtarlarınızı yönetmek ve döndürmek için Azure Key Vault'u kullanmanızı önerir. Uygulamanız anahtarlarınıza Key Vault'ta güvenli bir şekilde erişebilir, böylece bunları uygulama kodunuzla depolamaktan kaçınabilirsiniz. Anahtar yönetimi için Key Vault'u kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Key Vault ve PowerShell ile depolama hesabı anahtarlarını yönetme](../../key-vault/key-vault-overview-storage-keys-powershell.md)
- [Azure Anahtar Kasası ve Azure CLI ile depolama hesabı anahtarlarını yönetme](../../key-vault/key-vault-ovw-storage-keys.md)

## <a name="manually-rotate-access-keys"></a>Erişim tuşlarını el ile döndür

Microsoft, depolama hesabınızın güvenliğini sağlamaya yardımcı olmak için erişim anahtarlarınızı düzenli aralıklarla döndürmenizi önerir. Mümkünse, erişim anahtarlarınızı yönetmek için Azure Key Vault'u kullanın. Key Vault kullanmıyorsanız, anahtarlarınızı el ile döndürmeniz gerekir.

Anahtarlarınızı döndürebilmeniz için iki erişim anahtarı atanır. İki anahtara sahip olmak, uygulamanızın işlem boyunca Azure Depolama'ya erişimi korumasını sağlar.

> [!WARNING]
> Erişim anahtarlarınızı yeniden oluşturmak, depolama hesabı anahtarına bağlı olan tüm uygulamaları veya Azure hizmetlerini etkileyebilir. Depolama hesabına erişmek için hesap anahtarını kullanan tüm istemcilerin, medya hizmetleri, bulut, masaüstü ve mobil uygulamalar ve [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)gibi Azure Depolama alanı için grafik kullanıcı arabirimi uygulamaları da dahil olmak üzere yeni anahtarı kullanmak üzere güncelleştirilmelidir.

Depolama hesabı anahtarlarınızı döndürmek için aşağıdaki işlemi izleyin:

1. İkincil anahtarı kullanmak için uygulama kodunuzdaki bağlantı dizelerini güncelleştirin.
2. Depolama hesabınız için birincil erişim tuşunu yeniden oluşturun. Azure portalındaki **Erişim Tuşları** çubuğunda, **Yeni Anahtar1'i Yeniden Oluştur'u**tıklatın ve ardından yeni bir anahtar oluşturmak istediğinizi doğrulamak için **Evet'i** tıklatın.
3. Yeni birincil erişim tuşunu referans olarak kullanmak için bağlantı dizelerini güncelleştirin.
4. İkincil erişim tuşunu da aynı şekilde yeniden oluşturun.

> [!NOTE]
> Microsoft, tüm uygulamalarınızdaki anahtarlardan yalnızca birini aynı anda kullanmanızı önerir. Key 1'i bazı yerlerde, Key 2'yi bazı yerlerde kullanırsanız, bazı uygulamaların erişimini kaybetmeden anahtarlarınızı döndüremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure depolama hesabına genel bakış](storage-account-overview.md)
- [Depolama hesabı oluşturma](storage-account-create.md)
