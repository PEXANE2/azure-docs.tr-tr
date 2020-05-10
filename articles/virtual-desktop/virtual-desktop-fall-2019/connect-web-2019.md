---
title: Windows Sanal Masaüstü Web istemcisi 'ni bağlama-Azure
description: Web istemcisini kullanarak Windows sanal masaüstüne bağlanma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c264006d51f8f0f9ee2a56e994a0dc52d24bd7a8
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006032"
---
# <a name="connect-with-the-web-client"></a>Web istemcisiyle bağlanma

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir. Spring 2020 güncelleştirmesinde tanıtılan Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../connect-web.md)bakın.

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

Bir tarayıcıda, [Windows Sanal Masaüstü Web istemcisine](https://rdweb.wvd.microsoft.com/webclient) gidin ve Kullanıcı hesabınızla oturum açın.

>[!NOTE]
>Windows sanal masaüstü Spring 2020 sürümünü kullanıyorsanız, kaynaklarınıza bağlanmak için [Bu makaledeki](../connect-web.md) Web istemcisi bağlantısını kullanın.

>[!NOTE]
>Windows sanal masaüstü için kullanmak istediğinden farklı bir Azure Active Directory hesabıyla oturum açtıysanız, oturumunuzu açmanız veya özel bir tarayıcı penceresi kullanmanız gerekir.

Oturum açtıktan sonra, artık kaynakların bir listesini görmeniz gerekir. Kaynakları, **tüm kaynaklar** sekmesindeki normal bir uygulama gibi seçerek de başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Web istemcisini kullanma hakkında daha fazla bilgi edinmek için [Web istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client)konusunu inceleyin.
