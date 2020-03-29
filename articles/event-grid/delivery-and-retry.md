---
title: Azure Olay Izgara teslimi ve yeniden deneme
description: Azure Olay Idamı'nın olayları nasıl sağladığını ve teslim edilmemiş iletileri nasıl işleyeceğini açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: spelluru
ms.openlocfilehash: dda2fd98c4c0d330059156a5ec00baa97ffaf627
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921071"
---
# <a name="event-grid-message-delivery-and-retry"></a>Olay Grid ileti teslimi ve yeniden deneme

Bu makalede, Azure Olay Idamı teslim onaylanmadığında olayları nasıl işleyebilir açıklanmaktadır.

Olay Izgara dayanıklı teslimat sağlar. Her abonelik için her iletiyi en az bir kez iletin. Olaylar, her aboneliğin kayıtlı bitiş noktasına hemen gönderilir. Bir bitiş noktası bir olayın alındığını kabul etmiyorsa, Olay Ağıt olay teslimini yeniden dener.

## <a name="batched-event-delivery"></a>Toplu etkinlik teslimi

Olay Grid, her olayı abonelere ayrı ayrı göndermek için varsayılan dır. Abone, tek bir olayiçeren bir dizi alır. Yüksek işlem senaryolarında geliştirilmiş HTTP performansı için olay ızgarasını teslim için toplu etkinliklere göre yapılandırabilirsiniz.

Toplu teslimatın iki ayarı vardır:

* **Toplu iş başına maksimum olaylar** - Olay Izgarası toplu iş başına teslim edecek olay sayısı. Bu sayı hiçbir zaman aşılmaz, ancak yayımlama sırasında başka etkinlik yoksa daha az olay teslim edilebilir. Olay Grid, daha az olay varsa toplu iş oluşturmak için olayları geciktirmez. 1 ile 5,000 arasında olmalı.
* **Kilobaytlarda tercih edilen parti boyutu** - Kilobaytlarda parti boyutu için hedef tavan. En büyük olaylara benzer şekilde, yayımlama sırasında daha fazla olay yoksa toplu iş boyutu daha küçük olabilir. Tek *bir* olay tercih edilen boyuttan daha büyükse, toplu iş bir partinin tercih edilen toplu iş boyutundan daha büyük olması mümkündür. Örneğin, tercih edilen boyut 4 KB ise ve 10 KB'lik bir olay Olay Izgara'ya itilirse, 10-KB olayı düşürülmek yerine kendi toplu iş lerinde teslim edilir.

Portal, CLI, PowerShell veya SDK'lar aracılığıyla etkinlik başına abonelik bazında yapılandırılan toplu teslimat.

### <a name="azure-portal"></a>Azure portalı: 
![Toplu teslim ayarları](./media/delivery-and-retry/batch-settings.png)

### <a name="azure-cli"></a>Azure CLI
Olay aboneliği oluştururken aşağıdaki parametreleri kullanın: 

- **toplu iş başına maksimum olay** - Bir toplu işteki maksimum olay sayısı. 1 ile 5000 arasında bir sayı olmalı.
- **tercih edilen toplu-boy-in-kilobayt - Kilobayt** tercih edilen toplu boyut. 1 ile 1024 arasında bir sayı olmalı.

```azurecli
storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)
endpoint=https://$sitename.azurewebsites.net/api/updates

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name <event_subscription_name> \
  --endpoint $endpoint \
  --max-events-per-batch 1000 \
  --preferred-batch-size-in-kilobytes 512
```

Olay Ağıla Azure CLI'yi kullanma hakkında daha fazla bilgi için, [Azure CLI ile web bitiş noktasına depolama etkinliklerini yönlendir'e](../storage/blobs/storage-blob-event-quickstart.md)bakın.

## <a name="retry-schedule-and-duration"></a>Zamanlama ve süreyi yeniden deneyin

Olay Grid bir ileti teslim ettikten sonra bir yanıt için 30 saniye bekler. 30 saniye sonra, bitiş noktası yanıt vermediyse, ileti yeniden denemek için sıraya alınır. Olay Grid olay teslimi için üstel geri leme ilkesi kullanır. Olay Grid en iyi çaba temelinde aşağıdaki zamanlamaya teslim yeniden çalışır:

- 10 saniye
- 30 saniye
- 1 dakika
- 5 dakika
- 10 dakika
- 30 dakika
- 1 saat
- 24 saate kadar saatlik

Bitiş noktası 3 dakika içinde yanıt verirse, Olay Izgarası en iyi çaba temelinde olayı yeniden deneme kuyruğundan kaldırmaya çalışır, ancak yine de yinelenebilir.

Olay Grid tüm yeniden deneme adımları için küçük bir randomizasyon ekler ve fırsatçı bir bitiş noktası sürekli sağlıksız, aşağı uzun bir süre için, ya da boğulmuş gibi görünüyor bazı yeniden atlar.

Deterministik davranış için, [abonelik yeniden deneme ilkelerinde](manage-event-delivery.md)etkinlik saatini yaşamak için ayarlayın ve maksimum teslimat girişimleri.

