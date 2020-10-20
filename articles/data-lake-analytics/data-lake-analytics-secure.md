---
title: Birden çok kullanıcı için güvenli Azure Data Lake Analytics
description: Azure Data Lake Analytics ' de işleri çalıştırmak için birden çok Kullanıcı yapılandırmayı öğrenin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 05/30/2018
ms.openlocfilehash: 52bdd80dba1d5de150f403199fbd5c14d188a410
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220151"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics 'de iş bilgilerine Kullanıcı erişimini yapılandırma 

Azure Data Lake Analytics, işleri çalıştırmak için birden çok kullanıcı hesabı veya hizmet sorumlusu kullanabilirsiniz. 

Aynı kullanıcıların ayrıntılı iş bilgilerini görmesini sağlamak için, kullanıcıların iş klasörlerinin içeriğini okuyabilmeleri gerekir. İş klasörleri `/system/` dizininde bulunur. 

Gerekli izinler yapılandırılmamışsa, Kullanıcı bir hata görebilir: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>İş bilgilerine Kullanıcı erişimini yapılandırma

Klasörlerdeki ACL 'Leri yapılandırmak için **Kullanıcı Ekleme Sihirbazı** ' nı kullanabilirsiniz. Daha fazla bilgi için bkz. [Yeni Kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Daha ayrıntılı denetime ihtiyacınız varsa veya izinleri betikten sonra klasörleri aşağıdaki şekilde güvenli hale getirin:

1. Kök klasörde **yürütme** izinleri (bir erişim ACL 'si aracılığıyla) verin:
   - /
   
2. İş klasörlerini içeren klasörlerde **yürütme** ve **okuma** IZINLERI (bir erişim ACL 'si ve varsayılan ACL aracılığıyla) verin. Örneğin, 25 Mayıs 2018 tarihinde çalıştırılan belirli bir iş için bu klasörlere erişilmesi gerekir:
   - /Sistem
   - /System/jobservice
   - /System/jobservice/Jobs
   - /system/jobservice/jobs/Usql
   - /System/jobservice/Jobs/ussql/2018
   - /System/jobservice/Jobs/ussql/2018/05
   - /System/jobservice/Jobs/ussql/2018/05/25
   - /System/jobservice/Jobs/ussql/2018/05/25/11
   - /System/jobservice/Jobs/ussql/2018/05/25/11/01
   - /System/jobservice/Jobs/USD/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Sonraki adımlar
[Yeni kullanıcı ekleme](data-lake-analytics-manage-use-portal.md#add-a-new-user)
