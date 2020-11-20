---
title: Azure 'da SAP HANA boyutlandırma (büyük örnekler) | Microsoft Docs
description: Azure 'da SAP HANA boyutlandırma (büyük örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1750f1d61028cb186b02251b551b7a798e1df9d6
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94967542"
---
# <a name="sizing"></a>Boyutlandırma

HANA büyük örneğinin boyutlandırılması, genel olarak HANA için boyutdan farklı değildir. Diğer RDBMS 'den HANA 'ya taşımak istediğiniz mevcut ve dağıtılan sistemler için SAP, mevcut SAP sistemlerinizde çalışan bir dizi rapor sağlar. Veritabanı HANA 'ya taşınırsa, bu raporlar verileri denetler ve HANA örneği için bellek gereksinimlerini hesaplar. Bu raporları çalıştırma ve en son düzeltme eklerini veya sürümlerini edinme hakkında daha fazla bilgi için aşağıdaki SAP notlarını okuyun:

- [SAP Note #1793345-HANA 'da SAP Suite için boyutlandırma](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP Note #1872170 HANA ve S/4 HANA boyutlandırma raporunda Suite](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP Note #2121330-SSS: SAP BW HANA boyutlandırma raporunda](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP Note #1736976-HANA 'da siyah beyaz için boyutlandırma raporu](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Note #2296290-bit GENIŞLIĞI için HANA 'da yeni boyutlandırma raporu](https://launchpad.support.sap.com/#/notes/2296290)

Yeşil alan uygulamaları için SAP Quick sizer, HANA 'nın en üstünde SAP yazılımı uygulamasının bellek gereksinimlerini hesaplamak için kullanılabilir.

Veri hacmi arttıkça, HANA artışı için bellek gereksinimleri. Gelecekte neler olduğunu tahmin etmenize yardımcı olması için geçerli bellek tüketiminizi göz önünde bulundurun. Bellek gereksinimlerine bağlı olarak, talep içeriğinizi HANA büyük örnek SKU 'larından birine eşleyebilirsiniz.

**Sonraki adımlar**
- [Ekleme gereksinimlerini](hana-onboarding-requirements.md) inceleyin