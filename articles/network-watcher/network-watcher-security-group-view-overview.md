---
title: Azure Ağ İzleyicisi'nde güvenlik grubu görünümüne giriş | Microsoft Dokümanlar
description: Bu sayfa, Ağ İzleyicisi güvenlik görüntüleme özelliğine genel bir bakış sağlar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: damendo
ms.openlocfilehash: 18619cc0f7a21a39d0802575c484c68c5fa1a624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840766"
---
# <a name="introduction-to-effective-security-rules-view-in-azure-network-watcher"></a>Azure Ağ İzleyicisinde Etkili güvenlik kuralları görünümüne giriş

Ağ Güvenliği grupları bir alt ağ düzeyinde veya NIC düzeyinde ilişkilidir. Bir alt net düzeyinde ilişkilendirildiğinde, alt ağdaki tüm VM örnekleri için geçerlidir. Etkili güvenlik kuralları görünümü, yapılandırmahakkında bilgi sağlayan sanal bir makine için NIC ve alt ağ düzeyinde ilişkili tüm NSG'leri ve kuralları döndürür. Buna ek olarak, etkili güvenlik kuralları bir VM'deki NIC'lerin her biri için döndürülür. Etkili güvenlik kuralları görünümünü kullanarak, açık bağlantı noktaları gibi ağ güvenlik açıkları için bir VM değerlendirebilirsiniz. Ayrıca, Ağ Güvenlik Grubu'nuzun yapılandırılmış ve [onaylanan güvenlik kuralları arasındaki karşılaştırmaya](network-watcher-nsg-auditing-powershell.md)bağlı olarak beklendiği gibi çalışıp çalışmadığını da doğrulayabilirsiniz.

Daha genişletilmiş bir kullanım örneği güvenlik uyumluluğu ve denetimindedir. Kuruluşunuzdaki güvenlik yönetişimi için bir model olarak açıklayıcı bir güvenlik kuralları kümesi tanımlayabilirsiniz. Ağınızdaki Her VM'nin etkili kurallarıyla ön yazı kuralları karşılaştırılarak periyodik uyumluluk denetimi programlı bir şekilde uygulanabilir.

Portal kuralları her Ağ Arabirimi için görüntülenir ve gelen vs giden göre gruplanır. Bu, sanal bir makineye uygulanan kurallara basit bir görünüm sağlar. Bir csv dosyasına sekme olursa olsun tüm güvenlik kurallarını kolayca indirmek için bir indirme düğmesi sağlanır.

![güvenlik grubu görünümü][1]

Kurallar seçilebilir ve Ağ Güvenlik Grubu'nu, kaynak ve hedef öneklerini göstermek için yeni bir bıçak açılır. Bu bıçaktan doğrudan Ağ Güvenlik Grubu kaynağına gidebilirsiniz.

![Drilldown][2]

### <a name="next-steps"></a>Sonraki adımlar

[PowerShell ile Denetim Ağı Güvenlik Grubu ayarlarını](network-watcher-nsg-auditing-powershell.md) ziyaret ederek Ağ Güvenliği Grubu ayarlarınızı nasıl denetlerinizi öğrenin

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









