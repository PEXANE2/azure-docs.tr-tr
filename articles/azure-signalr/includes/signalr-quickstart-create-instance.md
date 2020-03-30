---
title: include dosyası
description: include dosyası
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 0a228f6d8b4f72acd3783f27bf192fe6bd13f988
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75392137"
---
## <a name="create-an-azure-signalr-service-instance"></a>Azure SignalR Hizmeti örneği oluşturma

Uygulamanız Azure’da bir SignalR hizmeti örneğine bağlanır.

1. Azure portalın sol üst köşesinde bulunan Yeni düğmesini seçin. Yeni ekranda arama kutusuna *SignalR hizmeti* yazın ve Enter tuşuna basın.

    ![SignalR Hizmetini Arama](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Arama sonuçlarından **SignalR Hizmeti**’ni seçtikten sonra **Oluştur**’u seçin.

1. Aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Yeni SignalR Hizmeti örneğinizi tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Yeni SignalR Hizmeti örneğinin oluşturulacağı abonelik. | 
    | **[Kaynak Grubu](../../azure-resource-manager/management/overview.md)** |  myResourceGroup | SignalR Hizmeti örneğinizin oluşturulacağı yeni kaynak grubunun adı. | 
    | **Konum** | Batı ABD | Size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Fiyatlandırma katmanı** | Ücretsiz | Azure SignalR Hizmetini ücretsiz deneyin. |
    | **Birim sayısı** |  Uygulanamaz | Birim sayısı, SignalR Hizmeti örneğinizin kaç bağlantı kabul edebileceğini belirtir. Bu yalnızca Standart katmanda yapılandırılabilir. |
    | **Servis modu** |  Sunucusuz | Azure İşlevler veya REST API ile kullanım için. |

    ![SignalR Hizmeti Oluşturma](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. SignalR Hizmeti örneğini dağıtmaya başlamak için **Oluştur**’u seçin.

1. Örnek dağıtıldıktan sonra portalda açın ve Ayarlar sayfasını bulun. Yalnızca Azure İşlevler bağlama veya REST API aracılığıyla Azure SinyalR Hizmeti kullanıyorsanız Hizmet Modu ayarını *Serverless* olarak değiştirin. Aksi takdirde *Klasik* veya *Varsayılan* olarak bırakın.