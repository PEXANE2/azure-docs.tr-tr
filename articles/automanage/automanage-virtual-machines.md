---
title: Sanal makineler için Azure oto yönetimi
description: Sanal makineler için Azure oto yönetimi hakkında bilgi edinin.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: deanwe
ms.custom: references_regions
ms.openlocfilehash: 8e48a5c896c4927b82f7d77f31b7f1c47fd156c5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942170"
---
# <a name="azure-automanage-for-virtual-machines"></a>Sanal makineler için Azure oto yönetimi

Bu makalede, aşağıdaki avantajları içeren sanal makineler için Azure oto yönetimi hakkında bilgiler yer almaktadır:

- Sanal makineleri en iyi uygulamalar için Azure hizmetleri 'ni seçme konusunda akıllıca onpanolar
- Her Azure en iyi uygulama başına hizmeti otomatik olarak yapılandırır
- Değişikliklerini için izler ve algılandığında düzeltme için düzeltir
- Basit bir deneyim (işaret, tıklama, ayarlama, unutma) sağlar


## <a name="overview"></a>Genel Bakış

Sanal makineler için Azure oto yönetimi, Azure 'da sanal makinenize faydalanabilir belirli hizmetlerin nasıl ekleneceğini ve nasıl yapılandırılacağını öğrenmenizi sağlayan bir hizmettir. Bu hizmetler, sanal makineler için güvenilirlik, güvenlik ve yönetimin geliştirilmesine yardımcı olur ve Azure [güncelleştirme yönetimi](../automation/update-management/update-mgmt-overview.md) ve [Azure Backup](../backup/backup-overview.md) gibi Azure en iyi yöntem hizmetleri gibi kabul edilir.

Sanal makinelerinizi Azure otomatik Yönet 'e ekledikten sonra, her bir en iyi uygulama hizmetini önerilen ayarlarına otomatik olarak yapılandırır. En iyi uygulamalar, hizmetlerin her biri için farklıdır. En iyi uygulama, her gün bir kez sanal makineyi yedeklemek ve altı aylık bir bekletme dönemi varsa, bu örnek Azure Backup olabilir.

Ayrıca, Azure otomatik yönetimi de DRFT için otomatik olarak izler ve algılandığında düzeltme için düzeltir. Bunun anlamı, sanal makineniz Azure 'u yeniden yönetmek için eklendi, bunu yalnızca Azure 'un en iyi yöntemleri uyarınca yapılandıracağız, ancak kendi yaşam döngüsünün tamamında bu en iyi uygulamalarla uyumlu olmaya devam ettiğinden emin olmak için makinenizi izliyoruz. Sanal makineniz bu uygulamalardan daha fazla veya daha sapmaya uygunsa, bunu düzeltecektir ve makinenizi istenen duruma geri çekeceğiz.

Son olarak, deneyim inanılmaz basittir.


## <a name="prerequisites"></a>Önkoşullar

Sanal makinelerinizde Azure oto yönetimi 'ni etkinleştirmeyi denemeden önce göz önünde bulundurmanız gereken birkaç önkoşul vardır.

- Yalnızca Windows Server VM 'Leri
- VM 'Ler çalışıyor olmalıdır
- Yalnızca ölçek olmayan küme VM 'Leri
- VM 'Ler desteklenen bir bölgede olmalıdır
- Kullanıcının doğru izinleri olması gerekir
- VM 'Ler farklı bir abonelikte bulunan bir Log Analytics çalışma alanına bağlanmalıdır

Aşağıdaki RBAC izni, **Kullanıcı erişimi yönetici** rolleriyle birlikte, oto yönetimi: **Owner** rolü ya da **katkıda bulunan** ' i etkinleştirmek için gereklidir.

Ayrıca, oto yönetimi 'nin yalnızca şu bölgelerde bulunan Windows VM 'Leri desteklediğini unutmayın: Batı Avrupa, Doğu ABD, Batı ABD 2, Kanada Orta, Orta Batı ABD.

## <a name="participating-services"></a>Katılım Hizmetleri

:::image type="content" source="media\automanage-virtual-machines\intelligently-onboard-services.png" alt-text="Hizmetleri akıllıca ekleyin.":::

