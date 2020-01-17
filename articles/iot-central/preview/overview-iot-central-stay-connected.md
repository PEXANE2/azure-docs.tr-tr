---
title: Azure IoT Central cihaz bağlantısı | Microsoft Docs
description: Bu makalede, Azure IoT Central cihazlarınızı bağlı ve sağlıklı tutmaya nasıl yardımcı olduğu hakkında bir Özet sağlanmaktadır.
author: aaronbjork
ms.author: abjork
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: ''
ms.openlocfilehash: 7c102e7ffde7faab5d864c1d9acaafe141664ebf
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76119559"
---
# <a name="stay-connected-with-azure-iot-central-preview-features"></a>Azure IoT Central bağlı kalın (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu makalede, Azure IoT Central cihazlarınızın bağlı ve sağlıklı kalmasına nasıl yardımcı olduğu hakkında genel bir bakış sunulmaktadır.

Ölçek düzeyinde çalışacak şekilde tasarlanan tüm IoT çözümlerinin yanı sıra, cihaz yönetimi için iyi yapılandırılmış bir yaklaşım önemlidir. Buluta "bağlı" cihazları almak yeterli değildir; çözümünüz geliştikçe, büyürken ve yaşışdıkça cihazları bağlı ve sağlıklı bir şekilde tutmaya yönelik bir yaklaşım gerekir. Azure IoT Central, IoT çözümünüzdeki cihazların uygulama yaşam döngüsü boyunca iyi bir şekilde gezildiğinden emin olmak için gerekli özellikleri sağlar.

## <a name="dashboards"></a>Panolar 
Yerleşik [panolar](howto-manage-devices.md#import-devices) cihaz sistem durumunu ve telemetrisini izlemek için özelleştirilebilir bir yüzey alanı sağlar. [Uygulama şablonundan](howto-use-app-templates.md) önceden oluşturulmuş bir panoyla başlayın veya uygulamanızın gereksinimlerine uyarlanmış kendi panolarınızı oluşturun. Panolar uygulamanızdaki tüm kullanıcılarla paylaşılabilir veya özel olarak saklanır.

## <a name="rules-and-actions"></a>Kurallar ve eylemler 
Cihaz durumu ve telemetri temelinde [özel kurallar](tutorial-create-telemetry-rules.md) oluşturarak ilgilenilmesi gereken cihazları hızlıca belirleyebilirsiniz. Doğru kişilere, düzeltici ölçülerin zamanında gerçekleşmesini sağlamak için Eylemler yapılandırın.

## <a name="jobs"></a>İş 
[İşler](../core/howto-run-a-job.md?toc=/azure/iot-central/preview/toc.json&bc=/azure/iot-central/preview/breadcrumb/toc.json) cihaz özelliklerine, ayarlarına ve komutlarına yönelik tek veya toplu güncelleştirmeler gerçekleştirmenize olanak sağlar. 

## <a name="user-roles-and-permissions"></a>Kullanıcı rolleri ve izinleri
[Roller ve izinler](howto-manage-users-roles.md) , her bir kullanıcının deneyimini uyarlamanızı sağlar. Rolleri doğrudan kullanıcı arabiriminden oluşturun ve uygun izinleri kolayca atayın. 




