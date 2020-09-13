---
title: Web istemcisi ile Windows sanal masaüstüne bağlanma-Azure
description: Web istemcisini kullanarak Windows sanal masaüstüne bağlanma.
author: Heidilohr
ms.topic: how-to
ms.date: 09/24/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: c8a4b22966c3d4db268e212bb3f2d1bbb78fee74
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400645"
---
# <a name="connect-to-windows-virtual-desktop-with-the-web-client"></a>Web istemcisiyle Windows sanal masaüstüne bağlanma

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/connect-web-2019.md)bakın.

Web istemcisi, Windows sanal masaüstü kaynaklarınıza, uzun yükleme işlemi olmadan bir Web tarayıcısından erişmenizi sağlar.

>[!NOTE]
>Web istemcisinde Şu anda mobil işletim sistemi desteği yok.

## <a name="supported-operating-systems-and-browsers"></a>Desteklenen işletim sistemleri ve tarayıcılar

HTML5 özellikli herhangi bir tarayıcı çalışması gerektiği sürece, aşağıdaki işletim sistemlerini ve tarayıcıları resmi olarak destekliyoruz.

| Tarayıcı           | Desteklenen işletim sistemi                     | Notlar               |
|-------------------|----------------------------------|---------------------|
| Microsoft Edge    | Windows                          |                     |
| Internet Explorer | Windows                          | Sürüm 11 veya üzeri |
| Apple Safari      | macOS                            |                     |
| Mozilla Firefox   | Windows, macOS, Linux            | Sürüm 55 veya üzeri |
| Google Chrome     | Windows, macOS, Linux, Chrome OS |                     |

## <a name="access-remote-resources-feed"></a>Uzak kaynak akışına erişin

Bir tarayıcıda, üzerinde Windows sanal masaüstü web istemcisinin Azure Resource Manager tümleşik sürümüne gidin <https://rdweb.wvd.microsoft.com/arm/webclient> ve Kullanıcı hesabınızla oturum açın.

>[!NOTE]
>Azure Resource Manager tümleştirme olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, <https://rdweb.wvd.microsoft.com/webclient> bunun yerine kaynaklarınıza bağlanın.
>
> US Gov portalını kullanıyorsanız, kullanın <https://rdweb.wvd.azure.us/arm/webclient/index.html> .

>[!NOTE]
>Windows sanal masaüstü için kullanmak istediğinden farklı bir Azure Active Directory hesabıyla oturum açtıysanız, oturumunuzu açmanız veya özel bir tarayıcı penceresi kullanmanız gerekir.

Oturum açtıktan sonra, artık kaynakların bir listesini görmeniz gerekir. Kaynakları, **tüm kaynaklar** sekmesindeki normal bir uygulama gibi seçerek de başlatabilirsiniz.

## <a name="using-an-input-method-editor"></a>Giriş Yöntemi Düzenleyicisi kullanma

Web istemcisi, **1.0.21.16 veya üzeri**sürümlerde uzak oturumunda bir Giriş Yöntemi Düzenleyicisi 'NI (IME) kullanmayı destekler. Uzak oturumda kullanmak istediğiniz klavyeye ait dil paketinin konak sanal makinesine yüklenmesi gerekir. Uzak oturumda dil paketleri ayarlama hakkında daha fazla bilgi edinmek için [Windows 10 çoklu oturum görüntüsüne dil paketleri ekleme ' ye](language-packs.md)göz atın.

Web istemcisini kullanarak IME girişini etkinleştirmek için:

1. Uzak oturuma bağlanmadan önce Web istemcisi **ayarları** paneline gidin.

2. **Giriş yöntemi düzenleyicisini etkinleştir** ayarını **Açık**olarak değiştirin.

3. Açılan menüde, uzak oturumda kullanacağınız klavyeyi seçin.

4. Uzak oturuma bağlanın.

Uzak oturuma odaklandığınızda Web istemcisi yerel IME penceresini bastırır. Uzak oturuma zaten bağlandıktan sonra IME ayarlarını değiştirmek herhangi bir etkiye sahip olmayacaktır.

>[!NOTE]
>Dil paketi konak sanal makinesine yüklenmemişse, uzak oturum varsayılan olarak Ingilizce (Birleşik Devletler) klavyesine sahip olur.

## <a name="next-steps"></a>Sonraki adımlar

Web istemcisini kullanma hakkında daha fazla bilgi edinmek için [Web istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client)konusunu inceleyin.
