---
title: include dosyası
description: include dosyası
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/01/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: 9bac92bc1cc94b88dc694b468b795049db4ac9df
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443966"
---
1. [Azure Portal](https://portal.azure.com) gidin.

1. Sol taraftaki **+ kaynak oluştur** ' u seçin ve ardından **işlev uygulaması**' nı seçin.

1. **Barındırma planı**için **plan App Service**seçin ve **App Service planı/konumu**seçin.

    ![İşlev uygulaması oluşturma](./media/functions-premium-create/create-function-app-resource.png)

1. **Yeni oluştur**' u seçin, **App Service bir plan** adı yazın, kendinize yakın bir [bölgede](https://azure.microsoft.com/regions/) bir konum seçin ya da Işlevlerinizin erişebileceği diğer hizmetlere yakın bir **konum** seçin ve ardından **fiyatlandırma katmanı**' nı seçin.

    ![App Service planı oluşturma](./media/functions-premium-create/new-app-service-plan.png)

1. **EP1** (elastik Premium) planını seçin ve ardından **Uygula**' yı seçin.

    ![Premium planı seç](./media/functions-premium-create/hosting-plan.png) 

1. Planı oluşturmak için **Tamam** ' ı seçin ve ardından görüntünün altındaki tabloda belirtilen geri kalan işlev uygulama ayarlarını kullanın. 

    ![Tamamlanmış App Service planı](./media/functions-premium-create/create-function-app.png)  

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. Önerilen değeri de kullanabilirsiniz. |
    | **OS** | Tercih edilen işletim sistemi | Hem Linux hem de Windows Premium planda desteklenir. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. Yalnızca seçtiğiniz **Işletim sisteminde** desteklenen diller görüntülenir. |
    | **[Depolama](../articles/storage/common/storage-quickstart-create-account.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Dilerseniz [depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz. |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Varsayılan | En yakın desteklenen bölgede aynı *uygulama adının* Application Insights kaynağını oluşturur. Bu ayarı genişleterek, **Yeni kaynak adını** değiştirebilir veya verilerinizi depolamak istediğiniz [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) 'da farklı bir **konum** seçebilirsiniz. |

1. Ayarlarınız doğrulandıktan sonra **Oluştur**' u seçin.

1. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-premium-create/function-app-create-notification.png)

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. **Panoya sabitle ' yi**de seçebilirsiniz. Sabitleme, panonuzdan bu işlev uygulama kaynağına döndürülmesini kolaylaştırır.