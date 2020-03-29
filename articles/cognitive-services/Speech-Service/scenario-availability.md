---
title: Senaryo Kullanılabilirliği - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma SDK programlama dilleri ve ortamları geniş bir yelpazede birçok senaryo özellikleri. Tüm senaryolar henüz tüm programlama dillerinde veya tüm ortamlarda kullanılamaz. Aşağıda her senaryonun kullanılabilirliği listelenmiştir.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: cbasoglu
ms.openlocfilehash: 6f8f892e7ca81881b0cc00e1708e3f05052c573c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76264212"
---
# <a name="scenario-availability"></a>Senaryo Kullanılabilirliği

Konuşma SDK programlama dilleri ve ortamları geniş bir yelpazede birçok senaryo özellikleri. Tüm senaryolar henüz tüm programlama dillerinde veya tüm ortamlarda kullanılamaz. Aşağıda her senaryonun kullanılabilirliği listelenmiştir.

- **Konuşma Tanıma (SR), İfade Listesi, Niyet, Çeviri ve Şirket İçi kaplar**
  - Ok bağlantısının olduğu tüm programlama dilleri/ortamları <img src="media/index/link.jpg" height="15" width="15"></img> quickstart [tabloda burada](https://aka.ms/csspeech).
- **Metinden Konuşmaya (TTS)**
  - C++/Windows & Linux
  - C#/Windows & UWP & Birliği
  - Java (Jre ve Android)
  - Python
  - Swift
  - Objective-C
  - TTS REST API her durumda kullanılabilir.
- **Anahtar Kelime Tespit (KWS)**
  - C++/Windows & Linux
  - C#/Windows & Linux
  - Python/Windows & Linux
  - Java/Windows & Linux & Android (Konuşma Cihazları SDK)
  - Anahtar kelime tespit (KWS) işlevi herhangi bir mikrofon türüyle çalışabilir, resmi KWS desteği, ancak şu anda Azure Kinect DK donanımında veya Konuşma Aygıtları SDK'da bulunan mikrofon dizileriyle sınırlıdır
- **Ses yardımcıları**
  - C++/Windows & Linux & macOS
  - C#/Windows
  - Java/Windows & Linux & macOS & Android (Konuşma Cihazları SDK)
- **Konuşma Transkripsiyonu**
  - C++/Windows & Linux
  - C# (Framework & .NET Core)/Windows & UWP & Linux
  - Java/Windows & Linux & Android (Konuşma Cihazları SDK)
- **Çok cihazlı Konuşma**
  - C++/Windows
  - C# (Framework & .NET Core)/Windows
- **Çağrı merkezi Transkripsiyonu**
  - REST API ve her durumda kullanılabilir
- **Codec Sıkıştırılmış Ses Girişi**
  - C++/Linux
  - C#/Linux
  - Java/Linux, Android ve iOS
