---
title: Azure Geçiş cihazı mimarisi
description: Sunucu değerlendirme ve geçişte kullanılan Azure Geçiş cihazına genel bir bakış sağlar.
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: d55d123bb056b46b5e78dd8ac836eeaf9b42fe70
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389027"
---
# <a name="azure-migrate-appliance-architecture"></a>Azure Geçiş cihazı mimarisi

Bu makalede, Azure Geçir cihazı mimarisi ve işlemleri açıklanmaktadır. Azure Geçir cihazı, Azure'a geçiş için VM'leri ve fiziksel sunucuları keşfetmek için şirket içinde dağıtılan hafif bir cihazdır. 

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Azure Geçir cihazı aşağıdaki senaryolarda kullanılır.

**Senaryo** | **Araç** | **Kullanıldığı yerler** 
--- | --- | ---
**VMware VM değerlendirmesi** | Azure Geçiş:Sunucu Değerlendirmesi | VMware VM'leri keşfedin.<br/><br/> Makine uygulamalarını ve bağımlılıkları keşfedin.<br/><br/> Makine meta verilerini ve performans meta verilerini toplayın ve Azure'a gönderin.
**VMware VM geçişi (aracısız)** | Azure Geçir:Sunucu Geçişi | VMware VM'leri keşfedin<br/><br/>  VMware VM'leri [aracısız geçişle çoğaltın.](server-migrate-overview.md)
**Hiper-V VM değerlendirmesi** | Azure Geçiş:Sunucu Değerlendirmesi | Hyper-V VM'leri keşfedin.<br/><br/> Makine meta verilerini ve performans meta verilerini toplayın ve Azure'a gönderin.
**Fiziksel makine** |  Azure Geçiş:Sunucu Değerlendirmesi |  Fiziksel sunucuları keşfedin.<br/><br/> Makine meta verilerini ve performans meta verilerini toplayın ve Azure'a gönderin.

## <a name="appliance-components"></a>Cihaz bileşenleri

Cihazın bir dizi bileşeni vardır.

- **Yönetim uygulaması**: Cihaz dağıtımı sırasında kullanıcı girişi için kullanılan bir web uygulamasıdır. Azure'a geçiş için makineleri değerlendirirken kullanılır.
- **Bulma aracısı**: Aracı makine yapılandırma verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır. 
- **Değerlendirme aracısı**: Aracı performans verilerini toplar. Azure'a geçiş için makineleri değerlendirirken kullanılır.
- **DRA aracısı**: VM çoğaltmayı yönetir ve çoğaltılan makinelerle Azure arasındaki iletişimi koordine eder. Yalnızca Aracısız geçiş kullanarak VMware VM'leri Azure'a kopyalarken kullanılır.
- **Ağ Geçidi**: Çoğaltılan verileri Azure'a gönderir. Yalnızca Aracısız geçiş kullanarak VMware VM'leri Azure'a kopyalarken kullanılır.
- **Otomatik güncelleme hizmeti**: Cihaz bileşenlerini günceller (her 24 saatte bir çalışır).



## <a name="appliance-deployment"></a>Cihaz dağıtımı

- Azure Geçiş cihazı [Hyper-V](how-to-set-up-appliance-hyper-v.md) veya [VMware](how-to-set-up-appliance-vmware.md) için bir şablon veya [VMware/Hyper-V](deploy-appliance-script.md)için powershell komut dosyası yükleyicisi ve [fiziksel sunucular](how-to-set-up-appliance-physical.md)kullanılarak ayarlanabilir. 
- Cihaz destek gereksinimleri ve dağıtım ön koşulları [cihaz destek matrisinde](migrate-appliance.md)özetlenmiştir.


## <a name="appliance-registration"></a>Cihaz kaydı

Cihaz kurulumu sırasında, cihazı Azure Geçiş'e kaydederseniz ve tabloda özetlenen eylemler oluşur.

