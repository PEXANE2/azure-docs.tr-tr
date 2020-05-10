---
title: Windows Sanal Masaüstü Web istemcisi 'ni bağlama-Azure
description: Web istemcisini kullanarak Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: f17d04d94546f31c5613fa9944c7399ea7db10ae
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006060"
---
# <a name="connect-with-the-web-client"></a>Web istemcisiyle bağlanma

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/connect-web-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Web istemcisi, Windows sanal masaüstü kaynaklarınıza, uzun yükleme işlemi olmadan bir Web tarayıcısından erişmenizi sağlar.

>[!NOTE]
>Web istemcisinde Şu anda mobil işletim sistemi desteği yok.

## <a name="supported-operating-systems-and-browsers"></a>Desteklenen işletim sistemleri ve tarayıcılar

HTML5 özellikli herhangi bir tarayıcı çalışması gerektiği sürece, aşağıdaki işletim sistemlerini ve tarayıcıları resmi olarak destekliyoruz.

| Tarayıcı           | Desteklenen işletim sistemi                     | Notlar               |
|-------------------|----------------------------------|---------------------|
| Microsoft Edge    | Windows                          |                     |
| Internet Explorer | Windows                          |                     |
| Apple Safari      | macOS                            |                     |
| Mozilla Firefox   | Windows, macOS, Linux            | Sürüm 55 veya üzeri |
| Google Chrome     | Windows, macOS, Linux, Chrome OS |                     |

## <a name="access-remote-resources-feed"></a>Uzak kaynak akışına erişin

Bir tarayıcıda, [Windows Sanal Masaüstü Web istemcisine](https://rdweb.wvd.microsoft.com/arm/webclient) gidin ve Kullanıcı hesabınızla oturum açın.

>[!NOTE]
>Windows sanal masaüstü Fall 2019 sürümünü kullanıyorsanız, kaynaklarınıza bağlanmak için [Bu makaledeki](./virtual-desktop-fall-2019/connect-web-2019.md) Web istemcisi bağlantısını kullanın.

>[!NOTE]
>Windows sanal masaüstü için kullanmak istediğinden farklı bir Azure Active Directory hesabıyla oturum açtıysanız, oturumunuzu açmanız veya özel bir tarayıcı penceresi kullanmanız gerekir.

Oturum açtıktan sonra, artık kaynakların bir listesini görmeniz gerekir. Kaynakları, **tüm kaynaklar** sekmesindeki normal bir uygulama gibi seçerek de başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Web istemcisini kullanma hakkında daha fazla bilgi edinmek için [Web istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client)konusunu inceleyin.
