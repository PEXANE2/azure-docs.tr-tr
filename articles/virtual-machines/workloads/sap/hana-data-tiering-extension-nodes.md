---
title: Azure'da SAP HANA için veri katmanlama ve uzantı düğümleri (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA için veri katmanlama ve uzantı düğümleri (Büyük Örnekler).
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
ms.openlocfilehash: c3553ac9704ac26d0bdaae0f93b89f41a87ac716
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617156"
---
# <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>SAP HANA veri katmanlama ve uzantı düğümlerini kullanma

SAP, sap BW için farklı SAP NetWeaver sürümleri ve SAP BW/4HANA için bir veri katmanlama modelini destekler. Veri katmanlama modeli hakkında daha fazla bilgi için, [SAP HANA uzantılı HANA'daki SAP belge SAP BW/4HANA ve SAP BW'ye](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#)bakın.
HANA Büyük Örneği ile, SSS ve SAP blog belgelerinde açıklandığı gibi SAP HANA uzantılı düğümlerin seçenek-1 yapılandırmasını kullanabilirsiniz. S72m, S192, S192m, S384 ve S384m: Opsiyon-2 yapılandırmaları aşağıdaki HANA Büyük Örnek SKUs ile ayarlanabilir. 

Belgelere baktığınızda, avantaj hemen görünmeyebilir. Ancak SAP boyutlandırma yönergelerine baktığınızda, seçenek-1 ve opsiyon-2 SAP HANA uzantıdüğümlerini kullanarak bir avantaj görebilirsiniz. Örnekler aşağıdadır:

- SAP HANA boyutlandırma yönergeleri genellikle bellek olarak veri hacmi nin iki katı gerektirir. SAP HANA örneğini sıcak verilerle çalıştırdığınızda, verilerle dolu belleğin yalnızca yüzde 50'si veya daha azına sahip olursunuz. Belleğin geri kalanı ideal sap HANA işini yaparken tutulur.
- Yani bir SAP BW veritabanı çalıştıran bellek 2 TB ile BIR HANA Büyük Örnek S192 biriminde, veri hacmi olarak sadece 1 TB var.
- Seçenek-1'in ek bir SAP HANA uzantı düğümü, ayrıca bir S192 HANA Büyük Örnek SKU kullanıyorsanız, veri hacmi için ek bir 2-TB kapasite sağlar. Seçenek-2 yapılandırmasında, sıcak veri hacmi için ek bir 4 TB alırsınız. Sıcak düğümle karşılaştırıldığında, "sıcak" uzantı düğümünün tam bellek kapasitesi seçenek-1 için veri depolamak için kullanılabilir. Çift bellek seçenek-2 SAP HANA uzantı nodyapılandırmaveri hacmi için kullanılabilir.
- Verileriniz için 3 TB kapasiteye ve seçenek-1 için 1:2 sıcaktan sıcaka oranına sahipsiniz. 5 TB veri ve seçenek-2 uzantı nodyapılandırması ile 1:4 oranı var.

Bellekle karşılaştırıldığında veri hacmi ne kadar yüksekse, istediğiniz sıcak verinin disk depolamada depolanması olasılığı da o kadar yüksek tir.

**Sonraki adımlar**
- [Azure'da SAP HANA (Büyük Örnekler) mimarisine](hana-architecture.md) bakın