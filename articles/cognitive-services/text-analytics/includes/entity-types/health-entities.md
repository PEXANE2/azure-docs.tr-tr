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
ms.openlocfilehash: 6880391fb54791fe5f597de2305d24f8c0e47ec6
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122538"
---
## <a name="health-entity-categories"></a>Sistem durumu varlık kategorileri:

Aşağıdaki varlık kategorileri [sistem durumu için metin analizi](../../how-tos/text-analytics-for-health.md)tarafından döndürülür.  Lütfen bu kapsayıcı önizlemesinde yalnızca Ingilizce metnin desteklendiğini ve her kapsayıcı görüntüsünde yalnızca tek bir model sürümü sağlandığını unutmayın.

### <a name="named-entity-recognition"></a>Adlandırılmış Varlık Tanıma

|Kategori  |Açıklama   |
|----------|--------------|
| Yaş | Geçirir. Örneğin *30 yıl eski*. |
| Yönettiveevent | Bir yönetim olayı. |
| BodyStructure | İnsan gövdenin ve diğer yapıların dahil olduğu parçalar. Örneğin *ARM*veya *kalp*. | 
| Gelişme ortamı | İlgilenme veya işleme yönetilen ortam. Örneğin *acil bir oda* | 
| ConditionQualifier | Koşul düzeyleri. Örneğin *hafif*, *genişletilmiş*veya *Dağıtılmış*. | 
| Tanı | Tıbbi koşullar. Örneğin, *hiper gerilim*. | 
| Yön | Yükleye. Örneğin *Left* veya *anterior*. | 
| Dozu | Bir yollara boyutu veya miktarı. Örneğin, *25mg*.  | 
| ExaminationName | İnceleme yöntemi veya yordamı. Örneğin, *X-Ray*. | 
| Relationalişleci | İki varlık arasındaki ilişkiyi tanımlayan bir işleç. Örneğin, veya *küçüktür* `>=` .  | 
| MeasurementUnit | Bir ölçü birimi (bir yüzde gibi). | 
| MeasurementValue | Ölçüm biriminin sayısal değeri. | 
| FamilyRelation | Bir familial ilişkisi. Örneğin, *Sister*.  | 
| Frequency | Frekans.   | 
| Cinsiyet | Cinsiyetleri. | 
| Gene | *TP53*gibi bir gene varlığı.   | 
| Healthumkuma | Yollara yönetme yöntemi. Örneğin, *sözlü yönetimi*. | 
| MedicationClass | Yollara sınıfları. Örneğin, *antibiotika*.  | 
| MedicationForm | Bir yollara formu. Örneğin, *kapsül*. | 
| MedicationName  | Genel ve marka adlı medications. Örneğin, *Ibuprofen*. | 
| MedicationRoute | Yollara yönetme yöntemi. Örneğin, *sözlü yönetimi*. | 
| SymptomOrSign  | İlllik belirtileri. Örneğin *Sore Throat*. | 
| Zaman | Sayısıdır. Örneğin *8 yıl* veya *2:30:00:00 Bu sabah* |
| TreatmentName  | İşleme adları. Örneğin, *örn.* | 
| Değişken | Gene varlığının bir genesel varyantı. | 

### <a name="relation-extraction"></a>İlişki ayıklama

İlişki ayıklama metinde bahsedilen kavramlar arasındaki anlamlı bağlantıları tanımlar. Örneğin, bir koşul adı bir saat ile ilişkilendirerek bir "koşul süresi" ilişkisi bulunur. Sistem durumu için Metin Analizi, aşağıdaki ilişkileri tanımlayabilir:

|Kategori  |Açıklama   |
|----------|--------------|
| DirectionOfBodyStructure | Gövde yapısının yönü. |
| Yönlüofcondition | Koşulun yönü. |
| Yönlüofincelenmesi | İnceleme yönü. |
| Yönlübir Işleme | Bir işleme yönü. |
| TimeOfCondition | Bir koşulun başladıkları noktada ile ilişkili zaman. |
| QualifierOfCondition | Bir koşulun ilişkili niteleyicisi. |
| DosageOfMedication | Yollara 'in bir dozu. |
| FormOfMedication | Bir yollara formu. |
| RouteOfMedication | Bir ilaç kullanan yol veya mod. Örneğin, *sözlü*. |
| FrequencyOfMedication | Bir yollara tüketilen sıklık. | 
| ValueOfCondition | Bir koşulla ilişkili sayısal değer. |
| UnitOfCondition | Bir koşulla ilişkili birim (örneğin, zaman). |
| TimeOfMedication | Yollara tüketilen zaman. |
| Zaman Işleme | Bir işleme yönetilme zamanı. | 
| FrequencyOfTreatment | Bir işleme yönetilme sıklığı. |
| Valueofincelenmesi | İnceleme ile ilişkili sayısal bir değer. | 
| UnitOfExamination | İnceleme ile ilişkili bir birim (yüzde gibi). |
| Ilişki Incelemesi | Bir varlık ve İnceleme arasındaki ilişki. | 
| Timeofincelemesini | İnceleme ile ilişkili zaman. |
| Kısaltma | Bir kısaltma.  | 
