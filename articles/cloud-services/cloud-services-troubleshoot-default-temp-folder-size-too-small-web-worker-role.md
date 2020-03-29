---
title: Varsayılan TEMP klasör boyutu bir rol için çok küçük | Microsoft Dokümanlar
description: Bulut hizmeti rolü, TEMP klasörü için sınırlı miktarda alana sahiptir. Bu makalede, alan tükenmesini önlemek için nasıl bazı öneriler sağlar.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 0b869b73a79872d9263058bedfead018e18721c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71154993"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Varsayılan TEMP klasör boyutu bulut hizmeti web/işçi rolünde çok küçük
Bir bulut hizmeti çalışanının veya web rolünün varsayılan geçici dizini, bir noktada tam hale gelebilen en fazla 100 MB boyutuna sahiptir. Bu makalede, geçici dizini için yer tükenmesini önlemek için nasıl açıklanmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Neden yerim tükeniyor?
Standart Windows ortamı değişkenleri TEMP ve TMP, uygulamanızda çalışan kodlar için kullanılabilir. Hem TEMP hem de TMP, maksimum boyutu 100 MB olan tek bir dizini işaret eder. Bu dizinde depolanan veriler bulut hizmetinin yaşam döngüsü boyunca kalıcı değildir; bulut hizmetindeki rol örnekleri geri dönüştürülürse, dizin temizlenir.

## <a name="suggestion-to-fix-the-problem"></a>Sorunu gidermek için öneri
Aşağıdaki alternatiflerden birini uygulayın:

* Yerel bir depolama kaynağını yapılandırın ve TEMP veya TMP kullanmak yerine doğrudan bu kaynağa erişin. Uygulamanızda çalışan koddan yerel bir depolama kaynağına erişmek için [RoleEnvironment.GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) yöntemini arayın.
* Yerel depolama kaynağını yapılandırın ve TEMP ve TMP dizinlerini yerel depolama kaynağının yolunu işaret edecek şekilde işaret edin. Bu değişiklik [RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) yöntemi içinde gerçekleştirilmelidir.

Aşağıdaki kod örneği, OnStart yöntemi nin içinden TEMP ve TMP için hedef dizinlerin nasıl değiştirilebildiğini gösterir:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Geçici Klasör'ASP.NET Azure Web Rolü boyutunu [nasıl artıracağınızaçıklayan](https://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx)bir blog okuyun.

Bulut hizmetleri için daha fazla [sorun giderme makalelerini](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/vs-azure-tools-debugging-cloud-services-overview.md) görüntüleyin.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rol sorunlarını nasıl gidereceklerini öğrenmek için [Kevin Williamson'ın blog serisini](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)görüntüleyin.