**Eylem** | **Şey** | **Izin**
--- | --- | ---
**Kaynak sağlayıcılar kaydedin** | Bu kaynak sağlayıcıları, cihaz kurulumu sırasında seçtiğiniz abonelikte kayıtlıdır: Microsoft.OffAzure, Microsoft.Migrate ve Microsoft.KeyVault.<br/><br/> Bir kaynak sağlayıcısı nın kaydedilmesi, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. | Kaynak sağlayıcılarını kaydetmek için abonelikte Bir Katılımcı veya Sahip rolüne ihtiyacınız var.
**Azure AD uygulama iletişimi oluşturma** | Azure Geçiş, cihazda çalışan aracılar ile Azure'da çalışan ilgili hizmetleri arasında iletişim (kimlik doğrulama ve yetkilendirme) için bir Azure Etkin Dizin (Azure AD) uygulaması oluşturur.<br/><br/> Bu uygulamanın Azure Kaynak Yöneticisi aramaları yapmak için ayrıcalıkları yoktur veya herhangi bir kaynağa RBAC erişimi yoktur. | Uygulamayı oluşturmak için Azure Geçiş için [bu izinlere](tutorial-prepare-vmware.md#assign-permissions-to-register-the-appliance) ihtiyacınız vardır.
**Azure AD uygulamaları oluşturma-Anahtar kasası** | Bu uygulama yalnızca VMware VM'lerin Azure'a aracısız geçişi için oluşturulur.<br/><br/> Yalnızca aracısız geçiş için kullanıcının aboneliğinde oluşturulan anahtar kasasına erişmek için kullanılır.<br/><br/> Cihazdan keşif başlatıldığında Azure anahtar kasasında (müşterinin kiracısında oluşturulmuş) RBAC erişimine sahiptir. | Uygulamayı oluşturmak için Azure Geçiş için [bu izinlere](tutorial-prepare-vmware.md#assign-permissions-to-register-the-appliance) ihtiyacınız vardır.



## <a name="collected-data"></a>Toplanan veriler

Tüm dağıtım senaryoları için istemci tarafından toplanan veriler [cihaz destek matrisinde](migrate-appliance.md)özetlenmiştir.

## <a name="discovery-and-collection-process"></a>Bulma ve toplama işlemi

![Mimari](./media/migrate-appliance-architecture/architecture.png)

Cihaz aşağıdaki işlemi kullanarak vCenter Sunucuları ve Hyper-V ana bilgisayarları/kümeleriyle iletişim kurar.

1. **Keşfi başlat**:
    - Hyper-V cihazında keşfi başlattığınızda, WinRM 5985 (HTTP) ve 5986 (HTTPS) bağlantı noktalarındaki Hyper-V ana bilgisayarlarıyla iletişim kurar.
    - VMware cihazında keşfetmeye başladığınızda, varsayılan olarak TCP bağlantı noktası 443'teki vCenter sunucusuyla iletişim kurar. vCenter sunucusu farklı bir bağlantı noktasında dinliyorsa, bunu cihaz web uygulamasında yapılandırabilirsiniz.
2. **Meta veri ve performans verilerini toplamak:**
    - Cihaz, 5985 ve 5986 bağlantı noktalarındaki Hyper-V ana bilgisayarından Hyper-V VM verilerini toplamak için Ortak Bilgi Modeli (CIM) oturumu kullanır.
    - Cihaz, vCenter Server'dan VMware VM verilerini toplamak için varsayılan olarak 443 portu ile iletişim kurar.
3. **Veri gönderme**: Cihaz toplanan verileri Azure Geçir Sunucusu Değerlendirmesi'ne ve Azure Geçir Sunucusu Geçişini SSL bağlantı noktası 443 üzerinden gönderir. Cihaz Azure'a Internet üzerinden bağlanabilir veya Public/Microsoft bakışlarıyla ExpressRoute'u kullanabilirsiniz.
    - Performans verileri için cihaz gerçek zamanlı kullanım verileri toplar.
        - Performans verileri VMware için her 20 saniyede bir ve Hyper-V için her 30 saniyede bir, her performans ölçümü için toplanır.
        - Toplanan veriler, 10 dakika boyunca tek bir veri noktası oluşturmak için toplanır.
        - En yüksek kullanım değeri, 20/30 saniyelik veri noktalarının tümünden seçilir ve değerlendirme hesaplaması için Azure'a gönderilir.
        - Değerlendirme özelliklerinde (50.90./95./99uncu) belirtilen yüzdelik değere göre, on dakikalık puanlar artan sırada sıralanır ve değerlendirmeyi hesaplamak için uygun yüzdelik değer kullanılır
    - Sunucu Geçişi için cihaz VM verilerini toplamaya başlar ve azure'a kopyalar.
4. **Değerlendirin ve geçirin**: Artık Azure Geçir Sunucusu Değerlendirmesi'ni kullanarak cihaz tarafından toplanan meta verilerden değerlendirmeler oluşturabilirsiniz. Ayrıca, aracısız VM çoğaltmayı düzenlemek için Azure Geçir Sunucu Geçişi'ni kullanarak VMware VM'leri taşımaya da başlayabilirsiniz.





## <a name="appliance-upgrades"></a>Cihaz yükseltmeleri

Cihaz üzerinde çalışan Azure Geçiş aracıları güncelleştirildikçe cihaz yükseltilir. Cihazda varsayılan olarak otomatik güncelleştirme etkin olduğundan bu otomatik olarak gerçekleşir. Aracıları el ile güncelleştirmek için bu varsayılan ayarı değiştirebilirsiniz.

kayıt defterinde otomatik güncelleştirmeyi HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance "AutoUpdate" anahtarını 0 (DWORD) olarak ayarlayarak kapatırsınız.

 

## <a name="next-steps"></a>Sonraki adımlar

Cihaz destek matrisini [gözden geçirin.](migrate-appliance.md)

