---
title: Azure Event Grid teslimi ve yeniden dene
description: Azure Event Grid olayların nasıl teslim edildiğini ve teslim edilmemiş iletileri nasıl işlediğini açıklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: fe7574d7e17b1763afb2292c15007dd87b056ef1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87087620"
---
# <a name="event-grid-message-delivery-and-retry"></a>İleti teslimini Event Grid ve yeniden deneyin

Bu makalede, teslimin onaylanmaması durumunda Azure Event Grid olayların nasıl işleyeceği açıklanır.

Event Grid dayanıklı teslim sağlar. Her bir abonelik için her iletiyi en az bir kez sunar. Olaylar, her aboneliğin kayıtlı uç noktasına hemen gönderilir. Bir uç nokta bir olayın alındığını kabul etmezse, olayın yeniden denenmesini Event Grid.

## <a name="batched-event-delivery"></a>Toplu olay teslimi

Her bir olayı abonelere ayrı olarak göndermek Event Grid varsayılan değer. Abone tek bir olaya sahip bir dizi alır. Yüksek işleme senaryolarında iyileştirilmiş HTTP performansına yönelik teslimin gönderilmesi için Event Grid yapılandırabilirsiniz.

Toplu teslimin iki ayarı vardır:

* **Toplu işlem başına en fazla olay** sayısı-Event Grid toplu işlem başına teslim edilir. Bu sayı hiçbir zaman aşılmayacak, ancak yayımlama sırasında başka hiçbir olay yoksa daha az olay teslim edilebilir. Daha az etkinlik varsa Batch oluşturmak için olayları gecikmez Event Grid. 1 ile 5.000 arasında olmalıdır.
* Kilobayt cinsinden toplu iş boyutu için **tercih edilen yığın boyutu** (kilobayt cinsinden). En yüksek olaylara benzer şekilde, yayımlama sırasında daha fazla olay yoksa, toplu iş boyutu daha küçük olabilir. Tek bir olay tercih edilen boyuttan daha büyükse bir toplu iş, tercih edilen toplu *iş boyutundan daha* büyük olabilir. Örneğin, tercih edilen boyut 4 KB ise ve 10 KB 'lik bir olay Event Grid 'e itiliyorsa, 10 KB 'lik olay bırakılması yerine kendi toplu işinde de teslim edilir.

Portal, CLı, PowerShell veya SDK 'lar aracılığıyla olay başına abonelik temelinde yapılandırılmış toplu teslim.

### <a name="azure-portal"></a>Azure portal: 
![Toplu teslim ayarları](./media/delivery-and-retry/batch-settings.png)

### <a name="azure-cli"></a>Azure CLI
Bir olay aboneliği oluştururken aşağıdaki parametreleri kullanın: 

- **en fazla etkinlik-toplu** işlem-toplu iş içindeki en fazla olay sayısı. 1 ile 5000 arasında bir sayı olmalıdır.
- **tercih edilen-kilobayt** olarak tercih edilen toplu işlem boyutu kilobayt olarak tercih edilir. 1 ile 1024 arasında bir sayı olmalıdır.

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

Azure CLı 'yı Event Grid kullanma hakkında daha fazla bilgi için bkz. [Azure CLI ile Depolama olaylarını Web uç noktasına yönlendirme](../storage/blobs/storage-blob-event-quickstart.md).

## <a name="retry-schedule-and-duration"></a>Zamanlamayı ve süreyi yeniden dene

Event Grid bir ileti teslim ettikten sonra yanıt için 30 saniye bekler. 30 saniye sonra, uç nokta yanıt vermediyse ileti yeniden denenmek üzere sıraya alınır. Event Grid, olay teslimi için bir üstel geri alma yeniden deneme İlkesi kullanır. En iyi çaba temelinde aşağıdaki zamanlamaya göre teslimi yeniden Event Grid:

- 10 saniye
- 30 saniye
- 1 dakika
- 5 dakika
- 10 dakika
- 30 dakika
- 1 saat
- 24 saate kadar saatlik

Uç nokta 3 dakika içinde yanıt verirse Event Grid, olayı en iyi çaba temelinde yeniden deneme sırasından kaldırmayı dener, ancak yinelemeler yine de alınabilir.

Event Grid, tüm yeniden deneme adımlarına küçük bir rastgele seçim ekler ve bir uç nokta sürekli sağlıksız, uzun bir süre boyunca azaltılamadığında ya da yoğun bir şekilde göründüğünden belirli yeniden denemeleri atlayabilir mümkün olduğunda olabilir.

Belirleyici davranış için, olay süresini, [abonelik yeniden deneme ilkelerindeki](manage-event-delivery.md)canlı ve en fazla teslimat denemesine ayarlayın.

Varsayılan olarak Event Grid, 24 saat içinde teslim edilmeyen tüm olayları sona ermez. Olay aboneliği oluştururken [yeniden deneme ilkesini özelleştirebilirsiniz](manage-event-delivery.md) . Maksimum teslim denemesi sayısı (varsayılan değer 30) ve olay yaşam süresi (varsayılan değer 1440 dakikadır) sağlarsınız.

## <a name="delayed-delivery"></a>Gecikmeli teslim

Bir uç nokta teslim hatalarıyla karşılaşıyorsa, Event Grid bu uç noktada olayların teslimini ve yeniden denenmesini geciktirmeyi dener. Örneğin, bir uç noktada yayımlanan ilk 10 olay başarısız olursa Event Grid, uç noktanın sorun yaşadığını varsayar ve sonraki tüm yeniden denemeleri *ve yeni* teslimler için birkaç saate kadar bir süre sonra geciktirecek olur.

