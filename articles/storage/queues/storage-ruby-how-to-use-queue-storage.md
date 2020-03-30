---
title: Ruby'den Sıra depolama nasıl kullanılır - Azure Depolama
description: Kuyrukoluşturmak ve silmek ve ileti eklemek, almak ve silmek için Azure Kuyruk hizmetini nasıl kullanacağınızı öğrenin. Ruby'de yazılmış örnekler.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/08/2016
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: c7211bc805f4ed1d026faedbfdc9d53d3c1dfd93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68721274"
---
# <a name="how-to-use-queue-storage-from-ruby"></a>Ruby’den Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Microsoft Azure Sıra Depolama hizmetini kullanarak sık karşılaşılan senaryoları nasıl gerçekleştireceklerini gösterir. Örnekler Ruby Azure API kullanılarak yazılır.
Kapsanan senaryolar arasında kuyruk iletileri **ekleme,** **gözetleme,** **alma**ve **silme** nin yanı sıra **kuyruk oluşturma ve silme**yer alır.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby Uygulaması Oluşturma
Bir Ruby uygulaması oluşturun. Talimatlar için [bkz.](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby)

## <a name="configure-your-application-to-access-storage"></a>Uygulamanızı Access Depolamasına Yapılandırın
Azure depolamaalanını kullanmak için, depolama REST hizmetleriyle iletişim kuran bir dizi kolaylık kitaplığı içeren Ruby azure paketini indirmeniz ve kullanmanız gerekir.

### <a name="use-rubygems-to-obtain-the-package"></a>Paketi edinmek için RubyGems kullanma
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın.
2. Mücevheri ve bağımlılıkları yüklemek için komut penceresine "mücevher yükleme azure" yazın.

### <a name="import-the-package"></a>Paketi içeri aktarma
En sevdiğiniz metin düzenleyicisini kullanın, depolama alanını kullanmak istediğiniz Ruby dosyasının üst bölümüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Azure Depolama Bağlantısı Kurma
Azure modülü, Azure **\_depolama hesabınıza\_** bağlanmak için gereken bilgiler için azure depolama hesabı ve **AZURE\_DEPOLAMA\_ACCESS_KEY** ortam değişkenlerini okur. Bu ortam değişkenleri ayarlanmazsa, **Azure::QueueService'i** kullanmadan önce hesap bilgilerini aşağıdaki kodu içeren belirtmeniz gerekir:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

Bu değerleri Azure portalında bir klasik veya Kaynak Yöneticisi depolama hesabından edinmek için:

1. [Azure portalına](https://portal.azure.com)giriş yapın.
2. Kullanmak istediğiniz depolama hesabına gidin.
3. Sağdaki Ayarlar dikey penceresinde **Erişim Anahtarları**'na tıklayın.
4. Açılan Erişim anahtarları dikey penceresinde, 1. ve 2. erişim anahtarını göreceksiniz. Bunlardan birini kullanabilirsiniz. 
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın. 

## <a name="how-to-create-a-queue"></a>Nasıl Yapılsın: Sıra Oluşturma
Aşağıdaki kod, kuyruklarla çalışmanızı sağlayan bir **Azure:QueueService** nesnesi oluşturur.

```ruby
azure_queue_service = Azure::QueueService.new
```

Belirtilen ada sahip bir sıra oluşturmak için **create_queue()** yöntemini kullanın.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl Alınır: Sıraya İleti Ekleme
Bir iletiyi kuyruğa eklemek için, yeni bir ileti oluşturmak ve kuyruğa eklemek için **create_message()** yöntemini kullanın.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl Yapılsın: Sonraki İletiye Göz At
Sıranın önündeki iletiye, sıranın önündeki **iletiyi peek\_iletileri()** yöntemini çağırarak kuyruktan çıkarmadan göz atabilirsiniz. Varsayılan olarak, **peek\_iletileri()** tek bir iletiye göz atar. Ayrıca, kaç iletiye göz atmak istediğinizi de belirtebilirsiniz.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-the-next-message"></a>Nasıl YapılSın: Sonraki İletiyi Sırayla Çözme
İletiyi iki adımda bir kuyruktan kaldırabilirsiniz.

1. **Liste\_iletilerini()** aradiğinizde, varsayılan olarak bir sonraki iletiyi kuyrukta alırsınız. Ayrıca, kaç ileti almak istediğinizi de belirtebilirsiniz. Liste iletilerinden döndürülen **\_iletiler()** bu kuyruktaki diğer kod okuma iletileri için görünmez olur. Bir parametre olarak saniyeler içinde görünürlük zaman atlamasını geçersiniz.
2. İletiyi kuyruktan kaldırmayı bitirmek için **delete_message()'yi**de aramanız gerekir.

İletiyi kaldırma işlemi, kodunuz donanım veya yazılım hatası nedeniyle bir iletiyi işlemezse, kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kodunuz, ileti işlendikten hemen sonra **delete\_ileti()** çağırır.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl YapılSın: Sıralı İletinin İçeriğini Değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Aşağıdaki kod, bir iletiyi güncelleştirmek için **update_message()** yöntemini kullanır. Yöntem, sıra iletisinin pop makbuzu ve iletinin kuyrukta ne zaman görüneceğini gösteren UTC tarih saati değerini içeren bir tuple döndürür.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Nasıl Yapılsın: İletileri Sıraya Almak için Ek Seçenekler
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.

1. Bir yığın mesaj alabilirsiniz.
2. Daha uzun veya daha kısa bir görünmezlik zaman aşımı ayarlayarak, kodunuzun her iletiyi tam olarak işlemek için daha fazla veya daha az zaman ayırmasına izin verebilirsiniz.

Aşağıdaki kod örneği, tek bir aramada 15 ileti almak için **liste\_iletileri()** yöntemini kullanır. Daha sonra her iletiyi yazdırır ve siler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-the-queue-length"></a>Nasıl Gidilir: Sıra Uzunluğunu Alın
Kuyruktaki ileti sayısının tahminini alabilirsiniz. Sıra **\_\_meta verisi al()** yöntemi, sıra yla ilgili yaklaşık ileti sayısını ve meta verilerini döndürmek için sıra hizmetinden sorar.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Nasıl Alınır: Sırayı Silme
Bir kuyruğ ve içindeki tüm iletileri silmek için, sıra nesnesindeki **sil\_queue()** yöntemini arayın.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Sonraki Adımlar
Artık sıra depolamanın temellerini öğrendiğiniz için, daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* Azure [Depolama Ekibi Blogunu](https://blogs.msdn.com/b/windowsazurestorage/) ziyaret edin
* GitHub'daki Ruby deposu [için Azure SDK'yı](https://github.com/WindowsAzure/azure-sdk-for-ruby) ziyaret edin

Bu makalede tartışılan Azure Kuyruk Hizmeti ile [Hizmet Veri Hizmetleri Kuyrukları](https://azure.microsoft.com/develop/ruby/how-to-guides/service-bus-queues/) makalesinde tartışılan Azure Hizmet Veri Sırası arasında karşılaştırma yapmak için [Azure Kuyrukları ve Hizmet Veri Servisi Kuyrukları - Karşılaştırıldığında ve Zıt](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
