---
title: Azure Olay Ağı etkinlik kaynakları
description: Azure abonelikleri, Konteyner Kayıt Defteri, özel konular, Olay Hub'ları, IoT Hub, Key Vault, Medya Hizmetleri, kaynak grupları, Servis Veri, Depolama, Haritalar, Uygulama Yapılandırması, Sinyal R, Makine Öğrenimi.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: spelluru
ms.openlocfilehash: d4a426ea1432d0266b7ae9344afefe8ddac1d030
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265005"
---
# <a name="event-sources-in-azure-event-grid"></a>Azure Olay Idamı'ndaki olay kaynakları

Olay kaynağı, olayın gerçekleştiği yerdir. Çeşitli Azure hizmetleri, olayları göndermek için otomatik olarak yapılandırılır. Ayrıca, olay gönderen özel uygulamalar da oluşturabilirsiniz. Olay dağıtımı için Olay Izgarasını kullanmak için azure'da özel uygulamaların barındırılması gerekmez.

Bu makalede, her olay kaynağı için içeriğe bağlantılar sağlar.

## <a name="azure-subscriptions"></a>Azure abonelikleri

Azure aboneliğindeki kaynaklardaki değişikliklere yanıt vermek için Azure Abonelikleri etkinliklerine abone olun.

|Başlık |Açıklama  |
|---------|---------|
| [Öğretici: Olay Izgarave Microsoft Ekipleri ile Azure Otomasyonu](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen sanal bir makine oluşturun. Olay, sanal makineyi etiketleyen bir Otomasyon runbook'u tetikler ve microsoft teams kanalına gönderilen bir iletiyi tetikler. |
| [Nasıl: portal üzerinden etkinliklere abone](subscribe-through-portal.md) | Azure aboneliği için etkinliklere abone olmak için portalı kullanın. |
| [Azure CLI: Azure aboneliği için etkinliklere abone olun](./scripts/event-grid-cli-azure-subscription.md) |Azure aboneliği için Olay Izgara aboneliği oluşturan ve olayları Bir WebHook'a gönderen örnek komut dosyası. |
| [PowerShell: Azure aboneliği için etkinliklere abone olun](./scripts/event-grid-powershell-azure-subscription.md)| Azure aboneliği için Olay Izgara aboneliği oluşturan ve olayları Bir WebHook'a gönderen örnek komut dosyası. |
| [Olay şeması](event-schema-subscriptions.md) | Azure abonelik etkinliklerindeki alanları gösterir. |

## <a name="container-registry"></a>Container Kayıt Defteri

Görüntülerdeki değişikliklere yanıt vermek için Konteyner Kayıt Defteri olaylarına abone olun.

|Başlık |Açıklama  |
|---------|---------|
| [Quickstart: konteyner kayıt defteri olayları gönderme](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Kapsayıcı Kayıt Defteri etkinliklerini göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |
| [Olay şeması](event-schema-container-registry.md) | Kapsayıcı Kayıt Defteri olaylarındaki alanları gösterir. |

## <a name="custom-topics"></a>Özel konu başlıkları

Uygulama olaylarına yanıt vermek için özel konulara abone olun.

|Başlık  |Açıklama  |
|---------|---------|
| [Quickstart: Azure CLI ile özel etkinlikler oluşturun ve yönlendirin](custom-event-quickstart.md) | Özel etkinlikler göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |
| [Quickstart: Azure PowerShell ile özel etkinlikler oluşturun ve yönlendirin](custom-event-quickstart-powershell.md) | Özel etkinlikler göndermek için Azure PowerShell'in nasıl kullanılacağını gösterir. |
| [Quickstart: Azure portalı ile özel etkinlikler oluşturun ve yönlendirin](custom-event-quickstart-portal.md) | Özel olaylar göndermek için portalın nasıl kullanılacağını gösterir. |
| [Hızlı başlangıç: Özel olayları Azure Sıra depolama alanına yönlendirin](custom-event-to-queue-storage.md) | Özel olayların Sıra depolama alanına nasıl gönderilebildiğini açıklar. |
| [Nasıl: özel konuya göndermek](post-to-custom-topic.md) | Bir olayı özel bir konuya nasıl yayınlayabildiğini gösterir. |
| [Azure CLI: Olay Ağı özel konu oluşturma](./scripts/event-grid-cli-create-custom-topic.md)|Özel bir konu oluşturan örnek komut dosyası. Komut dosyası bitiş noktasını ve anahtarı alır.|
| [Azure CLI: Özel bir konu için etkinliklere abone olun](./scripts/event-grid-cli-subscribe-custom-topic.md)|Özel bir konu için abonelik oluşturan örnek komut dosyası. Olayları Bir WebHook'a gönderir.|
| [PowerShell: Olay Izgara özel konu oluşturmak](./scripts/event-grid-powershell-create-custom-topic.md)|Özel bir konu oluşturan örnek komut dosyası. Komut dosyası bitiş noktasını ve anahtarı alır.|
| [PowerShell: özel bir konu için etkinliklere abone](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Özel bir konu için abonelik oluşturan örnek komut dosyası. Olayları Bir WebHook'a gönderir.|
| [Kaynak Yöneticisi şablonu: özel konu ve WebHook bitiş noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Özel bir konu ve bu özel konu için abonelik oluşturan bir Kaynak Yöneticisi şablonu. Olayları Bir WebHook'a gönderir. |
|
| [Kaynak Yöneticisi şablonu: özel konu ve Olay Hub'ları bitiş noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Özel bir konu için abonelik oluşturan Kaynak Yöneticisi şablonu. Etkinlikleri bir Azure Etkinlik Hub'larına gönderir. |
| [Olay şeması](event-schema.md) | Özel olaylardaki alanları gösterir. |

## <a name="event-hubs"></a>Event Hubs

Dosya olaylarını yakalamaya yanıt vermek için Olay Hub'larına abone olun. Olay Hub'ları olay kaynağı veya olay işleyicisi olarak hareket edebilir. Aşağıdaki makaleler, Olay Hub'larının kaynak olarak nasıl kullanılacağını gösterir.

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: Büyük verileri veri ambarına aktarın](event-grid-event-hubs-integration.md) | Olay Hub'ları bir Yakalama dosyası oluşturduğunda, Olay Grid bir etkinlik uygulamasını işlev uygulamasına gönderir. Uygulama Yakalama dosyasını alır ve verileri bir veri ambarına geçirir. |
| [Olay şeması](event-schema-event-hubs.md) | Olay Hub'ları etkinliklerindeki alanları gösterir. |

Olay Hub'larının işleyici olarak örnekleri için, [bkz.](event-handlers.md#event-hubs)

## <a name="iot-hub"></a>IoT Hub

Oluşturulan, silinen, bağlanan, bağlantısı kesilen ve telemetri olaylarına yanıt vermek için IoT Hub olaylarına abone olun.

|Başlık  |Açıklama  |
|---------|---------|
| [Logic Apps kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme](publish-iot-hub-events-to-logic-apps.md) | Bir mantık uygulaması, IoT Hub'ınıza her cihaz ekleninher bir bildirim e-postası gönderir. |
| [Eylemleri tetiklemek için Olay Izgarasını kullanarak IoT Hub olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md) | IoT Hub'ı Olay Izgarasıyla tümleştirmeye genel bakış. |
| [Olay şeması](event-schema-iot-hub.md) | IoT Hub etkinliklerindeki alanları gösterir. |
| [Aygıta bağlı ve aygıt bağlantısı kesilmiş olayları sipariş edin](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Aygıt bağlantısı durum olaylarının nasıl sıralanın yapılacağını gösterir. |

## <a name="key-vault-preview"></a>Anahtar Vault (önizleme)

Olay Grid ile Key Vault tümleştirmesi şu anda önizlemede. 

Bir sırrın süresi dolmak üzereyken, bir sırrın süresi dolduğunda veya bir sırrın yeni bir sürümü olduğunda bildirilecek Anahtar Kasa olaylarına abone olun. 

|Başlık  |Açıklama  |
|---------|---------|
| [Azure Olay Ağıt'ı ile Anahtar Kasa etkinliklerini izleme](../key-vault/event-grid-overview.md) | Key Vault'u Olay Izgarasıyla tümleştirmeye genel bakış. |
| [Öğretici: Olay Izgarası ile Key Vault etkinliklerini oluşturun ve izleyin](../key-vault/event-grid-tutorial.md) | Key Vault için Olay Ağı bildirimlerini nasıl ayarlayatılamayı öğrenin. |
| [Olay şeması](event-schema-key-vault.md) | Key Vault etkinliklerindeki alanları gösterir. |

## <a name="media-services"></a>Media Services

İş durumu etkinliklerine yanıt vermek için Medya Hizmetleri etkinliklerine abone olun.

|Başlık  |Açıklama  |
|---------|---------|
| [Genel bakış: Medya Hizmetleri olaylarına tepki](../media-services/latest/reacting-to-media-services-events.md) | Medya Hizmetlerini Olay Ağıla bütünleştirmeye genel bakış. |
| [Öğretici: Azure Medya Hizmetleri olaylarını CLI kullanarak özel bir web bitiş noktasına yönlendirin](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Medya Hizmetlerinden nasıl etkinlik gönderilir gösterir. |
| [Olay şeması](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Medya Hizmetleri etkinliklerindeki alanları gösterir. |

## <a name="resource-groups"></a>Kaynak grupları

Kaynak grubundaki kaynaklardaki değişikliklere yanıt vermek için kaynak grubu etkinliklerine abone olun.

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Olay Izgara ve Mantık Uygulamaları ile sanal makine değişikliklerini izleyin](monitor-virtual-machine-changes-event-grid-logic-app.md) | Bir mantık uygulaması sanal makinedeki değişiklikleri izler ve bu değişikliklerle ilgili e-postalar gönderir. |
| [Azure CLI: kaynak grubu için etkinliklere abone olun](./scripts/event-grid-cli-resource-group.md)| Kaynak grubu için olaylara abone olan örnek komut dosyası. Olayları Bir WebHook'a gönderir. |
| [Azure CLI: kaynak grubu için etkinliklere abone olun ve kaynak için filtre uygulayın](./scripts/event-grid-cli-resource-group-filter.md) | Kaynak grubu için olaylara abone olan ve olayları tek bir kaynak için filtreleyen örnek komut dosyası. |
| [PowerShell: bir kaynak grubu için etkinliklere abone olun](./scripts/event-grid-powershell-resource-group.md) | Kaynak grubu için olaylara abone olan örnek komut dosyası. Olayları Bir WebHook'a gönderir. |
| [PowerShell: bir kaynak grubu için etkinliklere abone olun ve kaynak için filtre uygulayın](./scripts/event-grid-powershell-resource-group-filter.md) | Kaynak grubu için olaylara abone olan ve olayları tek bir kaynak için filtreleyen örnek komut dosyası. |
| [Kaynak Yöneticisi şablonu: kaynak aboneliği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Azure aboneliği veya kaynak grubu için etkinliklere abone olur. Olayları Bir WebHook'a gönderir. |
| [Olay Şeması](event-schema-resource-groups.md) | Kaynak grubu etkinliklerindeki alanları gösterir. |

## <a name="service-bus"></a>Service Bus

Etkin bir dinleyici olmadan iletilere yanıt vermek için Servis Veri Servisi etkinliklerine abone olun.

|Başlık  |Açıklama  |
|---------|---------|
| [Öğretici: Azure Hizmet Veri Servisi'ni Azure Olay Izgara tümleştirme örneklerine](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay Grid, Service Bus konusundan işlev uygulamasına ve mantık uygulamasına ileti gönderir. |
| [Genel Bakış: Azure Servis Veri Servisi'ni Olay Izgara tümleştirmesine](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Hizmet Veri Veri Tos'u Olay Izgarasıyla tümleştirmeye genel bakış. |
| [Olay şeması](event-schema-service-bus.md) | Servis Veri Servisi etkinliklerindeki alanları gösterir. |

## <a name="storage"></a>Depolama

Oluşturulan ve silinen olaylara yanıt vermek için Blob Depolama etkinliklerine abone olun.

>[!NOTE]
> Yalnızca depolama türü **StorageV2 (genel amaçlı v2)** ve **BlobStorage** destek olay tümleştirmedepolama hesapları. **Depolama (genel amaçlı v1)** Olay Grid ile tümleştirme *desteklemez.*

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Blob depolama olaylarını Azure CLI ile özel bir web bitiş noktasına yönlendirin](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Blob depolama olaylarını WebHook'a göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |
| [Quickstart: PowerShell ile Blob depolama olaylarını özel bir web bitiş noktasına yönlendirin](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Blob depolama olaylarını WebHook'a göndermek için Azure PowerShell'in nasıl kullanılacağını gösterir. |
| [Quickstart: Azure portalı ile Blob depolama etkinlikleri oluşturun ve yönlendirin](blob-event-quickstart-portal.md) | Blob depolama olaylarını WebHook'a göndermek için portalın nasıl kullanılacağını gösterir. |
| [Azure CLI: Blob depolama hesabı için etkinliklere abone olun](./scripts/event-grid-cli-blob.md) | Blob depolama hesabı için etkinliğe abone olan örnek komut dosyası. Olayı bir WebHook'a gönderir. |
| [PowerShell: Blob depolama hesabı için etkinliklere abone olun](./scripts/event-grid-powershell-blob.md) | Blob depolama hesabı için etkinliğe abone olan örnek komut dosyası. Olayı bir WebHook'a gönderir. |
| [Kaynak Yöneticisi şablonu: Blob depolama alanı ve abonelik oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) | Bir Azure Blob depolama hesabı dağıtır ve o depolama hesabı için olaylara abone olur. Olayları Bir WebHook'a gönderir. |
| [Genel bakış: Blob depolama olaylarına tepki](../storage/blobs/storage-blob-event-overview.md) | Blob depolamayı Event Grid ile tümleştirmeye genel bakış. |
| [Olay şeması](event-schema-blob-storage.md) | Blob Depolama olaylarındaki alanları gösterir. |

## <a name="maps"></a>Haritalar
Coğrafi çit olaylarına yanıt vermek için Azure Haritalar etkinliklerine abone olun. Örneğin, bir uygulama, bir aygıt bir geofence'e her girdiğinde veya bir çitle her girdiğinde bir e-posta bildirimi sunabilir.

|Başlık  |Açıklama  |
|---------|---------|
| [Etkinlik Ağıt'ı kullanarak Azure Haritalar etkinliklerine tepki verme](../azure-maps/azure-maps-event-grid-integration.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure Haritalarını Olay Ağıtı ile tümleştirmeye genel bakış. |
| [Öğretici: Geofence'i ayarlama](../azure-maps/tutorial-geofence.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Bu öğretici, Azure Haritalar'ı kullanarak coğrafi çit oluşturmanız için temel adımlardan geçmenizi sağlar. Geofence sonuçlarını akış ve geofence sonuçlarına dayalı bir bildirim ayarlamak için Azure Event Grid'i kullanırsınız. |
| [Olay şeması](event-schema-azure-maps.md) | Azure Haritalar etkinliklerindeki alanları gösterir. |

## <a name="app-configuration"></a>Uygulama Yapılandırması
Anahtar değer değişikliği olaylarına yanıt vermek için Azure Uygulama Yapılandırma etkinliklerine abone olun.

|Başlık | Açıklama |
|---------|---------|
| [Olay Kılavuz'u kullanarak Azure Uygulama Yapılandırma olaylarına tepki verme](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure Uygulama Yapılandırması'nı Olay Ağıla tümleştirmeye genel bakış. |
| [Hızlı başlangıç: Azure Uygulama Yapılandırma olaylarını Azure CLI ile özel bir web bitiş noktasına yönlendirin](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure Uygulama Yapılandırma olaylarını Bir WebHook'a göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |
| [Olay şeması](event-schema-app-configuration.md) | Azure Uygulama Yapılandırma etkinliklerindeki alanları gösterir. |

## <a name="azure-signalr"></a>Azure SignalR
İstemci bağlantı olaylarına yanıt vermek için Azure SignalR Hizmeti etkinliklerine abone olun.

|Başlık | Açıklama |
|---------|---------|
| [Olay Kılavuz'u kullanarak Azure SignalR Hizmeti etkinliklerine tepki verme](../azure-signalr/signalr-concept-event-grid-integration.md) | Azure SignalR Hizmetini Olay Ağıt'ı ile tümleştirmeye genel bakış. |
| [Azure SignalR Hizmeti etkinlikleri Etkinlik Ağılasına nasıl gönderilir?](../azure-signalr/signalr-howto-event-grid-integration.md) | Azure SignalR Hizmeti etkinliklerinin Bir uygulamaya Nasıl Gönderilebildiğini Event Grid aracılığıyla gösterir. |
| [Olay şeması](event-schema-azure-signalr.md) | Azure SignalR Hizmeti etkinliklerindeki alanları gösterir. |

## <a name="azure-machine-learning"></a>Azure Machine Learning

Model kaydına yanıt vermek için Azure Machine Learning çalışma alanı etkinliklerine abone olun.

| Başlık | Açıklama |
| ----- | ----- |
| [Azure Machine Learning etkinliklerini tüketin](../machine-learning/concept-event-grid-integration.md) | Azure Machine Learning'i Olay Ağı'yla tümleştirmeye genel bakış. |
| [Azure Makine Öğrenimi için Azure Olay Izgara etkinliği şeması](event-schema-machine-learning.md) | Azure Machine Learning etkinliklerindeki alanları gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
