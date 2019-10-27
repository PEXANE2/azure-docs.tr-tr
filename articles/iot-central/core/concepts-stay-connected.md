---
title: Azure IoT Central cihaz bağlantısı | Microsoft Docs
description: Bu makalede, Azure IoT Central cihazlarınızı bağlı ve sağlıklı tutmaya nasıl yardımcı olduğu hakkında bir Özet sağlanmaktadır.
author: aaronbjork
ms.author: abjork
ms.date: 10/21/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: ''
ms.openlocfilehash: 7ce5ba37763bc78740f5f07f860fdc4b620b474a
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72954662"
---
# <a name="stay-connected-with-azure-iot-central"></a>Azure IoT Central bağlı kalın

Bu makalede, Azure IoT Central cihazlarınızın bağlı ve sağlıklı kalmasına nasıl yardımcı olduğu hakkında genel bir bakış sunulmaktadır.

Ölçek düzeyinde çalışacak şekilde tasarlanan tüm IoT çözümlerinin yanı sıra, cihaz yönetimi için iyi yapılandırılmış bir yaklaşım önemlidir. Cihazları "bağlı" duruma getirmek yeterli değildir, ancak çözümünüz geliştikçe, büyürken ve yaşışdıkça cihazları bağlı ve sağlıklı bir şekilde tutmaya yönelik bir yaklaşıma ihtiyacınız vardır. Azure IoT Central, IoT çözümünüzdeki cihazların uygulama yaşam döngüsü boyunca iyi bir şekilde gezildiğinden emin olmak için gerekli özellikleri sağlar.

## <a name="dashboards"></a>Panolar 
Yerleşik [panolar](howto-manage-devices.md#import-devices) cihaz sistem durumunu ve telemetrisini izlemek için özelleştirilebilir bir yüzey alanı sağlar. [Uygulama şablonundan](howto-use-app-templates-pnp.md) önceden oluşturulmuş bir panoyla başlayın veya uygulamanızın gereksinimlerine uyarlanmış kendi panolarınızı oluşturun. Panolar uygulamanızdaki tüm kullanıcılarla paylaşılabilir veya özel olarak saklanır.

## <a name="rules-and-actions"></a>Kurallar ve eylemler 
Cihaz durumu ve telemetri temelinde [özel kurallar](howto-create-event-rules.md) oluşturarak ilgilenilmesi gereken cihazları hızlıca belirleyebilirsiniz. Doğru kişilere, düzeltici ölçülerin zamanında gerçekleşmesini sağlamak için [Eylemler](howto-create-event-rules.md#configure-actions) yapılandırın.

## <a name="jobs"></a>İş 
[İşler](howto-run-a-job.md) cihaz özelliklerine, ayarlarına ve komutlarına yönelik tek veya toplu güncelleştirmeler gerçekleştirmenize olanak sağlar. 

## <a name="user-roles-and-permissions"></a>Kullanıcı rolleri ve izinleri
[Roller ve izinler](howto-manage-users-roles-pnp.md) , her bir kullanıcının deneyimini uyarlamanızı sağlar. Rolleri doğrudan kullanıcı arabiriminden oluşturun ve uygun izinleri kolayca atayın. 




