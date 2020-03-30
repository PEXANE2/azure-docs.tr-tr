---
title: Azure Monitör'de uyarı örneklerini yönetme
description: Azure'da uyarı örneklerini yönetme
ms.subservice: alerts
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1910b3b474012365e8117f584379b2b29f8ce3de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667627"
---
# <a name="manage-alert-instances-with-unified-alerts"></a>Birleşik uyarılarla uyarı örneklerini yönetme

Azure Monitor'daki [birleşik uyarılar deneyimiyle,](https://aka.ms/azure-alerts-overview) Azure'da farklı türde uyarılarınızı görebilirsiniz. Bu, tek bir bölmede birden çok aboneliği kapsar. Bu makalede, uyarı örneklerinizi nasıl görüntüleyebileceğiniz ve sorun giderme için belirli uyarı örneklerini nasıl bulabileceğiniz gösterilmektedir.

> [!NOTE]
> Yalnızca son 30 gün içinde oluşturulan uyarılara erişebilirsiniz.

## <a name="go-to-the-alerts-page"></a>Uyarılar sayfasına git

Uyarılar sayfasına aşağıdaki yollardan herhangi biri ile gidebilirsiniz:

- Azure [portalında,](https://portal.azure.com/) **Uyarıları İzle'yi** > **Alerts**seçin.  

     ![Monitör Uyarılarının Ekran Görüntüsü](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
- Belirli bir kaynağın bağlamını kullanın. Kaynak açın, **İzleme** bölümüne gidin ve **Uyarılar'ı**seçin. Açılış sayfası, belirli bir kaynaktaki uyarılar için önceden filtrelenmiştir.

     ![Kaynak İzleme Uyarılarının ekran görüntüsü](media/alerts-managing-alert-instances/alert-resource.JPG)

- Belirli bir kaynak grubunun bağlamını kullanın. Kaynak grubu açın, **İzleme** bölümüne gidin ve **Uyarılar'ı**seçin. Açılış sayfası, belirli bir kaynak grubundaki uyarılar için önceden filtrelenir.    

     ![Kaynak grubu İzleme Uyarıları ekran görüntüsü](media/alerts-managing-alert-instances/alert-rg.JPG)

## <a name="find-alert-instances"></a>Uyarı örneklerini bulma

**Uyarılar Özeti** sayfası, Azure'daki tüm uyarı örneklerinize genel bir bakış sağlar. **Birden çok abonelik** (en fazla 5'e kadar) seçerek veya kaynak **gruplarına,** belirli **kaynaklara**veya **zaman aralıklarına**filtre uygulayarak özet görünümünü değiştirebilirsiniz. Uyarılarınız için liste görünümüne gitmek için **Toplam Uyarılar'ı**veya önem gruplarından herhangi birini seçin.     

![Uyarılar Özeti sayfasının ekran görüntüsü](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
Tüm **Uyarılar** sayfasında, Azure'daki tüm uyarı örnekleri listelenir. Portala bir uyarı bildiriminden geliyorsanız, söz konusu uyarı örneğini daraltmak için kullanılabilen filtreleri kullanabilirsiniz.

> [!NOTE]
> Önem gruplarından herhangi birini seçerek sayfaya geldiyseniz, liste bu önem derecesi için önceden filtrelenir.

Önceki sayfada bulunan filtrelerdışında, monitör hizmeti (örneğin, ölçümler için platform), izleme durumu (ateşlenen veya çözülmüş), önem derecesi, uyarı durumu (yeni/kabul edilen/kapalı) veya akıllı grup kimliği temelinde filtre uygulayabilirsiniz.

![Tüm Uyarılar sayfasının ekran görüntüsü](media/alerts-managing-alert-instances/all-alerts.jpg)

> [!NOTE]
> Önem gruplarından herhangi birini seçerek sayfaya geldiyseniz, liste bu önem derecesi için önceden filtrelenir.

Herhangi bir uyarı örneğini **seçmek, uyarı ayrıntıları** sayfasını açarak belirli bir uyarı örneği hakkında daha fazla ayrıntı görmenizi sağlar.   

![Uyarı Ayrıntıları sayfasının ekran görüntüsü](media/alerts-managing-alert-instances/alert-details.jpg)  

