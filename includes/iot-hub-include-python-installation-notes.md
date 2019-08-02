---
title: include dosyası
description: include dosyası
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 103ad020b8d9f50ffe690f502b7cac08dab9237a
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640433"
---
* Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* **Windows**

    * [Python 2. x veya 3. x](https://www.python.org/downloads/). Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*'yi yüklemeniz veya yükseltmeniz](https://pip.pypa.io/en/stable/installing/) gerekebilir.

    * Windows işletim sistemi kullanıyorsanız, Python 'dan yerel dll 'lerin kullanılmasına izin vermek için, [Visual C++ yeniden dağıtılabilir paketin](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doğru sürümüne sahip olduğunuzdan emin olun. En son sürümü kullanmanızı öneririz.

    * Gerekirse, komutunu kullanarak [Azure-ıothub-Device-Client](https://pypi.org/project/azure-iothub-device-client/) paketini yükledikten sonra`pip install azure-iothub-device-client`

    * Gerekirse, komutunu kullanarak [Azure-ıothub-Service-Client](https://pypi.org/project/azure-iothub-service-client/) paketini yükledikten sonra`pip install azure-iothub-service-client`

* **Mac OS**

    Mac OS için Python 3.7.0 (veya 2,7) + libboost-1,67 + kıvrık 7.61.1 (tümü homebrew aracılığıyla yüklenir) gerekir. Diğer bir dağıtım/işletim sistemi büyük olasılıkla, çalışmayan ve çalışma zamanında ımporterror ile sonuçlanmayacak farklı artırma & bağımlılıklarının sürümlerini katıştırır.

    `azure-iothub-service-client` ve `azure-iothub-device-client` için *PIP* paketleri şu anda yalnızca Windows İşletim Sistemi için mevcuttur. Linux/Mac OS için lütfen [geliştirme ortamınızı Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) gönderisini hazırlama bölümündeki linux ve Mac OS özgü bölümlere bakın.

> [!NOTE]
> Örnek içinde iothub_client içeri aktarılırken birkaç hata raporu vardı. **Importerror** sorunlarıyla ilgili daha fazla bilgi için lütfen bkz. [ımporterror sorunlarıyla ilgilenme](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).
