---
title: Uyarı örneklerini yönetme
description: Azure genelinde uyarı örneklerini yönetme
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: dbc7d37ce311d203ee051b5a23632d2bf401e27a
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034837"
---
# <a name="manage-alert-instances"></a>Uyarı örneklerini yönetme
Azure Izleyici 'de [birleştirilmiş uyarılar deneyimiyle](https://aka.ms/azure-alerts-overview) birlikte, birden çok aboneliği tek bir bölmeden kapsayan Azure 'daki farklı uyarı tiplerinizi görebilirsiniz. Bu makalede, uyarı örneklerinizi nasıl görüntüleyebileceğinizi ve sorun giderme amacıyla belirli uyarı örneklerini bulmak için portala nasıl derinlemesine bir bakış yapılacağı gösterilmektedir.

> [!NOTE]
   >  Yalnızca son 30 gün içinde oluşturulan uyarılara UX veya REST API 'Leri aracılığıyla erişilebilir.

1. Uyarılar sayfasında üç farklı yol vardır

   + [Portalda](https://portal.azure.com/) **izleyici** ' yi seçin ve izleyici bölümünde **Uyarılar**' ı seçin.  
     ![İzleme](media/alerts-managing-alert-instances/monitoring-alerts-managing-alert-instances-toc.jpg)
  
   + Belirli bir **kaynak**bağlamı içinden uyarılara gidebilirsiniz. Bir kaynak açıldığında, Izleme bölümüne içindekiler tablosunda ilerleyin ve **Uyarılar**' ı seçerek giriş sayfası söz konusu kaynaktaki uyarılar için önceden filtrelenmiş olur.
   
     ![İzleme](media/alerts-managing-alert-instances/alert-resource.JPG)
    
   + Belirli bir **kaynak grubunun**bağlamı içinden uyarılara gidebilirsiniz. Bir kaynak grubu açıldıktan sonra Izleme bölümüne içindekiler tablosuna gidin ve **Uyarılar**' ı seçerek, giriş sayfası söz konusu kaynak grubundaki uyarılar için önceden filtrelenmiş olur.    
   
     ![İzleme](media/alerts-managing-alert-instances/alert-rg.JPG)

1. Uyarı **Özeti** sayfasına inirsiniz, bu da Azure genelinde tüm uyarı örneklerinizin genel bir görünümünü sağlar. Özet görünümünü **birden çok abonelik** (en fazla 5) seçerek veya **kaynak grupları**, belirli **kaynaklar**veya **zaman aralıklarında**filtreleyerek değiştirebilirsiniz. Uyarılarınızın liste görünümüne gitmek için toplam uyarı veya önem derecesindeki herhangi birine tıklayın.     
   ![Uyarı Özeti](media/alerts-managing-alert-instances/alerts-summary.jpg)
 
1. Tüm Uyarılar sayfasında, Azure 'da listelenen tüm uyarı örneklerini görebileceğiniz **her şey** gösterilir. Portala bir uyarı bildirimi geliyorsa, bu belirli uyarı örneğinde daraltmak için kullanılabilen filtreleri kullanabilirsiniz. (**Unutmayın**: herhangi bir önem derecesindeki herhangi birine tıklayarak sayfaya ulaşdıysanız, bu önem derecesine göre listenin ön filtresi yapılır). Önceki sayfada bulunan süzgeçlerden ayrı olarak, artık izleme hizmeti (örneğin, ölçümler için Platform), izleme koşulu (tetiklenen veya çözüldü), önem derecesi, uyarı durumu (yeni/onaylanmış/kapalı) veya akıllı Grup KIMLIĞI ' ne göre de filtre uygulayabilirsiniz.

   ![Tüm Uyarılar](media/alerts-managing-alert-instances/all-alerts.jpg)

   > [!NOTE]
   >  Herhangi bir önem derecesindeki herhangi birine tıklayarak sayfaya gelmişse, bu sayfada bir işlem yaptığınızda liste bu önem derecesine göre önceden filtrelenir.
 
1. Herhangi bir uyarı örneğine tıkladığınızda **Uyarı ayrıntıları** sayfası açılır ve bu belirli uyarı örneği hakkında daha fazla bilgi alabilirsiniz.   
   ![Uyarı ayrıntıları](media/alerts-managing-alert-instances/alert-details.jpg)  

