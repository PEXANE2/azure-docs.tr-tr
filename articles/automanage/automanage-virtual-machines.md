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
ms.openlocfilehash: 7772d57937393da1c48fa2658818d8a1a2b28a1f
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550793"
---
# <a name="azure-automanage-for-virtual-machines"></a>Sanal makineler için Azure oto yönetimi

Bu makalede, aşağıdaki avantajları içeren sanal makineler için Azure oto yönetimi hakkında bilgiler yer almaktadır:

- Sanal makineleri en iyi uygulamalar için Azure hizmetleri 'ni seçme konusunda akıllıca onpanolar
- Her Azure en iyi uygulama başına hizmeti otomatik olarak yapılandırır
- Değişikliklerini için izler ve algılandığında düzeltme için düzeltir
- Basit bir deneyim (işaret, tıklama, ayarlama, unutma) sağlar


## <a name="overview"></a>Genel Bakış

Sanal makineler için Azure oto yönetimi, Azure 'da sanal makinenize faydalanabilir belirli hizmetlerin nasıl ekleneceğini ve nasıl yapılandırılacağını öğrenmenizi sağlayan bir hizmettir. Bu hizmetler, sanal makineler için güvenilirlik, güvenlik ve yönetimin geliştirilmesine yardımcı olur ve Azure [güncelleştirme yönetimi](../automation/update-management/overview.md) ve [Azure Backup](../backup/backup-overview.md) gibi Azure en iyi yöntem hizmetleri gibi kabul edilir.

Sanal makinelerinizi Azure otomatik Yönet 'e ekledikten sonra, her bir en iyi uygulama hizmetini önerilen ayarlarına otomatik olarak yapılandırır. En iyi uygulamalar, hizmetlerin her biri için farklıdır. En iyi uygulama, her gün bir kez sanal makineyi yedeklemek ve altı aylık bir bekletme dönemi varsa, bu örnek Azure Backup olabilir.

Ayrıca, Azure otomatik yönetimi de DRFT için otomatik olarak izler ve algılandığında düzeltme için düzeltir. Bunun anlamı, sanal makineniz Azure 'u yeniden yönetmek için eklendi, bunu yalnızca Azure 'un en iyi yöntemleri uyarınca yapılandıracağız, ancak kendi yaşam döngüsünün tamamında bu en iyi uygulamalarla uyumlu olmaya devam ettiğinden emin olmak için makinenizi izliyoruz. Sanal makineniz bu uygulamalardan daha fazla veya daha sapmaya uygunsa, bunu düzeltecektir ve makinenizi istenen duruma geri çekeceğiz.

Son olarak, deneyim inanılmaz basittir.


## <a name="prerequisites"></a>Önkoşullar

Sanal makinelerinizde Azure oto yönetimi 'ni etkinleştirmeyi denemeden önce göz önünde bulundurmanız gereken birkaç önkoşul vardır.

- Yalnızca Windows Server VM 'Leri
- VM 'Ler desteklenen bir bölgede olmalıdır (aşağıdaki paragrafı görüntüleyin)
- Kullanıcının doğru izinleri olması gerekir (aşağıdaki paragrafa bakın)
- Oto yönetimi şu anda korumalı alan aboneliklerini desteklemiyor

Ayrıca, oto yönetimi 'nin yalnızca şu bölgelerde bulunan Windows VM 'Leri desteklediğini unutmayın: Batı Avrupa, Doğu ABD, Batı ABD 2, Kanada Orta, Orta Batı ABD, Japonya Doğu.

Mevcut bir oto Yönet hesabı kullanarak VM 'lerde oto yönetimi 'ni etkinleştirmek için VM 'lerinizi içeren kaynak grubunda **katkıda** bulunan rolüne sahip olmanız gerekir. Yeni bir bir oto Yönet hesabıyla bir oto yönetimi etkinleştirirseniz, aboneliğiniz üzerinde aşağıdaki izinlere sahip olmanız gerekir: **sahip** rolü veya **katkıda bulunan** **Kullanıcı erişimi yönetici** rolleriyle birlikte.

> [!NOTE]
> Farklı bir abonelikte bulunan bir çalışma alanına bağlı bir sanal makinede, oto yönetimi 'ni kullanmak istiyorsanız, her bir abonelikte yukarıda açıklanan izinlere sahip olmanız gerekir.

