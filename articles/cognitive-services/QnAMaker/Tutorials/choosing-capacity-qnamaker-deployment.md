---
title: Dağıtım - soru-cevap Oluşturucu için kaynak kapasitesi
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetinizi oluşturmadan önce yukarıdaki hizmetler hangi katmanı size uygun olduğuna karar vermelisiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 2b2879a1ebcf01a0433873e8da77c4ce55a078cd
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647007"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Soru-cevap Oluşturucu dağıtımınız için kapasite seçme

Soru-cevap Oluşturucu hizmetini üç Azure kaynaklarında bir bağımlılık alır:
1.  App Service (için çalışma zamanı)
2.  Azure Search (QnAs depolamak ve aramak için)
3.  App Insights (Sohbet günlükleri ve telemetri depolamak için isteğe bağlı)

Soru-cevap Oluşturucu hizmetinizi oluşturmadan önce yukarıdaki hizmetler hangi katmanı size uygun olduğuna karar vermelisiniz. 

Genellikle dikkate almanız gereken üç parametresi vardır:

1. **Hizmetten ihtiyacınız olan aktarım hızı**: Gereksinimlerinize göre App Service için uygun [uygulama planını](https://azure.microsoft.com/pricing/details/app-service/plans/) seçin. Yapabilecekleriniz [ölçeği](https://docs.microsoft.com/azure/app-service/manage-scale-up) veya uygulama azaltın. Bkz: daha fazla ayrıntı bu Azure arama SKU seçiminiz ayrıca etkilemelidir [burada](https://docs.microsoft.com/azure/search/search-sku-tier).

1. **Bilgi bankalarının boyutu ve sayısı**: Senaryonuz için uygun [Azure Search SKU](https://azure.microsoft.com/pricing/details/search/) 'sunu seçin. N katmanında izin verilen en fazla dizin, belirli bir katman içinde N-1 bilgi bankalarından yayımlayabilirsiniz. Ayrıca, en büyük boyutu ve katman izin verilen belge sayısını denetleyin.

    Örneğin, katmanınızda 15 ' in izin verilen dizini varsa, 14 bilgi tabanı (yayımlanan Bilgi Bankası başına 1 Dizin) yayımlayabilirsiniz. On beşinci Dizin, yazma ve test için tüm bilgi tabanları için kullanılır. 

1. **Kaynak olarak belge sayısı**: Soru-Cevap Oluşturma yönetim hizmetinin ücretsiz SKU 'SU, Portal ve API 'Ler aracılığıyla (her biri 1 MB boyutunda) yönetebileceğiniz belge sayısını sınırlar. Standart SKU yönetebileceğiniz belge sayısı için hiçbir sınır vardır. Daha fazla ayrıntı görmek [burada](https://aka.ms/qnamaker-pricing).

Aşağıdaki tabloda bazı üst düzey yönergeler verilmektedir.

|                        | Soru-cevap Oluşturucu Yönetimi | App Service | Azure Search | Sınırlamalar                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Deneme        | Ücretsiz SKU             | Ücretsiz Katmanı   | Ücretsiz Katmanı    | En fazla 2 KB'leri, 50 MB boyutunda yayımlama  |
| Geliştirme/Test Ortamı   | Standart SKU         | Paylaşılan      | Temel        | 14 adede kadar KB'leri, 2 GB boyutunu yayımlama    |
| Üretim ortamı | Standart SKU         | Temel       | Standart     | En fazla 49 KB'leri, 25 GB boyutunu yayımlama |

Soru-cevap Oluşturucu yığın yükseltmek için bkz: [soru-cevap Oluşturucu hizmetini](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu hizmetini](../How-To/upgrade-qnamaker-service.md)
