---
title: WebJob, arka plan görevleri, Azure 'da
description: WebJobs hakkında bilgi edinin.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 7714b090399b0b184e2e216ff6da7b10f2bf4386
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452280"
---
# <a name="webjobs-run-background-tasks-in-azure-app-service"></a>Web Işleri Azure App Service arka plan görevleri çalıştırır

Bu makalede, bir yürütülebilir dosyayı veya betiği karşıya yüklemek için [Azure Portal](https://portal.azure.com) kullanarak WebJobs dağıtımı gösterilmektedir. Visual Studio kullanarak Web Işleri geliştirme ve dağıtma hakkında bilgi için bkz. [Visual Studio kullanarak Web Işleri dağıtma](webjobs-dotnet-deploy-vs.md).

## <a name="overview"></a>Genel Bakış
WebJobs, bir program veya betiği Web uygulaması, API uygulaması veya mobil uygulama ile aynı örnekte çalıştırmanızı sağlayan bir [Azure App Service](index.yml) özelliğidir. WebJobs kullanmanın başka bir maliyeti yoktur.

> [!IMPORTANT]
> Web Işleri, Linux üzerinde App Service için henüz desteklenmiyor.

Azure WebJobs SDK, birçok programlama görevini basitleştirmek için WebJobs ile birlikte kullanılabilir. Daha fazla bilgi için bkz. [WebJobs SDK nedir?](https://github.com/Azure/azure-webjobs-sdk/wiki).

Azure Işlevleri, programları ve betikleri çalıştırmak için başka bir yol sağlar. WebJobs ve Işlevleri arasında bir karşılaştırma için bkz. [Flow, Logic Apps, işlevler ve WebJobs arasında seçim](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md)yapın.

## <a name="webjob-types"></a>WebJob türleri

Aşağıdaki tabloda, *sürekli* ve *tetiklenen* Web işleri arasındaki farklar açıklanmaktadır.


|Sürekli  |Diğini  |
|---------|---------|
| WebJob oluşturulduğunda hemen başlar. İşin sonlandırmasına devam etmek için program veya komut dosyası genellikle sonsuz bir döngü içinde çalışır. İş sona erdirmek için yeniden başlatabilirsiniz. | Yalnızca el ile veya bir zamanlamaya göre tetiklendiğinde başlatılır. |
| Web uygulamasının üzerinde çalıştığı tüm örneklerde çalışır. WebJob 'u isteğe bağlı olarak tek bir örnekle kısıtlayabilirsiniz. |Yük Dengeleme için Azure 'un seçtiği tek bir örnek üzerinde çalışır.|
| Uzaktan hata ayıklamayı destekler. | Uzaktan hata ayıklamayı desteklemez.|

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

## <a name="add-webjob-to-source-control"></a>Kaynak denetimine WebJob Ekle

Uygulamanızla yapılandırılmış kaynak denetiminiz varsa, WebJobs 'lar kaynak denetimi tümleştirmesinin parçası olarak dağıtılmalıdır. Kaynak denetimi uygulamanızla yapılandırıldıktan sonra, Azure portalından WebJob eklenemez.

## <a name="supported-file-types-for-scripts-or-programs"></a><a name="acceptablefiles"></a>Betikler veya programlar için desteklenen dosya türleri

Aşağıdaki dosya türleri desteklenir:

* . cmd,. bat,. exe (Windows cmd kullanarak)
* . ps1 (PowerShell kullanarak)
* . sh (Bash kullanarak)
* . php (PHP kullanarak)
* . Kopyala (Python kullanarak)
* . js (Node.js kullanarak)
* . jar (Java kullanarak)

## <a name="next-steps"></a>Sonraki adımlar

* [WebJob oluşturmayı](./webjobs-create-ieux.md) öğrenin
* [Web işlerinin](./webjobs-create-ieux-view-log.md) günlük geçmişini görüntüle
