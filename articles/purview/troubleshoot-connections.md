---
title: Azure purview 'da bağlantılarınızın sorunlarını giderme
description: Bu makalede, Azure purview 'da bağlantılarınızın sorunlarını gidermeye yönelik adımlar açıklanmaktadır.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: db3954b0368392d63680a8d92f13d6a0423e8ef5
ms.sourcegitcommit: e3151d9b352d4b69c4438c12b3b55413b4565e2f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100526889"
---
# <a name="troubleshoot-your-connections-in-azure-purview"></a>Azure purview 'da bağlantılarınızın sorunlarını giderme

Bu makalede, Azure purview 'daki veri kaynaklarında taramalar ayarlanırken bağlantı hatalarıyla ilgili sorunları nasıl giderebileceğiniz açıklanır.

## <a name="permission-the-credential-on-the-data-source"></a>Veri kaynağındaki kimlik bilgisini izin

Taramalar için bir kimlik doğrulama yöntemi olarak yönetilen bir kimlik veya hizmet sorumlusu kullanıyorsanız, bu kimliklerin veri kaynağınıza erişimine izin vermeniz gerekir.

Her kaynak türü için belirli yönergeler vardır:

- [Azure Blob Depolama](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Cosmos DB](register-scan-azure-cosmos-database.md#setting-up-authentication-for-a-scan)
- [Azure Veri Gezgini](register-scan-azure-data-explorer.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage 2. Nesil](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Veritabanı](register-scan-azure-sql-database.md)
- [Azure SQL veritabanı yönetilen örneği](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)
- [SQL Server](register-scan-on-premises-sql-server.md#setting-up-authentication-for-a-scan)
- [Power BI](register-scan-power-bi-tenant.md)

## <a name="storing-your-credential-in-your-key-vault-and-using-the-right-secret-name-and-version"></a>Kimlik bilgilerinizi Anahtar Kasanızda depolama ve doğru gizli adı ve sürümü kullanma

Ayrıca, kimlik bilgilerinizi Azure Key Vault Örneğinizde depolamanız ve doğru gizli adı ve sürümü kullanmanız gerekir.

Aşağıdaki adımları izleyerek bunu doğrulayın:

1. Key Vault gidin.
1. **Ayarlar** > **Gizli diziler**'i seçin.
1. Taramalarda veri kaynağınıza göre kimlik doğrulaması yapmak için kullandığınız gizli anahtarı seçin.
1. Kullanmak istediğiniz sürümü seçin ve **gizli anahtarı göster**' e tıklayarak parolanın veya hesap anahtarının doğru olduğunu doğrulayın. 

## <a name="verify-permissions-for-the-purview-managed-identity-on-your-azure-key-vault"></a>Azure Key Vault yönetilen kimlik için izinleri doğrulama

Azure Key Vault erişmek için, yönetilen kimlik için doğru izinlerin yapılandırıldığını doğrulayın.

Bunu doğrulamak için, aşağıdaki adımları uygulayın:

1. Anahtar kasanıza ve **erişim ilkeleri** bölümüne gidin

1. Gizli dizi üzerinde en az **Get** ve **list** izinlerine sahip olan *Geçerli erişim ilkeleri* bölümünün altında görüntülendiğini görünümlerinizin yönettiği kimliği

   :::image type="content" source="./media/troubleshoot-connections/verify-minimum-permissions.png" alt-text="Hem Get hem de List izin seçeneklerinin açılan seçimini gösteren resim":::

Purview yönetilen kimliğinizi listede görmüyorsanız, bunları eklemek için [taramalar için kimlik bilgilerini oluşturma ve yönetme](manage-credentials.md) bölümündeki adımları izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure purview veri kataloğuna gözatın](how-to-browse-catalog.md)
- [Azure purview Veri Kataloğu 'Nda arama](how-to-search-catalog.md)
