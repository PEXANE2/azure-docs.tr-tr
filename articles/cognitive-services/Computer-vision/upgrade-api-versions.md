---
title: Görüntü İşleme API'si v 3.0 sürümüne yükseltin
titleSuffix: Azure Cognitive Services
description: V 2.0 ve v 2.1 'den Görüntü İşleme API'si 'ın v 3.0'a nasıl yükseltileceğini öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 08/11/2020
ms.author: pafarley
ROBOTS: NOINDEX
ms.openlocfilehash: 16add0dce88d0f809dc291d3c9de33e1a853f257
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136508"
---
# <a name="upgrade-to-v30-of-computer-vision-api-from-v20-and-v21"></a>V 2.0 ve v 2.1 'den v 3.0 Görüntü İşleme API'si yükseltin

Bu kılavuzda, REST API kullanıcıları için v 2.0 veya v 2.1 Görüntü İşleme API'si v 2.1 ' den v 3.0'a geçiş için mevcut kodunuzun nasıl değiştirileceği gösterilmektedir. 

## <a name="upgrade-batch-read-file-to-read"></a>Sürümüne `Batch Read File` Yükselt`Read`


1. `Batch Read File`2. x IÇIN API yolunu aşağıdaki gibi değiştirin:


    |2. x 'i oku |3,0 okuyun  |
    |----------|-----------|
    |https://{Endpoint}/vizyon/**v 2.0/okuma/çekirdek/asyncBatchAnalyze**     |https://{Endpoint}/Vision/**v 3.0/okuma/çözümleme**[? Language]|
    
    Yeni bir isteğe bağlı _dil_ parametresi var. Belgenizin dilini bilmiyorsanız veya çok dilli olabilir, dahil değildir. 

2. `Get Read Results`2. x içinde IÇIN API yolunu aşağıdaki gibi değiştirin:

    |2. x 'i oku |3,0 okuyun  |
    |----------|-----------|
    |https://{uç noktası}/Vision/**v 2.0**/Read/**Operations**/{operationId}     |https://{Endpoint}/Vision/**v 3.0**/Read/**analiz zeresults**/{operationId}|

3. JSON sonuçlarını denetim altına almak için kodu değiştirin `Get Read Operation Result` . Çağrısı `Get Read Operation Result` başarılı olduğunda, JSON gövdesinde bir durum dizesi alanı döndürür. V 2.0 'ın aşağıdaki değerleri diğer bilişsel hizmet REST API 'Leriyle daha iyi hizalanacak şekilde değiştirilmiştir. 
 
    |2. x 'i oku |3,0 okuyun  |
    |----------|-----------|
    |`"NotStarted"` |   `"notStarted"`|
    |`"Running"` | `"running"`|
    |`"Failed"` | `"failed"`|
    |`"Succeeded"` | `"succeeded"`|
    
