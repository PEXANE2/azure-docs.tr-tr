---
title: Taramalar için kimlik bilgileri oluşturma ve yönetme
description: Bu makalede, Azure purview 'da kimlik bilgileri oluşturma ve yönetme adımları açıklanır.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: 4c964f3661e120026189a75d331e6db975b41c70
ms.sourcegitcommit: 90caa05809d85382c5a50a6804b9a4d8b39ee31e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97756084"
---
# <a name="credentials-for-source-authentication-in-azure-purview"></a>Azure purview 'da kaynak kimlik doğrulaması için kimlik bilgileri

Bu makalede, kayıtlı kimlik doğrulama bilgilerini hızlı bir şekilde yeniden kullanmak ve veri kaynağı taramalarınızda uygulamanız için Azure takip görünümü 'nde nasıl kimlik bilgileri oluşturabileceğiniz açıklanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure Anahtar Kasası. Bir oluşturma hakkında bilgi edinmek için bkz. [hızlı başlangıç: Azure Portal kullanarak bir Anahtar Kasası oluşturma](../key-vault/general/quick-create-portal.md).

## <a name="introduction"></a>Giriş
Kimlik bilgileri, Azure purview 'ın kayıtlı veri kaynaklarınızda kimlik doğrulamak için kullanabileceği kimlik doğrulama bilgileridir. Çeşitli kimlik doğrulama senaryoları (Kullanıcı adı/parola gerektiren temel kimlik doğrulaması gibi) için bir kimlik bilgisi nesnesi oluşturulabilir ve seçilen kimlik doğrulama yöntemi türüne göre gereken belirli bilgileri yakalar. Kimlik bilgileri, kimlik bilgileri oluşturma işlemi sırasında hassas kimlik doğrulama bilgilerini almak için mevcut Azure Anahtar Kasası sırlarınızı kullanır.

## <a name="using-purview-managed-identity-to-set-up-scans"></a>Taramaları ayarlamak için purview tarafından yönetilen kimliği kullanma
Taramaları ayarlamak için purview yönetilen kimliğini kullanıyorsanız, bu kimlik bilgilerini açık bir şekilde oluşturmanız ve anahtar kasanızın, bunları depolamak için takip görünümüne bağlamanız gerekmez. Veri kaynaklarınızı taramak için adım adım yönetilen kimliği ekleme hakkında ayrıntılı yönergeler için aşağıdaki veri kaynağına özel kimlik doğrulama bölümlerine bakın:

- [Azure Blob Depolama](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage 2. Nesil](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Veritabanı](register-scan-azure-sql-database.md)
- [Azure SQL Veritabanı Yönetilen Örneği](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)

## <a name="create-azure-key-vaults-connections-in-your-azure-purview-account"></a>Azure purview hesabınızda Azure Anahtar Kasası bağlantıları oluşturma

Bir kimlik bilgisi oluşturabilmeniz için önce, mevcut Azure Key Vault örneklerinden birini veya daha fazlasını Azure purview hesabınızla ilişkilendirmeniz gerekir.

1. Azure purview gezinme menüsünde, yönetim merkezine gidin ve kimlik bilgileri ' ne gidin.

2. Kimlik bilgileri komut çubuğundan Yönet Key Vault bağlantıları ' nı seçin.

    :::image type="content" source="media/manage-credentials/manage-kv-connections.png" alt-text="AKV bağlantılarını yönetme":::

3. Key Vault bağlantıları Yönet panelinden + yeni ' yi seçin 

4. Gerekli bilgileri sağlayın ve Oluştur ' u seçin.

5. Key Vault Azure purview hesabınızla başarıyla ilişkilendirildiğini onaylayın

    :::image type="content" source="media/manage-credentials/view-kv-connections.png" alt-text="AKV bağlantılarını görüntüle":::

## <a name="grant-the-purview-managed-identity-access-to-your-azure-key-vault"></a>Azure Key Vault yönetilen kimlik erişimine izin verin

Anahtar Kasası > erişim ilkelerine > erişim Ilkesi Ekle ' ye gidin. Gizli dizi izinleri açılan listesinde alma izni verin ve asıl görünüm MSI olacak asıl ' ı seçin. 

:::image type="content" source="media/manage-credentials/add-msi-to-akv.png" alt-text="Bu MSI 'yi AKV 'ye Ekle":::


:::image type="content" source="media/manage-credentials/add-access-policy.png" alt-text="Erişim İlkesi Ekle":::


:::image type="content" source="media/manage-credentials/save-access-policy.png" alt-text="Erişim ilkesini Kaydet":::

## <a name="create-a-new-credential"></a>Yeni kimlik bilgisi oluştur

Bugörünümde desteklenen kimlik bilgisi türü:
* Temel kimlik doğrulaması: **parolayı** Anahtar Kasası 'nda gizli dizi olarak ekleyeceksiniz
* Hizmet sorumlusu: **hizmet sorumlusu anahtarını** Anahtar Kasası 'nda gizli dizi olarak ekleyeceksiniz 
* SQL kimlik doğrulaması: **parolayı** Anahtar Kasası 'nda gizli dizi olarak ekleyeceksiniz
* Hesap anahtarı: **hesap anahtarını** Anahtar Kasası 'nda gizli dizi olarak ekleyeceksiniz

Daha fazla bilgi için bkz. [Key Vault için gizli dizi ekleme](../key-vault/secrets/quick-create-portal.md#add-a-secret-to-key-vault).

Gizli dizilerinizi Anahtar Kasanızda depoladıktan sonra kimlik bilgileri komut çubuğundan + yeni ' yi seçerek yeni kimlik bilgilerinizi oluşturun. Kimlik doğrulama yöntemini seçme ve öğesinden bir gizli anahtar seçilecek bir Key Vault örneği de dahil olmak üzere gerekli bilgileri sağlayın. Tüm ayrıntılar doldurulduktan sonra Oluştur ' a tıklayın.

:::image type="content" source="media/manage-credentials/new-credential.png" alt-text="Yeni kimlik bilgisi":::

Kimlik bilgileri listesi görünümünde yeni kimlik bilgilerinizin görüntülendiğini ve kullanıma hazırlandiğini doğrulayın

:::image type="content" source="media/manage-credentials/view-credentials.png" alt-text="Kimlik bilgisini görüntüle":::

## <a name="manage-your-key-vault-connections"></a>Anahtar Kasası bağlantılarınızı yönetin

1. Ada göre Key Vault bağlantıları arama/bulma

    :::image type="content" source="media/manage-credentials/search-kv.png" alt-text="Anahtar kasasında ara":::

1. Bir veya daha fazla Key Vault bağlantıyı silme
 
    :::image type="content" source="media/manage-credentials/delete-kv.png" alt-text="Anahtar kasasını Sil":::

## <a name="manage-your-credentials"></a>Kimlik bilgilerinizi yönetin

1. Kimlik bilgilerini ada göre ara/bul
  
2. Mevcut bir kimlik bilgisi seçin ve güncelleştirmeleri yapın

3. Bir veya daha fazla kimlik bilgisini sil
