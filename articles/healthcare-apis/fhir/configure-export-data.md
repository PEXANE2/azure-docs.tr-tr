---
title: FHıR için Azure API 'de dışarı aktarma ayarlarını yapılandırma
description: Bu makalede, FHıR için Azure API 'de dışarı aktarma ayarlarının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/18/2021
ms.author: matjazl
ms.openlocfilehash: ee110420c697afb6ecad857ba823c61d03c6be6c
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105047000"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Dışarı aktarma ayarını yapılandırın ve depolama hesabını ayarlayın

FHıR için Azure API, FHıR hesabı için Azure API 'sinden bir depolama hesabına veri vermenize olanak tanıyan $export komutunu destekler.

FHIR için Azure API 'de dışarı aktarma yapılandırması ile ilgili üç adım vardır:

1. FHıR hizmeti için Azure API üzerinde yönetilen kimliği etkinleştirin.
2. Azure depolama hesabı oluşturma (daha önce yapmadıysanız) ve FHıR için Azure API 'sine depolama hesabına izin atama.
3. FHıR için Azure API 'sinde depolama hesabı verme depolama hesabı olarak seçiliyor.

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Azure API 'sinde FHıR için yönetilen kimliği etkinleştirme

Azure API 'YI dışa aktarma için yapılandırmanın ilk adımı, hizmette sistem genelinde yönetilen kimliği etkinleştirmektir. Azure 'da Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler hakkında](../../active-directory/managed-identities-azure-resources/overview.md).

Bunu yapmak için FHıR hizmeti için Azure API 'sine gidin ve **kimlik**' i seçin. Durumu **on** olarak değiştirmek, fhır hizmeti IÇIN Azure API 'de yönetilen kimliği etkinleştirir.

![Yönetilen kimliği etkinleştir](media/export-data/fhir-mi-enabled.png)

Şimdi bir depolama hesabı oluşturup hizmetimizin atayarak bir sonraki adıma geçebilirsiniz.

## <a name="adding-permission-to-storage-account"></a>Depolama hesabına izin ekleniyor

Dışa aktarma içindeki bir sonraki adım, FHıR hizmetinin depolama hesabına yazabilmesi için Azure API 'SI için izin atama iznidir.

Bir depolama hesabı oluşturduktan sonra, depolama hesabında **Access Control (IAM)** bölümüne gidin ve **rol ataması Ekle**' yi seçin.

![Rol atamasını dışarı aktar](media/export-data/fhir-export-role-assignment.png)

Burada hizmet adınızla rol **Depolama Blobu verileri katkıda** bulunanı ekleyecek ve ardından **Kaydet**' i seçeceksiniz.

![Rol Ekle](media/export-data/fhir-export-role-add.png)

Artık $export için varsayılan depolama hesabı olarak FHıR için Azure API 'sindeki depolama hesabını seçebilirsiniz.

## <a name="selecting-the-storage-account-for-export"></a>$export için depolama hesabı seçiliyor

Son adım, FHıR için Azure API 'nin verileri uygulamasına aktarmak için kullanacağı Azure Depolama hesabını atacaktır. Bunu yapmak için, FHıR hizmeti için Azure API 'sindeki **tümleştirme** ' e gidin ve depolama hesabını seçin.

![FHıR dışarı aktarma depolaması](media/export-data/fhir-export-storage.png)

Bu son adımı tamamladıktan sonra, $export komutunu kullanarak verileri dışarı aktarmaya hazırsınız.

> [!Note]
> Yalnızca, FHıR için Azure API 'SI ile aynı abonelikte bulunan depolama hesaplarının, $export işlemler için hedef olarak kayıtlı olmasına izin verilir.

Veritabanı ayarlarını yapılandırma, erişim denetimi, tanılama günlüğünü etkinleştirme ve denetim günlüklerine veri eklemek için özel üstbilgiler kullanma hakkında daha fazla bilgi için, bkz.:

>[!div class="nextstepaction"]
>[Ek ayarlar](azure-api-for-fhir-additional-settings.md)