4. Son tanıma sonucu JSON ' dan ' i yorumlamak için kodunuzu değiştirin `Get Read Operation Result` . 

    JSON üzerinde aşağıdaki değişiklikleri aklınızda yapın:
    
    - V2. x içinde `"Get Read Operation Result"` durum olduğunda OCR tanıma JSON döndürür `"Succeeded"` . V 3.0 'da Bu alan `"succeeded"` .
    - Sayfa dizisinin kökünü almak için, JSON hiyerarşisini ' den ' e değiştirin `"recognitionResults"` `"analyzeResult"` / `"readResults"` . Sayfa başına çizgi ve sözcükler JSON hiyerarşisi değişmeden kalır, bu nedenle kod değişikliği gerekli değildir.
    -   Sayfa açısı `"clockwiseOrientation"` olarak yeniden adlandırıldı `"angle"` ve Aralık 0-360 derece-180 180 arasında olacak şekilde değiştirilmiştir. Kodunuza bağlı olarak, çoğu matematik işlevi her iki aralığı de işleyemediğinden değişiklik yapmanız gerekebilir.
    -   V 3.0 API 'SI, isteğe bağlı olarak şu iyileştirmeleri de sağlar:- `"createdDateTime"` ve `"lastUpdatedDateTime"` işleme süresini izleyebilmeniz için eklenir. Daha fazla bilgi için belgelere bakın. 
        - `"version"`sonuçları oluşturmak için kullanılan API 'nin sürümünü söyler
        - Sözcük başına `"confidence"` eklendi. Bu değer, 0,95 değeri, tanımanın doğru olması için %95 olasılığı olacağı için ayarlanır. Güvenirlik puanı, insan gözden geçirmesi için hangi metnin gönderileceğini seçmek üzere kullanılabilir. 
    
    
        2. X içinde, çıkış biçimi aşağıdaki gibidir: 
    
    ```json
    {
        {
                "status": "Succeeded",
                "recognitionResults": [
                    {
                    "page": 1,
                    "language": "en",
                    "clockwiseOrientation": 349.59,
                    "width": 2661,
                    "height": 1901,
                    "unit": "pixel",
                    "lines": [
                        {
                        "boundingBox": [
                            67,
                            646,
                            2582,
                            713,
                            2580,
                            876,
                            67,
                            821
                        ],
                        "text": "The quick brown fox jumps",
                        "words": [
                            {
                            "boundingBox": [
                                143,
                                650,
                                435,
                                661,
                                436,
                                823,
                                144,
                                824
                            ],
                            "text": "The",
                            },
            // The rest of result is omitted for brevity 
            
    }
    ```
    
    V 3.0 'da, ayarlandı:
    
    ```json
    {
        {
            "status": "succeeded",
            "createdDateTime": "2020-05-28T05:13:21Z",
            "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
            "analyzeResult": {
            "version": "3.0.0",
            "readResults": [
                {
                "page": 1,
                "language": "en",
                "angle": 0.8551,
                "width": 2661,
                "height": 1901,
                "unit": "pixel",
                "lines": [
                    {
                    "boundingBox": [
                        67,
                        646,
                        2582,
                        713,
                        2580,
                        876,
                        67,
                        821
                    ],
                    "text": "The quick brown fox jumps",
                    "words": [
                        {
                        "boundingBox": [
                            143,
                            650,
                            435,
                            661,
                            436,
                            823,
                            144,
                            824
                        ],
                        "text": "The",
                        "confidence": 0.958
                        },
        // The rest of result is omitted for brevity 
        
    }
    ```

## <a name="upgrade-from-recognize-text-to-read"></a>`Recognize Text`Sürümünden sürümüne yükselt`Read`
`Recognize Text`*tüm görüntü işleme API'si sürümlerinde kullanımdan*kaldırılmakta olan bir *Önizleme* işlemidir. `Recognize Text` `Read` ' Den (v 3.0) veya `Batch Read File` (v 2.0, v 2.1) sürümüne geçiş yapmanız gerekir. v 3.0 `Read` , metin tanıma ve ek özellikler için daha yeni, daha iyi modeller içerir, bu nedenle önerilir. Sürümünden sürümüne yükseltmek `Recognize Text` için `Read` :

1. `Recognize Text`V2. x IÇIN API yolunu aşağıdaki gibi değiştirin:


    |Metin Tanıma 2. x |3,0 okuyun  |
    |----------|-----------|
    |https://{Endpoint}/vizyon/**v 2.0/recognizeText [? modu]**|https://{Endpoint}/Vision/**v 3.0/okuma/çözümleme**[? Language]|
    
    _Mode_ parametresi ' de desteklenmez `Read` . Hem el ile hem de yazılı metin otomatik olarak desteklenecektir.
    
    V 3.0 'da yeni bir isteğe bağlı _dil_ parametresi mevcuttur. Belgenizin dilini bilmiyorsanız veya çok dilli olabilir, dahil değildir. 

2. `Get Recognize Text Operation Result`V2. x IÇIN API yolunu aşağıdaki gibi değiştirin:

    |Metin Tanıma 2. x |3,0 okuyun  |
    |----------|-----------|
    |https://{Endpoint}/Vision/**v 2.0/textOperations/**{operationId}|https://{Endpoint}/Vision/**v 3.0/okuma/analiz Zeresults**/{operationId}|