Varsayılan olarak, Olay Izgara24 saat içinde teslim edilmeyen tüm olayların süresi doluyor. Olay aboneliği oluştururken [yeniden deneme ilkesini özelleştirebilirsiniz.](manage-event-delivery.md) Maksimum teslim denemesi sayısı (varsayılan değer 30'dur) ve etkinlik süresinin canlı olmasını sağlarsınız (varsayılan değer 1440 dakikadır).

## <a name="delayed-delivery"></a>Gecikmiş Teslimat

Bir bitiş noktası teslim hataları deneyimleri gibi, Olay Grid teslim ve bu bitiş noktasına olayların yeniden deneme geciktirmeye başlar. Örneğin, bitiş noktası için yayınlanan ilk 10 olay başarısız olursa, Olay Izgarası bitiş noktasının sorunlar yaşadığını varsayacak ve sonraki tüm yeniden denemeleri *ve yeni* teslimatları bir süre geciktirecek - bazı durumlarda birkaç saate kadar.

Gecikmiş teslimatın işlevsel amacı, sağlıksız uç noktaların yanı sıra Olay Izgara sistemini de korumaktır. Geri tepme ve sağlıksız uç noktalara teslim gecikme olmadan, Olay Grid yeniden deneme ilkesi ve birim yetenekleri kolayca bir sistem ezmek olabilir.

## <a name="dead-letter-events"></a>Ölü harfli olaylar

Olay Izgarabir olay teslim edemediğinde, teslim edilmeyen olayı bir depolama hesabına gönderebilir. Bu işlem ölü harfler olarak bilinir. Varsayılan olarak, Olay Izgara ölü harfleri açmaz. Etkinleştirmek için, olay aboneliği oluştururken teslim edilmemiş olayları tutmak için bir depolama hesabı belirtmeniz gerekir. Teslimatları çözmek için olayları bu depolama hesabından çekersiniz.

Olay Grid, tüm yeniden deneme denemelerini denediğinde ölü harf konumuna bir olay gönderir. Olay Grid 400 (Kötü İstek) veya 413 (İstek Varlık Too Large) yanıt kodu alırsa, olayı hemen çıkmaz adadoğru gönderir. Bu yanıt kodları, olayın tesliminin hiçbir zaman başarılı olameyeceğini gösterir.

Bir olayı teslim etmek için yapılan son girişim ile ölü harf konumuna teslim edilen son girişim arasında beş dakikalık bir gecikme vardır. Bu gecikme, Blob depolama işlemleri sayısını azaltmak için tasarlanmıştır. Ölü harf konumu dört saat süreyle kullanılamıyorsa, olay bırakılır.

Ölü harf konumunu ayarlamadan önce, bir kapsayıcı ile bir depolama hesabınız olmalıdır. Olay aboneliğini oluştururken bu kapsayıcının bitiş noktasını sağlarsınız. Bitiş noktası şu şekildedir:`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Bir olay ölü mektup konumuna gönderildiğinde haberdar olmak isteyebilirsiniz. Teslim edilmeyen olaylara yanıt vermek için Olay Izgarasını kullanmak için, ölü harfli blob depolama alanı için [bir olay aboneliği oluşturun.](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) Ölü harfli blob depolama alanınız her teslim edilmemiş bir olay aldığında, Olay Grid işleyicinizi not alır. İşleyici, teslim edilmeyen olayları uzlaştırmak için yapmak istediğiniz eylemlerle yanıt verir.

Ölü harf konumu ayarlama örneği için, [Ölü harf ve yeniden deneme ilkelerine](manage-event-delivery.md)bakın.

## <a name="message-delivery-status"></a>İleti teslim durumu

Olay Izgarası, olayların alındığını kabul etmek için HTTP yanıt kodlarını kullanır. 

### <a name="success-codes"></a>Başarı kodları

Olay Grid **yalnızca** aşağıdaki HTTP yanıt kodlarını başarılı teslimatlar olarak kabul eder. Diğer tüm durum kodları başarısız teslimatlar olarak kabul edilir ve uygun olarak yeniden denenecek veya deadlettered olacaktır. Başarılı bir durum kodu aldıktan sonra, Olay Grid teslimin tamamlanmış olduğunu düşünür.

- 200 TAMAM
- 201 Oluşturuldu
- 202 Kabul Edildi
- 203 Yetkili Olmayan Bilgiler
- 204 İçerik Yok

### <a name="failure-codes"></a>Hata kodları

Yukarıdaki kümede (200-204) olmayan diğer tüm kodlar hata olarak kabul edilir ve yeniden denenecektir. Bazı özel yeniden deneme politikaları aşağıda özetlenen bağlı, tüm diğerleri standart üstel back-off modeli izleyin. Olay Izgara mimarisinin son derece paralelleştirilmiş doğası nedeniyle, yeniden deneme davranışının belirleyici olmadığını unutmamak gerekir. 

| Durum kodu | Davranışı yeniden deneyin |
| ------------|----------------|
| 400 Kötü İstek | 5 dakika veya daha fazla sonra yeniden deneyin (Deadletter hemen eğer deadletter kurulum) |
| 401 Yetkisiz | 5 dakika veya daha fazla sonra yeniden deneyin |
| 403 Yasak | 5 dakika veya daha fazla sonra yeniden deneyin |
| 404 Bulunamadı | 5 dakika veya daha fazla sonra yeniden deneyin |
| 408 İstek Zaman Aşımı | 2 dakika veya daha fazla sonra yeniden deneyin |
| 413 İstek Varlık Çok Büyük | 10 saniye veya daha fazla sonra yeniden deneyin (Deadletter hemen eğer deadletter kurulum) |
| 503 Hizmet Kullanılamıyor | 30 saniye veya daha fazla bir süre sonra yeniden deneyin |
| Diğer tüm | 10 saniye veya daha fazla bir süre sonra yeniden deneyin |


## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimlerinin durumunu görüntülemek için, [Olay Ağı iletiteslimini izleyin'](monitor-event-delivery.md)e bakın.
* Olay teslim seçeneklerini özelleştirmek için [Dead harfi ve yeniden deneme ilkelerine](manage-event-delivery.md)bakın.
