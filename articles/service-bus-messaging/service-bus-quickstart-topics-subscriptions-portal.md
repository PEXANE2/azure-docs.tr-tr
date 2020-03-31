---
title: Hizmet Veri Mes'i konuları ve abonelikleri oluşturmak için Azure portalını kullanın
description: 'Hızlı başlangıç: Bu hızlı başlangıçta, Azure portalını kullanarak bir Hizmet Veri Yolu konusunu ve bu konuya abonelikleri nasıl oluşturabileceğinizi öğrenirsiniz.'
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: a1e330a62a58daaa3fb2a2e8758d14791a3208c1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76260829"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Hızlı başlatma: Hizmet Veri Servisi konusu ve konuya abonelikler oluşturmak için Azure portalını kullanın
Bu hızlı başlangıçta, Bir Hizmet Veri Servisi konusu oluşturmak ve ardından bu konuya abonelikler oluşturmak için Azure portalını kullanırsınız. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Her iletinin tek bir tüketici tarafından işlendiği Servis Veri Servisi sıralarının aksine, konular ve abonelikler yayımlama/abone oltasını kullanarak bire bir iletişim biçimi sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir. Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsteğe bağlı olarak, hangi iletilerin hangi konu abonelikleri tarafından alındığını filtrelemenize veya bir konuyla sınırlamanıza olanak tanıyan, abonelik başına bir konu için filtre kurallarını kaydedebilirsiniz.

Servis Veri Servisi konuları ve abonelikleri, çok sayıda kullanıcı ve uygulama arasında çok sayıda iletiişlemek için ölçeklendirmenize olanak tanır.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Bir konuya ileti göndermeyi ve bu iletileri abonelik yoluyla nasıl alacağınızı öğrenmek için aşağıdaki makaleye bakın: TOC'daki programlama dilini seçin. 

> [!div class="nextstepaction"]
> [İleti yayımlama ve iletilere abone olma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
