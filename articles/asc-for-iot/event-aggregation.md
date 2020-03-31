---
title: IoT olay toplama için Azure Güvenlik Merkezi'ni anlama| Microsoft Dokümanlar
description: IoT olay toplama için Azure Güvenlik Merkezi hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d40dac0b-abd0-4ff5-82eb-0f511ee13725
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2019
ms.author: mlottner
ms.openlocfilehash: ca1d1a5761e62b2838a474dcb83f450987972998
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73928967"
---
# <a name="azure-security-center-for-iot-event-aggregation"></a>IoT olay toplama için Azure Güvenlik Merkezi

Azure Güvenlik Merkezi IoT güvenlik aracıları yerel cihazınızdan veri ve sistem olayları toplar ve bu verileri işleme ve analiz için Azure bulutuna gönderir. Güvenlik aracısı, yeni işlem ve yeni bağlantı olayları da dahil olmak üzere birçok türde aygıt olayı toplar. Hem yeni işlem hem de yeni bağlantı olayları bir saniye içinde bir aygıtta sık sık oluşabilir ve sağlam ve kapsamlı güvenlik için önemli olmakla birlikte, güvenlik aracılarının göndermek zorunda kaldığı ileti sayısı IoT Hub'ınıza hızla ulaşabilir veya bunları aşabilir kota ve maliyet limitleri. Ancak, bu olaylar cihazınızı korumak için çok önemli olan son derece değerli güvenlik bilgileri içerir.

Aygıtlarınızı korurken ek kotayı ve maliyetleri azaltmak için, IoT Aracıları için Azure Güvenlik Merkezi bu tür olayları toplar.

Olay toplama varsayılan olarak **Açık** ve önerilmese de, herhangi bir zamanda el ile **kapatılabilir.**

Toplama şu anda aşağıdaki olay türleri için kullanılabilir:
* ProcessCreate
* BağlantıOluşturma
* ProcessTerminate (yalnızca Windows)

## <a name="how-does-event-aggregation-work"></a>Olay toplama nasıl çalışır?
Olay toplama **açık**bırakıldığında, IoT aracıları için Azure Güvenlik Merkezi aralık dönemi veya zaman penceresi için olayları toplar.
Ara verme süresi geçtikten sonra, aracı daha fazla analiz için toplu olayları Azure bulutuna gönderir.
Toplu olaylar, Azure bulutuna gönderilene kadar bellekte depolanır.

Aracının bellek ayak izini azaltmak için, aracı zaten bellekte tutulan bir benzer olay toplar zaman, aracı bu özel olayın isabet sayısını artırır. Toplama süresi penceresi geçtiğinde, aracı, oluşan her belirli olay türünün isabet sayısını gönderir. Olay toplama sadece olay toplanan her tür hit sayıları toplama.

Olaylar yalnızca aşağıdaki koşullar yerine getirildiğinde aynı kabul edilir: 

* ProcessCreate olayları - **commandLine**, **yürütülebilir**, **kullanıcı adı**ve **userid** aynıdır
* ConnectionCreate olaylar - **commandLine**, **userId**, **yön**, **yerel adres,** **uzak adres**, **protokolü ve hedef **bağlantı noktası** aynı olduğunda
* ProcessTerminate olayları - **yürütülebilir** ve **çıkış durumu** aynı olduğunda

### <a name="working-with-aggregated-events"></a>Toplu etkinliklerle çalışma

Toplama sırasında, toplanmayan olay özellikleri atılır ve 0 değeri yle günlük analitiğinde görünür. 
* ProcessCreate olayları - **processId**ve **parentProcessId** 0 olarak ayarlanır
* BağlantıCreate olayları - **processId**ve **kaynak bağlantı noktası** 0 olarak ayarlanır

## <a name="event-aggregation-based-alerts"></a>Olay toplama tabanlı uyarılar 
Analizden sonra, Azure Güvenlik Merkezi IoT şüpheli toplu olaylar için güvenlik uyarıları oluşturur. Toplanan olaylardan oluşturulan uyarılar, her bir toplu olay için yalnızca bir kez görüntülenir.

Her olay için toplama başlangıç zamanı, bitiş saati ve isabet sayısı, soruşturmalar sırasında kullanılmak üzere Log Analytics'teki olay **ExtraDetails** alanında günlüğe kaydedilir. 

Her bir araya yapılan olay, toplanan uyarıların 24 saatlik bir dönemini temsil eder. Her olayın sol üst kısmındaki olay seçenekleri menüsünü kullanarak, her bir toplu olayı tek tek **görevden** alabilirsiniz.    

## <a name="event-aggregation-twin-configuration"></a>Olay toplama ikiz yapılandırma
**Azureiotsecurity** modülünün ikiz kimliğinin [aracı yapılandırma nesnesi](how-to-agent-configuration.md) içinde IoT olay toplama için Azure Güvenlik Merkezi yapılandırmayapılandırmasına değişiklik yapın.

| Yapılandırma adı | Olası değerler | Ayrıntılar | Açıklamalar |
|:-----------|:---------------|:--------|:--------|
| toplamaEnabledProcessCreate | boole | Olay oluşturma etkinliklerini etkinleştirme / devre dışı bırakma |
| toplamaIntervalProcessCreate | ISO8601 Timespan dizesi | Olay oluşturma işlemi için toplama aralığı |
| toplamaEnabledConnectionCreate | boole| Bağlantı oluşturma olayları için olay toplamayı etkinleştirme / devre dışı bırakma |
| toplamaIntervalConnectionCreate | ISO8601 Timespan dizesi | Bağlantı oluşturma olayları için toplama aralığı |
| toplamaEtkinSüreçSon | boole | İşlem sonlandırma olayları için olay toplamayı etkinleştirme / devre dışı bırakma | Yalnızca Windows|
| toplamaIntervalProcessTerminate | ISO8601 Timespan dizesi | İşlem sonlandırma olayları için toplama aralığı | Yalnızca Windows|
|

## <a name="default-configurations-settings"></a>Varsayılan yapılandırma ayarları

| Yapılandırma adı | Varsayılan değerler |
|:-----------|:---------------|
| toplamaEnabledProcessCreate | true |
| toplamaIntervalProcessCreate | "PT1H"|
| toplamaEnabledConnectionCreate | true |
| toplamaIntervalConnectionCreate | "PT1H"|
| toplamaEtkinSüreçSon | true |
| toplamaIntervalProcessTerminate | "PT1H"|
|

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT güvenlik aracısı toplama için Azure Güvenlik Merkezi'ni ve kullanılabilir olay yapılandırma seçeneklerini öğrendiniz.

IoT dağıtımı için Azure Güvenlik Merkezi'ni kullanmaya devam etmek için aşağıdaki makaleleri kullanın:

- [Güvenlik aracısı kimlik doğrulama yöntemlerini](concept-security-agent-authentication-methods.md) anlama
- Bir güvenlik [aracısı](how-to-deploy-agent.md) seçme ve dağıtma
- IoT hizmeti ön [koşulları](service-prerequisites.md) için Azure Güvenlik Merkezi'ni inceleyin
- [IoT Hub'ınızda Azure Güvenlik Merkezi ioT hizmetini](quickstart-onboard-iot-hub.md) nasıl etkinleştirin öğrenin
- [IoT SSS](resources-frequently-asked-questions.md) için Azure Güvenlik Merkezi'nden hizmet hakkında daha fazla bilgi edinin
