---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4c8e7e5272f180c482ca7fdd44302f49eb888b25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188231"
---
Kurumsal bir çözümle oluşturulmuş (önerilen) bir kök sertifika kullanın veya kendi imzalı bir sertifika oluşturun. Kök sertifikayı oluşturduktan sonra, Genel sertifika verilerini (özel anahtar değil) Base64 kodlanmış X.509 .cer dosyası olarak dışa aktarın. Ardından, ortak sertifika verilerini Azure sunucusuna yükleyin.

* **Kurumsal sertifika:** Kurumsal bir çözüm kullanıyorsanız, varolan sertifika zincirinizi kullanabilirsiniz. Kullanmak istediğiniz kök sertifikası için .cer dosyasını edinin.
* **Kendi imzalı kök sertifikası:** Kurumsal sertifika çözümü kullanmıyorsanız, kendi imzalı kök sertifikası oluşturun. Aksi takdirde, oluşturduğunuz sertifikalar P2S bağlantılarınızla uyumlu olmaz ve istemciler bağlanmaya çalıştıklarında bir bağlantı hatası alır. Azure PowerShell, MakeCert veya OpenSSL kullanabilirsiniz. Aşağıdaki makalelerdeki adımlar, uyumlu bir kendi imzalı kök sertifikasının nasıl oluşturacağınızı açıklar:

  * [Windows 10 PowerShell yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): Bu yönergeler, sertifika oluşturmak için Windows 10 ve PowerShell gerektirir. Kök sertifikadan oluşturulan istemci sertifikaları tüm desteklenen P2S istemcilere yüklenebilir.
  * [MakeCert yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): Sertifikaları oluşturmak için kullanabileceğiniz bir Windows 10 bilgisayarınızın yoksa MakeCert kullanın. MakeCert amortismana hazır olsa da, sertifika oluşturmak için yine de kullanabilirsiniz. Kök sertifikadan oluşturduğunuz istemci sertifikaları desteklenen herhangi bir P2S istemcisi üzerinde yüklenebilir.
  * [Linux yönergeleri](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)
