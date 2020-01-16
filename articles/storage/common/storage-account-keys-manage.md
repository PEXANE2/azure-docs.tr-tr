---
title: Hesap erişim anahtarlarını yönetme
titleSuffix: Azure Storage
description: Depolama hesabı erişim anahtarlarınızı görüntüleme, yönetme ve döndürme hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/18/2019
ms.author: tamram
ms.openlocfilehash: 13adf6de420b54299d04a226dab81e75cbb9fef2
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75975779"
---
# <a name="manage-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını yönetme

Bir depolama hesabı oluşturduğunuzda, Azure 2 512 bit depolama hesabı erişim anahtarları oluşturur. Bu anahtarlar, paylaşılan anahtar yetkilendirmesi aracılığıyla Depolama hesabınızdaki verilere erişim yetkisi vermek için kullanılabilir.

Microsoft, erişim anahtarlarınızı yönetmek için Azure Key Vault kullanmanızı ve anahtarlarınızı düzenli olarak döndürmenizi ve yeniden oluşturmanızı önerir. Azure Key Vault kullanmak, anahtarlarınızı uygulamalarınıza kesinti olmadan döndürmeyi kolaylaştırır. Ayrıca, anahtarlarınızı el ile de döndürebilirsiniz.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-access-keys-and-connection-string"></a>Erişim anahtarlarını ve bağlantı dizesini görüntüle

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

## <a name="use-azure-key-vault-to-manage-your-access-keys"></a>Erişim anahtarlarınızı yönetmek için Azure Key Vault kullanın

Microsoft, erişim anahtarlarınızı yönetmek ve döndürmek için Azure Key Vault kullanmayı önerir. Uygulamanız Key Vault içindeki Anahtarlarınıza güvenli bir şekilde erişebilir, böylece bunları uygulama kodunuzla depolamaktan kaçınabilirsiniz. Anahtar yönetimi için Key Vault kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Key Vault ve PowerShell ile depolama hesabı anahtarlarını yönetme](../../key-vault/key-vault-overview-storage-keys-powershell.md)
- [Azure Key Vault ve Azure CLı ile depolama hesabı anahtarlarını yönetme](../../key-vault/key-vault-ovw-storage-keys.md)

## <a name="manually-rotate-access-keys"></a>Erişim tuşlarını el ile döndürme

Microsoft, depolama hesabınızı güvende tutmaya yardımcı olmak için erişim anahtarlarınızı düzenli aralıklarla döndürmenizi önerir. Mümkünse, erişim anahtarlarınızı yönetmek için Azure Key Vault kullanın. Key Vault kullanmıyorsanız, anahtarlarınızı el ile döndürmeniz gerekir.

Anahtarlarınızı döndürebilmeniz için iki erişim tuşu atanır. İki anahtara sahip olmak, uygulamanızın işlem boyunca Azure depolama 'ya erişimini korumasını sağlar.

> [!WARNING]
> Erişim anahtarlarınızın yeniden oluşturulması, depolama hesabı anahtarına bağlı olan tüm uygulamaları veya Azure hizmetlerini etkileyebilir. Depolama hesabına erişmek için hesap anahtarını kullanan istemciler, Media Services, bulut, masaüstü ve mobil uygulamalar gibi yeni anahtarı kullanacak şekilde ve [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)gibi Azure depolama için grafik kullanıcı arabirimi uygulamalarına yönelik olarak güncelleştirilmeleri gerekir.

Depolama hesabı anahtarlarınızı döndürmek için bu işlemi izleyin:

1. İkincil anahtarı kullanmak için uygulama kodunuzda bağlantı dizelerini güncelleştirin.
2. Depolama hesabınız için birincil erişim tuşunu yeniden oluşturun. Azure portal **erişim tuşları** dikey penceresinde, yeniden **Oluştur KEY1**' a tıklayın ve sonra yeni bir anahtar oluşturmak istediğinizi onaylamak için **Evet** ' e tıklayın.
3. Yeni birincil erişim tuşunu referans olarak kullanmak için bağlantı dizelerini güncelleştirin.
4. İkincil erişim tuşunu da aynı şekilde yeniden oluşturun.

> [!NOTE]
> Microsoft, tüm uygulamalarınızda aynı anda yalnızca bir tane anahtar kullanılmasını önerir. Anahtar 1 ' i bazı yerlerde ve anahtar 2 ' de kullanırsanız, bazı uygulama erişimi kaybetmeksizin anahtarlarınızı döndüremezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure depolama hesabına genel bakış](storage-account-overview.md)
- [Depolama hesabı oluşturma](storage-account-create.md)
