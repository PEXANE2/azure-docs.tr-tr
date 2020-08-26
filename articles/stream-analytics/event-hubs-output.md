---
title: Azure Stream Analytics çıkışı Event Hubs
description: Bu makalede, verilerin Azure Stream Analytics Azure Event Hubs 'a nasıl çıktısının yapılacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: d18d4aa4bf9306bcdd667faa53f0d888c090e2fd
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88875944"
---
# <a name="event-hubs-output-from-azure-stream-analytics"></a>Azure Stream Analytics çıkışı Event Hubs

[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) hizmeti, yüksek düzeyde ölçeklenebilir bir yayınla-abone ol olay alma olayıdır. Saniyede milyonlarca olay toplayabilirler. Bir olay hub 'ının çıkış olarak bir kullanımı, bir Stream Analytics işinin çıktısının başka bir akış işinin girişi haline geldiği zaman olur. En büyük ileti boyutu ve toplu işlem boyutu iyileştirmesi hakkında daha fazla bilgi için bkz. [çıktı toplu iş boyutu](#output-batch-size) bölümü.

## <a name="output-configuration"></a>Çıkış yapılandırması

Aşağıdaki tabloda, Olay Hub 'larından çıkış olarak veri akışlarını yapılandırmak için gereken parametreler bulunur.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgu çıkışını bu olay hub 'ına yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Olay hub’ı ad alanı | Bir mesajlaşma varlıkları kümesi için kapsayıcı. Yeni bir olay hub 'ı oluşturduğunuzda bir olay hub 'ı ad alanı da oluşturmuş olursunuz. |
| Olay hub'ı adı | Olay Hub 'ınız çıktısının adı. |
| Olay Hub 'ı ilke adı | Olay Hub 'ının **Yapılandır** sekmesinde oluşturabileceğiniz paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin adı, sizin ayarladığınız izinler ve anahtarlara erişim vardır. |
| Olay Hub 'ı ilke anahtarı | Olay Hub 'ı ad alanına erişimin kimliğini doğrulamak için kullanılan paylaşılan erişim anahtarı. |
| Bölüm anahtarı sütunu | İsteğe bağlı. Olay Hub 'ı çıkışı için bölüm anahtarını içeren bir sütun. |
| Olay serileştirme biçimi | Çıkış verileri için serileştirme biçimi. JSON, CSV ve avro desteklenir. |
| Encoding | CSV ve JSON için, UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| Sınırlayıcı | Yalnızca CSV serileştirme için geçerlidir. Stream Analytics CSV biçimindeki verileri serileştirmek için bir dizi ortak sınırlayıcıları destekler. Desteklenen değerler virgül, noktalı virgül, boşluk, sekme ve dikey çubuklardır. |
| Biçimlendir | Yalnızca JSON serileştirme için geçerlidir. **Satır ayrımı** , çıktının her bir JSON nesnesine yeni bir satırla ayrılmış şekilde biçimlendirildiğini belirtir. **Satır ayrılmış**' i SEÇERSENIZ, JSON tek seferde bir nesne okur. Tüm içerik geçerli bir JSON değil. **Dizi** , çıktının JSON nesneleri dizisi olarak biçimlendirildiğini belirtir.  |
| Özellik sütunları | İsteğe bağlı. Yük yerine giden iletinin Kullanıcı özellikleri olarak eklenmesi gereken virgülle ayrılmış sütunlar. Bu özellik hakkında daha fazla bilgi, [Çıkış Için özel meta veri özellikleri](#custom-metadata-properties-for-output)bölümünde bulunur. |

## <a name="partitioning"></a>Bölümleme

Bölümleme, Bölüm hizalamasına göre farklılık gösterir. Olay Hub 'ı çıkışı için bölüm anahtarı, yukarı akış (önceki) sorgu adımıyla eşit olarak hizalandığında, yazıcı sayısı, Olay Hub 'ı çıkışındaki bölüm sayısıyla aynıdır. Her yazıcı, olayları belirli bir bölüme göndermek için [Eventhubsender sınıfını](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) kullanır. Olay Hub 'ı çıkışı için bölüm anahtarı, yukarı akış (önceki) sorgu adımıyla hizalanmazsa, yazıcı sayısı önceki adımdaki bölüm sayısıyla aynıdır. Her yazıcı, olayları tüm çıkış bölümlerine göndermek için **Eventhubclient** Içindeki [sendbatchasync sınıfını](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) kullanır. 

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

İleti başına en büyük ileti boyutu 256 KB veya 1 MB 'tır. Daha fazla bilgi için bkz. [Event Hubs sınırları](../event-hubs/event-hubs-quotas.md). Giriş/çıkış bölümleme hizalı olmadığında, her olay ayrı ayrı paketlenmiştir `EventData` ve en fazla ileti boyutuna bir toplu iş olarak gönderilir. [Özel meta veri özellikleri](#custom-metadata-properties-for-output) kullanılıyorsa bu da olur. Giriş/çıkış bölümlendirme hizalandığında, birden çok olay tek bir `EventData` örneğe paketlenmiştir, en fazla ileti boyutuna kadar ve gönderilir.

## <a name="custom-metadata-properties-for-output"></a>Çıkış için özel meta veri özellikleri

Sorgu sütunlarını, giden iletilerinize Kullanıcı özellikleri olarak ekleyebilirsiniz. Bu sütunlar yük içine gitmez. Özellikler, çıkış iletisindeki bir sözlük biçiminde bulunur. *Anahtar* , sütun adı ve *değer* Özellikler sözlüğündeki sütun değeridir. Tüm Stream Analytics veri türleri, kayıt ve dizi dışında desteklenir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)
