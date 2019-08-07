---
title: Azure ayırmalarını yönetme
description: Azure ayırmalarını nasıl yönetebileceğinizi öğrenin.
ms.service: billing
author: bandersmsft
manager: yashesvi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/06/2019
ms.author: banders
ms.openlocfilehash: b161fc7cd4faa75dd87613c297c12f1edd862510
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840009"
---
# <a name="manage-reservations-for-azure-resources"></a>Azure kaynakları için ayırmaları yönetme

Bir Azure ayırması satın aldıktan sonra, ayırmayı farklı bir aboneliğe uygulamanız, ayırmayı yönetebilen veya ayırmanın kapsamını değiştirmeniz gerekebilir. Ayrıca, başka bir aboneliğe satın aldığınız örneklerin bazılarını uygulamak için bir ayırmayı iki ayırmaya bölebilirsiniz.

Azure ayrılmış sanal makine örnekleri satın aldıysanız, ayırma için iyileştirme ayarını değiştirebilirsiniz. Rezervasyon iskontosu aynı serideki sanal makinelere uygulanabilir veya belirli bir VM boyutu için veri merkezi kapasitesini ayırabilirsiniz. Ayrıca, ayırmaları tam olarak kullanılmak üzere iyileştirmenize de çalışırsınız.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reservation-order-and-reservation"></a>Rezervasyon siparişi ve rezervasyon

Bir ayırmayı satın aldığınızda iki nesne oluşturulur: **Rezervasyon siparişi** ve **rezervasyon**.

Satın alma sırasında, bir rezervasyon siparişinin altında bir ayırma vardır. Bölünmüş, birleştirme, kısmi para iadesi veya Exchange gibi eylemler **rezervasyon siparişi**altında yeni ayırmalar oluşturur.

Bir rezervasyon siparişi görüntülemek için **rezervasyonlar** ' a gidin > ayırmayı seçin ve ardından **rezervasyon siparişi kimliği**' ne tıklayın.

![Rezervasyon siparişi KIMLIĞINI gösteren rezervasyon siparişi ayrıntıları örneği ](./media/billing-manage-reserved-vm-instance/reservation-order-details.png)

Ayırma, izinleri rezervasyon siparişinden devralır.

## <a name="change-the-reservation-scope"></a>Ayırma kapsamını değiştirme

 Rezervasyon indiriminiz, sanal makineler, SQL veritabanları, Azure Cosmos DB veya rezervasyon ile eşleşen diğer kaynaklar için geçerlidir ve rezervasyon kapsamında çalışır. Faturalandırma bağlamı, ayırmayı satın almak için kullanılan aboneliğe bağımlıdır.

Bir ayırmanın kapsamını güncelleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmet** > **ayırmaları**' ni seçin.
3. Ayırmayı seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Kapsamı değiştirin.

Paylaşılan sunucudan tek bir kapsamda değişiklik yaparsanız yalnızca sahip olduğunuz abonelikleri seçebilirsiniz. Yalnızca rezervasyonla aynı fatura bağlamında bulunan abonelikleri seçebilirsiniz.

Kapsam yalnızca Kullandıkça Öde tarifesine (MS-AZR-0003P veya MS-AZR-0023P), kurumsal teklif MS-AZR-0017P veya MS-AZR-0148P veya CSP abonelik türlerini içeren tek tek abonelikler için geçerlidir.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme

Ayırma siparişi veya rezervasyon üzerindeki rollere kişi ekleyerek rezervasyon Yönetimi yetkisini sağlayabilirsiniz. Varsayılan olarak, rezervasyon siparişi ve hesap yöneticisi 'nin sahip olduğu kişi rezervasyon siparişi ve rezervasyon üzerinde sahip rolüne sahiptir.

Rezervasyon indirimlerinden ve ayırmaların erişimini, rezervasyon iskontosunu alan aboneliklerden bağımsız olarak yönetebilirsiniz. Bir rezervasyon siparişi veya ayırmayı yönetmek için birine izin verdiğinizde, bu kullanıcılara aboneliği yönetme izni vermez. Benzer şekilde, bir kullanıcıya rezervasyon kapsamındaki bir aboneliği yönetme izni verirseniz, bu kullanıcılara rezervasyon siparişi veya ayırmayı yönetme hakkı vermez.

Bir Exchange veya para iadesi gerçekleştirmek için, kullanıcının rezervasyon emrine erişimi olması gerekir. Bir kişiye izin verirken ayırmayı değil, rezervasyon siparişi için izin verilmesi en iyisidir.


