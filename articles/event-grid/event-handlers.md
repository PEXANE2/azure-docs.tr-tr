---
title: Azure Olay Izgara olay işleyicileri
description: Azure Olay Idamı için desteklenen olay işleyicilerini açıklar. Azure Otomasyonu, Fonksiyonlar, Etkinlik Hub'ları, Karma Bağlantılar, Mantık Uygulamaları, Servis Veri Yol, Sıra Depolama, Webhooks.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 73118823aec9b8d4bd872986b13f19496240c0b9
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393451"
---
# <a name="event-handlers-in-azure-event-grid"></a>Azure Olay Kılavuz'undaki olay işleyicileri

Olay işleyicisi, olayın gönderildiği yerdir. İşleyici olayı işlemek için başka bir eylem de alır. Bazı Azure hizmetleri olayları işlemek için otomatik olarak yapılandırılır. Olayları işlemek için herhangi bir WebHook'u da kullanabilirsiniz. Etkinliklerle başa çıkmak için WebHook'un Azure'da barındırılması gerekmez. Olay Grid yalnızca HTTPS WebHook uç noktalarını destekler.

Bu makalede, her olay işleyicisi için içeriğe bağlantılar sağlar.

## <a name="azure-automation"></a>Azure Otomasyonu

Olayları otomatik runbook'larla işlemek için Azure Otomasyonu'nu kullanın.

