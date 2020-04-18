---
title: Bir Azure sanal makinesini stok toplama ile yönetme | Microsoft Docs
description: Bir sanal makineyi stok toplama ile yönetme
services: automation
ms.subservice: change-inventory-management
keywords: stok, otomasyon, değişiklik, izleme
ms.date: 01/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0627d2daa70c276535dc43b722e22e1d73b0c8d2
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617366"
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

[Azure Portal](https://portal.azure.com/) oturum açın.

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

1. Stok sayfasında, sayfanın üst kısmındaki **Ayarları Düzelt'i** tıklatın.
2. Yeni bir koleksiyon ayarı eklemek **için, Windows Kayıt Defteri,** **Windows Dosyaları**veya Linux **Dosyaları** sekmesini seçerek eklemek istediğiniz ayar kategorisine gidin.
3. Uygun kategoriyi seçin ve sayfanın üst kısmında **Ekle'yi** tıklatın.

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
|Etkin     | Ayarı uygulanırsa doğru ve aksi takdirde False.        |
|Öğe Adı     | İzlenecek dosyanın dostu adı.        |
|Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.       |
|Yolu girin     | Dosyayı denetleme yolu, örneğin, **c:\temp\myfile.txt**.

### <a name="linux-files"></a>Linux Dosyaları

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarı uygulanırsa doğru ve aksi takdirde False.        |
|Öğe Adı     | İzlenecek dosyanın dostu adı.        |
|Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
|Yolu girin     | Dosya için kontrol yolu, örneğin, **/etc/*.conf**.       |
|Yol Türü     | İzlenecek öğetürü. Değerler Dosya ve Dizin'dir.        |
|Özyineleme     | İzlenecek öğe ararken özyineleme kullanılıyorsa doğru ve aksi takdirde False.        |
|Sudo Kullan     | Madde yi kontrol ederken sudo kullanılıyorsa doğru ve yanlış olur.         |
|Bağlantılar     | Dizinler arasında geçiş yaparken sembolik bağlantıların nasıl ele alındığına işaret eden değer. Olası değerler şunlardır: <br> Yoksay - Sembolik bağlantıları yoksayar ve başvurulan dosyaları veya dizinleri içermez<br>İzle - Özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları veya dizinleri de içerir<br>Yönet - Sembolik bağlantıları izler ve döndürülen içeriğin işlenmesinde değişiklik yapılmasına olanak sağlar      |

## <a name="manage-machine-groups"></a>Makine gruplarını yönetme

Envanter, Azure Monitor günlüklerinde makine grupları oluşturmanıza ve görüntülemenize olanak tanır. Makine grupları, Azure Monitor günlüklerinde bir sorgu tarafından tanımlanan makine koleksiyonlarıdır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Makine gruplarınızı görüntülemek için Stok sayfasındaki **Makine grupları** sekmesini seçin.

![Stok sayfasındamakine gruplarını görüntüleme](./media/automation-vm-inventory/inventory-machine-groups.png)

Listeden bir makine grubu seçmek, Makine grupları sayfasını açar. Bu sayfa, makine grubu yla ilgili ayrıntıları gösterir. Bu ayrıntılar, grubu tanımlamak için kullanılan günlük analizi sorgusunu içerir. Sayfanın alt kısmında, bu grubun bir parçası olan makinelerin sayfalı bir listesi yer alır.

![Makine grubu sayfasını görüntüleme](./media/automation-vm-inventory/machine-group-page.png)

Makine grubunu klonlamak için **+ Klon'u** tıklatın. Gruba grup için yeni bir ad ve takma ad vermelisiniz. Tanım şu anda değiştirilebilir. Sorguyu değiştirdikten sonra, seçilecek makineleri önizlemek için **sorguyu doğrula'yı** tıklatın. Gruptan memnun olduğunuzda, makine grubunu oluşturmak için **Oluştur'u** tıklatın.

Yeni bir makine grubu oluşturmak istiyorsanız, **+ Makine grubu oluşturun'u**tıklatın. Bu düğme, yeni **grubunuzu** tanımlayabileceğiniz bir makine grubu oluştur sayfasını açar. Grubu oluşturmak için **Oluştur**’a tıklayın.

![Yeni makine grubu oluşturma](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-virtual-machine-from-management"></a>Sanal makinenizin yönetim bağlantısını kesme

Sanal makinenizi stok yönetiminden kaldırmak için:

1. Azure portalının sol tarafındaki bölmeden **Log Analytics**'i ve sanal makineyi eklerken kullandığınız çalışma alanını seçin.
2. Günlük Analitiği sayfasında **Kaynak** menüsünü açın.
3. **Çalışma Alanı Veri Kaynakları**altında Sanal **Makineler** seçin.
4. Listeden bağlantısını kesmek istediğiniz sanal makineyi seçin. Sanal makinenin yanında, **OMS Bağlantısı** sütunda **Bu çalışma alanı** ifadesini içeren bir yeşil onay işareti bulunur.

   >[!NOTE]
   >Operations Management Suite (OMS) artık Azure Monitor günlükleri olarak adlandırılır.
   
5. Bir sonraki sayfanın üst kısmında **Bağlantıyı Kesme'yi**tıklatın.
6. Onay penceresinde, makineyi yönetimden ayırmak için **Evet'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makinelerinizdeki dosya ve kayıt defteri ayarlarında yapılan değişiklikleri yönetme hakkında bilgi almak için bkz. [Değişiklik İzleme çözümüyle ortamınızdaki yazılım değişikliklerini izleme](../log-analytics/log-analytics-change-tracking.md).
* Sanal makinelerinizdeki Windows ve paket güncelleştirmelerini yönetme hakkında bilgi edinmek için [Azure'da Güncelleştirme Yönetimi çözümüne](../operations-management-suite/oms-solution-update-management.md)bakın.