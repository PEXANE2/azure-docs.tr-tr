---
title: Azure'da alt ağ uzantısı | Microsoft Dokümanlar
description: Azure'da alt ağ uzantısı hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anupam-p
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2019
ms.author: anupand
ms.openlocfilehash: f718471c3f79e9a33b0e03b088f8c8d2ae0231d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73587517"
---
# <a name="subnet-extension"></a>Alt ağ uzantısı
Genel buluta iş yükü geçişi dikkatli planlama ve koordinasyon gerektirir. Önemli noktalardan biri IP adreslerinizi koruyabilmek olabilir. Özellikle uygulamalarınızda IP adresi bağımlılığı varsa veya belirli IP adreslerini kullanmak için uyumluluk gereksinimleriniz varsa bu önemli olabilir. Azure Sanal Ağı, seçtiğiniz bir IP adresi aralığını kullanarak VNet ve Subnets oluşturmanıza izin vererek bu sorunu sizin için çözer.

Yukarıdaki gereksinim, bazı uygulamaları şirket içinde tutmak için ek bir gereksinimle birleştiğinde geçişler biraz zorolabilir. Bu durumda, her iki taraftaki IP adreslerini yeniden numaralandırmadan uygulamaları Azure ve şirket içi arasında bölmeniz gerekir. Ayrıca, uygulamaların aynı ağda yatmaktadır gibi iletişim kurmasına izin vermeniz gerekir.

Yukarıdaki soruna bir çözüm subnet uzantısıdır. Bir ağı genişletmek, uygulamaların farklı fiziksel konumlarda var olduklarında aynı yayın alanı üzerinden konuşmasına olanak tanır ve ağ topolojinizi yeniden architecta etme gereksinimini ortadan kaldırır. 

Ağınızı genişletmek genel olarak iyi bir uygulama olmasa da, aşağıdaki kullanım örnekleri bunu gerekli kılabilir.

- **Aşamalı Geçiş**: En yaygın senaryo geçişinizi aşamalı olarak yapmak istemenizdir. Önce birkaç uygulama getirmek istiyorsunuz ve zaman içinde uygulamaların geri kalanını Azure'a taşıyın.
- **Gecikme süresi**: Düşük gecikme süresi gereksinimleri, bazı uygulamaları veri merkezinize mümkün olduğunca yakın olduğundan emin olmak için şirket içinde tutmanız için başka bir neden olabilir.
- **Uyumluluk**: Diğer bir kullanım örneği de, bazı uygulamalarınızı şirket içinde tutmak için uyumluluk gereksinimleriniz olmasıolabilir.
 
> [!NOTE] 
> Gerekli olmadıkça alt ağlarınızı uzatmamalısınız. Alt ağlarınızı genişlettiğiniz durumlarda, bunu bir ara adım haline getirmeye çalışmalısınız. Zamanla, şirket içi ağınızdaki uygulamaları yeniden numaralandırmayı denemeli ve bunları Azure'a geçirmelisiniz.

Bir sonraki bölümde, alt ağlarınızı Azure'a nasıl genişletebileceğinizi tartışacağız.


## <a name="extend-your-subnet-to-azure"></a>Alt ağınızı Azure'a genişletin
 Katman-3 kaplama ağı tabanlı bir çözüm kullanarak şirket içi alt ağlarınızı Azure'a genişletebilirsiniz. Çoğu çözüm, katman-3 kaplama ağı kullanarak katman-2 ağını genişletmek için VXLAN gibi bir kaplama teknolojisi kullanır. Aşağıdaki diyagram genelleştirilmiş bir çözüm gösterir. Bu çözümde, Azure ve şirket içi olan her iki tarafta da aynı alt ağ bulunur. 

![Alt ağ uzantısı örneği](./media/subnet-extension/subnet-extension.png)

Alt ağdaki IP adresleri Azure'daki ve şirket içi VM'lere atanır. Hem Azure hem de şirket içi ağlarına bir NVA takılır. Azure'daki bir VM şirket içi ağda bir VM ile konuşmaya çalıştığında, Azure NVA paketi yakalar, kapsüller ve VPN/Express Route üzerinden şirket içi ağa gönderir. Şirket içi NVA paketi alır, decapsulates ve kendi ağında amaçlanan alıcıya iletir. İade trafiği benzer bir yol ve mantık kullanır.

Yukarıdaki örnekte, Azure NVA ve şirket içi NVA iletişim kurar ve birbirinin arkasındaki IP adresleri hakkında bilgi edinir. Daha karmaşık ağlar da NVAs ve arkalarındaki IP adresleri arasındaki eşleme tutan bir haritalama hizmeti olabilir. Bir NVA bir paket aldığında, arkasında hedef IP adresi olan NVA'nın adresini bulmak için eşleme hizmetini sorgular.

Bir sonraki bölümde, Azure'da test ettiğimiz alt ağ uzantısı çözümleriyle ilgili ayrıntıları bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar 
[Satıcı çözümlerini kullanarak alt ağınızı Azure'a genişletin.](https://github.com/microsoft/Azure-LISP)