|Başlık  |Açıklama  |
|---------|---------|
|[Öğretici: Olay Izgarave Microsoft Ekipleri ile Azure Otomasyonu](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen sanal bir makine oluşturun. Olay, sanal makineyi etiketleyen bir Otomasyon runbook'u tetikler ve microsoft teams kanalına gönderilen bir iletiyi tetikler. |

## <a name="azure-functions"></a>Azure İşlevleri

Olaylara sunucusuz yanıt vermek için Azure Işlevleri'ni kullanın.

İşleyici olarak Azure İşlevleri’ni kullanırken, genel HTTP tetikleyicileri yerine Event Grid tetikleyicisini kullanın. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md#webhook-event-delivery) uygulamanız gerekir.

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Olayları işlevle işleme](custom-event-to-function.md) | İşlenmek üzere bir işleve özel bir olay gönderir. |
| [Azure İşlevler için Olay Izgara tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) | İşlevlerde Olay Izgara tetikleyicisinin kullanılmasına genel bakış. |
| [Öğretici: Olay Izgara'yı kullanarak yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirin](resize-images-on-storage-blob-upload-event.md) | Kullanıcılar görüntüleri web uygulaması üzerinden depolama hesabına yükler. Bir depolama blob oluşturulduğunda, Olay Grid yüklenen görüntüyü yeniden boyutlandıran işlev uygulamasına bir olay gönderir. |
| [Öğretici: Büyük verileri veri ambarına aktarın](event-grid-event-hubs-integration.md) | Olay Hub'ları bir Yakalama dosyası oluşturduğunda, Olay Grid bir etkinlik uygulamasını işlev uygulamasına gönderir. Uygulama Yakalama dosyasını alır ve verileri bir veri ambarına geçirir. |
| [Öğretici: Azure Hizmet Veri Servisi'ni Azure Olay Izgara tümleştirme örneklerine](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay Grid, Service Bus konusundan işlev uygulamasına ve mantık uygulamasına ileti gönderir. |

## <a name="event-hubs"></a>Event Hubs

Çözümünüz olayları işleyebilir daha hızlı aldığında Olay Hub'larını kullanın. Başvurunuz, Olay Hub'larından gelen olayları kendi zamanında işler. Gelen olayları işlemek için olay işleme ölçeklendirebilirsiniz.

Olay Hub'ları olay kaynağı veya olay işleyicisi olarak hareket edebilir. Aşağıdaki makalede, Olay Hub'larının işleyici olarak nasıl kullanılacağı gösterilmektedir.

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure CLI ve Olay Izgarası ile özel etkinlikleri Azure Etkinlik Hub'larına yönlendirin](custom-event-to-eventhub.md) | Özel bir olayı bir uygulama tarafından işlenmek üzere olay merkezine gönderir. |
| [Kaynak Yöneticisi şablonu: özel konu ve Olay Hub'ları bitiş noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Özel bir konu için abonelik oluşturan Kaynak Yöneticisi şablonu. Etkinlikleri bir Azure Etkinlik Hub'larına gönderir. |

## <a name="hybrid-connections"></a>Karma Bağlantılar

Kurumsal ağ içinde bulunan ve herkese açık bir bitiş noktası olmayan uygulamalara olay göndermek için Azure Röle Karma Bağlantıları'nı kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: etkinlikleri karma bağlantıya gönderme](custom-event-to-hybrid-connection.md) | Dinleyici uygulaması tarafından işlenmek üzere varolan karma bağlantıya özel bir olay gönderir. |

## <a name="logic-apps"></a>Logic Apps

Olaylara yanıt vermek için iş süreçlerini otomatikleştirmek için Logic Apps'ı kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Olay Izgara ve Mantık Uygulamaları ile sanal makine değişikliklerini izleyin](monitor-virtual-machine-changes-event-grid-logic-app.md) | Bir mantık uygulaması sanal makinedeki değişiklikleri izler ve bu değişikliklerle ilgili e-postalar gönderir. |
| [Öğretici: Logic Apps'ı kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderin](publish-iot-hub-events-to-logic-apps.md) | Bir mantık uygulaması, IoT hub'ınıza her cihaz ekleninher bir bildirim e-postası gönderir. |
| [Öğretici: Azure Hizmet Veri Servisi'ni Azure Olay Izgara tümleştirme örneklerine](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay Grid, Service Bus konusundan işlev uygulamasına ve mantık uygulamasına ileti gönderir. |

## <a name="service-bus"></a>Service Bus

### <a name="service-bus-queues"></a>Service Bus kuyrukları

Olay Izgara'daki olayları doğrudan Arabelleğe Alma'da kullanılmak üzere Servis Veri Yolu kuyruklarına yönlendirebilir veya kurumsal uygulamalarda komut & denetim senaryoları.

Azure portalında, etkinlik aboneliği oluştururken bitiş noktası türü olarak "Servis Veri Hizmeti Sırası"nı seçin ve ardından Servis Veri Servisi kuyruğunu seçmek için "bir bitiş noktası seçin" seçeneğini tıklayın.

#### <a name="using-cli-to-add-a-service-bus-queue-handler"></a>Servis Veri Çiş sıra işleyicisi eklemek için CLI kullanma

Azure CLI için aşağıdaki örnek abone oluyor ve bir Hizmet Veri Servisi kuyruğuna olay ızgarası konusunu bağlar:

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
    --name <my-event-subscription> \
    --source-resource-id /subscriptions/{SubID}/resourceGroups/{RG}/providers/Microsoft.EventGrid/topics/topic1 \
    --endpoint-type servicebusqueue \
    --endpoint /subscriptions/{SubID}/resourceGroups/TestRG/providers/Microsoft.ServiceBus/namespaces/ns1/queues/queue1
```

### <a name="service-bus-topics"></a>Servis Veri Günü konuları

Hizmet Veri Yolu konularıyla Azure sistem olaylarını işlemek için veya komut & kontrol ileti senaryoları için Event Grid'deki olayları doğrudan Hizmet Veri Yolu konularına yönlendirebilirsiniz.

Azure portalında, etkinlik aboneliği oluştururken bitiş noktası türü olarak "Servis Veri Aracı Konusu"nu seçin ve ardından Bir Hizmet Veri Aracı konusu seçmek için "seç ve bitiş noktası"nı tıklatın.

#### <a name="using-cli-to-add-a-service-bus-topic-handler"></a>Servis Veri Çöse konu işleyicisi eklemek için CLI kullanma

Azure CLI için aşağıdaki örnek abone oluyor ve bir Hizmet Veri Servisi kuyruğuna olay ızgarası konusunu bağlar:

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
    --name <my-event-subscription> \
    --source-resource-id /subscriptions/{SubID}/resourceGroups/{RG}/providers/Microsoft.EventGrid/topics/topic1 \
    --endpoint-type servicebustopic \
    --endpoint /subscriptions/{SubID}/resourceGroups/TestRG/providers/Microsoft.ServiceBus/namespaces/ns1/topics/topic1
```

## <a name="queue-storage"></a>Kuyruk Depolama

Çekilmesi gereken olayları almak için Sıra depolama alanını kullanın. Yanıtlanması çok uzun süren uzun bir işlem olduğunda Kuyruk depolama alanını kullanabilirsiniz. Uygulamayı sıra depolama alanına olay göndererek olayları kendi zamanlamasıyla çekebilir ve işleyebilir.

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure CLI ve Olay Izgarası ile özel olayları Azure Kuyruğu depolama alanına yönlendirin](custom-event-to-queue-storage.md) | Özel olayların Sıra depolama alanına nasıl gönderilebildiğini açıklar. |

## <a name="webhooks"></a>WebHooks

Olaylara yanıt veren özelleştirilebilir uç noktalar için web hooks kullanın.

|Başlık  |Açıklama  |
|---------|---------|
| Quickstart: oluşturmak ve özel olaylar rota - [Azure CLI](custom-event-quickstart.md), [PowerShell](custom-event-quickstart-powershell.md), ve [portal](custom-event-quickstart-portal.md). | WebHook'a özel olayların nasıl gönderilebildiğini gösterir. |
| Quickstart: Ile özel bir web bitiş noktasına Blob depolama olayları rota - [Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json), [PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json), ve [portal](blob-event-quickstart-portal.md). | Blob depolama olaylarının WebHook'a nasıl gönderilebildiğini gösterir. |
| [Quickstart: konteyner kayıt defteri olayları gönderme](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Kapsayıcı Kayıt Defteri etkinliklerini göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |
| [Genel bakış: olayları bir HTTP bitiş noktasına alma](receive-events.md) | Olay Aboneliği'nden olayları almak ve olayları almak ve diziden arındırmak için bir HTTP bitiş noktasını nasıl doğrulayabilmek için açıklanır. |

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
