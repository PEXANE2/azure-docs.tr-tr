---
title: Azure Relay Karma Bağlantılar-WebSockets-Node
description: Azure Relay Karma Bağlantılar Web Yuvaları için bir Node.js konsol uygulaması yazın.
ms.topic: conceptual
ms.date: 06/23/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: cf660d7a776c0dfa48b6c023fa0fa3516a3a0ea5
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90527420"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-nodejs"></a>Node.js geçiş Karma Bağlantılar WebSockets ile çalışmaya başlama

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, Azure Relay Karma Bağlantılar kullanarak ileti gönderen ve alan Node.js gönderen ve alıcı uygulamalar oluşturacaksınız. Genel olarak Azure Relay hakkında bilgi edinmek için bkz. [Azure Relay](relay-what-is-it.md). 

Bu hızlı başlangıçta aşağıdaki adımları uygulayın: 

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamaları çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/).
- Azure aboneliği. Bir tane yoksa, başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="create-a-namespace"></a>Ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetinden ileti göndermek için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

1. Sunucu uygulamasını çalıştırın: Node.js komut istemine `node listener.js` yazın.
2. İstemci uygulamasını çalıştırın: Node.js komut istemine `node sender.js` yazın ve bazı metinler girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

    ![Konsol pencereleri, hem sunucu hem de istemci uygulamalarını test ediyor.](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Tebrikler, Node.js kullanarak uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, ileti göndermek ve almak için WebSockets kullanan Node.js istemci ve sunucu uygulamaları oluşturdunuz. Azure Relay Karma Bağlantılar özelliği, ileti göndermek ve almak için HTTP kullanmayı da destekler. Azure Relay Karma Bağlantılar ile HTTP 'yi nasıl kullanacağınızı öğrenmek için bkz. [Node.js http hızlı](relay-hybrid-connections-http-requests-node-get-started.md)başlangıcı.

Bu hızlı başlangıçta, istemci ve sunucu uygulamaları oluşturmak için Node.js kullandınız. .NET Framework kullanarak istemci ve sunucu uygulamalarının nasıl yazılacağını öğrenmek için bkz. [.net WebSockets hızlı başlangıç](relay-hybrid-connections-dotnet-get-started.md) veya [.net http hızlı başlangıç](relay-hybrid-connections-http-requests-dotnet-get-started.md).