## <a name="participating-services"></a>Katılım Hizmetleri

:::image type="content" source="media\automanage-virtual-machines\intelligently-onboard-services.png" alt-text="Hizmetleri akıllıca ekleyin.":::

Katılan Azure hizmetlerinin yanı sıra bunların desteklenen yapılandırma profillerinin eksiksiz listesi için bkz. [sanal makinelerin En Iyi uygulamaları için bkz. Azure oto yönetimi](virtual-machines-best-practices.md) .

 Sizi bu katılım hizmetlerine otomatik olarak ekleyeceğiz. Bunlar, [bulut benimseme çerçevesinden](/azure/cloud-adoption-framework/manage/azure-server-management)bulabileceğiniz en iyi yöntemler teknik incelemesi için önemlidir.

Bu hizmetlerin tümü için otomatik olarak, otomatik yapılandırma, izleme için izleyici ve drara algılanırsa aracılık.


## <a name="enabling-automanage-for-vms-in-azure-portal"></a>Azure portal VM 'Ler için oto yönetimi etkinleştiriliyor

Azure portal, var olan bir sanal makinede ya da yeni bir sanal makine oluştururken, oto yönetimini etkinleştirebilirsiniz. Bu işlemin kısa adımları için, [sanal makineler Için oto yönetimi hızlı](quick-create-virtual-machines-portal.md)başlangıcı ' na göz atın.

SANAL makineniz için Automanage 'u ilk kez etkinleştirdiğinizde, **automanage: Azure sanal makine en iyi yöntemleri** için Azure Portal araması yapabilirsiniz. **Mevcut VM 'de etkinleştir**' e tıklayın, eklemek Istediğiniz VM 'leri seçin, **Seç**' e tıklayın, **Etkinleştir**' e tıklayın ve işiniz bitti demektir.

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

Otomatikmanage hesabı, güvenlik bağlamına veya otomatik işlemlerin oluştuğu kimliğe sahiptir. Genellikle, hesabı otomatik Yönet seçeneği, seçmeniz için gereksizdir, ancak kaynaklarınızın otomatik yönetimini bölmek istediğiniz bir yetkilendirme senaryosu varsa (Belki iki sistem yöneticisi arasında), bu seçenek bu yöneticilerin her biri için bir Azure kimliği tanımlamanızı sağlar.

Azure portal deneyiminde, sanal makinelerinizdeki oto yönetimini etkinleştirirken, el ile Yönet hesabı atamanıza veya el ile oluşturmanıza imkan tanıyan, **Azure VM en iyi uygulama** dikey penceresinde gelişmiş bir açılan menü bulunur.

Oto Yönet hesabına hem **katkıda bulunan** hem de **kaynak ilkesi katılımcısı** rollerinin, sizin eklediğiniz makineleri içeren abonelikler (ler) e-olarak verilmesi sağlanır. Tüm aboneliklerde hesap **katılımcısı** ve **kaynak ilkesi katkıda** bulunan izinlerini oto yönetmesine izin veren birden çok abonelik genelinde makinelerde aynı oto Yönet hesabını kullanabilirsiniz.

VM 'niz başka bir abonelikteki bir Log Analytics çalışma alanına bağlıysa, oto Yönet hesabına hem **katkıda** bulunan hem de **kaynak ilkesi katılımcısı** da bu diğer abonelikte de verilecektir.

Yeni bir bir oto Yönet hesabıyla bir oto yönetimi etkinleştirirseniz, aboneliğiniz üzerinde aşağıdaki izinlere sahip olmanız gerekir: **sahip** rolü veya **katkıda bulunan** **Kullanıcı erişimi yönetici** rolleriyle birlikte.

Var olan bir oto Yönet hesabıyla bir oto yönetimi etkinleştirirseniz, VM 'lerinizi içeren kaynak grubunda **katkıda** bulunan rolüne sahip olmanız gerekir.

> [!NOTE]
> En Iyi Yönet uygulamalarını devre dışı bıraktığınızda, tüm ilişkili aboneliklerdeki hesap izinlerini oto Yönet olarak kalır. Aboneliğin ıAM sayfasına gidip izinleri el ile kaldırın veya tekrar Yönet hesabını silin. Hala herhangi bir makine yönetiliyorsa, oto Yönet hesabı silinemez.


