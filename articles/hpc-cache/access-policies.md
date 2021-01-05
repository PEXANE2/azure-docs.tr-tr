---
title: Azure HPC önbelleğinde erişim ilkeleri kullanma
description: Azure HPC Cache 'te depolama hedeflerine istemci erişimini sınırlamak için özel erişim ilkeleri oluşturma ve uygulama
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 12/28/2020
ms.author: v-erkel
ms.openlocfilehash: 795b194eb7cd31e633128c22ddffe808b32e07da
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97802430"
---
# <a name="use-client-access-policies"></a>İstemci erişim ilkelerini kullanma

Bu makalede, depolama hedeflerinizin özel istemci erişim ilkelerinin nasıl oluşturulacağı ve uygulanacağı açıklanır.

İstemci erişim ilkeleri, istemcilerin depolama hedefi dışarı aktarmaları için nasıl bağlanabilme şeklini denetler. İstemci konağında veya ağ düzeyinde kök sıkıştırarak ve okuma/yazma erişimi gibi işlemleri denetleyebilirsiniz.

Erişim ilkeleri bir ad alanı yoluna uygulanır, bu da bir NFS depolama sisteminde iki farklı dışarı aktarma için farklı erişim ilkeleri kullanabileceğiniz anlamına gelir.

Bu özellik, farklı istemci gruplarının depolama hedeflerine nasıl erişebileceğini kontrol etmeniz gereken iş akışlarına yöneliktir.

Depolama hedefi erişimi üzerinde ayrıntılı denetime ihtiyacınız yoksa varsayılan ilkeyi kullanabilir veya varsayılan ilkeyi ek kurallarla özelleştirebilirsiniz.

## <a name="create-a-client-access-policy"></a>İstemci erişim ilkesi oluşturma

İlke oluşturmak ve yönetmek için Azure portal **istemci erişim ilkeleri** sayfasını kullanın. <!-- is there AZ CLI for this? -->

[![istemci erişim ilkeleri sayfasının ekran görüntüsü. Birçok ilke tanımlanmıştır ve bazıları kurallarını göstermek için genişletilir](media/policies-overview.png)](media/policies-overview.png#lightbox)

Her ilke kurallardan oluşur. Kurallar, ana bilgisayarlara en küçük kapsamdan (ana bilgisayar) en büyüğe (varsayılan) göre uygulanır. Eşleşen ilk kural uygulanır ve sonraki kurallar yok sayılır.

Yeni bir erişim ilkesi oluşturmak için, listenin en üstündeki **+ erişim Ilkesi Ekle** düğmesine tıklayın. Yeni erişim ilkesine bir ad verin ve en az bir kural girin.

![erişim ilkelerinin ekran görüntüsü birden fazla kuralla doldurulmuş şekilde düzenleme dikey penceresi. Kuralı kaydetmek için Tamam ' ı tıklatın.](media/add-policy.png)

Bu bölümün geri kalanında, kurallarda kullanabileceğiniz değerler açıklanmaktadır.

### <a name="scope"></a>Kapsam

Kapsam terimi ve adres filtresi, kural tarafından hangi istemcilerin etkilendiğini tanımlamak için birlikte çalışır.

Kuralın tek bir istemciye (ana bilgisayar), bir dizi IP adresine (ağ) veya tüm istemcilere (varsayılan) uygulanıp uygulanmadığını belirtmek için bunları kullanın.

Kuralınız için uygun **kapsam** değerini seçin:

* **Ana bilgisayar** -kural, tek bir istemciye uygulanır
* **Ağ** -kural, bir IP adresi aralığındaki istemcilere uygulanır
* **Varsayılan** -kural tüm istemcilere uygulanır.

Bir ilkedeki kurallar bu sırayla değerlendirilir. İstemci bağlama isteği bir kuralla eşleştiğinde, diğerleri yok sayılır.

### <a name="address-filter"></a>Adres filtresi

**Adres filtresi** değeri, kuralla eşleşen istemcileri belirtir.

Kapsamı **ana bilgisayar** olarak ayarlarsanız, filtrede yalnızca bir IP adresi belirtebilirsiniz. Varsayılan kapsam tüm istemcilerle eşleştiğinden, **varsayılan** kapsam ayarı için **adres filtresi** alanına herhangi bir IP adresi giremezsiniz.

Bu kural için IP adresini veya adres aralığını belirtin. Adres aralığını belirtmek için CıDR gösterimini (örnek: 0.1.0.0/16) kullanın.

### <a name="access-level"></a>Erişim düzeyi

Kapsama ve filtreyle eşleşen istemcilere verilecek ayrıcalıkları belirleyin.

Seçenekler **okuma/yazma**, **salt okunurdur** veya **erişim yok**.

### <a name="suid"></a>SUıD

Depolamadaki dosyaların erişim üzerinde kullanıcı kimlikleri ayarlaması için **SUID** kutusunu işaretleyin.

Kullanıcı ayrıcalıkların bu dosyayla ilgili bir görevi yerine getirebilmesi için, genellikle kullanıcının ayrıcalıklarını geçici olarak artırmak üzere SUıD kullanılır.

### <a name="submount-access"></a>Alt bağlama erişimi

Belirtilen istemcilerin bu dışa aktarma alt dizinlerini doğrudan takmasına izin vermek için bu kutuyu işaretleyin.

### <a name="root-squash"></a>Kök sıkıştırarak

Bu kuralla eşleşen istemciler için kök sıkıştırarak 'in uygulanıp ayarlanamayacağını seçin.

Bu değer, depolama dışarı aktarma düzeyinde kök squsize izin vermenizi sağlar. Ayrıca, [önbellek düzeyinde kök sıkıştırarak](configuration.md#configure-root-squash)'i de ayarlayabilirsiniz.

Kök squon 'u açarsanız, anonim KIMLIK Kullanıcı değerini şu seçeneklerden birine de ayarlamanız gerekir:

* **-2** (hiç kimse)
* **65534** (hiç kimse)
* **-1** (erişim yok)
* **65535** (erişim yok)
* **0** (ayrıcalıksız kök)

## <a name="update-access-policies"></a>Erişim ilkelerini güncelleştirme

**İstemci erişim ilkeleri** sayfasındaki tablosundan erişim ilkelerini düzenleyebilir veya silebilirsiniz.

Bu ilkeyi düzenlenmek üzere açmak için ilke adına tıklayın.

Bir ilkeyi silmek için, listede adının yanındaki onay kutusunu işaretleyin ve ardından listenin en üstünde bulunan **Sil** düğmesine tıklayın. "Varsayılan" adlı ilkeyi silemezsiniz.

> [!NOTE]
> Kullanımda olan bir erişim ilkesini silemezsiniz. Silmeyi denemeden önce, ilkeyi içeren herhangi bir ad alanı yolundan kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Depolama hedeflerinizin ad alanı yollarında erişim ilkeleri uygulayın. Nasıl yapılacağını öğrenmek için [toplanan ad alanını ayarlama](add-namespace-paths.md) makalesini okuyun.
