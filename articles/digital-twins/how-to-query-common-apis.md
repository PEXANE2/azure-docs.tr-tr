---
title: Ortak sorgu desenleri - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins yönetim API'leri için çeşitli yaygın API sorgu desenleri öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 133c0e0dcc07afb85a0f3af9ae51d2207abac293
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589122"
---
# <a name="how-to-query-azure-digital-twins-apis-for-common-tasks"></a>Ortak görevler için Azure Digital Twins API'leri nasıl sorgulanır?

Bu makalede, Azure Dijital İkizler örneğiniz için sık karşılaşılan senaryoları yürütmenize yardımcı olacak sorgu desenleri gösterilmektedir. Bu, Dijital İkizler örneğinizin zaten çalıştığını varsayar. Postman gibi herhangi bir REST istemcisi kullanabilirsiniz. 

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]


## <a name="queries-for-spaces-and-types"></a>Boşluklar ve türler için sorgular

Bu bölümde, sağlanan alanlar hakkında daha fazla bilgi almak için örnek sorgular gösterilmektedir. Örnek sorgularla kimlik doğrulaması alın HTTP isteklerini yapın ve yer tutucuları kurulumunuzdaki değerlerle değiştirin. 

- Kök düğümleri olan boşluklar alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?$filter=ParentSpaceId eq null
    ```

- Ada göre bir alan alın ve aygıtları, sensörleri, hesaplanmış değerleri ve sensör değerlerini ekleyin. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=Focus Room A1&includes=fullpath,devices,sensors,values,sensorsvalues
    ```

- Boşlukları ve cihaz/sensör bilgilerini alın, ebeveyni verilen boşluk kimliğidir ve [verilen alana göre](how-to-navigate-apis.md#api-navigation)2-5 seviyelerdedir. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?spaceId=YOUR_SPACE_ID&includes=fullpath,devices,sensors,values,sensorsvalues&traverse=Down&minLevel=1&minRelative=true&maxLevel=5&maxRelative=true
    ```

- Verilen kimlikle alanı alın ve hesaplanmış ve sensör değerlerini ekleyin.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?ids=YOUR_SPACE_ID&includes=Values,sensors,SensorsValues
    ```

- Belirli bir alan için özellik anahtarlarını alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/propertykeys?spaceId=YOUR_SPACE_ID
    ```

- *AreaInSqMeters* adlı özellik anahtarı ile boşluk alın ve değeri 30'dur. Dize işlemleri de yapabilirsiniz, örneğin, özellik anahtarı `name = X contains Y`içeren boşluklar olsun.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?propertyKey=AreaInSqMeters&propertyValue=30
    ```

- Adı *Sıcaklık* ve ilişkili bağımlılıkları ve ontolojileri ile tüm adları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/types?names=Temperature&includes=space,ontologies,description,fullpath
    ```


## <a name="queries-for-roles-and-role-assignments"></a>Roller ve rol atamaları için sorgular

Bu bölümde, roller ve atamaları hakkında daha fazla bilgi almak için bazı sorgular gösterilmektedir. 

- Azure Digital Twins tarafından desteklenen tüm rolleri alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```

- Dijital İkizler örneğinde tüm rol ödevlerini alın. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=down
    ```

- Belirli bir yolda rol atamaları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/A_SPATIAL_PATH
    ```

## <a name="queries-for-devices"></a>Aygıtlar için sorgular

Bu bölümde, aygıtlarınız hakkında belirli bilgiler almak için Yönetim API'lerini nasıl kullanabileceğinize ilişkin bazı örnekler gösterilmektedir. Tüm API çağrılarının kimlik doğrulaması HTTP isteklerini almalısınız.

- Tüm cihazları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices
    ```

- Tüm aygıt durumlarını bulun.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/devices/statuses
    ```

- Belirli bir cihaz alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID
    ```

- Tüm aygıtları kök alanına bağla.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?maxLevel=1
    ```

- 2 ile 4 arasında olan seviyelerdeki boşluklara bağlı tüm aygıtları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4
    ```

- Tüm aygıtları doğrudan belirli bir uzay kimliğine iliştirin.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID
    ```

- Tüm aygıtları belirli bir alana ve onun soyundan gelenlere iliştirin.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down
    ```

- Bu alan hariç, bir alanın torunlarına bağlı tüm aygıtları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true
    ```

- Bir alanın doğrudan çocuklarına bağlı tüm aygıtları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true
    ```

- Bir alanın atalarından birine bağlı tüm cihazları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Up&maxLevel=-1&maxRelative=true
    ```

- 5'ten daha küçük veya eşit düzeyde bir alanın torunlarına bağlı tüm aygıtları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&maxLevel=5
    ```

- Kimlik YOUR_SPACE_ID olan alanla aynı düzeydeki boşluklara bağlı tüm *aygıtları*alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true
    ```

- Aygıtınız için IoT Hub aygıt bağlantı dizesini alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID?includes=ConnectionString
    ```

- Bağlı sensörler de dahil olmak üzere verilen donanım kimliğine sahip cihazı alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?hardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=sensors
    ```

- Bu durumda *Hareket* ve *Sıcaklık,* belirli veri türleri için sensörler alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/sensors?dataTypes=Motion,Temperature
    ```

## <a name="queries-for-matchers-and-user-defined-functions"></a>Eşleştirme ve kullanıcı tanımlı işlevler için sorgular 

- Tüm uygun eşleşenleri ve kimliklerini alın.

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers
    ```

- Belirli bir eşleştirme hakkında, onunla ilişkili boşluklar ve kullanıcı tanımlı işlev de dahil olmak üzere ayrıntıları alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID?includes=description, conditions, fullpath, userdefinedfunctions, space
    ```

- Bir şimileyiciyi sensöre göre değerlendirin ve hata ayıklama amacıyla günlüğe kaydetmeyi etkinleştirin. Bu HTTP GET iletisinin dönüşü, eşleştirme ve sensörün veri türüne ait olup olmadığını bildirir. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID/evaluate/YOUR_SENSOR_ID?enableLogging=true
    ```

- Kullanıcı tanımlı işlevlerin kimliğini alın. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions
    ```

- Belirli bir kullanıcı tanımlı işlevin içeriğini alma 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions/YOUR_USER_DEFINED_FUNCTION_ID/contents
    ```


## <a name="queries-for-users"></a>Kullanıcılar için sorgular

Bu bölümde, Azure Digital Twins'teki kullanıcıları yönetmek için bazı örnek API sorguları gösterilmektedir. Yer tutucuları kurulumunuzdaki değerlerle değiştirmek için bir HTTP GET isteğinde bulunun. 

- Tüm kullanıcıları alın. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users
    ```

- Belirli bir kullanıcı alın.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users/ANY_USER_ID
    ```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim API'nizle kimlik doğrulaması yapmayı öğrenmek için [API'lerle kimlik doğrulama'yı](./security-authenticating-apis.md)okuyun.

API uç noktalarınız hakkında daha fazla bilgi edinmek [için Digital Twins Swagger'ı nasıl kullanacağınızı](./how-to-use-swagger.md)okuyun.
