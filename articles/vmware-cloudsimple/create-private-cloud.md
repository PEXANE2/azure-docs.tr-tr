---
title: CloudSimple tarafından Azure VMware çözümü-CloudSimple özel bulutu oluşturma
description: İşlem esnekliği ve sürekliliği ile VMware iş yüklerini buluta genişletmek için CloudSimple özel bulutu oluşturmayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: aacdb57c312946a9ec2b17a8d41aa9150efc277d
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640980"
---
# <a name="create-a-cloudsimple-private-cloud"></a>CloudSimple özel bulutu oluşturma

Özel bulut, ESXi Konakları, vCenter, vSAN ve NSX 'i destekleyen yalıtılmış bir VMware yığınına sahiptir. Özel bulutlar CloudSimple portalı aracılığıyla yönetilir. Kendi yönetim etki alanında kendi vCenter Server kuruluşları vardır. Yığın adanmış düğümlerde ve yalıtılmış tam donanım düğümlerinde çalışır.

Özel bir bulut oluşturmak, ağ altyapısının çeşitli yaygın ihtiyaçlarını ele almanıza yardımcı olur:

* **Büyüme**. Mevcut altyapınız için bir donanım yenileme noktasına ulaştıysanız, özel bir bulut gerekli yeni donanım yatırımı olmadan genişletmenizi sağlar.

* **Hızlı genişletme**. Geçici veya plansız bir kapasite olması halinde bir özel bulut, gecikme olmadan ek kapasite oluşturmanıza olanak sağlar.

* **Daha fazla koruma**. Üç veya daha fazla düğümden oluşan özel bir bulutla, otomatik artıklık ve yüksek kullanılabilirlik koruması alırsınız.

* **Uzun süreli altyapı ihtiyaçları**. Veri merkezleriniz kapasiteniz varsa veya maliyetlerinizi düşürmek için yeniden yapılandırmak istiyorsanız, özel bir bulut, veri merkezlerini devre dışı bırakmanıza ve bulut tabanlı bir çözüme geçiş yaparken Kurumsal işlemleriniz ile uyumlu bir çözüm yapmanıza olanak sağlar.

Özel bir bulut oluşturduğunuzda, tek bir vSphere kümesi ve bu kümede oluşturulan tüm yönetim VM 'Leri alırsınız.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

[Cloudsimple portalına](access-cloudsimple-portal.md)erişin.

## <a name="create-a-new-private-cloud"></a>Yeni bir özel bulut oluşturun

1. **Tüm Hizmetler**’i seçin.
2. **Cloudsimple Hizmetleri**için arama yapın.
3. Özel bulutunuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. **Özel bulut oluştur** ' a tıklayarak cloudsimple portalı için yeni bir tarayıcı sekmesi açın. İstenirse, Azure oturum açma kimlik bilgilerinizle oturum açın.

    ![Azure 'dan özel bulut oluşturma](media/create-private-cloud-from-azure.png)

5. CloudSimple portalında, özel bulutunuz için bir ad sağlayın.
6. Özel bulutunuzun **konumunu** seçin.
7. Azure 'da satın aldığınız verilerle tutarlı olan **düğüm türünü**seçin.  [CS28 veya CS36 seçeneğini](cloudsimple-node.md#vmware-solution-by-cloudsimple-nodes-sku)belirleyebilirsiniz. İkinci seçenek, en yüksek işlem ve bellek kapasitesini içerir.
8. **Düğüm sayısını**belirtin.  Özel bir bulut oluşturmak için en az üç düğüm gerekir.

    ![Özel bulut oluşturma-temel bilgi](media/create-private-cloud-basic-info.png)

9. İleri **' ye tıklayın: Gelişmiş Seçenekler**.
10. VSphere/vSAN alt ağları için CıDR aralığını girin. CıDR aralığının şirket içi veya diğer Azure alt ağlarınızdan (sanal ağlar) veya ağ geçidi alt ağıyla çakışmadığından emin olun.

    **CIDR aralığı seçenekleri:** /24,/23,/22 veya/21. A/24 CıDR aralığı en fazla dokuz düğüm destekler, a/23 CıDR aralığı en fazla 41 düğümü destekler ve/22 ve/21 CıDR aralığı en fazla 64 düğümü destekler (bir özel buluttaki en fazla düğüm sayısı).

    > [!IMPORTANT]
    > VSphere/vSAN CıDR aralığındaki IP adresleri özel bulut altyapısı tarafından kullanılmak üzere ayrılmıştır.  Bu aralıkta IP adresini herhangi bir sanal makinede kullanmayın.

11. İleri **' ye tıklayın: Gözden geçirin ve**oluşturun.
12. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, **önceki**' ye tıklayın.
13. **Oluştur**'a tıklayın.

Özel bulut sağlama işlemi başlar. Özel bulutun sağlanması iki saate kadar sürebilir.

Mevcut bir özel bulutu genişletmeden ilgili yönergeler için bkz. [özel bulutu genişletme](expand-private-cloud.md).
