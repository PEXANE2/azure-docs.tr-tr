---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 79fb8517ec6880e8a3eae0e74275567a24644b87
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188174"
---
## <a name="deployment-customization"></a>Dağıtım özelleştirmesi

Dağıtım işlemi, gönderdiğiniz. zip dosyasının, bir çalıştırma uygulaması içerdiğini varsayar. Varsayılan olarak, hiçbir özelleştirme çalıştırılmaz. Sürekli tümleştirme ile aldığınız yapı süreçlerini etkinleştirmek için, uygulama ayarlarınıza aşağıdakileri ekleyin:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

. Zip Push dağıtımını kullandığınızda, bu ayar varsayılan olarak **false 'tur** . Varsayılan değer sürekli tümleştirme dağıtımları için **geçerlidir** . **Doğru**olarak ayarlandığında, dağıtım sırasında dağıtımınız ile ilgili ayarlar kullanılır. Bu ayarları, uygulama ayarları veya. zip dosyanızın kökünde bulunan bir. Deployment yapılandırma dosyasında yapılandırabilirsiniz. Daha fazla bilgi için bkz. Dağıtım başvurusunda [Depo ve dağıtımla ilgili ayarlar](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) .