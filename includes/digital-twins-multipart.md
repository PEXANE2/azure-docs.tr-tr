---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 02/07/2020
ms.custom: include file
ms.openlocfilehash: 0e7cb7e4aaa9862a2b4af51593c29793ea54dd14
ms.sourcegitcommit: 9add86fb5cc19edf0b8cd2f42aeea5772511810c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2020
ms.locfileid: "77111210"
---
> [!NOTE]
> Çok parçalı istekler genellikle üç parça gerektirir:
> * Bir **Content-Type** üst bilgisi:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Bir **içerik-değerlendirme**:
>   * `form-data; name="metadata"`
> * Karşıya yüklenecek dosya içeriği
>
> **Içerik türü** ve **Content-Disposition** , kullanım senaryosuna göre değişir.

Çok parçalı istekler, bir REST istemcisi veya C# [Postman](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#make-a-multipart-post-request)gibi bir araç aracılığıyla programlı bir şekilde yapılabilir. REST istemci araçları karmaşık çok parçalı istekler için farklı destek düzeylerine sahip olabilir. Yapılandırma ayarları ayrıca araç ve araç arasında biraz farklılık gösterebilir. İhtiyaçlarınıza en uygun aracı doğrulayın.

> [!IMPORTANT]
> Azure Digital TWINS yönetim API 'Lerine yapılan çok parçalı isteklerin genellikle iki bölümü vardır:
> * **Content-Type** ve/veya **Content-Disposition** tarafından belirtilen blob meta verileri (ÖRNEĞIN, ilişkili bir MIME türü)
> * Karşıya yüklenecek bir dosyanın yapılandırılmamış içeriğini içeren blob içeriği
>
> **Düzeltme Eki** istekleri için iki bölümden hiçbiri gerekli değildir. **Post** veya Create işlemleri için her ikisi de gereklidir.

[Doluluk hızlı başlangıç kaynak kodu](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) , Azure C# Digital TWINS yönetim API 'lerinde çok parçalı isteklerin nasıl yapılacağını gösteren tüm örnekleri içerir.