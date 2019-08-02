---
title: Azure 'da Nesnelerin İnterneti (IoT) güvenliğini sağlama | Microsoft Docs
description: " Azure Nesnelerin İnterneti (IoT) Hizmetleri, çok çeşitli yetenekler sunmaktadır. Bu makale, Azure 'da IoT çözümlerinizi nasıl güvence altına öğrenmenize yardımcı olur. "
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 670fcc5e92cdb9937026d91ee85b7e8e856a3025
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727041"
---
# <a name="internet-of-things-security-overview"></a>Nesnelerin İnterneti güvenliğe genel bakış
Azure Nesnelerin İnterneti (IoT) Hizmetleri, çok çeşitli yetenekler sunmaktadır. Bu kurumsal sınıf hizmetler şunları yapmanızı sağlar:

* Cihazlardan veri toplama
* Hareket halinde veri akışı çözümleme
* Büyük veri gruplarını depolama ve sorgulama
* Gerçek zamanlı ve geçmiş verileri görselleştirme
* Arka ofis sistemleriyle tümleştirme

Bu özellikleri sunmak için, Azure IoT Çözüm Hızlandırıcıları, önceden yapılandırılmış çözümler olarak özel uzantılara sahip birden çok Azure hizmetini birlikte sunar. Bu önceden yapılandırılmış çözümler, IoT çözümlerinizi sunmak için geçirmeniz gereken süreyi azaltmak üzere genel IoT çözümü düzenlerinin temel uygulamalarıdır. IoT yazılım geliştirme setlerini kullanarak, kendi gereksinimlerinizi karşılamak için bu çözümleri özelleştirebilir ve genişletebilirsiniz. Ayrıca bu çözümleri, yeni IoT çözümleri geliştirirken örnekler ya da şablonlar olarak da kullanabilirsiniz.

Azure IoT Çözüm Hızlandırıcıları, IoT gereksinimlerinize yönelik güçlü çözümdür. Ancak, IoT çözümlerinizi başlangıçtan itibaren göz önünde bulundurularak tasarlanan en önemli öneme sahiptir. IoT cihazlarının çok sayıda olması nedeniyle, herhangi bir güvenlik olayı önemli sonuçlarla hızlı bir şekilde yaygın bir olay haline gelebilir.

IoT çözümlerinizi nasıl güvenli hale getirmenize anlamanıza yardımcı olmak için aşağıdaki bilgileri sunuyoruz.

## <a name="security-architecture"></a>Güvenlik mimarisi
Bir sistem tasarlarken, bu sisteme yönelik olası tehditleri anlamak ve sistem tasarlandıkça ve uygun hale getirebileceği için uygun savunma savunmaları eklemek önemlidir. Bir saldırganın bir sistemin güvenliğinin nasıl tehlikeye alamadığını anlamak için, ürünü güvenlikle birlikte Başlat ' dan tasarlamak önemlidir.

[Nesnelerin interneti güvenlik mimarisini](/azure/iot-fundamentals/iot-security-architecture)okuyarak IoT güvenlik mimarisi hakkında bilgi edinebilirsiniz.

Bu makalede, aşağıdaki konular ele alınmaktadır:

* [Güvenlik, tehdit modeliyle başlar](/azure/iot-fundamentals/iot-security-architecture#security-starts-with-a-threat-model)
* [IoT 'de güvenlik](/azure/iot-fundamentals/iot-security-architecture#security-in-iot)
* [Azure IoT başvuru mimarisi için tehdit modelleme](/azure/iot-fundamentals/iot-security-architecture)

## <a name="security-from-the-ground-up"></a>Baştan sona güvenlik
IoT, dünyanın dört bir yanındaki işletmeler için benzersiz güvenlik, gizlilik ve uyumluluk sorunları doğurur. Bu sorunların yazılım etrafında ve nasıl uygulandıklarını gösteren geleneksel siber teknolojisinin aksine, IoT ve fiziksel çalışma LDS yakınlarından ne olur? IoT çözümlerini korumak, cihazların güvenli şekilde sağlanması, bu cihazlar ile bulut arasında güvenli bağlantı sağlanması ve işlem ve depolama sırasında buluttaki veri korumasının güvenliğini sağlamayı gerektirir. Bununla birlikte, bu işlevselliğe karşı çalışma, kaynak kısıtlı cihazlardır, dağıtımların coğrafi dağılımı ve bir çözüm içindeki birçok cihazdan oluşur.

[Baştan sona nesnelerin interneti güvenliği](/azure/iot-fundamentals/iot-security-ground-up)okuyarak bu alanlardaki güvenliği nasıl ele alabileceğinizi öğrenebilirsiniz.

Makalede aşağıdaki konular ele alınmaktadır:

* [Baştan sona güvenli altyapı](/azure/iot-fundamentals/iot-security-ground-up#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure – işletmeniz için güvenli IoT altyapısı](/azure/iot-fundamentals/iot-security-ground-up#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>En İyi Uygulamalar
IoT altyapısının güvenliğini sağlamak, ayrıntılı bir güvenlik kapsamlı strateji gerektirir. Bulutta verilerin güvenliğini sağlama, genel İnternet üzerinden aktarım sırasında veri bütünlüğünü koruma, cihazları güvenli bir şekilde sağlamak için her katman genel altyapıda daha fazla güvenlik güvencesi oluşturur.

Nesnelerin İnterneti en iyi [güvenlik uygulamalarını](/azure/iot-fundamentals/iot-security-best-practices)okuyarak nesnelerin interneti en iyi güvenlik uygulamaları hakkında bilgi edinebilirsiniz.

Makalede aşağıdaki konular ele alınmaktadır:

* [IoT donanım üreticisi/tümleştirici](/azure/iot-fundamentals/iot-security-best-practices#iot-hardware-manufacturerintegrator)
* [IoT çözüm geliştiricisi](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-developer)
* [IoT çözüm dağıtıcı](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-deployer)
* [IoT çözüm operatörü](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-operator)