Bir ayırma için erişim yönetimini devretmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Erişiminiz olan ayırmaları listelemek için **tüm hizmetler** > **ayırmasını** seçin.
3. Diğer kullanıcılara erişim vermek istediğiniz rezervasyonu seçin.
4. Seçin **erişim denetimi (IAM)** .
5. **Rol atama** > rolüsahibini > Ekle ' yi seçin. Sınırlı erişim vermek istiyorsanız farklı bir rol seçebilirsiniz.
6. Sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın.
7. Kullanıcıyı ve ardından **Kaydet**'i seçin.

## <a name="split-a-single-reservation-into-two-reservations"></a>Tek bir ayırmayı iki ayırmaya bölme

 Bir ayırma içinde birden fazla kaynak örneği satın aldıktan sonra, bu ayırma içindeki örnekleri farklı aboneliklere atamak isteyebilirsiniz. Varsayılan olarak, tüm örneklerde tek bir abonelik ya da paylaşılan bir kapsam vardır. Örneğin, 10 ayırma örneği satın aldınız ve ' A abonelik olacak kapsam belirttiniz. Artık yedi ayırma kapsamını A aboneliğine ve kalan üç abonelik ile B aboneliğine değiştirmek isteyebilirsiniz. ayırmayı bölmek, parçalı kapsam yönetimine ait örnekleri dağıtmanıza olanak tanır. Abonelik ayırmayı, paylaşılan kapsam ' i seçerek kolaylaştırabilirsiniz. Ancak maliyet yönetimi veya bütçeleme amaçlarıyla, belirli aboneliklerde miktarlar ayırabilirsiniz.

 Bir ayırmayı PowerShell, CLı veya API aracılığıyla iki ayırmaya ayırabilirsiniz.

### <a name="split-a-reservation-by-using-powershell"></a>PowerShell kullanarak bir ayırmayı bölme

