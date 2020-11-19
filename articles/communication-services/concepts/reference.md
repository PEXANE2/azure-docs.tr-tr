---
title: Azure Iletişim Hizmetleri için başvuru belgelerine genel bakış
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri başvuru belgeleri hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/29/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: a6d554b6addb85e444da9e2d72b66812252730cd
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94888632"
---
# <a name="reference-documentation-overview"></a>Başvuru belgelerine genel bakış

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Aşağıdaki tabloda, kullanılabilen Iletişim Hizmetleri paketlerinin ve ilgili başvuru belgelerinin ayrıntıları verilmektedir:

<!--note that this table also exists here and should be synced: https://github.com/Azure/Communication/blob/master/README.md -->

| Alan           | JavaScript | .NET | Python | Java SE | iOS | Android | Diğer                          |
| -------------- | ---------- | ---- | ------ | ---- | -------------- | -------------- | ------------------------------ |
| Azure Resource Manager | -         | [NuGet](https://www.nuget.org/packages/Azure.ResourceManager.Communication)    |   [Pypı](https://pypi.org/project/azure-mgmt-communication/)    |  -  | -              | -  | [GitHub aracılığıyla git](https://github.com/Azure/azure-sdk-for-go/releases/tag/v46.3.0) |
| Common         | [npm](https://www.npmjs.com/package/@azure/communication-common)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Common/)    | YOK      | [Maven](https://search.maven.org/search?q=a:azure-communication-common)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases/tag/1.0.0-beta.1)            | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-common)             | -                              |
| Yönetim | [npm](https://www.npmjs.com/package/@azure/communication-administration)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Administration)    | [Pypı](https://pypi.org/project/azure-communication-administration/)      | [Maven](https://search.maven.org/search?q=a:azure-communication-administration)   | -              | -              | -                            |
| Sohbet           | [npm](https://www.npmjs.com/package/@azure/communication-chat)        | [NuGet](https://www.nuget.org/packages/Azure.Communication.Chat)     | [Pypı](https://pypi.org/project/azure-communication-chat/)     | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | [GitHub](https://github.com/Azure/azure-sdk-for-ios/releases)  | [Maven](https://search.maven.org/search?q=a:azure-communication-chat)   | -                              |
| SMS            | [npm](https://www.npmjs.com/package/@azure/communication-sms)         | [NuGet](https://www.nuget.org/packages/Azure.Communication.Sms)    | [Pypı](https://pypi.org/project/azure-communication-sms/)       | [Maven](https://search.maven.org/artifact/com.azure/azure-communication-sms)   | -              | -              | -                              |
| Events        | [npm](https://www.npmjs.com/package/@azure/communication-calling)         | -      | -      | -     | [GitHub](https://github.com/Azure/Communication/releases/tag/v1.0.0-beta.2) ([docs](/objectivec/communication-services/calling/))     | [Maven](https://search.maven.org/artifact/com.azure.android/azure-communication-calling/)            | -                              |
| Başvuru Belgeleri     | [belgeler](https://azure.github.io/azure-sdk-for-js/communication.html)         | [belgeler](https://azure.github.io/azure-sdk-for-net/communication.html)      | -      | [belgeler](http://azure.github.io/azure-sdk-for-java/communication.html)     | -     | -            | -                              |