---
title: Azure Röle Karma Bağlantıları - Düğümdeki WebSoks
description: Azure Relay Karma Bağlantılar Web Yuvaları için bir Node.js konsol uygulaması yazın.
services: service-bus-relay
documentationcenter: node
author: spelluru
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 818db4db082a441877b573fd52361e63becce374
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75352667"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-nodejs"></a>Düğüm.js'deki Röle Hibrit Bağlantılar WebSockets ile başlayın

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, Azure Röle'de Hibrit Bağlantılar WebSockets'i kullanarak ileti gönderen ve alan Node.js uygulamaları oluşturursunuz. Azure Rölesi hakkında genel bilgi edinmek için Azure [Röle'ye](relay-what-is-it.md)bakın. 

Bu hızlı başlangıçta, aşağıdaki adımları atarsınız: 

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamaları çalıştırın. 

## <a name="prerequisites"></a>Ön koşullar

- [Düğüm.js](https://nodejs.org/en/).
- Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

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

    ![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Tebrikler, Node.js kullanarak uçtan uca bir Karma Bağlantılar uygulaması oluşturdunuz!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, ileti göndermek ve almak için WebSockets'i kullanan Node.js istemcisi ve sunucu uygulamaları oluşturdunuz. Azure Röle'nin Karma Bağlantılar özelliği, ileti göndermek ve almak için HTTP'yi kullanmayı da destekler. Azure Relay Karma Bağlantıları ile HTTP'yi nasıl kullanacağınızı öğrenmek için [Node.js HTTP quickstart](relay-hybrid-connections-http-requests-node-get-started.md)bölümüne bakın.

Bu hızlı başlatmada, istemci ve sunucu uygulamaları oluşturmak için Node.js kullandınız. .NET Framework'ü kullanarak istemci ve sunucu uygulamalarının nasıl yazılabildiğini öğrenmek için [.NET WebSockets quickstart](relay-hybrid-connections-dotnet-get-started.md) veya [.NET HTTP quickstart](relay-hybrid-connections-http-requests-dotnet-get-started.md)adresine bakın.


