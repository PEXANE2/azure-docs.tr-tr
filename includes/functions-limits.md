---
author: ggailey777
ms.service: cost-management-billing
ms.topic: include
ms.date: 05/09/2019
ms.author: glenga
ms.openlocfilehash: 4643bb7f95e4fd1249d3ab6699c1f835c77f18fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77198352"
---
| Kaynak | [Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan) | [Premium planı](../articles/azure-functions/functions-scale.md#premium-plan) | [Uygulama Hizmeti planı](../articles/azure-functions/functions-scale.md#app-service-plan)<sup>1</sup> |
| --- | --- | --- | --- |
| Ölçeği genişletme | Olay odaklı | Olay odaklı | [Manuel/otomatik ölçek](../articles/app-service/manage-scale-up.md) | 
| Maksimum örnekler | 200 | 100 | 10-20 |
|Varsayılan [zaman uzatma süresi](../articles/azure-functions/functions-scale.md#timeout) (dk) |5 | 30 |30<sup>2</sup> |
|Maksimum [zaman uzatma süresi](../articles/azure-functions/functions-scale.md#timeout) (dk) |10 | sınırsız<sup>8</sup> | sınırsız<sup>3</sup> |
| Maksimum giden bağlantılar (örnek başına) | 600 aktif (1200 toplam) | Sınırsız | Sınırsız |
| Maksimum istek boyutu (MB)<sup>4</sup> | 100 | 100 | 100 |
| Maksimum sorgu dize uzunluğu<sup>4</sup> | 4096 | 4096 | 4096 |
| Maksimum istek URL uzunluğu<sup>4</sup> | 8192 | 8192 | 8192 |
| Örnek başına [ACU](../articles/virtual-machines/windows/acu.md) | 100 | 210-840 | 100-840 |
| Maksimum bellek (örnek başına GB) | 1,5 | 3.5-14 | 1.75-14 |
| Plan başına fonksiyon uygulamaları |100 |100 |sınırsız<sup>5</sup> |
| [Uygulama Hizmeti planları](../articles/app-service/overview-hosting-plans.md) | [Bölge](https://azure.microsoft.com/global-infrastructure/regions/) başına 100 |Kaynak grubu başına 100 |Kaynak grubu başına 100 |
| Depolama<sup>6</sup> |1 GB |250 GB |50-1000 GB Arası |
| Uygulama başına özel etki alanları</a> |500<sup>7</sup> |500 |500 |
| Özel etki alanı [SSL desteği](../articles/app-service/configure-ssl-bindings.md) |sınırsız SNI SSL bağlantısı dahil | sınırsız SNI SSL ve 1 IP SSL bağlantısı dahil |sınırsız SNI SSL ve 1 IP SSL bağlantısı dahil | 

<sup>1</sup> Çeşitli Uygulama Hizmeti planı seçenekleri için belirli sınırlar [için, Uygulama Hizmeti planı sınırlarına](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.  
<sup>2</sup> Varsayılan olarak, Bir Uygulama Hizmeti planındaki Fonksiyonlar 1.x çalışma süresi için zaman ası sınırsızdır.  
<sup>3</sup> Uygulama Hizmeti planının [Her Zaman A...da](../articles/azure-functions/functions-scale.md#always-on)ayarlanmasını gerektirir. Standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/)ödeyin.  
<sup>4</sup> Bu sınırlar [ana bilgisayarda ayarlanır.](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config)  
<sup>5</sup> Barındırabileceğiniz işlev uygulamalarının gerçek sayısı uygulamaların etkinliğine, makine örneklerinin boyutuna ve ilgili kaynak kullanımına bağlıdır.  
<sup>6</sup> Depolama sınırı, aynı Uygulama Hizmeti planındaki tüm uygulamalardaki geçici depolama alanının toplam içerik boyutudur. Tüketim planı, geçici depolama için Azure Dosyaları'nı kullanır.  
<sup>7</sup> İşlev uygulamanız [bir Tüketim planında](../articles/azure-functions/functions-scale.md#consumption-plan)barındırıldığında, yalnızca CNAME seçeneği desteklenir. Premium plan veya [Uygulama Hizmeti planındaki](../articles/azure-functions/functions-scale.md#app-service-plan)işlev uygulamaları için, CNAME veya A kaydı nı kullanarak özel bir etki alanının haritasını oluşturabilirsiniz. [Premium plan](../articles/azure-functions/functions-scale.md#premium-plan)  
<sup>8</sup> 60 dakikaya kadar garantilidir.