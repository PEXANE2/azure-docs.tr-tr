---
title: Azure Relay Karma Bağlantılar-.NET 'teki HTTP istekleri
description: .NET’te Azure Relay Karma Bağlantılar HTTP istekleri için bir C# konsol uygulaması yazın.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 270e1bf0f9655705afe4d3657af03152957227c3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85317067"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>.NET’te Relay Karma Bağlantılar HTTP istekleri ile çalışmaya başlama
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, HTTP protokolünü kullanarak ileti gönderen ve alan .NET gönderici ve alıcı uygulamaları oluşturacaksınız. Uygulamalar Azure Relay Karma Bağlantılar özelliğini kullanır. Genel olarak Azure Relay hakkında bilgi edinmek için bkz. [Azure Relay](relay-what-is-it.md). 

Bu hızlı başlangıçta aşağıdaki adımları uygulayın:

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamaları çalıştırın. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
* Azure aboneliği. Bir tane yoksa, başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="create-a-namespace"></a>Ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetine ileti göndermek Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](../../includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın. Konsol penceresinde aşağıdaki metni görürsünüz:

    ```
    Online
    Server listening
    ```
1. İstemci uygulamasını çalıştırın. İstemci penceresinde `hello!` görürsünüz. İstemci sunucuya bir HTTP isteği gönderdi ve sunucu bir ile yanıt verdi `hello!` . 
3. Şimdi, konsol pencerelerini kapatmak için her iki pencerede **ENTER**’a basın. 

Tebrikler, tamamlanmış bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, ileti göndermek ve almak için HTTP kullanan .NET istemci ve sunucu uygulamaları oluşturdunuz. Azure Relay Karma Bağlantılar özelliği, ileti göndermek ve almak için WebSockets kullanımını da destekler. WebSockets Azure Relay Karma Bağlantılar nasıl kullanacağınızı öğrenmek için bkz. [WebSockets hızlı başlangıç](relay-hybrid-connections-dotnet-get-started.md).

Bu hızlı başlangıçta, istemci ve sunucu uygulamaları oluşturmak için .NET Framework kullandınız. Node.js kullanarak istemci ve sunucu uygulamalarının nasıl yazılacağını öğrenmek için [Node.js WebSockets hızlı başlangıç](relay-hybrid-connections-node-get-started.md) veya [Node.js http hızlı başlangıç](relay-hybrid-connections-http-requests-dotnet-get-started.md)başlığına bakın.
