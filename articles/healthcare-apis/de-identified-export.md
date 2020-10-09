---
title: FHıR için Azure API 'SI için de tanımlanan verileri (Önizleme) dışa aktarma
description: Bu makalede, belirtilen dışarı aktarmanın nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 9/28/2020
ms.author: matjazl
ms.openlocfilehash: bdbab0e032764d07119402686051d391376cb913
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91844003"
---
# <a name="exporting-de-identified-data-preview"></a>Belirtilen verileri dışarı aktarma (Önizleme)

> [!Note] 
> Geçersiz şekilde tanımlanan dışa aktarma kullanılırken sonuçlar, müşteri tarafından seçilen veriler ve işlevler gibi etkenlere bağlı olarak değişir. Microsoft, belirtilen dışarı aktarma çıkışlarını değerlendiremez veya müşterinin kullanım örnekleri ve uyumluluk ihtiyaçları için acceptability 'ı tespit edemiyor. Belirtilen herhangi bir yasal, mevzuata veya uyumluluk gereksinimini karşılamak için de, belirtilen dışarı aktarmanın garantisi yoktur.

$Export komutu, FHıR sunucusundan de belirtilen verileri dışarı aktarmak için de kullanılabilir. Anonimleştirme [Için Fhır araçlarından](https://github.com/microsoft/FHIR-Tools-for-Anonymization)anonimleştirme altyapısını kullanır ve sorgu parametrelerinde anonimleştirme yapılandırma ayrıntılarını alır. Kendi anonimleştirme yapılandırma dosyanızı oluşturabilir veya HIPAA güvenli Harbveya yöntemi için bir başlangıç noktası olarak [örnek yapılandırma dosyasını](https://github.com/microsoft/FHIR-Tools-for-Anonymization#sample-configuration-file-for-hipaa-safe-harbor-method) kullanabilirsiniz. 

 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

|Sorgu parametresi            | Örnek |Optionitesi| Açıklama|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |Üzerinde DemoConfig.js|Geçersiz şekilde tanımlanan dışarı aktarma için gereklidir |Yapılandırma dosyasının adı. Yapılandırma dosyası biçimini [buradan](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format)görebilirsiniz. Bu dosya, dışarı aktarma konumu olarak yapılandırılmış aynı Azure depolama hesabı içinde, **anonimleştirme** adlı bir kapsayıcı içinde tutulmalıdır. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Geçersiz şekilde tanımlanan dışarı aktarma için isteğe bağlı|Bu, yapılandırma dosyasının ETag ' i. Blob özelliğinden Azure Depolama Gezgini 'ni kullanarak ETag 'i edinebilirsiniz|

> [!IMPORTANT]
> Hem ham dışa aktarma hem de dışa aktarma yapılandırmasının parçası olarak belirtilen Azure depolama hesabına dışarı aktarma yazma işlemleri. Farklı bir şekilde tanımlanan yapılandırmaya karşılık gelen farklı kapsayıcılar kullanmanız ve kapsayıcı düzeyinde Kullanıcı erişimini yönetmeniz önerilir.