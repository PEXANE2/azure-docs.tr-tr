---
title: Azure'da SAP HANA'ya altyapı ve bağlantı (büyük örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA'yı kullanmak için gerekli bağlantı altyapısını yapılandırın (büyük örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/12/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cea89087742f1987f693b8bfb627bd71038a0c14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616916"
---
# <a name="sap-hana-large-instances-deployment"></a>SAP HANA (büyük örnekler) dağıtımı 

Bu makalede, Microsoft'tan Azure'da (büyük örnekler) SAP HANA satın alma işlemini tamamladığınızı varsayar. Bu makaleyi okumadan önce, genel arka plan için, [HANA büyük örnekleri ortak terimler](hana-know-terms.md) ve [HANA büyük örnekleri SUS](hana-available-skus.md)bakın.


Microsoft, HANA büyük örnek birimlerini dağıtmak için aşağıdaki bilgileri gerektirir:

- Müşteri adı.
- İş iletişim bilgileri (e-posta adresi ve telefon numarası dahil).
- Teknik iletişim bilgileri (e-posta adresi ve telefon numarası dahil).
- Teknik ağ iletişim bilgileri (e-posta adresi ve telefon numarası dahil).
- Azure dağıtım bölgesi (örneğin, Batı ABD, Avustralya Doğu veya Kuzey Avrupa).
- Azure'da SAP HANA (büyük örnekler) SKU (yapılandırma).
- Her Azure dağıtım bölgesi için:
    - Azure sanal ağlarını HANA'nın büyük örneklerine bağlayan ER-P2P bağlantıları için bir /29 IP adres aralığı.
    - HANA büyük örnekleri sunucu IP havuzu için kullanılan bir /24 CIDR Blok.
    - Şirket içi HANA Büyük Örnek birimlerine doğrudan yönlendirme yi veya farklı Azure bölgelerindeki HANA Büyük Örnek birimleri arasında yönlendirmeyi etkinleştirmek için [ExpressRoute Global Reach'i](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) kullanırken başka bir /29 IP adresi aralığı ayırmanız gerekir. Bu özel aralık, daha önce tanımladığınız diğer IP adresi aralıklarıyla çakışmayabilir.
- SANAL AĞ adres alanı özniteliğinde kullanılan IP adresi aralığı değerleri, HANA'nın büyük örneklerine bağlanan her Azure sanal ağının alanı özniteliği.
- Her HANA büyük örnek sistemi için veriler:
  - Tam nitelikli bir etki alanı adı ile ideal olarak istenen ana bilgisayar adı.
  - Server IP havuzu adres aralığının dışında HANA büyük örnek birimi için istenen IP adresi. (Sunucu IP havuzu adres aralığındaki ilk 30 IP adresi HANA büyük örnekleri nde dahili kullanım için ayrılmıştır.)
  - SAP HANA örneği için SAP HANA SID adı (gerekli SAP HANA ile ilgili disk hacimleri oluşturmak için gereklidir). Microsoft NFS birimleri üzerinde sidadm için izinleri oluşturmak için HANA SID gerekir. Bu birimler HANA büyük örnek birimine eklenir. HANA SID, monte edilen disk birimlerinin ad bileşenlerinden biri olarak da kullanılır. Birimde birden fazla HANA örneği çalıştırmak istiyorsanız, birden çok HANA SID'yi listelmelisiniz. Her biri ayrı bir birim kümesi atanmış alır.
  - Linux OS'de, sidadm kullanıcısının bir grup kimliği vardır. Bu kimlik, gerekli SAP HANA ile ilgili disk birimlerini oluşturmak için gereklidir. SAP HANA yüklemesi genellikle 1001 kişilik grup kimliğine sahip sapsys grubunu oluşturur. Sidadm kullanıcısı bu grubun bir parçasıdır.
  - Linux os'de, sidadm kullanıcısının bir kullanıcı kimliği vardır. Bu kimlik, gerekli SAP HANA ile ilgili disk birimlerini oluşturmak için gereklidir. Birimde birkaç HANA örneği çalıştırıyorsanız, tüm sidadm kullanıcılarını listele. 
- Azure HANA'nın büyük örneklerindeki SAP HANA'nın doğrudan bağlanabilmek için Azure aboneliği için Azure abonelik kimliği. Bu abonelik kimliği, HANA büyük örnek birimi veya birimleriyle ücretlendirilecek olan Azure aboneliğine başvurur.

Önceki bilgileri sağladıktan sonra, Microsoft Azure'da SAP HANA'yı (büyük örnekler) karşılar. Microsoft, Azure sanal ağlarınızı HANA'nın büyük örneklerine bağlamak için size bilgi gönderir. HANA büyük örnek birimlerine de erişebilirsiniz.

Microsoft dağıttıktan sonra HANA büyük örneklerine bağlanmak için aşağıdaki sırayı kullanın:

1. [Azure VM'leri HANA'nın büyük örneklerine bağlama](hana-connect-azure-vm-large-instances.md)
2. [Bir VNet'i HANA'ya bağlama büyük örnekleri ExpressRoute](hana-connect-vnet-express-route.md)
3. [Ek ağ gereksinimleri (isteğe bağlı)](hana-additional-network-requirements.md)

