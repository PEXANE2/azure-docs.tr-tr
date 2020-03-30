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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77198436"
---
#### <a name="azure-storage-sdk-version-in-functions-1x"></a>Fonksiyonlar 1.x'te Azure Depolama SDK sürümü

Functions 1.x'te, Depolama tetikleyicileri ve bağlamaları Azure Depolama SDK'sının[(WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet paketi) 7.2.1 sürümünü kullanır. Depolama SDK'sının farklı bir sürümüne başvurur ve işlev imzanızda bir Depolama SDK türüne bağlanırsanız, Işlevler çalışma zamanı bu türe bağlanamayabileceğini bildirebilir. Çözüm, projenizin [WindowsAzure.Storage 7.2.1'e](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)atıfta bulunarak olduğundan emin olmaktır.