1. Aşağıdaki komutu çalıştırarak rezervasyon siparişi KIMLIĞINI alın:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzReservationOrder
    ```

2. Bir ayırmanın ayrıntılarını alın:

    ```powershell
    Get-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Ayırmayı ikiye ayırın ve örnekleri dağıtın:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Aşağıdaki komutu çalıştırarak kapsamı güncelleştirebilirsiniz:

    ```powershell
    Update-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancel-exchange-or-refund-reservations"></a>İptal, Exchange veya para iadesi rezervasyonları

Belirli sınırlamalara sahip rezervasyonları iptal edebilir, Exchange veya para iadesi yapabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Ayrılmış VM örnekleri için en iyileştirme ayarını değiştir

 Ayrılmış bir sanal makine örneği satın aldığınızda, örnek boyutu esnekliği veya kapasite önceliği ' ni seçersiniz. Örnek boyutu esnekliği, aynı [VM Boyut grubundaki](https://aka.ms/RIVMGroups)diğer VM 'lere rezervasyon iskontosunu uygular. Kapasite önceliği, dağıtımlarınız için veri merkezi kapasitesini önceliklendirir. Bu seçenek, ihtiyacınız olduğunda sanal makine örneklerini başlatma yeteneizin daha fazla güvenilirlik sağlar.

Varsayılan olarak, ayırma kapsamı paylaşıldığında, örnek boyutu esnekliği açık olur. Veri merkezi kapasitesi VM dağıtımları için öncelik vermez.

Kapsamın tek olduğu rezervasyonlar için, sanal makine örneği boyutu esnekliği yerine kapasiteyi kapasite önceliği açısından iyileştirebilirsiniz.

Ayırma için optimizasyon ayarını güncelleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmet** > **ayırmaları**' ni seçin.
3. Ayırmayı seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. **En iyileştirme** ayarını değiştirin.

## <a name="optimize-reservation-use"></a>Rezervasyon kullanımını iyileştirme

Azure rezervasyon tasarrufları yalnızca sürekli kaynak kullanımı sonucunu elde etmenizi sağlar. Bir rezervasyon satın alma yaptığınızda, bir veya üç yıllık dönem üzerinde% 100 olası kaynak kullanımı için bir ön maliyet ödeyin. Mümkün olduğunca fazla kullanım ve tasarruf sağlamak için rezervasyonunuzu en üst düzeye çıkarmayı deneyin. Aşağıdaki bölümlerde, bir ayırmanın nasıl izleneceği ve kullanımını iyileştireleceği açıklanmaktadır.

### <a name="view-reservation-use-in-the-azure-portal"></a>Azure portal rezervasyon kullanımını görüntüleyin

Ayırma kullanımını görüntülemenin bir yolu Azure portal.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmet** > [**ayırmaları**](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) ' ni seçin ve bir ayırma için **kullanımı (%)** olarak aklınızda edin.  
  ![Rezervasyonların listesini gösteren resim](./media/billing-manage-reserved-vm-instance/reservation-list.png)
3. Bir ayırma seçin.
4. Zaman içinde ayırma kullanımı eğilimi gözden geçirin.  
  ![Ayırma kullanımını gösteren resim ](./media/billing-manage-reserved-vm-instance/reservation-utilization-trend.png)

### <a name="view-reservation-use-with-api"></a>API ile rezervasyon kullanımını görüntüleme

Bir Kurumsal Anlaşma (EA) müşterisiyseniz, kuruluşunuzdaki rezervasyonların nasıl kullanıldığını programlı bir şekilde görüntüleyebilirsiniz. Kullanım verileri aracılığıyla kullanılmayan ayırma alırsınız. Rezervasyon ücretlerini gözden geçirdikten sonra, verilerin gerçek maliyet ve itfası maliyetleri arasında bölündüğünü aklınızda bulundurun. Gerçek maliyet, Aylık faturanızı mutabık kılmak için veri sağlar. Ayrıca, rezervasyon satın alma maliyeti ve rezervasyon uygulaması ayrıntıları da vardır. , Ayırma kullanımı için geçerli fiyat eşit olarak dağıtılmış olması dışında, itfası maliyeti gerçek maliyet gibidir. Kullanılmayan rezervasyon saatleri, itfası maliyeti verilerinde gösterilir. EA müşterilerinin kullanım verileri hakkında daha fazla bilgi için bkz. [Kurumsal Anlaşma rezervasyon maliyetlerini ve kullanımını edinme](billing-understand-reserved-instance-usage-ea.md).

Diğer abonelik türleri için, [rezervasyon siparişi ve rezervasyonuna göre API ayırmaları özetler listesini](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation)kullanın.

### <a name="optimize-your-reservation"></a>Ayırmayı iyileştirin

Kuruluşunuzun ayırmalarının eksik kullanıldığını fark ederseniz:

- Kuruluşunuzun oluşturduğu sanal makinelerin, ayırma için VM boyutuyla eşleştiğinden emin olun.
- Örnek boyutu esnekliğin açık olduğundan emin olun. Daha fazla bilgi için bkz. [ayırmaları yönetme-ayrılmış VM örnekleri için en iyileştirme ayarını değiştir](#change-optimize-setting-for-reserved-vm-instances).
- Daha geniş bir uygulama için ayırma kapsamını _paylaşılan_ olarak değiştirin. Daha fazla bilgi için bkz. [bir ayırma kapsamını değiştirme](#change-the-reservation-scope).
- Kullanılmayan miktarı değiş tokuş etmeyi göz önünde bulundurun. Daha fazla bilgi için bkz. [iptaller ve alışverişlerde](#cancel-exchange-or-refund-reservations).


## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız varsa, [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)

Bir hizmet planı satın alın:
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Cosmos DB ayrılmış kapasiteye sahip Azure Cosmos DB kaynakları için ön ödeme](../cosmos-db/cosmos-db-reserved-capacity.md)

Yazılım planı satın alın:
- [Azure ayırmalarının Red Hat yazılım planları için ön ödeme](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Azure Ayırmaları ile SUSE yazılım planları için ön ödeme yapma](../virtual-machines/linux/prepay-suse-software-charges.md)

İndirim ve kullanımı anlayın:
- [VM rezervasyon iskontosunun nasıl uygulanacağını anlayın](billing-understand-vm-reservation-charges.md)
- [Red Hat Enterprise Linux yazılım planı iskontosunun nasıl uygulanacağını anlayın](../billing/billing-understand-rhel-reservation-charges.md)
- [SUSE Linux Enterprise yazılım planı iskontosunun nasıl uygulandığını anlayın](../billing/billing-understand-suse-reservation-charges.md)
- [Diğer rezervasyon iskontolarının nasıl uygulanacağını anlayın](billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri rezervasyonlar içermez](billing-reserved-instance-windows-software-costs.md)