Gecikmeli teslimin işlevsel amacı, sağlıksız uç noktaların yanı sıra Event Grid sistemini de koruyasağlamaktır. Arka arkaya ve sağlıklı uç noktalara teslim olmadan, Event Grid yeniden deneme ilkesi ve birim özellikleri bir sistemi kolayca açabilir.

## <a name="dead-letter-events"></a>Atılacak mektup olayları
Event Grid belirli bir süre içinde veya olayı belirli bir sayıda teslim etmeye çalıştıktan sonra, teslim edilmemiş olayı bir depolama hesabına gönderebilir. Bu işlem, **atılacak**olarak bilinir. **Aşağıdaki koşullardan biri** karşılandığında bir olay atılacak Event Grid. 

- Etkinlik, yaşam süresi aralığında teslim edilmemiş
- Olayı teslim etmeye yönelik denemeler sayısı sınırı aştı

Koşullardan biri karşılanıyorsa, olay bırakılır veya atılacak.  Event Grid, varsayılan olarak, atılacak bir duruma getirin. Bunu etkinleştirmek için, olay aboneliğini oluştururken teslim edilmemiş olayları barındıracak bir depolama hesabı belirtmeniz gerekir. Teslimleri çözümlemek için bu depolama hesabından olay çekebilirsiniz.

Event Grid, tüm yeniden deneme girişimlerini denediği zaman atılacak harf konumuna bir olay gönderir. Event Grid bir 400 (Hatalı Istek) veya 413 (varlık çok büyük Istek) yanıt kodu alırsa, olayı hemen atılacak ileti uç noktasına gönderir. Bu yanıt kodları olayın teslimini belirtir hiçbir şekilde başarısız olur.

Son bir olay teslim girişimi ve atılacak ileti konumuna teslim edildiğinde oluşan beş dakikalık bir gecikme vardır. Bu gecikme, BLOB depolama işlemlerinin sayısını azaltmaya yöneliktir. Atılacak ileti konumu dört saat kullanılamaz durumdaysa, olay bırakılır.

Atılacak mektup konumunu ayarlamadan önce, kapsayıcısı olan bir depolama hesabınız olmalıdır. Olay aboneliği oluştururken bu kapsayıcı için uç noktayı sağlarsınız. Uç nokta şu biçimdedir:`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Atılacak mektup konumuna bir olay gönderildiğinde bildirim almak isteyebilirsiniz. Teslim edilmemiş olaylara yanıt vermek için Event Grid kullanmak için, atılacak ileti blobu depolaması için [bir olay aboneliği oluşturun](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) . Atılacak ileti BLOB depolama alanı teslim edilmemiş bir olay aldığında Event Grid işleyicisine bildirir. İşleyici, teslim edilmemiş olayları uzlaştırmak için almak istediğiniz eylemlerle yanıt verir.

Geçersiz bir harf konumu ayarlamaya ilişkin bir örnek için, bkz. [atılacak mektup ve yeniden deneme ilkeleri](manage-event-delivery.md).

## <a name="message-delivery-status"></a>İleti teslimi durumu

Event Grid, olayların alındığını bildirmek için HTTP yanıt kodlarını kullanır. 

### <a name="success-codes"></a>Başarı kodları

Event Grid, **yalnızca** aşağıdaki http yanıt kodlarını başarılı teslimatlar olarak değerlendirir. Diğer tüm durum kodları başarısız teslimler olarak değerlendirilir ve uygun şekilde yeniden denenecek veya devredilecek. Başarılı bir durum kodu aldıktan sonra, teslimi tamamlandı olarak kabul eder Event Grid.

- 200 TAMAM
- 201 oluşturuldu
- 202 kabul edildi
- 203 yetkili olmayan bilgiler
- 204 Içerik yok

### <a name="failure-codes"></a>Hata kodları

Yukarıdaki küme içinde olmayan diğer tüm kodlar (200-204) başarısızlık olarak kabul edilir ve yeniden denenecek. Bazıları aşağıda özetlenen özel yeniden deneme ilkelerine sahiptir ve tüm diğerleri standart üstel geri ödeme modelini izler. Event Grid mimarisinin mimarisinden kaynaklanan, yeniden deneme davranışının belirleyici olmadığından emin olmak önemlidir. 

| Durum kodu | Yeniden deneme davranışı |
| ------------|----------------|
| 400 Hatalı İstek | 5 dakika veya daha uzun bir süre sonra yeniden dene (sahipsiz ayarla ayarı varsa hemen |
| 401 Yetkisiz | 5 dakika veya daha uzun bir süre sonra yeniden deneyin |
| 403 Yasak | 5 dakika veya daha uzun bir süre sonra yeniden deneyin |
| 404 Bulunamadı | 5 dakika veya daha uzun bir süre sonra yeniden deneyin |
| 408 İstek Zaman Aşımı | 2 dakika veya daha uzun bir süre sonra yeniden deneyin |
| 413 istek varlığı çok büyük | 10 saniye veya daha kısa bir süre sonra yeniden dene (sahipsiz kurulum yoksa hemen |
| 503 Hizmet Kullanılamıyor | 30 saniye veya daha uzun bir süre sonra yeniden deneyin |
| Tüm diğerleri | 10 saniye veya daha fazla süre sonra yeniden deneyin |


## <a name="next-steps"></a>Sonraki adımlar

* Olay tesliminin durumunu görüntülemek için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Olay teslimi seçeneklerini özelleştirmek için, bkz. [atılacak mektup ve yeniden deneme ilkeleri](manage-event-delivery.md).
