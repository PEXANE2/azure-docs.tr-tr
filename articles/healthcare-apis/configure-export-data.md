---
title: FHıR için Azure API 'de dışarı aktarma ayarlarını yapılandırma
description: Bu makalede, FHıR için Azure API 'de dışarı aktarma ayarlarının nasıl yapılandırılacağı açıklanır.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/5/2020
ms.author: matjazl
ms.openlocfilehash: 46a55b83b38593a514d40a9f75d99739a1efb8a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871809"
---
# <a name="configure-export-setting-and-export-the-data-to-a-storage-account"></a>Dışarı aktarma ayarını yapılandırın ve verileri bir depolama hesabına aktarın

FHıR için Azure API, FHıR hesabı için Azure API 'sinden bir depolama hesabına veri vermenize olanak tanıyan $export komutunu destekler.

FHIR için Azure API 'de dışarı aktarma gerçekleştirmede dört adım vardır:

1. FHıR hizmeti için Azure API 'de yönetilen kimliği etkinleştirme
2. Azure Storage hesabı oluşturma (daha önce yapmadıysanız) ve FHıR için Azure API 'sine depolama hesabına izin atama
3. FHıR için Azure API 'sinde depolama hesabı verme depolama hesabı olarak seçiliyor
4. FHıR için Azure API 'sindeki $export komutunu çağırarak dışarı aktarmayı yürütme

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Azure API 'sinde FHıR için yönetilen kimliği etkinleştirme

Azure API 'sini dışa aktarma için yapılandırmanın ilk adımı, hizmette sistem genelinde yönetilen kimliği etkinleştirmek içindir. Azure 'da Yönetilen kimlikler hakkında [buradan](../active-directory/managed-identities-azure-resources/overview.md)bilgi edinebilirsiniz.

Bunu yapmak için FHıR hizmeti için Azure API 'ye gidin ve kimlik dikey penceresini seçin. Durumu on olarak değiştirmek, FHıR hizmeti için Azure API 'de yönetilen kimliği etkinleştirir.

![Yönetilen kimliği etkinleştir](media/export-data/fhir-mi-enabled.png)

Şimdi bir sonraki adıma geçebilir ve bir depolama hesabı oluşturabilir ve hizmetimizin atayabilirsiniz.

## <a name="adding-permission-to-storage-account"></a>Depolama hesabına izin ekleniyor

Dışarı aktarma içindeki bir sonraki adım, FHıR hizmetinin depolama hesabına yazabilmesi için Azure API 'SI için izin atamaya yöneliktir.

Depolama hesabı oluşturduktan sonra depolama hesabı 'nda Access Control (ıAM) dikey penceresine gidin ve rol atamaları Ekle ' yi seçin.

![Yönetilen kimliği etkinleştir](media/export-data/fhir-export-role-assignment.png)

Burada, hizmet adınızla rol Depolama Blobu verileri katkıda bulunanı ekleyeceğiz.

![Yönetilen kimliği etkinleştir](media/export-data/fhir-export-role-add.png)

Artık, $export için varsayılan depolama hesabı olarak FHıR için Azure API 'sinde depolama hesabını seçebileceğiniz bir sonraki adıma hazırlıyoruz.

## <a name="selecting-the-storage-account-for-export"></a>$export için depolama hesabı seçiliyor

$Export komutu çağırmadan önce son adım, FHıR için Azure API 'nin verileri uygulamasına aktarmak için kullanacağı Azure Depolama hesabını atacaktır. Bunu yapmak için, Azure portal 'deki FHıR hizmeti için Azure API 'sindeki tümleştirme dikey penceresine gidin ve depolama hesabını seçin 

![Yönetilen kimliği etkinleştir](media/export-data/fhir-export-storage.png)

Bundan sonra $export komutunu kullanarak verileri dışarı aktarmaya hazırız.

## <a name="exporting-the-data-using-export-command"></a>$export komutu kullanarak verileri dışarı aktarma

Dışa aktarma için FHıR için Azure API yapılandırıldıktan sonra, verileri hizmetten dışarı aktarmak için $export komutunu, belirtidiğimiz depolama hesabına dışarı aktarmak için kullanmaya devam edebilir. FHıR sunucusunda $export komutu çağırmayı öğrenmek için lütfen şu adresten $export belirtimindeki belgeleri okuyun:[https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html)

> [!IMPORTANT]
> Şu anda FHıR için Azure API 'sinin, ' de dışarı aktarma belirtiminde tanımlandığı şekilde yalnızca sistem düzeyinde dışarı aktarmayı desteklediğini unutmayın [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html) . Ayrıca, $export olan sorgu parametrelerini de desteklemiyoruz.

>[!div class="nextstepaction"]
>[Ek ayarlar](azure-api-for-fhir-additional-settings.md)