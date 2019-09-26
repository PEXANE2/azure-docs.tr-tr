---
title: IoT çözümü için Azure Güvenlik Merkezi 'Ni yapılandırma | Microsoft Docs
description: IoT için Azure Güvenlik Merkezi 'ni kullanarak uçtan uca IoT çözümünüzü nasıl yapılandıracağınızı öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ae2207e8-ac5b-4793-8efc-0517f4661222
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2019
ms.author: mlottner
ms.openlocfilehash: a546d153c6fe4f14ccc8c21308bd4a33385870c3
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299702"
---
# <a name="quickstart-configure-your-iot-solution"></a>Hızlı Başlangıç: IoT çözümünüzü yapılandırma

Bu makalede IoT için Azure Güvenlik Merkezi 'ni kullanarak IoT güvenlik çözümünüzün ilk yapılandırmasını gerçekleştirme hakkında bir açıklama sunulmaktadır. 

## <a name="azure-security-center-for-iot"></a>IoT için Azure Güvenlik Merkezi

IoT için Azure Güvenlik Merkezi, Azure tabanlı IoT çözümleri için kapsamlı uçtan uca güvenlik sağlar.

IoT için Azure Güvenlik Merkezi ile, Azure 'daki tüm IoT cihazlarınızı, IoT platformlarını ve arka uç kaynaklarını tek bir panoda izleyebilirsiniz.

IoT Hub etkinleştirildikten sonra IoT için Azure Güvenlik Merkezi, IoT Hub ve IoT çözümünüz ile ilişkili diğer Azure hizmetlerini otomatik olarak tanımlar.

Otomatik ilişki algılamayı buna ek olarak, IoT çözümünüzün bir parçası olarak hangi diğer Azure kaynak gruplarının etiketleyebileceği de seçim yapabilirsiniz. 

Seçimleriniz, tüm abonelikleri, kaynak grupları veya tek kaynakları eklemenize olanak tanır. 

Tüm kaynak ilişkilerini tanımladıktan sonra IoT için Azure Güvenlik Merkezi, bu kaynaklara yönelik güvenlik önerileri ve uyarılar sağlamak için Azure Güvenlik Merkezi 'Nden yararlanır.

## <a name="add-azure-resources-to-your-iot-solution"></a>IoT çözümünüze Azure kaynakları ekleme

IoT çözümünüze yeni kaynak eklemek için aşağıdakileri yapın: 

1. **IoT Hub** Azure Portal açın. 
1. Sol taraftaki menüden **güvenlik** altında **kaynakları** seçin ve açın. 
1. **Düzenle** ' yi seçin ve IoT çözümünüze ait kaynak gruplarını seçin.
1. **Ekle**'yi tıklatın. 

Tebrikler! IoT çözümünüze yeni bir kaynak grubu eklediniz.

IoT için Azure Güvenlik Merkezi artık yeni eklenen kaynak gruplarınızı izler ve IoT çözümünüzün bir parçası olarak ilgili güvenlik önerilerini ve uyarılarını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik modülleri oluşturmayı öğrenmek için bir sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Güvenlik modülleri oluştur](quickstart-create-security-twin.md)