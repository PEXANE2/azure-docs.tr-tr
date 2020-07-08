---
title: Azure dijital TWINS için müşteri verileri istek özellikleri
titleSuffix: Azure Digital Twins
description: Bu makalede, Azure dijital TWINS 'de kişisel verileri dışarı ve silmeye yönelik süreçler gösterilmektedir.
author: baanders
ms.author: baanders
ms.date: 05/05/2020
ms.topic: conceptual
ms.service: digital-twins
services: digital-twins
ms.openlocfilehash: 9822044de53f5f74df302e05e0e6c5cd06f3eb92
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84737164"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verileri isteme özelliklerinin özeti

Azure dijital TWINS, bir iş ortamının güvenli dijital temsillerini oluşturmaya yönelik bir geliştirici platformudur. Temsiller, kullanıcılar tarafından seçilen veri kaynaklarından canlı durum verileri tarafından çalıştırılır.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Azure dijital TWINS 'de *dijital TWINS* adlı dijital gösterimler, gerçek dünyada ortamlardaki varlıkları temsil eder ve tanımlayıcılarla ilişkilendirilir. Microsoft hiçbir bilgi vermez ve tanımlayıcıların kullanıcılarla bağıntılı olmasını sağlayacak verilere erişemez. 

Azure dijital TWINS 'deki dijital TWINS 'in birçoğu, kişisel varlıkları doğrudan temsil etmez. temsil edilen tipik nesneler bir Office toplantı odası veya bir fabrika tabanı olabilir. Ancak, kullanıcılar bazı varlıkları kişisel olarak tanımlanabilir olarak düşünebiliyor ve bu süre sonunda, dijital TWINS 'i bireylere bağlayan kendi varlıklarını veya envanter izleme yöntemlerini koruyabiliyor. Azure dijital TWINS, dijital TWINS ile ilişkili tüm verileri kişisel veriler gibi yönetir ve depolar.

Bir veri konusu isteğinde başvurulabilen kişisel verileri görüntülemek, dışarı aktarmak ve silmek için Azure dijital TWINS Yöneticisi, kullanıcılar ve roller için [**Azure Portal**](https://portal.azure.com/) veya dijital TWINS Için [**Azure DIJITAL TWINS REST API 'leri**](how-to-use-apis-sdks.md) kullanabilir. Azure portal ve REST API 'Leri, kullanıcıların bu tür veri konusu isteklere hizmet vermesi için farklı yöntemler sağlar.

## <a name="identifying-customer-data"></a>Müşteri verilerini tanımlama

Azure dijital TWINS, *kişisel verileri* yöneticilerle ve kullanıcılarla ilişkili veriler olacak şekilde değerlendirir. 

Azure dijital TWINS, ortama erişimi olan kullanıcıların [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) *nesne kimliğini* depolar. Azure portal Azure dijital TWINS Kullanıcı e-posta adreslerini görüntüler, ancak bu e-posta adresleri Azure dijital TWINS içinde depolanmaz. Azure Active Directory, Azure Active Directory nesne KIMLIĞI kullanılarak dinamik olarak aranır.

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Azure dijital TWINS yöneticileri, kullanıcılarla ilgili verileri silmek için Azure portal kullanabilir. Azure dijital TWINS REST API 'Lerini kullanarak tek tek dijital TWINS 'lerde silme işlemleri gerçekleştirmek da mümkündür. Kullanılabilir API 'Ler hakkında daha fazla bilgi için bkz. [Azure Digital TWINS REST API 'leri belgeleri](https://docs.microsoft.com/rest/api/azure-digitaltwins/).

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Azure dijital TWINS, dijital TWINS ile ilgili verileri depolar. Kullanıcılar bu verileri REST API 'Leri aracılığıyla alabilir ve görüntüleyebilir ve Kopyala ve Yapıştır ' ı kullanarak bu verileri dışarı aktarabilir. 

Kullanıcı rolleri ve rol atamaları dahil olmak üzere müşteri verileri seçilebilir, kopyalanabilir ve Azure portal yapıştırılabilirler. 

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Azure dijital TWINS hizmeti API 'lerinin tam listesi için bkz. [Azure Digital TWıNS REST API 'leri belgeleri](https://docs.microsoft.com/rest/api/azure-digitaltwins/).