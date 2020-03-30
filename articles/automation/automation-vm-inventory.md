---
title: Bir Azure sanal makinesini stok toplama ile yönetme | Microsoft Docs
description: Bir sanal makineyi stok toplama ile yönetme
services: automation
ms.subservice: change-inventory-management
keywords: stok, otomasyon, değişiklik, izleme
ms.date: 01/28/2020
ms.topic: conceptual
ms.openlocfilehash: d0324038b8a38d7eba84e5472b8f90439b0322c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844828"
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Bir Azure sanal makinesini stok toplama ile yönetme

Bir Azure sanal makinesinin kaynak sayfasından sanal makine için stok izlemeyi etkinleştirebilirsiniz. Bilgisayarlarınızda aşağıdaki stok bilgilerini toplayabilir ve görüntüleyebilirsiniz:

- Windows yazılımı (Windows uygulamaları ve Windows güncelleştirmeleri), hizmetler, dosyalar ve Kayıt Defteri anahtarları
- Linux yazılımı (paketler) daemons ve dosyaları

Bu yöntem, stok toplamayı ayarlama ve yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar.

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz yoksa, ücretsiz [bir hesap oluşturun.](https://azure.microsoft.com/free/)

Bu makalede, çözümü yapılandırmak için bir VM'niz olduğunu varsayar. Bir Azure sanal makineniz yoksa bir [sanal makine](../virtual-machines/windows/quick-create-portal.md) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Sanal makine kaynak sayfasından stok toplamayı etkinleştirme

1. Azure portalının sol tarafında **Sanal makineler**'i seçin.
2. Sanal makine listesinden bir sanal makine seçin.
3. **Kaynak** menüsünde, **İşlemler**altında **Stok'u**seçin.
4. Veri günlüklerinizi depolamak için bir Günlük Analizi çalışma alanı seçin.
    Bu bölge için kullanabileceğiniz bir çalışma alanı yoksa, varsayılan bir çalışma alanı ve otomasyon hesabı oluşturmanız istenir.
5. Bilgisayarınızı eklemeye başlamak için **Etkinleştir**'i seçin.

   ![Ekleme seçeneklerini görüntüleme](./media/automation-vm-inventory/inventory-onboarding-options.png)

    Çözümün etkinleştirildiği durum çubuğunda bildirilir. Bu işlemin tamamlanması 15 dakika sürebilir. Bu süre zarfında, pencereyi kapatabilirsiniz veya açık tutabilirsiniz ve çözüm etkinleştirildiğinde sizi bilgilendirebilir. Dağıtım durumunu bildirimler bölmesinden izleyebilirsiniz.

   ![Eklemeden hemen sonra stok çözümünü görüntüleme](./media/automation-vm-inventory/inventory-onboarded.png)

Dağıtım tamamlandığında durum çubuğu kaybolur. Sistem stok verilerini toplamaktadır ve veriler henüz görünmeyebilir. Tam bir veri toplama işlemi 24 saat sürebilir.

## <a name="configure-your-inventory-settings"></a>Stok ayarlarınızı yapılandırma

Varsayılan olarak yazılım, Windows hizmetleri ve Linux daemon'ları toplama işlemi için yapılandırılmıştır. Windows kayıt defteri ve dosya stoğunu toplamak için stok toplama ayarlarını yapılandırın.

1. **Stok** görünümünde, pencerenin üst kısmındaki **Ayarları Edit** düğmesini seçin.
2. Yeni bir toplama ayarı eklemek için **Windows Kayıt Defteri**, **Windows Dosyaları** ve **Linux Dosyaları** sekmelerini seçerek eklemek istediğiniz ayar kategorisine gidin.
3. Uygun kategoriyi seçin ve pencerenin üst kısmında **Ekle'yi** tıklatın.

Aşağıdaki tablolar, çeşitli kategoriler için yapılandırılabilen her özellik hakkında bilgi sağlar.

### <a name="windows-registry"></a>Windows Kayıt Defteri

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Windows Kayıt Defteri Anahtarı   | Dosyanın denetleneceği yol. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="windows-files"></a>Windows Dosyaları

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Yolu girin     | Dosyanın denetleneceği yol. Örneğin: “c:\temp\myfile.txt”

### <a name="linux-files"></a>Linux Dosyaları

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Yolu girin     | Dosyanın denetleneceği yol. Örneğin: “/etc/*.conf”       |
|Yol Türü     | İzlenecek öğenin türü için olası değerler: Dosya ve Dizin        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - Sembolik bağlantıları yoksayar ve başvurulan dosyaları veya dizinleri içermez<br>**İzle** - Özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları veya dizinleri de içerir<br>**Yönet** - Sembolik bağlantıları izler ve döndürülen içeriğin işlenmesinde değişiklik yapılmasına olanak sağlar      |

## <a name="manage-machine-groups"></a>Makine gruplarını yönetme

Envanter, Azure Monitor günlüklerinde makine grupları oluşturmanıza ve görüntülemenize olanak tanır. Makine grupları, Azure Monitor günlüklerinde bir sorgu tarafından tanımlanan makine koleksiyonlarıdır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Makine gruplarınızı görüntülemek için Stok sayfasındaki **Makine grupları** sekmesini seçin.

![Stok sayfasındamakine gruplarını görüntüleme](./media/automation-vm-inventory/inventory-machine-groups.png)

Listeden bir makine grubu seçmek, Makine grupları sayfasını açar. Bu sayfa, makine grubu yla ilgili ayrıntıları gösterir. Bu ayrıntılar, grubu tanımlamak için kullanılan günlük analizi sorgusunu içerir. Sayfanın alt kısmında, bu grubun bir parçası olan makinelerin sayfalı bir listesi yer alır.

![Makine grubu sayfasını görüntüleme](./media/automation-vm-inventory/machine-group-page.png)

Makine grubunu klonlamak için **+ Klon** düğmesini tıklatın. Burada gruba grup için yeni bir ad ve takma ad vermelisiniz. Tanım şu anda değiştirilebilir. Sorguyu değiştirdikten sonra seçilecek makineleri önizlemek için **sorguyu doğrula'ya** basın. Grupla mutlu olduğunuzda makine grubunu oluşturmak için **Oluştur'u** tıklatın

Yeni bir makine grubu oluşturmak istiyorsanız, **+ Makine grubu oluşturun'u**seçin. Bu düğme, yeni grubunuzu tanımlayabileceğiniz **bir makine grubu oluştur sayfasını** açar. Grubu oluşturmak için **Oluştur**’a tıklayın.

![Yeni makine grubu oluşturma](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-virtual-machine-from-management"></a>Sanal makinenizin yönetim bağlantısını kesme

Sanal makinenizi stok yönetiminden kaldırmak için:

1. Azure portalının sol tarafındaki bölmeden **Log Analytics**'i ve sanal makineyi eklerken kullandığınız çalışma alanını seçin.
2. **Log Analytics** penceresinin **Kaynak** menüsünün **Çalışma Alanı Veri Kaynakları** kategorisinden **Sanal makineler**'i seçin.
3. Listeden bağlantısını kesmek istediğiniz sanal makineyi seçin. Sanal makinenin yanında, **OMS Bağlantısı** sütunda **Bu çalışma alanı** ifadesini içeren bir yeşil onay işareti bulunur.

   >[!NOTE]
   >OMS artık Azure Monitor günlükleri olarak adlandırılır.
   
4. Sonraki sayfanın en üstünde **Bağlantıyı kes**'i seçin.
5. Onay penceresinde **Evet**'i seçin.
    Bu eylem makinenin yönetim paneli bağlantısını keser.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makinelerinizdeki dosya ve kayıt defteri ayarlarında yapılan değişiklikleri yönetme hakkında bilgi almak için bkz. [Değişiklik İzleme çözümüyle ortamınızdaki yazılım değişikliklerini izleme](../log-analytics/log-analytics-change-tracking.md).
* Sanal makinelerinizdeki Windows ve paket güncelleştirmelerini yönetme hakkında bilgi edinmek için [Azure'da Güncelleştirme Yönetimi çözümüne](../operations-management-suite/oms-solution-update-management.md)bakın.

