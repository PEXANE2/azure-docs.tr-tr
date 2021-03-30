---
title: include dosyası
description: include dosyası
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: f738daab7ddcf0403f546e7c9ffeaeccb66bc6b7
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "91355661"
---
## <a name="create-an-azure-signalr-service-instance"></a>Azure SignalR Hizmeti örneği oluşturma

Uygulamanız Azure’da bir SignalR hizmeti örneğine bağlanır.

1. Azure portalın sol üst köşesinde bulunan Yeni düğmesini seçin. Yeni ekranda arama kutusuna *SignalR hizmeti* yazın ve Enter tuşuna basın.

    ![Ekran görüntüsünde, Azure portal SignalR hizmeti araması gösterilmektedir.](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Arama sonuçlarından **SignalR Hizmeti**’ni seçtikten sonra **Oluştur**’u seçin.

1. Aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Yeni SignalR Hizmeti örneğinizi tanımlayan ad. Geçerli karakterler: `a-z`, `0-9` ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Yeni SignalR Hizmeti örneğinin oluşturulacağı abonelik. | 
    | **[Kaynak Grubu](../../azure-resource-manager/management/overview.md)** |  myResourceGroup | SignalR Hizmeti örneğinizin oluşturulacağı yeni kaynak grubunun adı. | 
    | **Konum** | Batı ABD | Size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Fiyatlandırma katmanı** | Ücretsiz | Azure SignalR Hizmetini ücretsiz deneyin. |
    | **Birim sayısı** |  Uygulanamaz | Birim sayısı, SignalR Hizmeti örneğinizin kaç bağlantı kabul edebileceğini belirtir. Bu yalnızca Standart katmanda yapılandırılabilir. |
    | **Hizmet modu** |  Sunucusuz | Azure Işlevleri veya REST API kullanmak için. |

    ![Ekran görüntüsünde, değer içeren SignalR temel kavramları sekmesi gösterilmektedir.](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. SignalR Hizmeti örneğini dağıtmaya başlamak için **Oluştur**’u seçin.

1. Örnek dağıtıldıktan sonra portalda açın ve ayarlar sayfasını bulun. Azure SignalR hizmetini Azure Işlevleri bağlama veya REST API aracılığıyla kullanıyorsanız, hizmet modu ayarını *sunucusuz* olarak değiştirin. Aksi halde *Klasik* veya *varsayılan* olarak bırakın.