3. JSON sonuçlarını denetim altına almak için kodu değiştirin `Get Recognize Text Operation Result` . Çağrısı `Get Read Operation Result` başarılı olduğunda, JSON gövdesinde bir durum dizesi alanı döndürür. 
 
    |Metin Tanıma 2. x |3,0 okuyun  |
    |----------|-----------|
    |`"NotStarted"` |   `"notStarted"`|
    |`"Running"` | `"running"`|
    |`"Failed"` | `"failed"`|
    |`"Succeeded"` | `"succeeded"`|


4. Son tanıma sonucu JSON ' dan desteğini yorumlamak için kodunuzu değiştirin `Get Recognize Text Operation Result` `Get Read Operation Result` .

    JSON üzerinde aşağıdaki değişiklikleri aklınızda yapın:    

    - V2. x içinde `"Get Read Operation Result"` durum olduğunda OCR tanıma JSON döndürür `"Succeeded"` . V 3.0 'da Bu alan `"succeeded"` .
    - Sayfa dizisinin kökünü almak için, JSON hiyerarşisini ' den ' e değiştirin `"recognitionResult"` `"analyzeResult"` / `"readResults"` . Sayfa başına çizgi ve sözcükler JSON hiyerarşisi değişmeden kalır, bu nedenle kod değişikliği gerekli değildir.
    -   V 3.0 API 'SI, isteğe bağlı olarak kullanabileceğiniz aşağıdaki geliştirmeleri de sağlar. Daha fazla ayrıntı için API Başvurusu ' na bakın `"createdDateTime"` ve `"lastUpdatedDateTime"` işleme süresini takip edebilmeniz için eklenmiştir. Daha fazla bilgi için belgelere bakın. 
        - `"version"`sonuçları oluşturmak için kullanılan API 'nin sürümünü söyler
        - Sözcük başına `"confidence"` eklendi. Bu değer, 0,95 değeri, tanımanın doğru olması için %95 olasılığı olacağı için ayarlanır. Güvenirlik puanı, insan gözden geçirmesi için hangi metnin gönderileceğini seçmek üzere kullanılabilir. 
        - `"angle"`metnin, saat yönünde (-180, 180]) ölçülen genel yönü.
        -  `"width"`ve `"height"` size belgenizin boyutlarını verir ve `"unit"` belge türüne bağlı olarak bu boyutların birimini (piksel veya inç) sağlar.
        - `"page"`çok sayfalı belgeler destekleniyor
        - `"language"`belgenin giriş dili (isteğe bağlı _dil_ parametresinden).


    2. X içinde, çıkış biçimi aşağıdaki gibidir: 
    
    ```json
    {
        {
                "status": "Succeeded",
                "recognitionResult": [
                    {
                    "lines": [
                        {
                        "boundingBox": [
                            67,
                            646,
                            2582,
                            713,
                            2580,
                            876,
                            67,
                            821
                        ],
                        "text": "The quick brown fox jumps",
                        "words": [
                            {
                            "boundingBox": [
                                143,
                                650,
                                435,
                                661,
                                436,
                                823,
                                144,
                                824
                            ],
                            "text": "The",
                            },
            // The rest of result is omitted for brevity 
            
    }
    ```
    
    V 3.0 'da, ayarlandı:
    
    ```json
    {
        {
            "status": "succeeded",
            "createdDateTime": "2020-05-28T05:13:21Z",
            "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
            "analyzeResult": {
            "version": "3.0.0",
            "readResults": [
                {
                "page": 1,
                "angle": 0.8551,
                "width": 2661,
                "height": 1901,
                "unit": "pixel",
                "lines": [
                    {
                    "boundingBox": [
                        67,
                        646,
                        2582,
                        713,
                        2580,
                        876,
                        67,
                        821
                    ],
                    "text": "The quick brown fox jumps",
                    "words": [
                        {
                        "boundingBox": [
                            143,
                            650,
                            435,
                            661,
                            436,
                            823,
                            144,
                            824
                        ],
                        "text": "The",
                        "confidence": 0.958
                        },
        // The rest of result is omitted for brevity 
        
    }
    ```
    
## <a name="all-other-operations"></a>Diğer tüm işlemler

V2 arasında başka bir son değişiklik yoktur. X ve v 3.0 Görüntü İşleme API'si. Yalnızca ile değiştirilecek olan API yolunu değiştirebilirsiniz `v2.0` `v3.0` .