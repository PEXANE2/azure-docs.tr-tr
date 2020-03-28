---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/04/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: c590b61ee1424d32d83dc5f758682fde37492c3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80057104"
---
1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**'u seçin.

1. **Yeni** **sayfada, İşlem** > **Fonksiyonu Uygulaması'nı**seçin.

1. Temel **Bilgiler** sayfasında, aşağıdaki tabloda belirtildiği gibi işlev uygulaması ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **İşlev Uygulaması adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a-z` (büyük/küçük `0-9`harf `-`duyarsız) ve .  |
    |**Yayımlamak**| Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# işlevleri için **.NET Core'u** seçin. |
    |**Sürüm**| Sürüm numarası | Yüklü çalışma saatinizin sürümünü seçin.  |
    |**Bölge**| Tercih edilen bölge | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    ![Temel Bilgiler](./media/functions-create-function-app-portal/function-app-create-basics.png)

1. Sonraki'ni Seçin **: Hosting**. **Barındırma** sayfasında aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Depolama hesabı](../articles/storage/common/storage-account-create.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları uzunluğu 3 ila 24 karakter arasında olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir. Depolama [hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements)karşılaması gereken varolan bir hesabı da kullanabilirsiniz. |
    |**İşletim sistemi**| Tercih edilen işletim sistemi | Çalışma zamanı yığını seçiminize göre bir işletim sistemi sizin için önceden seçilir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | **[Planlama](../articles/azure-functions/functions-scale.md)** | **Tüketim (Sunucusuz)** | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Varsayılan **Tüketim** planında, kaynaklar işlevlerinizin gerektirdiği şekilde dinamik olarak eklenir. Bu [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) barındırma, yalnızca işlevlerinizin çalıştığı süre için ödeme yapıyor. Bir App Service planı içinde çalıştırdığınızda [işlev uygulamanızın ölçeklendirmesini](../articles/azure-functions/functions-scale.md) yönetmeniz gerekir.  |

    ![Barındırma](./media/functions-create-function-app-portal/function-app-create-hosting.png)

1. Sonraki'ni Seçin **: İzleme**. **İzleme** sayfasında aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Uygulama Öngörüleri](../articles/azure-functions/functions-monitoring.md)** | Varsayılan | Desteklenen en yakın bölgede aynı *Uygulama adının* Uygulama Öngörüleri kaynağını oluşturur. Bu ayarı genişleterek, **Yeni kaynak adını** değiştirebilir veya verilerinizi depolamak istediğiniz Bir Azure [coğrafyasında](https://azure.microsoft.com/global-infrastructure/geographies/) farklı bir **Konum** seçebilirsiniz. |

    ![İzleme](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

1. Uygulama yapılandırma seçimlerini incelemek için **Gözden Geçir + oluştur'u** seçin.

1. Gözden **Geçir + oluştur** sayfasında, ayarlarınızı gözden geçirin ve ardından işlev uygulamasını sağlamak ve dağıtmak için **Oluştur'u** seçin.

1. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. **Panoya Sabitle'yi**de seçebilirsiniz. Sabitleme, panonuzdan bu işlev uygulaması kaynağına geri dönmeyi kolaylaştırır.

    ![Dağıtım bildirimi](./media/functions-create-function-app-portal/function-app-create-notification2.png)