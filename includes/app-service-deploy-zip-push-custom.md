---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 79fb8517ec6880e8a3eae0e74275567a24644b87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188174"
---
## <a name="deployment-customization"></a>Dağıtım özelleştirme

Dağıtım işlemi, ittirdiğiniz .zip dosyasının çalıştırılan hazır bir uygulama içerdiğini varsayar. Varsayılan olarak, hiçbir özelleştirme çalıştırılır. Sürekli tümleştirme yle elde ettiğiniz aynı yapı işlemlerini etkinleştirmek için uygulama ayarlarınıza aşağıdakileri ekleyin:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

.zip push dağıtımını kullandığınızda, bu ayar varsayılan olarak **yanlıştır.** Varsayılan değer, sürekli tümleştirme dağıtımları için **geçerlidir.** **Doğru**ayarlandığında, dağıtımla ilgili ayarlarınız dağıtım sırasında kullanılır. Bu ayarları uygulama ayarları olarak veya .zip dosyanızın kökünde bulunan bir .deployment yapılandırma dosyasında yapılandırabilirsiniz. Daha fazla bilgi için dağıtım başvurusunda [Depo ve dağıtımla ilgili ayarlara](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) bakın.