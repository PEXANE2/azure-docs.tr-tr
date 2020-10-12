---
title: Azure RTOS için Güvenlik Modülü API’si
description: Azure RTOS için güvenlik modülü için başvuru API 'SI.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: 8191d68f701a3be6a37d8623d2d900f6aa6c1851
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941730"
---
# <a name="security-module-for-azure-rtos-api-preview"></a>Azure RTOS API için güvenlik modülü (Önizleme)

Bu API yalnızca Azure RTOS için güvenlik modülüyle kullanılmak üzere tasarlanmıştır. Ek kaynaklar için bkz. [Azure RTOS GitHub kaynağı Için güvenlik modülü](https://github.com/azure-rtos/azure-iot-preview/releases). 

## <a name="enable-security-module-for-azure-rtos"></a>Azure RTOS için güvenlik modülünü etkinleştir

**nx_azure_iot_security_module_enable**

### <a name="prototype"></a>Prototype

```c
UINT nx_azure_iot_security_module_enable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Açıklama

Bu yordam, Azure IoT güvenlik modülü alt sistemini sunar. Bir iç durum makinesi güvenlik olaylarının toplanmasını yönetir ve bunları Azure IoT Hub gönderir. Veri toplamayı yönetmek için yalnızca bir NX_AZURE_IOT_SECURITY_MODULE örneği gerekir ve gereklidir.

### <a name="parameters"></a>Parametreler

| Ad | Açıklama |
|---------|---------|
| nx_azure_iot_ptr [in]    | Bir işaretçisi `NX_AZURE_IOT` .  |

### <a name="return-values"></a>Dönüş değerleri

|Dönüş değerleri  |Açıklama |
|---------|---------|
|NX_AZURE_IOT_SUCCESS|   Azure IoT güvenlik modülü başarıyla etkinleştirildi.     |
|NX_AZURE_IOT_FAILURE   |  Azure IoT güvenlik modülü bir iç hata nedeniyle etkinleştirilemedi.    |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Güvenlik modülü geçerli bir #NX_AZURE_IOT örneği gerektirir.      |

### <a name="allowed-from"></a>İzin verilen

İş Parçacıkları

## <a name="disable-azure-iot-security-module"></a>Azure IoT güvenlik modülünü devre dışı bırak

**nx_azure_iot_security_module_disable**


### <a name="prototype"></a>Prototype

```c
UINT nx_azure_iot_security_module_disable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Açıklama

Bu yordam, Azure IoT güvenlik modülü alt sistemini devre dışı bırakır.

### <a name="parameters"></a>Parametreler

| Ad | Açıklama |
|---------|---------|
| nx_azure_iot_ptr [in]    | Bir işaretçisi `NX_AZURE_IOT` . NULL ise singleton örneği devre dışıdır. |

### <a name="return-values"></a>Dönüş değerleri

|Dönüş değerleri  |Açıklama |
|---------|---------|
|NX_AZURE_IOT_SUCCESS     |   Azure IoT güvenlik modülü başarıyla devre dışı bırakıldığında başarılı olur.      |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Azure IoT Hub örneği, Singleton bileşik örneğinden farklı.       |
|NX_AZURE_IOT_FAILURE    |  Azure IoT güvenlik modülü bir iç hata nedeniyle devre dışı bırakılamadı.       |

### <a name="allowed-from"></a>İzin verilen

İş Parçacıkları


## <a name="next-steps"></a>Sonraki adımlar

Azure RTOS güvenlik modülünü kullanmaya başlama hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- IoT RTOS güvenlik modülüne [genel bakış](iot-security-azure-rtos.md)Için Defender 'ı inceleyin.