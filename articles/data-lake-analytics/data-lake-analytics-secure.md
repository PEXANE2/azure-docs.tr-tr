---
title: Birden fazla kullanıcı için Güvenli Azure Veri Gölü Analizi
description: Azure Veri Gölü Analitiği'nde birden çok kullanıcıyı işleri çalıştıracak şekilde nasıl yapılandırılamayı öğrenin.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 9fbc94259d6fdfb6758204efd6e6f0a346dc58da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60813378"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Azure Veri Gölü Analizi'nde iş bilgilerine kullanıcı erişimini iş bilgilerine göre yapılandırma 

Azure Veri Gölü Analitiği'nde, işleri çalıştırmak için birden çok kullanıcı hesabı veya hizmet ilkesi kullanabilirsiniz. 

Aynı kullanıcıların ayrıntılı iş bilgilerini görebilmeleri için, kullanıcıların iş klasörlerinin içeriğini okuyabilmesi gerekir. İş klasörleri dizinde `/system/` bulunur. 

Gerekli izinler yapılandırılmamışsa, kullanıcı bir hata görebilir:`Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>İş bilgilerine kullanıcı erişimini yapılandırma

Klasörlerdeki Aç'ları yapılandırmak için **Kullanıcı Ekle Sihirbazı'nı** kullanabilirsiniz. Daha fazla bilgi için [bkz.](data-lake-analytics-manage-use-portal.md#add-a-new-user)

Daha fazla parçalı denetime ihtiyacınız varsa veya izinleri komut dosyasına almanız gerekiyorsa, klasörleri aşağıdaki gibi güvenli hale alın:

1. **İzinleri** yürütme izinlerini (erişim ACL'si aracılığıyla) kök klasöründe verir:
   - /
   
2. İş klasörlerini içeren klasörlerde **izinleri** (erişim ACL ve varsayılan ACL aracılığıyla) verir ve **okuyun.** Örneğin, 25 Mayıs 2018 tarihinde çalıştırılan belirli bir iş için bu klasörlere erişilmesi gerekir:
   - /sistem
   - /sistem/jobservice
   - /sistem/jobservice/jobs
   - /system/jobservice/jobs/Usql
   - /system/jobservice/jobs/Usql/2018
   - /system/jobservice/jobs/Usql/2018/05
   - /system/jobservice/jobs/Usql/2018/05/25
   - /system/jobservice/jobs/Usql/2018/05/25/11
   - /system/jobservice/jobs/Usql/2018/05/25/11/01
   - /system/jobservice/jobs/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Sonraki adımlar
[Yeni kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user)
