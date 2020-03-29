---
title: Portalda otomatik ölçeklendirme bir bulut hizmeti | Microsoft Dokümanlar
description: Azure'daki bulut hizmeti web rolü veya çalışan rolü için otomatik ölçek kurallarını yapılandırmak için portalı nasıl kullanacağınızı öğrenin.
services: cloud-services
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 05/18/2017
ms.author: tagore
ms.openlocfilehash: 5880544137855a2ea5bcd6d6e4bada46563564ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75360846"
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a>Portaldaki Bulut Hizmeti için otomatik ölçekleme nasıl yapılandırılabilen

Çalışma içinde veya dışında bir ölçek tetikleyen bir bulut hizmeti çalışanı rolü için koşullar ayarlanabilir. Rol için koşullar, rolün CPU, disk veya ağ yükünü temel alabilir. İleti kuyruğuna veya aboneliğinizle ilişkili başka bir Azure kaynağının ölçüsüne dayalı bir koşul da ayarlayabilirsiniz.

> [!NOTE]
> Bu makalede Bulut Hizmeti web ve çalışan rolleri üzerinde duruluyor. Doğrudan sanal bir makine (klasik) oluşturduğunuzda, bir bulut hizmetinde barındırılır. Standart bir sanal makineyi [kullanılabilirlik kümesiyle](../virtual-machines/windows/classic/configure-availability-classic.md) ilişkilendirerek ölçeklendirebilir ve el ile açabilir veya kapatabilirsiniz.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Uygulamanız için ölçekleme yapılandırmadan önce aşağıdaki bilgileri göz önünde bulundurmanız gerekir:

* Ölçekleme çekirdek kullanımından etkilenir.

    Daha büyük rol örnekleri daha fazla çekirdek kullanır. Bir uygulamayı yalnızca aboneliğiniz için çekirdek sınırı içinde ölçeklendirebilirsiniz. Örneğin, aboneliğinizin 20 çekirdek sınırı olduğunu varsayın. İki orta ölçekli bulut hizmeti (toplam 4 çekirdek) içeren bir uygulama çalıştırırsanız, aboneliğinizdeki diğer bulut hizmeti dağıtımlarını yalnızca kalan 16 çekirdekle ölçeklendirebilirsiniz. Boyutlar hakkında daha fazla bilgi için [Bulut Hizmet Boyutları'na](cloud-services-sizes-specs.md)bakın.

* Sıra iletisi eşiğine göre ölçeklendirebilirsiniz. Kuyrukların nasıl kullanılacağı hakkında daha fazla bilgi için [Sıra Depolama Hizmeti'ni nasıl kullanacağına](../storage/queues/storage-dotnet-how-to-use-queues.md)bakın.

* Aboneliğinizle ilişkili diğer kaynakları da ölçeklendirebilirsiniz.

* Uygulamanızın yüksek kullanılabilirliğini sağlamak için, uygulamanın iki veya daha fazla rol örneğiyle dağıtıldığından emin olmalısınız. Daha fazla bilgi için [Hizmet Düzeyi Sözleşmeleri'ne](https://azure.microsoft.com/support/legal/sla/)bakın.

* Otomatik Ölçek yalnızca tüm roller **Hazır** durumunda olduğunda olur.  


## <a name="where-scale-is-located"></a>Ölçeğin bulunduğu yer
Bulut hizmetinizi seçtikten sonra bulut servis bıçağının görünür olması gerekir.

1. Bulut servis bıçağında, **Roller ve Örnekler** döşemesinde bulut hizmetinin adını seçin.   
   **ÖNEMLİ**: Rolün altındaki rol örneğini değil, bulut hizmeti rolünü tıklattığınızdan emin olun.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. **Ölçek** döşemesini seçin.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Otomatik ölçek
Bir rol için ölçek ayarlarını iki mod **manuel** veya **otomatik**olarak yapılandırabilirsiniz. Manuel beklediğiniz gibi, örnekleri mutlak sayısını ayarlayın. Otomatik ancak nasıl ve ne kadar ölçeklendirmeniz gerektiğini yöneten kurallar belirlemenize olanak sağlar.

Zamanlama ve performans **kuralları**için ölçeği seçeneğine **göre** ayarlayın.

![Bulut hizmetleri, profil ve kuralla ayarları ölçeklendir](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Varolan bir profil.
2. Üst profil için bir kural ekleyin.
3. Başka bir profil ekleyin.

**Profil Ekle'yi**seçin. Profil, ölçek için hangi modu kullanmak istediğinizi belirler: **her zaman,** **yineleme,** **sabit tarih**.

Profili ve kuralları yapılandırıldıktan sonra üstteki **Kaydet** simgesini seçin.

#### <a name="profile"></a>Profil
Profil, ölçek için ve ayrıca bu ölçek aralığı etkin olduğunda en küçük ve maksimum örnekleri ayarlar.

* **Her zaman**

    Bu örnek aralığını her zaman kullanılabilir tutun.  

    ![Her zaman ölçeklendirilen bulut hizmeti](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Yineleme**

    Ölçeklendirmek için haftanın gün kümesini seçin.

    ![Yineleme zamanlaması olan bulut hizmet ölçeği](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Sabit Tarih**

    Rolü ölçeklendirmek için sabit bir tarih aralığı.

    ![Sabit bir tarihe sahip CLoud hizmet ölçeği](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Profili yapılandırıldıktan sonra, profil bıçağının altındaki **Tamam** düğmesini seçin.

#### <a name="rule"></a>Kural
Kurallar bir profile eklenir ve ölçeği tetikleyen bir koşulu temsil eder.

Kural tetikleyicisi, koşullu değer ekleyebileceğiniz bulut hizmetinin (CPU kullanımı, disk etkinliği veya ağ etkinliği) bir ölçüsüne dayanır. Ayrıca, bir ileti kuyruğunu veya aboneliğinizle ilişkili başka bir Azure kaynağının ölçüsüne dayalı tetikleyiciye sahip olabilirsiniz.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Kuralı yapılandırıldıktan sonra, kural bıçağının altındaki **Tamam** düğmesini seçin.

## <a name="back-to-manual-scale"></a>Manuel ölçeğe geri dön
[Ölçek ayarlarına](#where-scale-is-located) gidin ve **Ölçeği** el **ile girdiğim bir örnek sayısına**göre ayarlayın.

![Bulut hizmetleri, profil ve kuralla ayarları ölçeklendir](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Bu ayar, otomatik ölçekleme giderimi rolünden kaldırır ve ardından örnek sayısını doğrudan ayarlayabilirsiniz.

1. Ölçek (manuel veya otomatik) seçeneği.
2. Örneklerini ölçeklendirecek şekilde ayarlamak için bir rol örneği kaydırıcısı.
3. Ölçeklendirilen rolün örnekleri.

Ölçek ayarlarını yapılandırıldıktan sonra üstteki **Kaydet** simgesini seçin.