Katılan Azure hizmetlerinin yanı sıra bunların desteklenen yapılandırma profillerinin eksiksiz listesi için bkz. [sanal makinelerin En Iyi uygulamaları için bkz. Azure oto yönetimi](virtual-machines-best-practices.md) .

 Sizi bu katılım hizmetlerine otomatik olarak ekleyeceğiz. Bunlar, [bulut benimseme çerçevesinden](https://docs.microsoft.com/azure/cloud-adoption-framework/manage/azure-server-management)bulabileceğiniz en iyi yöntemler teknik incelemesi için önemlidir.

Bu hizmetlerin tümü için otomatik olarak, otomatik yapılandırma, izleme için izleyici ve drara algılanırsa aracılık.


## <a name="enabling-automanage-for-vms-in-azure-portal"></a>Azure portal VM 'Ler için oto yönetimi etkinleştiriliyor

Azure portal, var olan bir sanal makinede ya da yeni bir sanal makine oluştururken, oto yönetimini etkinleştirebilirsiniz. Bu işlemin kısa adımları için, [sanal makineler Için oto yönetimi hızlı](quick-create-virtual-machines-portal.md)başlangıcı ' na göz atın.

SANAL makineniz için Automanage 'u ilk kez etkinleştirdiğinizde, **automanage: Azure sanal makine en iyi yöntemleri**için Azure Portal araması yapabilirsiniz. **Mevcut VM 'de etkinleştir**' e tıklayın, eklemek Istediğiniz VM 'leri seçin, **Seç**' e tıklayın, **Etkinleştir**' e tıklayın ve işiniz bitti demektir.

Bu hizmetleri yönetmek için bu VM ile etkileşimde bulunabilmeniz gereken tek zaman, sanal makinenizin düzeltilmesi için denediğimiz olaydır, ancak bunu yapamadı. VM 'nizi başarılı bir şekilde düzeltmemiz durumunda sizi uyarmadan yine de uyumluluğa geri getirilecektir.


## <a name="configuration-profiles"></a>Yapılandırma profilleri

Sanal makineniz için oto yönetimi etkinleştirirken bir yapılandırma profili gerekir. Yapılandırma profilleri bu hizmetin temelidir. Bu hizmetler, makinelerinizi hangi hizmetlere eklediğimiz ve bir ölçüde bu hizmetlerin yapılandırmasının ne olacağını tanımlar.

### <a name="default-configuration-profiles"></a>Varsayılan yapılandırma profilleri

Şu anda iki yapılandırma profili bulunur.

- **Azure sanal makine en iyi yöntemleri-geliştirme ve test** yapılandırma profili geliştirme ve test makineleri için tasarlanmıştır.
- **Azure sanal makine en iyi uygulamaları-üretim** yapılandırma profili üretime yöneliktir.

Bu farklıın nedeni, çalışan iş yüküne bağlı olarak belirli hizmetlerin önerilmeidir. Örneğin, bir üretim makinesinde sizi Azure Backup için otomatik olarak ekleyeceğiz. Ancak, geliştirme/test makineleri genellikle daha düşük iş etkisi olduğundan, bir geliştirme ve test makinesi için bir yedekleme hizmeti gereksiz bir maliyet olacaktır.

### <a name="customizing-a-configuration-profile-using-preferences"></a>Tercihleri kullanarak yapılandırma profilini özelleştirme

Size eklediğimiz standart hizmetlere ek olarak, belirli bir tercih alt kümesini yapılandırmanıza izin veririz. En iyi yöntemlerimizi ihlal eden bir dizi yapılandırma seçeneği içinde bu tercihlere izin verilir. Örneğin, Azure Backup olması durumunda yedeklemenin sıklığını ve haftanın hangi gününde olduğunu tanımlamanızı sağlayacak. Ancak, Azure Backup tamamen devre dışı geçiş *yapmanıza izin vermeyecektir.*

> [!NOTE]
> Geliştirme/test yapılandırma profilinde VM 'yi hiç yedekliyoruz.

Varsayılan yapılandırma profilinin ayarlarını Tercihler aracılığıyla ayarlayabilirsiniz. [Burada](virtual-machines-custom-preferences.md)bir tercih oluşturma hakkında bilgi edinin.

> [!NOTE]
> Oto yönetimi etkinken VM 'nizin yapılandırma profilini değiştiremezsiniz. Bu VM için, bu sanal makine için yeniden yönetme 'yi devre dışı bırakmanız ve ardından, istenen yapılandırma profili ve tercihleriyle tekrar yönetmeyi yeniden etkinleştirmeniz gerekir.


## <a name="automanage-account"></a>Hesabı oto Yönet

Otomatikmanage hesabı, güvenlik bağlamına veya otomatik işlemlerin oluştuğu kimliğe sahiptir. Genellikle, hesabı otomatik Yönet seçeneği, seçmeniz için gereksizdir, ancak otomatik yönetimi bölmek istediğiniz bir temsili senaryosu varsa (Belki iki sistem yöneticisi arasında), bu seçenek bu yöneticilerin her biri için bir Azure kimliği tanımlamanızı sağlar.

Azure portal deneyiminde, sanal makinelerinizdeki oto yönetimini etkinleştirirken, el ile Yönet hesabı atamanıza veya el ile oluşturmanıza imkan tanıyan, **Azure VM en iyi uygulama** dikey penceresinde gelişmiş bir açılan menü bulunur.

> [!NOTE]
> Aşağıdaki RBAC izni, **Kullanıcı erişimi yönetici** rolleriyle birlikte, oto yönetimi: **Owner** rolü ya da **katkıda bulunan** ' i etkinleştirmek için gereklidir.


## <a name="status-of-vms"></a>VM 'lerin durumu

Azure portal, otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin. Burada her bir sanal makinenin genel durumunu görürsünüz.

:::image type="content" source="media\automanage-virtual-machines\configured-status.png" alt-text="Yapılandırılmış sanal makinelerin listesi.":::

Listelenen her VM için şu ayrıntılar görüntülenir: ad, yapılandırma profili, yapılandırma tercihi, durum, hesap, abonelik ve kaynak grubu.

**Durum** sütununda aşağıdaki durumlar görüntülenebilir:
- *Devam eden* -VM yeni etkinleştirildi ve yapılandırılıyor
- *Yapılandırıldı* -VM yapılandırıldı ve bir DRT algılanmadı
- *Başarısız* -VM 'de düzeltebilecekler ve düzeltilemedi

**Durumu** *başarısız*olarak görürseniz, sanal makinenizin bulunduğu kaynak grubu aracılığıyla dağıtımda sorun giderebilirsiniz. **Kaynak grupları**' na gidin, kaynak grubunuzu seçin, **dağıtımlar** ' a tıklayın ve hata ayrıntılarıyla birlikte *başarısız* durumuna bakın.


## <a name="disabling-automanage-for-vms"></a>VM 'Ler için oto yönetimini devre dışı bırakma

Belirli sanal makinelerde, tek bir güne kadar her gün tekrar yönetmeyi seçebilirsiniz. Örneğin, makineniz bazı süper duyarlı güvenli iş yükünü çalıştırıyor ve Azure 'dan daha da çok daha fazla işlem yapmanız gerekir. bu nedenle, makineyi Azure 'un en iyi yöntemleri dışında yapılandırmanız gerekir.

Azure portal bu işlemleri yapmak için otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin. Tekrar Yönet ' e tıklayarak devre dışı bırakmak istediğiniz sanal makinenin yanındaki onay kutusunu işaretleyin ve ardından yeniden **Aç düğmesine tıklayın** .

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Bir sanal makinede oto yönetimi devre dışı bırakılıyor.":::

Kabul etmiş önce **devre dışı bırakmak**için ortaya çıkan açılan pencerede iletiyi dikkatle okuyun.

```
Disabling automanagement in a VM results in the following behavior:

1.    The configuration of the VM and the services it's onboarded to will not be changed
2.    Any changes incurred by those services will remain billable and will continue to be incurred
3.    Any Automanage behaviors will stop immediately
```

İlk ve daha önce, sanal makineyi eklendi ve yapılandırdığımız hizmetlerden herhangi birinden kurmayacak. Bu nedenle, bu hizmetler tarafından tahakkuk eden ücretler faturalandırılabilir olmaya devam edecektir. Gerekirse Pano 'ya ihtiyacınız olacaktır. Herhangi bir oto yönetimi davranışı hemen durdurulur. Örneğin, artık VM 'yi DRFT için izliyoruz.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal makineler için oto yönetimi 'nin, en iyi uygulama Azure hizmetlerini bilmeniz, bu makaleye ekleme ve yapılandırma ihtiyacını ortadan kaldırabildiğiniz bir yol sağladığını öğrendiniz. Ayrıca, eklendi sanal makineler için otomatik olarak yönetmeye yönelik bir makine, ayarlanan yapılandırma profillerinden Drifts, otomatik olarak yeniden uyumluluğa geri alınacaktır.

Azure portal sanal makineler için oto yönetimini etkinleştirmeyi deneyin.

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)
