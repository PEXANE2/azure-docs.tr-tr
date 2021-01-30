---
title: Sağlık için adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/02/2020
ms.author: aahi
ms.openlocfilehash: 614d0fe69cee88791559758d5e08dda66672669b
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99097297"
---
[Sistem durumu için metin analizi](../../how-tos/text-analytics-for-health.md) aşağıdaki kategorilerdeki tıp kavramlarını algılar.  (Lütfen bu kapsayıcı önizlemesinde yalnızca Ingilizce metnin desteklendiğini ve her kapsayıcı görüntüsünde yalnızca tek bir model sürümü sağlandığını unutmayın.)


| Kategori  | Açıklama  |
|---------|---------|
| [ANATOMISI](#anatomy) | gövde ve anamemik sistemleri, siteleri, konumları veya bölgeleri hakkında bilgi yakalayan kavramlar. |
 | [DEMOGRAFIK BILGILERE](#demographics) | cinsiyet ve yaş hakkındaki bilgileri yakalayan kavramlar. |
 | [INCELEMESI](#examinations) | tanılama yordamları ve testler hakkında bilgi yakalayan kavramlar. |
 | [GENOMIKS](#genomics) | genes ve varyantlar hakkındaki bilgileri yakalayan kavramlar. |
 | [SAĞLıK](#healthcare) | yönetim olayları, ortamlar ve sağlık hizmetleri ile ilgili bilgileri yakalayan kavramlar. |
 | [TıBBI KOŞUL](#medical-condition) | tanılar, belirtiler veya imzalar hakkındaki bilgileri yakalayan kavramlar. |
 | [YOLLARA](#medication) | yollara adlarını, sınıfları, dozu ve yönetim yolunu içeren yollara hakkındaki bilgileri yakalayan kavramlar. |
 | [Ağ](#social) | aile ilişkisi gibi medically ilgili sosyal yönleri hakkında bilgi yakalayan kavramlar. |
 | [Düzeltme](#treatment) | karşılayan yordamlar hakkında bilgi yakalayan kavramlar. |
  
Her kategori iki kavram grubu içerebilir:

* **Varlıklar** -tanılama, yollara adı veya işleme adı gibi tıp kavramlarını yakalayan şartlar.  Örneğin, *bronz* bir tanıdır ve *aspırin* bir yollara adıdır.  Bu gruptaki bahsetmeler bir UMLS kavram KIMLIĞIYLE bağlantılı olabilir.
* **Öznitelikler** -algılanan varlık hakkında daha fazla bilgi sağlayan ifadeler, örneğin, *önemli* bir *bronz* veya *81 mg* *için bir koşul niteleyicisi.*  Bu kategorideki bahsetmeler bir UMLS kavram KIMLIĞIYLE bağlanmayacak.

Ayrıca, hizmet, öznitelikler ve varlıklar arasındaki ilişkiler dahil olmak üzere farklı kavramlar arasındaki ilişkileri, örneğin *gövde yapısına* yönelik *yönü* , *yollara adına* ve varlıklar arasındaki ilişkileri (örneğin, kısaltma algılama *) sağlar.*

## <a name="anatomy"></a>Anatomisi

### <a name="entities"></a>Varlıklar

**BODY_STRUCTURE** gövde sistemleri, anatomik konumlar veya bölgeler ve gövde siteleri. Örneğin, ARM, Knee, abdomen, burun, liver, baş, havemen sistemi, lymphocytes.

:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure.png" alt-text="Gövde yapısı varlığına bir örnek.":::


:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure-2.png" alt-text="Gövde yapısı varlığının genişletilmiş bir örneği.":::

### <a name="attributes"></a>Öznitelikler

Gövde yapısını oluşturan, sol, yan yana, üst, posterior gibi **Yön** yönlü terimler.

:::image type="content" source="../../media/ta-for-health/anatomy-attributes.png" alt-text="Yönlü özniteliğe bir örnek.":::

### <a name="supported-relations"></a>Desteklenen ilişkiler

* **DIRECTION_OF_BODY_STRUCTURE**

## <a name="demographics"></a>Demografik bilgiler

### <a name="entities"></a>Varlıklar

**Yaş** -hasta, Aile üyeleri ve diğerleri dahil olmak üzere tüm yaş hüküm ve tümceleri. Örneğin, 40-yıl-eski, 51 Yo, 3 ay eski, Yetişkin, Kant, ELDERLY, Başak, Minor, orta-eski.

:::image type="content" source="../../media/ta-for-health/age-entity.png" alt-text="Yaş varlığına bir örnek.":::

:::image type="content" source="../../media/ta-for-health/age-entity-2.png" alt-text="Bir yaş varlığına ait başka bir örnek.":::


**Cinsiyeti** -konunun cinsiyetini açığa çıkarmaz. Örneğin, Erkek, kadın, kadın, genttaman, anım.

:::image type="content" source="../../media/ta-for-health/gender-entity.png" alt-text="Cinsiyet varlığına bir örnek.":::

### <a name="attributes"></a>Öznitelikler

**RELATIONAL_OPERATOR** -bir demografik varlık ve ek bilgiler arasındaki ilişkiyi ifade eden ifadeler.

:::image type="content" source="../../media/ta-for-health/relational-operator.png" alt-text="İlişkisel işlece bir örnek.":::

## <a name="examinations"></a>Examinations

### <a name="entities"></a>Varlıklar

**EXAMINATION_NAME** – tanılama yordamları ve testleri. Örneğin, MRI, ECG, IV test, hemoglobin, platelets sayısı, *Bristol stool Scale* gibi sistemleri ölçeklendirin.

:::image type="content" source="../../media/ta-for-health/exam-name-entities.png" alt-text="Bir sınav varlığına bir örnek.":::

:::image type="content" source="../../media/ta-for-health/exam-name-entities-2.png" alt-text="İnceleme adı varlığına ait başka bir örnek.":::

### <a name="attributes"></a>Öznitelikler

**Yön** : bir incelemeyi bir inceleme olarak nitelendirir.

:::image type="content" source="../../media/ta-for-health/exam-direction-attribute.png" alt-text="İnceleme adı varlığına sahip bir Direction özniteliği örneği.":::

**MEASUREMENT_UNIT** – inceleme birimi. Örneğin, *hemoglobin > 9,5 g/dl*, *g/dl* dönemi *hemoglobin* testinin birimidir.

:::image type="content" source="../../media/ta-for-health/exam-unit-attribute.png" alt-text="Bir inceleme adı varlığına sahip ölçü birimi özniteliği örneği.":::

**MEASUREMENT_VALUE** : İnceleme değeri. Örneğin, *hemoglobin > 9,5 g/dl*' de, *9,5* terimi *hemoglobin* testinin değeridir.

:::image type="content" source="../../media/ta-for-health/exam-value-attribute.png" alt-text="Bir inceleme adı varlığına sahip ölçüm değeri özniteliği örneği.":::

**RELATIONAL_OPERATOR** – bir inceleme ve ek bilgi arasındaki ilişkiyi ifade eden ifadeler. Örneğin, bir hedef incelemesi için gerekli ölçüm değeri.

:::image type="content" source="../../media/ta-for-health/exam-relational-operator-attribute.png" alt-text="Bir inceleme adı varlığına sahip ilişkisel işlece bir örnek.":::

**Zaman** – bir incelemenizin başlangıcı ve/veya uzunluğu (süresi) Ile ilgili zamana bağlı şartlar. Örneğin, test gerçekleştiği zaman.

:::image type="content" source="../../media/ta-for-health/exam-time-attribute.png" alt-text="İnceleme adı varlığına sahip bir Time özniteliği örneği.":::

### <a name="supported-relations"></a>Desteklenen ilişkiler

+ **DIRECTION_OF_EXAMINATION**
+   **RELATION_OF_EXAMINATION**
+   **TIME_OF_EXAMINATION**
+   **UNIT_OF_EXAMINATION**
+   **VALUE_OF_EXAMINATION**

## <a name="genomics"></a>Genomiks

### <a name="entities"></a>Varlıklar

**Gene** – genes 'nin tüm bahsetmeleri. Örneğin, MTRR, F2.

:::image type="content" source="../../media/ta-for-health/genomics-entities.png" alt-text="Bir gene varlığına bir örnek.":::

**Varyant** : gene varyasyonların tüm bahsetmeleri. Örneğin, c. 524C>T, (MTRR): r.1462_1557del96
  
## <a name="healthcare"></a>Sağlık

### <a name="entities"></a>Varlıklar
  
**ADMINISTRATIVE_EVENT** – sağlık sistemiyle ilgili, yönetim/yarı yönetim doğası olan olaylar. Örneğin kayıt, giriş, deneme, çalışma girişi, aktarım, Boşalma, barındırma, barındırma 

:::image type="content" source="../../media/ta-for-health/healthcare-event-entity.png" alt-text="Bir sağlık olayı varlığına bir örnek.":::

**CARE_ENVIRONMENT** : hastalara önem verilen bir ortam veya konum. Örneğin, acil bir oda, doktor ofisi, Cardio birimi, Hospice, hosplik.

:::image type="content" source="../../media/ta-for-health/healthcare-environment-entity.png" alt-text="Bu ekran görüntüsünde bir sağlık ortamı varlığının örneği gösterilmektedir.":::

**HEALTHCARE_PROFESSION** – bir sağlık hizmeti lisanslanmış veya lisanslı değil. Örneğin, dentist, pathologist, neurologist, Radiologist, ilaç macist, nutritionist, fiziksel terist, chirouygutor.

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity.png" alt-text="Bu ekran görüntüsünde bir sağlık ortamı varlığının başka bir örneği gösterilmektedir.":::

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity-2.png" alt-text="Bir sağlık ortamı varlığına yönelik başka bir örnek.":::

## <a name="medical-condition"></a>Tıbbi koşul

### <a name="entities"></a>Varlıklar

**Tanılama** – dimevsimo, Syndrome, Kirme. Örneğin, breakst Cancer, Alzheimer, HTN, CHF, spuinal kablo yaralama.

:::image type="content" source="../../media/ta-for-health/medical-condition-entity.png" alt-text="Tıbbi koşul varlığına bir örnek.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-entity-2.png" alt-text="Tıbbi koşul varlığına ait başka bir örnek.":::

**SYMPTOM_OR_SIGN** – öznel veya amaç bulgusunda veya diğer tanılar. Örneğin, çest sorunlu, headache, acsh, SOB, abdomen, yumuşak, sorunsuz sesler, iyi bir sorun oluştu.

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity.png" alt-text="Tıbbi koşul işareti veya belirti varlığına bir örnek.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity-2.png" alt-text="Tıbbi koşul işareti veya belirti varlığına yönelik başka bir örnek.":::

### <a name="attributes"></a>Öznitelikler

Tıbbi bir koşulu tanımlamakta kullanılan **CONDITION_QUALIFIER** kalite koşulları. Aşağıdaki alt kategorilerin hepsi niteleyiciler olarak kabul edilir:

1.  Zaman ile ilgili ifadeler: qualitatively, ani, dar, tarih ve longgibi zaman boyutunu tanımlayan koşullardır. 
2.  Kalite ifadeleri: Bunlar, yazma, Sharp gibi tıbbi koşulun "yapısını" tanımlayan koşullardır.
3.  Önem derecesi ifadeleri: ciddi, hafif, bir bit, denetlenmeyen.
4.  Extensivity ifadeleri: yerel, odak, dağıtma.
5.  Taban ayırma ifadeleri: radyan, aydınlık.
6.  Koşul ölçeği: bazı durumlarda bir koşul, sınırlı sıralı değerler listesi olan bir ölçeğe göre belirlenir. Örneğin, III pantik Cancer adlı hastalar.
7.  Koşul kursu: geliştirme, çalışma, çözümleme, yeniden görev gibi bir koşulun kursla veya ilerleyişine ilişkin bir terim. 

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis.png" alt-text="Koşul niteleyicisi özniteliği ve bir tanılama varlığına bir örnek.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-2.png" alt-text="Bir koşul niteleyicisi özniteliği ve bir tanılama varlığına ait başka bir örnek.":::

:::image type="content" source="../../media/ta-for-health/conditional-qualifier-symptom-medication.png" alt-text="Belirti ve yollara varlıkları içeren bir koşul niteleyicisi özniteliği örneği.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-3.png" alt-text="Bu ekran görüntüsünde, bir tanılama varlığına sahip bir koşul niteleyicisi özniteliği örneği gösterilmektedir.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-symptom.png" alt-text="Bu ekran görüntüsünde, bir tanılama varlığına sahip bir koşul niteleyicisi özniteliğinin ek bir örneği gösterilir.":::

Gövde tıbbi koşullarınızı karakterleştiren **Yön** yönlü terimler.

:::image type="content" source="../../media/ta-for-health/medical-condition-direction-attribute.png" alt-text="Tıbbi koşul varlığına sahip bir Direction özniteliği örneği.":::

**Sıklık** -bir tıbbi koşulun ne sıklıkla gerçekleştiği, gerçekleştiği veya oluşma sıklığı.

:::image type="content" source="../../media/ta-for-health/medical-condition-frequency-attribute.png" alt-text="Tıbbi koşul varlığına sahip sıklık özniteliği örneği.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-frequency-attribute-2.png" alt-text="Bir belirti veya imza varlığı içeren bir Direction özniteliği örneği.":::

**MEASUREMENT_UNIT** -tıbbi bir koşulu karakterleştiren birim. Örneğin, *1,5 x2x1 cm tumor veya* *cm* terimi, *tumor* için ölçü birimidir. 

:::image type="content" source="../../media/ta-for-health/medical-condition-measure-unit-attribute.png" alt-text="Tıbbi koşul varlığına sahip bir ölçü birimi özniteliği örneği.":::

**MEASUREMENT_VALUE** -bir tıp koşulunu karakterleştiren değer. Örneğin, *1,5 x2x1 cm tumor veya* *1.5 x2x1* terimi, *tumor* için ölçüm değeridir. 

:::image type="content" source="../../media/ta-for-health/medical-condition-measure-value-attribute.png" alt-text="Ekran görüntüsünde, belirti veya imza varlığı içeren bir Direction özniteliği örneği gösterilmektedir.":::

**RELATIONAL_OPERATOR** -tıbbi koşul ek bilgileri arasındaki ilişkiyi ifade eden ifadeler. Örneğin, saat veya ölçüm değeri. 

:::image type="content" source="../../media/ta-for-health/medical-condition-relational-operator.png" alt-text="Ekran görüntüsünde, bir belirti veya imza varlığı içeren bir Direction özniteliği örneği gösterilmektedir.":::

Tıbbi koşulun başlangıcı ve/veya uzunluğu (süresi) ile ilgili **zamana** bağlı şartlar. Örneğin, bir belirti başlatıldığında (Onset) veya bir dimevsims oluştuğunda.

:::image type="content" source="../../media/ta-for-health/medical-condition-time-attribute.png" alt-text="Ekran görüntüsünde, bir belirti veya imza varlığına sahip bir Direction özniteliği örneği gösterilmektedir.":::

### <a name="supported-relations"></a>Desteklenen ilişkiler

+ **DIRECTION_OF_CONDITION**
+   **QUALIFIER_OF_CONDITION**
+   **TIME_OF_CONDITION**
+   **UNIT_OF_CONDITION**
+   **VALUE_OF_CONDITION**

## <a name="medication"></a>Yollara

### <a name="entities"></a>Varlıklar

**MEDICATION_CLASS** : benzer bir eylem mekanizmasına sahip bir medications kümesi, ilgili bir eylem mekanizması, benzer bir cheyasal yapısı ve/veya aynı şekilde değerlendirmek için kullanılır. Örneğin, ACE, opioıd, antibiotik, sorun rahatlıcılar.

:::image type="content" source="../../media/ta-for-health/medication-entities-class.png" alt-text="Yollara sınıf varlığına bir örnek.":::

**MEDICATION_NAME** – telif haklı marka adları ve marka olmayan adlar dahil olmak üzere, yollara bahsetmeler. Örneğin, Advıl, Ibuprofen.

:::image type="content" source="../../media/ta-for-health/medication-entities-name.png" alt-text="Bir yollara Name varlığına bir örnek.":::

### <a name="attributes"></a>Öznitelikler

**Dozu** -sıralanan yollara miktarı. Örneğin, Kordium Chlorıde çözümü *1000 ml*'yi kullanın.

:::image type="content" source="../../media/ta-for-health/medication-dosage.png" alt-text="Bir yollara dozu özniteliği örneği.":::

**Sıklık** -bir yollara ne sıklıkta alınması gerekir.

:::image type="content" source="../../media/ta-for-health/medication-frequency.png" alt-text="Yollara sıklık özniteliği örneği.":::

:::image type="content" source="../../media/ta-for-health/medication-frequency-2.png" alt-text="Başka bir yollara sıklık özniteliği örneği.":::

**MEDICATION_FORM** -yollara formu. Örneğin, çözüm, Pill, kapsül, tablet, Patch, gel, Paste, fohar, püskürtme, düşme, kremi, Syrup.

:::image type="content" source="../../media/ta-for-health/medication-form.png" alt-text="Bir yollara form özniteliği örneği.":::

**MEDICATION_ROUTE** -yollara yönetim yöntemi. Örneğin, Oral, vaginal, IV, epidural, topical, intarled.

:::image type="content" source="../../media/ta-for-health/medication-route.png" alt-text="Bir yollara Route özniteliği örneği.":::

**RELATIONAL_OPERATOR** -yollara ve ek bilgiler arasındaki ilişkiyi ifade eden ifadeler. Örneğin, gerekli ölçüm değeri.

:::image type="content" source="../../media/ta-for-health/medication-relational-operator.png" alt-text="Ekran görüntüsünde bir yollara varlığına sahip bir ilişkisel işleç özniteliği örneği gösterilmektedir.":::

:::image type="content" source="../../media/ta-for-health/medication-time.png" alt-text="Ekran görüntüsü, bir yollara varlığıyla ilişkisel işleç özniteliğinin başka bir örneğini gösterir.":::

### <a name="supported-relations"></a>Desteklenen ilişkiler

+ **DOSAGE_OF_MEDICATION**
+   **FORM_OF_MEDICATION**
+   **FREQUENCY_OF_MEDICATION**
+   **ROUTE_OF_MEDICATION**
+   **TIME_OF_MEDICATION**

## <a name="social"></a>Sosyal

### <a name="entities"></a>Varlıklar

**FAMILY_RELATION** – ilgilinin aile akrabalarının bahsetme. Örneğin, Baba, kız, eşdüzey öğeler, ebeveynler.

:::image type="content" source="../../media/ta-for-health/family-relation.png" alt-text="Ekran görüntüsü bir işleme süresi özniteliğinin başka bir örneğini gösterir.":::

## <a name="treatment"></a>Düzeltme

### <a name="entities"></a>Varlıklar

**TREATMENT_NAME** – terxeutik yordamlar. Örneğin, Knee değiştirme Surgery, kemik Marrow transplant, TAVI, Diet.

:::image type="content" source="../../media/ta-for-health/treatment-entities-name.png" alt-text="Bir işleme adı varlığına bir örnek.":::

### <a name="attributes"></a>Öznitelikler

Bir işleme göre **yönlü yön** yönlü terimler.

:::image type="content" source="../../media/ta-for-health/treatment-direction.png" alt-text="Ekran görüntüsünde bir işleme yönü özniteliği örneği gösterilmektedir.":::

**Sıklık** -bir bir işleme ne sıklıkta gerçekleşeceğini veya ne zaman gerçekleşeceğini.

:::image type="content" source="../../media/ta-for-health/treatment-frequency.png" alt-text="Ekran görüntüsü bir işleme yönü özniteliğinin başka bir örneğini gösterir.":::
 
**RELATIONAL_OPERATOR** -ve ek bilgiler arasındaki ilişkiyi ifade eden ifadeler.  Örneğin, önceki yordamdan ne kadar süre geçtiğinde.

:::image type="content" source="../../media/ta-for-health/treatment-relational-operator.png" alt-text="Bir işleme ilişkisel işleç özniteliği örneği.":::

Bir işleme ilişkin başlangıç ve/veya Uzunluk (süre) ile ilgili **zamana** bağlı şartlar. Örneğin, işleme verilen tarih.

:::image type="content" source="../../media/ta-for-health/treatment-time.png" alt-text="Ekran görüntüsünde bir işleme süresi özniteliği örneği gösterilmektedir.":::

### <a name="supported-relations"></a>Desteklenen ilişkiler

+ **DIRECTION_OF_TREATMENT**
+   **TIME_OF_TREATMENT**
+   **FREQUENCY_OF_TREATMENT**
