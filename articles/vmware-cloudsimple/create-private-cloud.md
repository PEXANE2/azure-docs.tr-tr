---
title: Azure VMware çözümleri (AVS)-AVS özel bulutu oluşturma
description: Çalışma esnekliği ve sürekliliği ile VMware iş yüklerini buluta genişletmek için bir AVS özel bulutu oluşturmayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 128a0a1eec03878d0deba93048c54324aab7d888
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024798"
---
# <a name="create-an-avs-private-cloud"></a>AVS özel bulutu oluşturma

AVS özel bulutu, ESXi Konakları, vCenter, vSAN ve NSX 'i destekleyen yalıtılmış bir VMware yığınına sahiptir. AVS özel bulutlar, AVS portalı aracılığıyla yönetilir. Kendi yönetim etki alanında kendi vCenter Server kuruluşları vardır. Yığın adanmış düğümlerde ve yalıtılmış tam donanım düğümlerinde çalışır.

Bir AVS özel bulutu oluşturmak, ağ altyapısının çeşitli yaygın ihtiyaçlarını ele almanıza yardımcı olur:

* **Büyüme**. Mevcut altyapınız için bir donanım yenileme noktasına ulaştıysanız, bir AVS özel bulutu, yeni donanım yatırımı gerekmeden genişletmenizi sağlar.

* **Hızlı genişletme**. Geçici veya plansız bir kapasite olması halinde, bir AVS özel bulutu gecikme olmadan ek kapasite oluşturmanıza olanak sağlar.

* **Daha fazla koruma**. Üç veya daha fazla düğümün bir AVS özel bulutu sayesinde, otomatik artıklık ve yüksek kullanılabilirlik koruması alırsınız.

* **Uzun süreli altyapı ihtiyaçları**. Veri merkezleriniz kapasiteniz varsa veya maliyetlerinizi düşürmek için yeniden yapılandırmak istiyorsanız, bir AVS özel bulutu, veri merkezlerini devre dışı bırakmanıza ve bulut tabanlı bir çözüme, kurumsal işlemleriniz ile uyumlu bir şekilde geçiş yapmanıza olanak sağlar.

Bir AVS özel bulutu oluşturduğunuzda, bu kümede oluşturulan tek bir vSphere kümesi ve tüm yönetim sanal makinelerini alırsınız.

## <a name="before-you-begin"></a>Başlamadan önce

AVS özel bulutunuzu oluşturabilmeniz için önce düğümlerin sağlanması gerekir. Düğüm sağlama hakkında daha fazla bilgi için bkz. [Azure VMware çözümleri için düğüm sağlama (AVS)](create-nodes.md).

AVS özel bulutu için vSphere/vSAN alt ağları için bir CıDR aralığı ayırın. Bir AVS özel bulutu, bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware Stack ortamı (ESXi Konakları, vCenter, vSAN ve NSX ile) olarak oluşturulur. Yönetim bileşenleri, vSphere/vSAN alt ağları CıDR için seçilen ağa dağıtılır. Ağ CıDR aralığı, dağıtım sırasında farklı alt ağlara bölünmüştür. VSphere/vSAN alt ağ adresi alanı benzersiz olmalıdır. Bu, AVS ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır. AVS ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur. VSphere/vSAN alt ağları hakkında daha fazla bilgi için bkz. VLAN ve alt ağlara genel bakış.

* Minimum vSphere/vSAN alt ağları CıDR aralığı ön eki:/24
* Maksimum vSphere/vSAN alt ağları CıDR aralığı ön eki:/21


## <a name="access-the-avs-portal"></a>AVS portalına erişme

[AVS portalına](access-cloudsimple-portal.md)erişin.

## <a name="create-a-new-avs-private-cloud"></a>Yeni bir AVS özel bulutu oluşturun

1. **Tüm Hizmetler**’i seçin.
2. **AVS Hizmetleri**için arama yapın.
3. AVS özel bulutunuzu oluşturmak istediğiniz AVS hizmetini seçin.
4. **Genel bakış**'dan, AVS portalı için yeni bir tarayıcı sekmesi açmak üzere **AVS özel bulutu oluştur** ' a tıklayın. İstenirse, Azure oturum açma kimlik bilgilerinizle oturum açın.

    ![Azure 'dan AVS özel bulutu oluşturma](media/create-private-cloud-from-azure.png)

5. AVS portalında, AVS özel bulutunuz için bir ad sağlayın.
6. AVS özel bulutunuzun **konumunu** seçin.
7. Azure 'da satın aldığınız verilerle tutarlı olan **düğüm türünü**seçin.
8. **Düğüm sayısını**belirtin. Bir AVS özel bulutu oluşturmak için en az üç düğüm gerekir.
5. CloudSimple portalında, özel bulutunuz için bir ad sağlayın.
6. Özel bulutunuzun **konumunu** seçin.
7. Azure 'da sağladığınız verilerle tutarlı bir **düğüm türü**seçin.
8. **Düğüm sayısını**belirtin.  Özel bir bulut oluşturmak için en az üç düğüm gerekir.

    ![AVS özel bulutu oluşturma-temel bilgi](media/create-private-cloud-basic-info.png)

9. Ileri ' ye tıklayın **: Gelişmiş Seçenekler**.
10. VSphere/vSAN alt ağları için CıDR aralığını girin. CıDR aralığının şirket içi veya diğer Azure alt ağlarınızdan (sanal ağlar) veya ağ geçidi alt ağıyla çakışmadığından emin olun.

    **CIDR aralığı seçenekleri:** /24,/23,/22 veya/21. A/24 CıDR aralığı en fazla dokuz düğüm destekler, a/23 CıDR aralığı en fazla 41 düğümü destekler ve/22 ve/21 CıDR aralığı en fazla 64 düğümü destekler (bir AVS özel bulutundaki en fazla düğüm sayısı).

    > [!IMPORTANT]
    > VSphere/vSAN CıDR aralığındaki IP adresleri, AVS özel bulut altyapısı tarafından kullanılmak üzere ayrılmıştır. Bu aralıkta IP adresini herhangi bir sanal makinede kullanmayın.

11. Ileri ' ye tıklayın. **İnceleme ve oluşturma**.
12. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, **önceki**' ye tıklayın.
13. **Oluştur**’a tıklayın.

AVS özel bulut sağlama işlemi başlar. AVS özel bulutunun sağlanması iki saate kadar sürebilir.

Mevcut bir AVS özel bulutunu genişletme yönergeleri için bkz. [AVS özel bulutu genişletme](expand-private-cloud.md).
