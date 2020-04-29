---
title: include dosyası
description: include dosyası
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2020
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 6a37850eb6536c5399d63144e60ea210fbc194d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77198436"
---
#### <a name="azure-storage-sdk-version-in-functions-1x"></a>1. x Işlevleri içindeki Azure Storage SDK sürümü

1. x Işlevleri içinde depolama Tetikleyicileri ve bağlamaları, Azure Storage SDK 'sının ([windowsazure. Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet paketi) 7.2.1 sürümünü kullanır. Depolama SDK 'sının farklı bir sürümüne başvurdıysanız ve işlev imzanıza bir depolama SDK 'Sı türüne bağlarsanız, Işlevler çalışma zamanı bu türe bağlanamaz. Çözüm, projenizin [windowsazure. Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)başvurduğundan emin olmak için.
