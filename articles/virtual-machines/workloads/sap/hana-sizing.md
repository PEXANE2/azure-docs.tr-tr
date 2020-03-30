---
title: SAP HANA'nın Azure'da Boyutlandırılması (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'nın Azure'da boyutlandırması (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 404f8318816edcc2cfd1c50ca42304ff6ec93039
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616903"
---
# <a name="sizing"></a>Boyutlandırma

HANA Büyük Örnek için boyutlandırma genel olarak HANA için boyutlandırma farklı değildir. Sap, diğer RDBMS'den HANA'ya taşımak istediğiniz varolan ve dağıtılan sistemler için, varolan SAP sistemlerinizde çalışan bir dizi rapor sağlar. Veritabanı HANA'ya taşınırsa, bu raporlar verileri denetler ve HANA örneği için bellek gereksinimlerini hesaplar. Bu raporların nasıl çalıştırılacakları ve en son düzeltme ekleri veya sürümlerini nasıl elde edeceğimiz hakkında daha fazla bilgi için aşağıdaki SAP Notlarını okuyun:

- [SAP Note #1793345 - HANA'da SAP Suite için boyutlandırma](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP Note #1872170 - HANA ve S/4 HANA boyutlandırma raporunda süit](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP Not #2121330 - SSS: SAP BW HANA boyutlandırma raporu](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP Not #1736976 - HANA BW için boyutlandırma raporu](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Not #2296290 - HANA BW için yeni boyutlandırma raporu](https://launchpad.support.sap.com/#/notes/2296290)

Yeşil alan uygulamaları için, SAP Hızlı Boyutlayıcı HANA üstüne SAP yazılımının uygulanmasıbellek gereksinimlerini hesaplamak için kullanılabilir.

Veri hacmi arttıkça HANA için bellek gereksinimleri artar. Gelecekte ne olacağını tahmin etmek için mevcut bellek tüketiminize dikkat edin. Bellek gereksinimlerine bağlı olarak, talebinizi HANA Büyük Örnek SUK'larından birine eşleyebilirsiniz.

**Sonraki adımlar**
- [Onboarding gereksinimlerine](hana-onboarding-requirements.md) bakın