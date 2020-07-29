---
title: Sağlık için adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/28/2020
ms.author: aahi
ms.openlocfilehash: 4ac65e85e05f408b8d2f37a1d6845dc9e28e2bab
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373088"
---
## <a name="health-entity-categories"></a>Sistem durumu varlık kategorileri:

Aşağıdaki varlık kategorileri [sistem durumu için metin analizi](../../how-tos/text-analytics-for-health.md)tarafından döndürülür.  Lütfen bu kapsayıcı önizlemesinde yalnızca Ingilizce metnin desteklendiğini ve her kapsayıcı görüntüsünde yalnızca tek bir model sürümü sağlandığını unutmayın.

### <a name="named-entity-recognition"></a>Adlandırılmış Varlık Tanıma

|Kategori  |Açıklama   |
|----------|--------------|
| ÖLÇERIN | Geçirir. |
| BODY_STRUCTURE | İnsan gövdenin ve diğer yapıların dahil olduğu parçalar. | 
| CONDITION_QUALIFIER | *Hafif*, *genişletilmiş*veya *Dağıtılmış*gibi koşul düzeyleri. | 
| TANı | Tıbbi koşullar. Örneğin, *hiper gerilim* . | 
| Görünüm | *Left* veya *anterior*gibi yönergeler. | 
| DOZU | Bir yollara boyutu veya miktarı.  | 
| EXAMINATION_NAME | İnceleme yöntemi veya yordamı. | 
| EXAMINATION_RELATION | ölçüm birimi ve İnceleme arasındaki ilişki.  | 
| EXAMINATION_UNIT | İnceleme için ölçüm birimi. | 
| EXAMINATION_VALUE | İnceleme Ölçüm biriminin değeri. | 
| FAMILY_RELATION | *Sister*gibi bir familial ilişkisi.  | 
| LEMIYOR | Frekans.   | 
| DENETLE | Cinsiyetleri. | 
| GENE | *TP53*gibi bir gene varlığı.   | 
| MEDICATION_CLASS | Yollara sınıfları. Örneğin, *antibiotika*.  | 
| MEDICATION_NAME  | Genel ve marka adlı medications.| 
| ROUTE_OR_MODE  | Yollara yönetme yöntemi. | 
| SYMPTOM_OR_SIGN  | İlllik belirtileri. | 
| TIME  | Sayısıdır. Örneğin, "8 yıl" veya "2:30:00:00 Bu sabah" |
| TREATMENT_NAME  | İşleme adları. | 
| VARIANT  | Gene varlığının bir genesel varyantı | 

### <a name="relation-extraction"></a>İlişki ayıklama

İlişki ayıklama metinde bahsedilen kavramlar arasındaki anlamlı bağlantıları tanımlar. Örneğin, bir koşul adı bir saat ile ilişkilendirerek bir "koşul süresi" ilişkisi bulunur. Sistem durumu için Metin Analizi, aşağıdaki ilişkileri tanımlayabilir:

* DIRECTION_OF_BODY_STRUCTURE  
* TIME_OF_CONDITION
* QUALIFIER_OF_CONDITION  
* DOSAGE_OF_MEDICATION 
* FORM_OF_MEDICATION  
* ROUTE_OR_MODE_OF_MEDICATION   
* STRENGTH_OF_MEDICATION 
* ADMINISTRATION_RATE_OF_MEDICATION   
* FREQUENCY_OF_MEDICATION 
* TIME_OF_MEDICATION 
* TIME_OF_TREATMENT 
* FREQUENCY_OF_TREATMENT  
* VALUE_OF_EXAMINATION
* UNIT_OF_EXAMINATION 
* RELATION_OF_EXAMINATION 
* TIME_OF_EXAMINATION  
* GRUBUNUN 
