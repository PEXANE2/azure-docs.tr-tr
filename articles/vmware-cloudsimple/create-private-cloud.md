---
title: CloudSimple tarafından Azure VMware Çözümü - CloudSimple Özel Bulut Oluştur
description: VMware iş yüklerini operasyonel esneklik ve süreklilikle buluta yaymak için CloudSimple Private Cloud'un nasıl oluşturulup oluşturulmayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4f700ac34b6c6e2a651366bee7dd1785c608064f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024798"
---
# <a name="create-a-cloudsimple-private-cloud"></a>CloudBasit Özel Bulut Oluştur

Özel Bulut, ESXi ana bilgisayarları, vCenter, vSAN ve NSX'i destekleyen yalıtılmış bir VMware yığınıdır. Özel Bulutlar CloudSimple portalı üzerinden yönetilir. Kendi yönetim etki alanında kendi vCenter sunucuları vardır. Yığın özel düğümler ve yalıtılmış çıplak metal donanım düğümleri üzerinde çalışır.

Özel Bulut oluşturmak, ağ altyapısı için çeşitli ortak gereksinimleri karşılamanıza yardımcı olur:

* **Büyüme**. Mevcut altyapınız için bir donanım yenileme noktasına ulaştıysanız, Özel Bulut, yeni bir donanım yatırımı gerektirmeden genişletmenize olanak tanır.

* **Hızlı genişleme**. Herhangi bir geçici veya planlanmamış kapasite gerekirse, Özel Bulut gecikmeden ek kapasite oluşturmanıza olanak tanır.

* **Artırılmış koruma.** Üç veya daha fazla düğümden oluşan Özel Bulut ile otomatik artıklık ve yüksek kullanılabilirlik koruması elde elabilirsiniz.

* **Uzun vadeli altyapı ihtiyaçları.** Veri merkezleriniz kapasitedeyse veya maliyetlerinizi düşürmek için yeniden yapılandırmak istiyorsanız, Özel Bulut, kurumsal işlemlerinizle uyumlu kalırken veri merkezlerini emekliye ayırmanızı ve bulut tabanlı bir çözüme geçiş yapmanızı sağlar.

Bir Özel Bulut oluşturduğunuzda, tek bir vSphere kümesine ve bu kümede oluşturulan tüm yönetim VM'lerini alırsınız.

## <a name="before-you-begin"></a>Başlamadan önce

Özel Bulut'unuzu oluşturmadan önce düğümlerin sağlanması gerekir. Düğümsağlama hakkında daha fazla bilgi için [CloudSimple tarafından Azure VMware Çözümü için Hüküm düğümlerine](create-nodes.md)bakın.

Özel Bulut için vSphere/vSAN alt ağları için bir CIDR aralığı ayırın. Özel Bulut, bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware yığını ortamı (ESXi ana bilgisayarları, vCenter, vSAN ve NSX ile) olarak oluşturulur. Yönetim bileşenleri vSphere/vSAN alt ağları CIDR için seçilen ağa dağıtılır. Ağ CIDR aralığı dağıtım sırasında farklı alt ağlara ayrılmıştır. vSphere/vSAN alt ağ adres alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuranabilecek herhangi bir ağla örtüşmemelidir. CloudSimple ile iletişim sağlayan ağlar şirket içi ağları ve Azure sanal ağları içerir. vSphere/vSAN alt ağları hakkında daha fazla bilgi için V'ler ve alt ağlara genel bakış bilgisi ne bakın.

* Minimum vSphere/vSAN alt ağları CIDR aralığı öneki: /24
* Maksimum vSphere/vSAN alt ağları CIDR aralığı öneki: /21


## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

[CloudSimple portalına](access-cloudsimple-portal.md)erişin.

## <a name="create-a-new-private-cloud"></a>Yeni Bir Özel Bulut Oluşturun

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Services'ı**arayın.
3. Özel Bulut'unuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. **Genel Bakış'tan**CloudSimple portalı için yeni bir tarayıcı sekmesi açmak için Özel **Bulut Oluştur'u** tıklatın. İstenirse, Azure oturum açınızın kimlik bilgileriyle oturum açın.

    ![Azure'dan Özel Bulut Oluşturma](media/create-private-cloud-from-azure.png)

5. CloudSimple portalında, Özel Bulut'unuzun adını belirtin.
6. Özel Bulut'unuzun **Konumu'nü** seçin.
7. **Azure'da**sağlananlarla tutarlı düğüm türünü seçin.
8. **Düğüm sayısını**belirtin.  Özel Bulut oluşturmak için en az üç düğüm gerekir.

    ![Özel Bulut Oluşturma - Temel bilgiler](media/create-private-cloud-basic-info.png)

9. **İleri'yi tıklatın: Gelişmiş seçenekler.**
10. vSphere/vSAN alt ağları için CIDR aralığını girin. CIDR aralığının şirket içi veya diğer Azure alt ağlarınızın (sanal ağlar) veya ağ geçidi alt ağıyla örtüşmediğinden emin olun.

    **CIDR aralığı seçenekleri:** /24, /23, /22 veya /21. Bir /24 CIDR aralığı dokuz düğüme kadar destekler, bir /23 CIDR aralığı 41 düğüme kadar destekler ve /22 ve /21 CIDR aralığı 64 düğüme kadar (Özel Buluttaki maksimum düğüm sayısını) destekler.

    > [!IMPORTANT]
    > vSphere/vSAN CIDR serisindeki IP adresleri Özel Bulut altyapısı tarafından kullanılmak üzere ayrılmıştır.  Bu aralıktaki IP adresini herhangi bir sanal makinede kullanmayın.

11. **İleri'yi tıklatın: Gözden geçirin ve oluşturun.**
12. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, **Önceki'yi**tıklatın.
13. **Oluştur'u**tıklatın.

Özel Bulut sağlama işlemi başlar. Özel Bulut'un sağlanması iki saat kadar sürebilir.

Varolan bir Özel Bulutu genişletme yönergeleri [için](expand-private-cloud.md)bkz.
