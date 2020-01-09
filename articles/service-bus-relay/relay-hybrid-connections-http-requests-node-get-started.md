---
title: Azure Relay Karma Bağlantılar-düğümdeki HTTP istekleri
description: Node’da Azure Relay Karma Bağlantılar HTTP istekleri için bir Node.js konsol uygulaması yazın.
services: service-bus-relay
documentationcenter: node
author: clemensv
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 11/01/2018
ms.author: clemensv
ms.openlocfilehash: d71386b86bf7133bb73ddce2e65c3b88743009ad
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75462034"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-node"></a>Node’da Relay Karma Bağlantılar HTTP istekleri ile çalışmaya başlama

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, HTTP protokolünü kullanarak ileti gönderen ve alan Node. js gönderici ve alıcı uygulamaları oluşturacaksınız. Uygulamalar Azure Relay Karma Bağlantılar özelliğini kullanır. Genel olarak Azure Relay hakkında bilgi edinmek için bkz. [Azure Relay](relay-what-is-it.md). 

Bu hızlı başlangıçta aşağıdaki adımları uygulayın:

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamaları çalıştırın.

## <a name="prerequisites"></a>Ön koşullar
- [Node.js](https://nodejs.org/en/).
- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-a-namespace-using-the-azure-portal"></a>Azure portalı kullanılarak ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection-using-the-azure-portal"></a>Azure portalını kullanarak karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için bir Node.js konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-http-requests-node-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma

Relay hizmetine ileti göndermek için herhangi bir HTTP istemcisini kullanabilir veya bir Node.js konsol uygulaması yazabilirsiniz.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-http-requests-node-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

1. Sunucu uygulamasını çalıştırın: Node.js komut istemine `node listener.js` yazın.
2. İstemci uygulamasını çalıştırın: Node.js komut istemine `node sender.js` yazın ve bazı metinler girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni çıkardığından emin olun.

Tebrikler, Node.js kullanarak uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, ileti göndermek ve almak için HTTP kullanan Node. js istemcisi ve sunucu uygulamaları oluşturdunuz. Azure Relay Karma Bağlantılar özelliği, ileti göndermek ve almak için WebSockets kullanımını da destekler. WebSockets Azure Relay Karma Bağlantılar nasıl kullanacağınızı öğrenmek için bkz. [WebSockets hızlı başlangıç](relay-hybrid-connections-node-get-started.md).

Bu hızlı başlangıçta, istemci ve sunucu uygulamaları oluşturmak için Node. js ' yi kullandınız. .NET Framework kullanarak istemci ve sunucu uygulamalarının nasıl yazılacağını öğrenmek için bkz. [.net WebSockets hızlı başlangıç](relay-hybrid-connections-dotnet-get-started.md) veya [.net http hızlı başlangıç](relay-hybrid-connections-http-requests-dotnet-get-started.md).