## <a name="status-of-vms"></a>VM 'lerin durumu

Azure portal, otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin. Burada her bir sanal makinenin genel durumunu görürsünüz.

:::image type="content" source="media\automanage-virtual-machines\configured-status.png" alt-text="Yapılandırılmış sanal makinelerin listesi.":::

Listelenen her VM için şu ayrıntılar görüntülenir: ad, yapılandırma profili, yapılandırma tercihi, durum, hesap, abonelik ve kaynak grubu.

**Durum** sütununda aşağıdaki durumlar görüntülenebilir:
- *Devam eden* -VM yeni etkinleştirildi ve yapılandırılıyor
- *Yapılandırıldı* -VM yapılandırıldı ve bir DRT algılanmadı
- *Başarısız* -VM 'de düzeltebilecekler ve düzeltilemedi
- *Bekliyor* -VM Şu anda çalışmıyor ve bir sonraki çalıştırıldığında, yeniden Yönet işlemi VM 'yi ekleme veya düzeltme girişiminde bulunur

**Durumu** *başarısız* olarak görürseniz, sanal makinenizin bulunduğu kaynak grubu aracılığıyla dağıtımda sorun giderebilirsiniz. **Kaynak grupları**' na gidin, kaynak grubunuzu seçin, **dağıtımlar** ' a tıklayın ve hata ayrıntılarıyla birlikte *başarısız* durumuna bakın.


## <a name="disabling-automanage-for-vms"></a>VM 'Ler için oto yönetimini devre dışı bırakma

Belirli sanal makinelerde, tek bir güne kadar her gün tekrar yönetmeyi seçebilirsiniz. Örneğin, makineniz bazı süper duyarlı güvenli iş yükünü çalıştırıyor ve Azure 'dan daha da çok daha fazla işlem yapmanız gerekir. bu nedenle, makineyi Azure 'un en iyi yöntemleri dışında yapılandırmanız gerekir.

Azure portal bu işlemleri yapmak için otomatik olarak yönetilen tüm sanal **makinelerinizi listeleyen otomatik Yönet – Azure sanal makine en iyi uygulamaları** sayfasına gidin. Tekrar Yönet ' e tıklayarak devre dışı bırakmak istediğiniz sanal makinenin yanındaki onay kutusunu işaretleyin ve ardından yeniden **Aç düğmesine tıklayın** .

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Bir sanal makinede oto yönetimi devre dışı bırakılıyor.":::

Kabul etmiş önce **devre dışı bırakmak** için ortaya çıkan açılan pencerede iletiyi dikkatle okuyun.

> [!NOTE]
> Bir VM 'de oto yönetimini devre dışı bırakmak aşağıdaki davranışa neden olur:
>
> - VM 'nin yapılandırması ve bu hizmetin değişeklendi.
> - Bu hizmetler tarafından tahakkuk eden ücretler faturalandırılabilir ve tahakkuk olmaya devam eder.
> - Tüm Oto yönetimi davranışları hemen durdurulur.


İlk ve daha önce, sanal makineyi eklendi ve yapılandırdığımız hizmetlerden herhangi birinden kurmayacak. Bu nedenle, bu hizmetler tarafından tahakkuk eden ücretler faturalandırılabilir olmaya devam edecektir. Gerekirse Pano 'ya ihtiyacınız olacaktır. Herhangi bir oto yönetimi davranışı hemen durdurulur. Örneğin, artık VM 'yi DRFT için izliyoruz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal makineler için oto yönetimi 'nin, en iyi uygulama Azure hizmetlerini bilmeniz, bu makaleye ekleme ve yapılandırma ihtiyacını ortadan kaldırabildiğiniz bir yol sağladığını öğrendiniz. Ayrıca, eklendi sanal makineler için otomatik olarak yönetmeye yönelik bir makine, ayarlanan yapılandırma profillerinden Drifts, otomatik olarak yeniden uyumluluğa geri alınacaktır.

Azure portal sanal makineler için oto yönetimini etkinleştirmeyi deneyin.

> [!div class="nextstepaction"]
> [Azure portal sanal makineler için oto yönetimini etkinleştir](quick-create-virtual-machines-portal.md)