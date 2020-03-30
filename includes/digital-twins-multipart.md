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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77111210"
---
> [!NOTE]
> Çok parçalı istekler genellikle üç parça gerektirir:
> * **İçerik Türü** üstbilgi:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Bir **İçerik-Disposition**:
>   * `form-data; name="metadata"`
> * Yüklenmesi gereken dosya içeriği
>
> **İçerik Türü** ve **İçerik-Disposition** kullanım senaryosuna bağlı olarak değişir.

Çok parçalı istekler programlı (C# yoluyla), bir REST istemcisi veya [Postacı](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#make-a-multipart-post-request)gibi bir araç aracılığıyla yapılabilir. REST istemci araçları, karmaşık çok parçalı istekler için farklı düzeyde destek düzeyine sahip olabilir. Yapılandırma ayarları da aletten araca biraz farklılık gösterebilir. İhtiyaçlarınıza en uygun aracı doğrulayın.

> [!IMPORTANT]
> Azure Dijital İkizler Yönetimi API'lerine yapılan çok parçalı isteklerin genellikle iki bölümü vardır:
> * İçerik Türü ve/veya **İçerik-Disposition** tarafından bildirilen blob **Content-Type** meta verileri (ilişkili MIME türü gibi)
> * Yüklenecek bir dosyanın yapılandırılmamış içeriğini içeren blob içeriği
>
> **Patch** istekleri için iki parçadan hiçbiri gerekmez. Her ikisi de **POST** veya oluşturma işlemleri için gereklidir.

[Doluluk Hızlı Başlatma kaynak kodu,](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) Azure Dijital İkizler Yönetimi API'lerine karşı çok parçalı isteklerin nasıl yapılacağını gösteren eksiksiz C# örnekleri içerir.