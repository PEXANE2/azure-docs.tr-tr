---
title: Azure Lab Services ile Arcmap\argıs Masaüstü için laboratuvar ayarlama | Microsoft Docs
description: Argıs kullanarak sınıflar için laboratuvar ayarlamayı öğrenin.
author: nicolela
ms.topic: article
ms.date: 02/04/2021
ms.author: nicolela
ms.openlocfilehash: 530597a72b19afa1e80b5c7640b105d86479b1c1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101740121"
---
# <a name="set-up-a-lab-for-arcmaparcgis-desktop"></a>Arcmap\argıs Masaüstü için laboratuvar ayarlama

ArcGIS, bir dizi coğrafi bilgi sistemi (GIS [) türüdür.](https://www.esri.com/en-us/arcgis/products/arcgis-solutions/overview)  ArcGIS haritaları make\analyze ve [Ortam Sistemleri Araştırma Enstitüsü](https://www.esri.com/en-us/home) (Esri) tarafından sağlanan coğrafi verilerle çalışacak şekilde kullanılır.  Argıs Desktop birçok uygulama içerse de, bu makalede ArcMap kullanımı için laboratuvarların nasıl ayarlanacağı gösterilmektedir.  [ArcMap](https://desktop.arcgis.com/en/arcmap/latest/map/main/what-is-arcmap-.htm) , 2B haritalar oluşturmak, düzenlemek ve analiz etmek için kullanılır.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

ArcMap 'i kullanmaya yönelik bir laboratuvar ayarlamaya başlamak için bir Azure aboneliğine ve laboratuvar hesabına ihtiyacınız vardır.  Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilirsiniz.  Yeni laboratuvar hesabı oluşturma hakkında daha fazla bilgi için bkz. [Laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md).  Ayrıca var olan bir laboratuvar hesabı da kullanabilirsiniz.

### <a name="lab-account-settings"></a>Laboratuvar hesabı ayarları

Aşağıdaki tabloda açıklandığı gibi laboratuvar hesabı ayarlarınızı etkinleştirin.  Azure Marketi görüntülerinin nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [Laboratuvar oluşturucuları için kullanılabilen Azure Marketi görüntülerini belirtme](./specify-marketplace-images.md).

| Laboratuvar hesabı ayarı | Yönergeler |
| ------------------- | ------------ |
|Market görüntüsü| Laboratuvar hesabınızda kullanılmak üzere Windows 10 Pro veya Windows 10 Pro N görüntüsünü etkinleştirin.|

### <a name="licensing-server"></a>Lisanslama sunucusu

Masaüstü tarafından sunulan bir lisans türü, [Eşzamanlı Kullanım lisanslarından](https://desktop.arcgis.com/en/license-manager/latest/license-manager-basics.htm)biridir.  Bu, lisans sunucunuza argıs Lisans Yöneticisi 'Ni yüklemenizi gerektirir.  Lisans Yöneticisi, yazılımın aynı anda çalıştırılabilen kopyalarının sayısını izler.  Sunucunuzda lisans yöneticisini ayarlama hakkında daha fazla bilgi için, bkz. [Lisans Yöneticisi Kılavuzu](https://desktop.arcgis.com/en/license-manager/latest/welcome.htm).

Lisans sunucusu genellikle şirket içi ağınızda bulunur veya bir Azure sanal ağı içindeki bir Azure sanal makinesinde barındırılır.  Lisans sunucunuz kurulduktan sonra, sanal ağı [Laboratuvar hesabınızla](./tutorial-setup-lab-account.md) [eşetmeniz](./how-to-connect-peer-virtual-network.md) gerekir.  Laboratuvar sanal makinelerinizin lisans sunucusuna erişebilmesi ve tam tersi şekilde laboratuvar oluşturmadan önce ağ eşlemesini yapmanız gerekir.

Daha fazla bilgi için bkz. [bir lisans sunucusunu paylaşılan kaynak olarak ayarlama](how-to-create-a-lab-with-shared-resource.md).

### <a name="lab-settings"></a>Laboratuvar ayarları

Argıs Masaüstü için kullanmanızı önerdiğimiz sanal makinenin (VM) boyutu uygulamalara, uzantılara ve öğrencilerin kullanacağı belirli sürümlere bağlıdır.  VM boyutu, öğrencilerin gerçekleştirmesi beklenen iş yüklerine de bağlıdır.  VM boyutunu tanımlamaya yardımcı olması için, ArcGIS [Masaüstü sistem gereksinimleri](https://desktop.arcgis.com/en/system-requirements/latest/arcgis-desktop-system-requirements.htm) bölümüne bakın.  Potansiyel VM boyutunu tanımladıktan sonra, yeterli performans sağlamak için öğrencilerin iş yüklerini sınamanızı öneririz.

Bu makalede, diğer bir ArcGIS Masaüstü uzantısının kullanılmlarından bağımsız olarak, [ArcMap 'in sürüm 10.7.1](https://desktop.arcgis.com/en/system-requirements/10.7/arcgis-desktop-system-requirements.htm)için [ **Orta** sanal makine boyutunu](administrator-guide.md#vm-sizing) kullanmanızı öneririz.  Ancak, sınıfınızın ihtiyaçlarına bağlı olarak, **büyük** veya hatta bir küçük/ **Orta ölçekli GPU (görselleştirme)** VM boyutu gerekebilir.  Örneğin, ArcGIS Desktop 'ta bulunan [uzamsal analist uzantısı](https://desktop.arcgis.com/en/arcmap/latest/tools/spatial-analyst-toolbox/gpu-processing-with-spatial-analyst.htm) , gelişmiş performans için bir GPU 'yu destekler, ancak GPU kullanımı gerektirmez.

| Laboratuvar ayarı | Değer ve açıklama |
| ------------ | ------------------ |
|Sanal makine boyutu| **Orta**.  İlişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir.|  

### <a name="template-machine"></a>Şablon makinesi

Bu bölümdeki adımlarda, şablon VM 'nin nasıl ayarlanacağı gösterilmektedir:

1.  Şablon sanal makinesini başlatın ve RDP kullanarak makineye bağlanın.

2.  ESRI tarafından sağlanan yönergeleri kullanarak ArcGIS Masaüstü bileşenlerini indirip yükleyin.  Bu adımlar, eşzamanlı kullanım lisanslama için lisans yöneticisinin atanmasını içerir: 
    - [ArcGIS Desktop 'ı yüklemeye ve yapılandırmaya giriş](https://desktop.arcgis.com/en/arcmap/latest/get-started/installation-guide/introduction.htm)

3.  Öğrenciler için dış yedekleme depolaması ayarlayın.  Öğrenciler, yaptıkları tüm değişiklikler oturumlarda kaydedildiğinden, dosyaları doğrudan atanmış VM 'lerine kaydedebilir.  Bununla birlikte, öğrencilerin çalışmalarını, birkaç nedenden dolayı VM 'lerden dış depolama birimine yedeklemesini öneririz:
    - Öğrenciler, sınıf ve laboratuvar bittikten sonra çalışmalarını erişmelerini sağlamak için.  
    - Öğrenciye ait VM 'leri hatalı duruma alırsa ve görüntünün [sıfırlanması](how-to-set-virtual-machine-passwords.md#reset-vms)gerekir.

    ArcGIS ile her öğrencinin her bir iş oturumunun sonunda aşağıdaki dosyaları yedeklemesi gerekir:

    - bir projenin düzen bilgilerini depolayan MXD dosyası.
    - ArcGIS tarafından üretilen tüm verileri depolayan dosya geodatabases.
    - Öğrenciye ait tarama dosyaları, şekil dosyaları, GeoTIFF vb. gibi başka veriler de kullanılıyor olabilir.

    OneDrive 'ı yedekleme depolaması için kullanmanızı öneririz.  Şablon VM 'de OneDrive 'ı ayarlamak için, [OneDrive 'ı yükleyin ve yapılandırın](how-to-prepare-windows-template.md#install-and-configure-onedrive)makalesindeki adımları izleyin. 

4.  Son olarak, Öğrenciler sanal makinesini oluşturmak için şablon VM 'yi [yayımlayın](how-to-create-manage-template.md#publish-the-template-vm) .

### <a name="auto-shutdown-and-disconnect-settings"></a>Otomatik olarak kapatmalar ve bağlantı kesme ayarları

Bir laboratuvarın [otomatik olarak kapanması ve bağlantılarının kesilmesi ayarları](cost-management-guide.md#automatic-shutdown-settings-for-cost-control) , BIR öğrencinin VM 'sinin kullanılmadığı sırada kapandığından emin olmanıza yardımcı olur.  Bu ayarlar, öğrencilerinizin gerçekleştireceği iş yükleri türlerine göre ayarlanmalıdır ve bu sayede VM 'leri işinin ortasında kapanmaz.  Örneğin, **sanal makineler boşta kaldığında kullanıcıların bağlantısını kes** ayarı, belirli bir süre boyunca fare veya klavye girişi algılanmadığında öğrencinin RDP oturumlarından bağlantısını keser.  Bu ayar, öğrenci 'nin uzun sorgular çalıştırmak veya işleme beklemek gibi, fare veya klavyeyi etkin bir şekilde kullanmayan iş yükleri için yeterli zamana izin vermelidir.

Argıs için, bu ayarlar için aşağıdaki değerleri öneririz:
- Sanal makineler boştayken kullanıcıların bağlantısını kes
    - Boşta durumu algılandıktan 30 dakika sonra
- Kullanıcıların bağlantısı kesildiğinde sanal makineleri kapat
    - kullanıcının bağlantısı kesildiğinde 15 dakika

## <a name="cost"></a>Maliyet

Bu sınıf için olası bir maliyet tahminini ele alalım. Bu tahmin, lisans sunucusunu çalıştırmanın maliyetini içermez. 25 öğrencilerden oluşan bir sınıf kullanacağız. 20 saatlik zamanlanan sınıf zamanı vardır. Ayrıca, her öğrenci, zamanlanan sınıf zamanı dışında ev ödevleri veya atamalar için 10 saatlik kota alır. Seçtiğiniz sanal makine boyutu, 42 laboratuvar birimi olan **Orta** idi.

25 öğrenci \* (20 zamanlanan saat + 10 kota saati) \* 42 laboratuvar birimi * 0,01 saat başına usd = 315,00 ABD Doları

>[!IMPORTANT]
> Maliyet tahmini yalnızca örnek amaçlıdır.  Fiyatlandırma hakkındaki güncel Ayrıntılar için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).  

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar, herhangi bir laboratuvarı ayarlamak için ortaktır.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)