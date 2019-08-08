---
title: CloudSimple özel bulutu tarafından Azure VMware çözümü oluşturma
description: İşlem esnekliği ve sürekliliği ile VMware iş yüklerini buluta genişletmek için CloudSimple özel bulutu oluşturmayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 02a2bd311ea1e89a49eb12ef57a167a08eea5f98
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812259"
---
# <a name="create-a-cloudsimple-private-cloud"></a>CloudSimple özel bulutu oluşturma

Özel bir bulut oluşturmak, ağ altyapısının çeşitli yaygın ihtiyaçlarını ele almanıza yardımcı olur:

* **Büyüme**. Mevcut altyapınız için bir donanım yenileme noktasına ulaştıysanız, özel bir bulut gerekli yeni donanım yatırımı olmadan genişletmenizi sağlar.

* **Hızlı genişletme**. Geçici veya plansız bir kapasite olması halinde bir özel bulut, gecikme olmadan ek kapasite oluşturmanıza olanak sağlar.

* **Daha fazla koruma**. Üç veya daha fazla düğümden oluşan özel bir bulutla, otomatik artıklık ve yüksek kullanılabilirlik koruması alırsınız.

* **Uzun süreli altyapı ihtiyaçları**. Veri merkezleriniz kapasiteniz varsa veya maliyetlerinizi düşürmek için yeniden yapılandırmak istiyorsanız, özel bir bulut, veri merkezlerini devre dışı bırakmanıza ve bulut tabanlı bir çözüme geçiş yaparken Kurumsal işlemleriniz ile uyumlu bir çözüm yapmanıza olanak sağlar.

Özel bir bulut oluşturduğunuzda, tek bir vSphere kümesi ve bu kümede oluşturulan tüm yönetim VM 'Leri alırsınız.

## <a name="before-you-begin"></a>Başlamadan önce

Özel bulutunuzu oluşturabilmeniz için önce düğümlerin sağlanması gerekir.  Düğüm sağlama hakkında daha fazla bilgi için bkz. [CloudSimple-Azure Ile VMware çözümü için düğüm sağlama](create-nodes.md) .

Özel bulut için vSphere/vSAN alt ağları için bir CıDR aralığı ayırın. Özel bulut, bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware yığını (ESXi Konakları, vCenter, vSAN ve NSX) ortamı olarak oluşturulur. Yönetim bileşenleri, vSphere/vSAN alt ağları CıDR için seçilen ağda dağıtılır. Ağ CıDR aralığı, dağıtım sırasında farklı alt ağlara bölünmüştür.  VSphere/vSAN alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır.  CloudSimple ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur.  VSphere/vSAN alt ağları hakkında daha fazla bilgi için bkz. [VLAN ve alt ağlara genel bakış](cloudsimple-vlans-subnets.md).

* Minimum vSphere/vSAN alt ağları CıDR aralığı ön eki:/24 
* Maksimum vSphere/vSAN alt ağları CıDR aralığı ön eki:/21

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

[Cloudsimple portalına](access-cloudsimple-portal.md)erişin.

## <a name="create-a-new-private-cloud"></a>Yeni bir özel bulut oluşturun

1. **Kaynaklar** sayfasında, **Yeni özel bulut**' a tıklayın.

    ![Özel bulut oluşturma-nasıl başlatılır](media/create-pc-button.png)

2. Özel bulut kaynaklarını barındıracak konumu seçin.

3. Özel bulut için sağladığınız CS28 veya CS36 node türünü seçin. İkinci seçenek, en yüksek işlem ve bellek kapasitesini içerir.

4. Özel bulut için düğüm sayısını seçin. En fazla kullanılabilir düğüm sayısını seçebilirsiniz. [](create-nodes.md)

    ![Özel bulut oluşturma-temel ayarlar](media/create-private-cloud-basic-info.png)

5. İleri **' ye tıklayın: Gelişmiş Seçenekler**.

6. VSphere/vSAN alt ağları için CıDR aralığını girin. CıDR aralığının şirket içi veya diğer Azure alt ağlarınızdan (sanal ağlar) veya ağ geçidi alt ağıyla çakışmadığından emin olun.  Azure sanal ağlarında tanımlı bir CıDR aralığı kullanmayın.
    
    **CIDR aralığı seçenekleri:** /24,/23,/22 veya/21. A/24 CıDR aralığı en fazla dokuz düğüm destekler, a/23 CıDR aralığı en fazla 41 düğümü destekler ve/22 ve/21 CıDR aralığı en fazla 64 düğümü destekler (bir özel buluttaki en fazla düğüm sayısı).

    > [!CAUTION]
    > VSphere/vSAN CıDR aralığındaki IP adresleri özel bulut altyapısı tarafından kullanılmak üzere ayrılmıştır.  Bu aralıkta IP adresini herhangi bir sanal makinede kullanmayın.

7. İleri **' ye tıklayın: Gözden geçirin ve**oluşturun.

8. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, **önceki**' ye tıklayın.

9.           **Oluştur**'a tıklayın.

Özel bulut sağlama, Oluştur ' a tıkladığınızda başlayacaktır.  CloudSimple portalındaki [Tasks](https://docs.azure.cloudsimple.com/activity/#tasks) sayfasından ilerlemeyi izleyebilirsiniz.  Sağlama işlemi 30 dakika ila iki saat arasında olabilir.  Sağlama tamamlandıktan sonra bir e-posta alacaksınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bulutu Genişlet](expand-private-cloud.md)