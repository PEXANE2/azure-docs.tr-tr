---
title: Azure'da IBM iş yükleri | Microsoft Dokümanlar
description: Microsoft Azure'u kullanarak IBM z/OS iş yüklerinizi yeniden barındırmak için Microsoft iş ortaklarının ana bilgisayar emülatörlerini ve diğer hizmetleri kullanın.
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 02/22/2019
tags: ''
keywords: ''
ms.openlocfilehash: 13c83c53cdad719d6a4bed4cc1852b85d62082e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68834590"
---
# <a name="ibm-workloads-on-azure"></a>Azure'da IBM iş yükleri

Z/OS'ye dayalı birçok IBM ana bilgisayar iş yükü, işlevsellik kaybı olmadan ve kullanıcılar temel sistemlerindeki değişiklikleri fark etmeden Azure'da çoğaltılabilir. Azure'da uygulamaları yeniden barındırmak, ihtiyacınız olan ana bilgisayar benzeri özelliklerin yanı sıra bulutun esnekliğini, kullanılabilirliğini ve olası maliyet tasarruflarını sağlar.

Azure, mevcut IBM ana bilgisayar ortamlarıyla tümleştirmeyi destekleyerek, anlamlı olan uygulamaları geçirmenize, gerektiğinde karma çözümler çalıştırmanıza ve zaman içinde geçiş yapmanızı sağlar. Azure için varolan ana bilgisayar tabanlı programları tamamen yeniden yazabiliyor olsanız da, bunları yeniden barındırmak daha yaygındır. Yeniden yazma, geçiş projelerine maliyet, karmaşıklık ve zaman ekler. Yeniden barındırma ile şunları yapabilirsiniz:

- Uygulamaları bulut tabanlı bir emülatöre taşıyın.

- Veritabanını bulut tabanlı bir veritabanına geçirin.

- Kod dönüştürme motorlarını kullanarak modülleri ve kodu değiştirin.

Ayrıca, WebSphere ve MQ da dahil olmak üzere IBM yazılımı artık Azure Marketi'ndedir. IBM yazılımı lisansıyla, sanal bir makineyi hızla başlatmak için Azure tarafından sağlanan isteğe bağlı altyapı ölçeklemelerinden yararlanabilirsiniz.

IBM ana bilgisayar sistemlerini Azure'a geçirmenize yardımcı olmak için kapsamlı bir iş ortağı ekosistemi kullanılabilir. Çoğu, uygulamaları aşamalı olarak yeniden yazmaya veya değiştirmeye başlamadan önce mümkün olan her yerde yeniden kullanmanın pragmatik bir yaklaşımını izler. [Azure Ana Bilgisayar Geçiş merkezi'ndeki](https://azure.microsoft.com/migration/mainframe/)iş ortaklarından daha fazla rehberlik ve yardım alın.

**Sonraki adımlar**

- [Ana bilgisayar göçü: mitler ve gerçekler](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [Azure'da IBM zD&T geliştirme/test ortamını yükleme](./install-ibm-z-environment.md)
- [IBM zD&T v1'de Uygulama Geliştiricileri Kontrollü Dağıtım (ADCD) kurma](./demo.md)
- [Azure'da IBM DB2 pureScale](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
