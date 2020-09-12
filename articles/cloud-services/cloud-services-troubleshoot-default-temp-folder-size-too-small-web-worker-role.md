---
title: Varsayılan GEÇICI klasör boyutu bir rol için çok küçük | Microsoft Docs
description: Bir bulut hizmeti rolü, GEÇICI klasör için sınırlı miktarda alana sahiptir. Bu makalede, boş alan çalıştırmanın nasıl önleneceğini gösteren bazı öneriler sunulmaktadır.
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
ms.openlocfilehash: 77c4f627668b91c4bc21ad73b3511c1775943229
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89460210"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Varsayılan GEÇICI klasör boyutu bir bulut hizmeti Web/çalışan rolünde çok küçük
Bir bulut hizmeti çalışanının veya Web rolünün varsayılan geçici dizini en fazla 100 MB olabilir, bu da bazı bir noktada dolu olabilir. Bu makalede, geçici dizin için boş alan kalmadı.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Neden çalışma alanım?
TEMP ve TMP standart Windows ortam değişkenleri, uygulamanızda çalışan kod tarafından kullanılabilir. Hem TEMP hem de TMP, en fazla 100 MB boyutunda tek bir dizine işaret noktasıdır. Bu dizinde depolanan tüm veriler, bulut hizmetinin yaşam döngüsü boyunca kalıcı değildir; bir bulut hizmetindeki rol örnekleri geri dönüştürüldüğünde, Dizin temizlenir.

## <a name="suggestion-to-fix-the-problem"></a>Sorunu gidermeye yönelik öneri
Aşağıdaki seçeneklerden birini uygulayın:

* Yerel bir depolama kaynağı yapılandırın ve TEMP veya TMP kullanmak yerine doğrudan erişin. Uygulamanızda çalışan koddan yerel bir depolama kaynağına erişmek için [Roleenvironment. GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) yöntemini çağırın.
* Yerel bir depolama kaynağı yapılandırın ve yerel depolama kaynağının yolunu işaret etmek için TEMP ve TMP dizinlerini işaret edin. Bu değişiklik [Roleentrypoint. OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) yöntemi içinde gerçekleştirilmelidir.

Aşağıdaki kod örneği, OnStart yönteminin içinden TEMP ve TMP için hedef dizinlerin nasıl değiştirileceğini göstermektedir:

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
[Azure Web rolü ASP.NET geçici klasörünün boyutunu nasıl artırabileceğinizi](https://docs.microsoft.com/archive/blogs/kwill/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder)açıklayan bir blog okuyun.

Cloud Services için daha fazla [sorun giderme makalesini](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-debugging-cloud-services-overview) görüntüleyin.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme hakkında bilgi edinmek için [Kevin Williamson 'ın blog serisini](https://docs.microsoft.com/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data)görüntüleyin.
