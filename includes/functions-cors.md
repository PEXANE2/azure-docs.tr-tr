---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: a5f829fc0e153c3a427fd1c7cc7d5c613cd624f6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "83648259"
---
Azure Işlevleri, çıkış noktaları arası kaynak paylaşımını (CORS) destekler. CORS, [portalda](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors) ve [Azure CLI](/cli/azure/functionapp/cors)aracılığıyla yapılandırılır. CORS izin verilen kaynaklar listesi, işlev uygulaması düzeyinde geçerlidir. CORS etkinken yanıtlar `Access-Control-Allow-Origin` üstbilgiyi içerir. Daha fazla bilgi için bkz. [Çıkış noktaları arası kaynak paylaşma](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors). 