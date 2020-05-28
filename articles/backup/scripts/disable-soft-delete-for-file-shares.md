---
title: Betik örneği-dosya paylaşımının geçici silme özelliğini devre dışı bırak
description: Bir depolama hesabındaki dosya paylaşımları için geçici silmeyi devre dışı bırakmak üzere bir komut dosyası kullanmayı öğrenin.
ms.topic: sample
ms.date: 02/02/2020
ms.openlocfilehash: 172a7bd19ffd07ca7900a457c447362343e5f185
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122799"
---
# <a name="disable-soft-delete-for-file-shares-in-a-storage-account"></a>Depolama hesabındaki dosya paylaşımları için geçici silmeyi devre dışı bırak

Bu belgede, bir depolama hesabındaki dosya paylaşımları için geçici silme devre dışı bırakma işlemi açıklanmaktadır.

Şu adımları uygulayın:

1. Armclient 'ı yükler. Yüklemeyi öğrenmek için [Bu bağlantıyı](https://github.com/projectkudu/ARMClient)ziyaret edin.

2. Aşağıdaki iki istek gövdesi dosyasını makinenizde bir klasöre kaydedin.

    ```json
    rqbody-enableSoftDelete.json

    {
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled":true,
        "days": 14
        }
    },
    "cors": {
        "corsRules": []
    }
    }

    rqbody-disableSoftDelete.json

    {
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled":false,
        "days": 0
        }
    },
    "cors": {
        "corsRules": []
    }
    }
    ```

3. Depolama hesabı Azure Resource Manager (ARM) KIMLIĞINIZI kullanışlı tutun. Örneğin, `/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare/providers/Microsoft.Storage/storageAccounts/inquirytest`

4. **Armclient oturumunu**çalıştırarak kimlik bilgilerinizi kullanarak oturum açın.

5. Depolama hesabındaki dosya paylaşımlarının geçerli geçici silme özelliklerini alın.

    Aşağıdaki GET işlemi, *inquirytest* hesabındaki dosya paylaşımlarının geçici silme özelliklerini getirir:

    ```cmd
    armclient get /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01
    ```

    ```output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/de
    fault",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "cors": {
        "corsRules": []
        },
        "shareDeleteRetentionPolicy": {
        "enabled": true,
        "days": 14
        }
    }
    }
    ```

6. Depolama hesabındaki dosya paylaşımları için geçici silmeyi devre dışı bırakın.

    Aşağıdaki PUT işlemi, *inquirytest* hesabındaki dosya paylaşımlarının geçici silme özelliklerini devre dışı bırakır:

    ```cmd
    armclient put /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01 .\rqbody-disableSoftDelete.json
    ```

    ```Output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/de
    fault",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled": false,
        "days": 0
        }
    }
    }
    ```

7. Geçici silmeyi yeniden etkinleştirmek istiyorsanız aşağıdaki örneği kullanın.

    Aşağıdaki PUT işlemi, "inquirytest" hesabındaki dosya paylaşımlarının geçici silme özelliklerini sunar.

    ```cmd
    armclient put /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01 .\rqbody-EnableSoftDelete.json
    ```

    ```Output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled": true,
        "days": 14
        }
    }
    }
    ```
