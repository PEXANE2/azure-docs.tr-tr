---
title: Azure İşlevler Olay Izgara yerel hata ayıklama
description: Olay Ağı olayı tarafından tetiklenen Azure işlevlerini yerel olarak hata ayıklamayı öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 10/18/2018
ms.author: cshoe
ms.openlocfilehash: 97509001aa66c2c1bf0c91b6b2a5ab25f9d6ec88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74227078"
---
# <a name="azure-function-event-grid-trigger-local-debugging"></a>Azure İşlevi Olay Izgara yerel hata ayıklama tetikleme

Bu makalede, bir depolama hesabı tarafından yükseltilen bir Azure Olay Ağı olayı işleyen yerel bir işlevin nasıl hata ayıklanılacağını gösterin. 

## <a name="prerequisites"></a>Ön koşullar

- Varolan bir işlev uygulaması oluşturma veya kullanma
- Varolan bir depolama hesabı oluşturma veya kullanma
- Azure'un yerel işlevinizi aramasına izin vermek için [ngrok'u](https://ngrok.com/) indirin

## <a name="create-a-new-function"></a>Yeni bir işlev oluşturma

İşlev uygulamanızı Visual Studio'da açın ve Çözüm Gezgini'ndeki proje adına sağ tıklayın ve **Yeni Azure İşi > ekle'ye**tıklayın.

Yeni *Azure İşlevi* penceresinde **Olay Izgara tetikleyicisini** seçin ve **Tamam'ı**tıklatın.

![Yeni işlev oluşturma](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-function.png)

İşlev oluşturulduktan sonra, kod dosyasını açın ve dosyanın üst kısmında yorumlanan URL'yi kopyalayın. Bu konum Olay Izgara tetikleyicisi yapılandırılırken kullanılır.

![Yeri kopyalama](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-copy-location.png)

Daha sonra, ile başlayan satırda `log.LogInformation`bir kesme noktası ayarlayın.

![Kesme noktasını ayarlama](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-set-breakpoint.png)


Ardından, hata ayıklama oturumu başlatmak için **F5 tuşuna basın.**

## <a name="allow-azure-to-call-your-local-function"></a>Azure'un yerel işlevinizi aramasına izin ver

Makinenizde debubasyona sahip bir işleve girmek için, Azure'un yerel işlevinizle buluttan iletişim kurmasını sağlamanın bir yolunu etkinleştirmeniz gerekir.

[ngrok](https://ngrok.com/) yardımcı programı, Azure'un makinenizde çalışan işlevi araması için bir yol sağlar. Aşağıdaki komutu kullanarak *ngrok* başlatın:

```bash
ngrok http -host-header=localhost 7071
```
Yardımcı program ayarlandıkça, komut penceresi aşağıdaki ekran görüntüsüne benzer olmalıdır:

![Ngrok başlat](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-ngrok.png)

*ngrok* çalıştırıldığında oluşturulan **HTTPS** URL'sini kopyalayın. Bu değer, olay ızgarası bitiş noktasını yapılandırırken kullanılır.

## <a name="add-a-storage-event"></a>Depolama olayı ekleme

Azure portalını açın ve bir depolama hesabına gidin ve **Etkinlikler** seçeneğini tıklayın.

![Depolama hesabı olayı ekleme](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-event.png)

*Etkinlikler* penceresinde, **Etkinlik Aboneliği** düğmesini tıklatın. Çift *Abonelik* penceresinde, Bitiş *Noktası Türü* açılır penceresine tıklayın ve **Web Hook'u**seçin.

![Abonelik türünü seçin](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-type.png)

Bitiş noktası türü yapılandırıldıktan sonra, bitiş noktası değerini yapılandırmak için **bitiş noktası seç'e** tıklayın.

![Uç nokta türünü seçin](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint.png)

*Abone Bitiş Noktası* değeri üç farklı değerden oluşur. Önek *ngrok*tarafından oluşturulan HTTPS URL'sidir. URL'nin geri kalanı işlev kodu dosyasında bulunan URL'den gelir ve sonunda işlev adı eklenir. Işlev kodu dosyasından URL ile başlayarak, *ngrok* URL değiştirir `http://localhost:7071` `{functionname}`ve işlev adı değiştirir.

Aşağıdaki ekran görüntüsü, son URL'nin nasıl görünmesi gerektiğini gösterir:

![Bitiş noktası seçimi](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-event-subscription-endpoint-selection.png)

Uygun değeri girdikten sonra **Seçimi Onayla'yı**tıklatın.

> [!IMPORTANT]
> *Ngrok'a*her başladığınızda, HTTPS URL'si yenilenir ve değer değişir. Bu nedenle, işlevinizi *ngrok*aracılığıyla Azure'a her kez maruz kaldığınızda yeni bir Etkinlik Aboneliği oluşturmanız gerekir.

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme

Artık, yerel işlevinizin işlemesi için bir Olay Izgara olayını tetiklemek için depolama hesabınıza bir dosya yükleyebilirsiniz. 

[Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) açın ve depolama hesabınıza bağlanın. 

- **Blob Kapları** Genişlet 
- Sağ tıklayın ve **Blob Konteyner oluştur'u**seçin.
- Kapsayıcı **testini** adlandırın
- *Test* kapsayıcısını seçin
- **Yükle** düğmesini tıklatın
- **Dosyaları Yükle'yi** tıklatın
- Bir dosya seçin ve blob kapsayıcısına yükleyin

## <a name="debug-the-function"></a>İşlevhataayıklama

Olay Izgarası yeni bir dosyanın depolama kapsayıcısına yüklendiğini fark ettikten sonra, kesme noktası yerel işlevinizde vurulur.

![Ngrok başlat](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-breakpoint.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynakları temizlemek için depolama hesabınızdaki **test** kapsayıcısını silin.

## <a name="next-steps"></a>Sonraki adımlar

- [Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme](../event-grid/resize-images-on-storage-blob-upload-event.md)
- [Azure İşlevler için Olay Izgara tetikleyicisi](./functions-bindings-event-grid.md)
