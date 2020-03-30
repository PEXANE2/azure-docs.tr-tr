---
author: yegu-ms
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: yegu
ms.openlocfilehash: 61e93e3700b9a396d2ac4fdcbb51fc5c874cf9cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68286325"
---
.NET uygulamaları, önbellek istemcisi uygulamalarının yapılandırmasını basitleştiren bir NuGet paketi kullanarak Visual Studio’da yapılandırılabilecek **StackExchange.Redis** Cache istemcisi kullanabilir. 

> [!NOTE]
> Daha fazla bilgi için [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) GitHub sayfasına ve [Redis istemci belgeleri için StackExchange.Azure Önbelleği sayfasına](https://github.com/StackExchange/StackExchange.Redis#documentation)bakın.
>
>

Visual Studio’da StackExchange.Redis NuGet paketi kullanarak bir istemci uygulamasını yapılandırmak için, **Çözüm Gezgini**’nde sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin. 

![NuGet paketlerini yönetme](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Arama metin kutusuna **StackExchange.Redis** ya da **StackExchange.Redis.StrongName** yazın, sonuçlardan istediğiniz sürümü seçin ve **Yükle**’ye tıklayın.

> [!NOTE]
> **StackExchange.Redis** istemci kitaplığının kesin adlandırılmış bir sürümünü kullanmak isterseniz, **StackExchange.Redis.StrongName** seçeneğin seçin; istemezseniz **StackExchange.Redis** seçeneğin seçin.
>
>

![StackExchange.Redis NuGet paketi](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

NuGet paketi indirir ve Redis istemcisi için StackExchange.Azure Önbelleği ile Redis için Azure Önbelleğine erişmek için istemci uygulamanız için gerekli montaj başvurularını ekler.

> [!NOTE]
> Daha önce projenizi StackExchange.Redis kullanacak şekilde yapılandırdıysanız **NuGet Paket Yöneticisi**’nden paket güncelleştirmelerini denetleyebilirsiniz. StackExchange.Redis NuGet paketinin güncelleştirilmiş sürümlerini kontrol etmek ve yüklemek için **NuGet Paket Yöneticisi** penceresinde **güncelleştirmeleri** tıklatın. StackExchange.Redis NuGet paketin bir güncelleştirme varsa projenizi güncelleştirilmiş sürüme yükseltebilirsiniz.
>
>

Ayrıca, **Araçlar** menüsünden **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu**’na tıklayıp **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu çalıştırarak StackExchange.Redis NuGet paketini yükleyebilirsiniz.

```
Install-Package StackExchange.Redis
```
