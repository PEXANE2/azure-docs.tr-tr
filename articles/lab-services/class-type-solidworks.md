---
title: Azure Lab Services Mühendislik için bir Solidçalışmalar Laboratuvarı ayarlama | Microsoft Docs
description: Solidçalışmalar kullanarak mühendislik kursları için laboratuvar ayarlamayı öğrenin.
services: lab-services
author: nicolela
ms.service: lab-services
ms.topic: article
ms.date: 06/03/2020
ms.author: nicolela
ms.openlocfilehash: d3f6a855f4b162f0b14ff01ed0a3560897383c49
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85341764"
---
# <a name="set-up-a-lab-for-engineering-classes-using-solidworks"></a>Solidçalışmalar kullanarak mühendislik sınıfları için laboratuvar ayarlama

[Solidçalışmalar](https://www.solidworks.com/) , düz nesnelerin modellenmesi Için bir 3B bilgisayar destekli TASARıM (CAD) ortamı sağlar ve çeşitli mühendislik alanlarında kullanılır.  Solidçalışmalarla mühendisler, tasarımlarını kolayca oluşturabilir, görselleştirin, benzetimini yapabilir ve belgeleyin.

Üniversiteler tarafından yaygın olarak kullanılan bir lisanslama seçeneği, "ağ lisanslaması" olur.   Bu seçenekle, kullanıcılar bir lisanslama sunucusu tarafından yönetilen bir lisans havuzunu paylaşır.  Bu tür bir lisans bazen "kayan" lisans olarak adlandırılır çünkü yalnızca eşzamanlı kullanıcı sayısı için yeterli lisansa sahip olmanız gerekir.  Bir Kullanıcı Kesintisdledı kullanılarak yapıldığında, lisansları merkezi olarak yönetilen lisans havuzuna geri dönerek başka bir kullanıcı tarafından yeniden kullanılabilir.

Bu makalede, Kesintisdçalışmaları 2019 ve ağ lisansı kullanan bir sınıfı nasıl ayarlayabileceğinizi göstereceğiz.

## <a name="license-server"></a>Lisans sunucusu

Solidçalışmalar ağ lisansı, lisans sunucunuzda SolidNetWork Lisans Yöneticisi 'nin yüklü ve etkinleştirilmiş olmasını gerektirir.  Bu lisans sunucusu, genellikle şirket içi ağınızda veya Azure 'daki özel bir ağda bulunur.  Sunucuda SolidNetWork Lisans yöneticisini ayarlama hakkında daha fazla bilgi için, bkz. Solidçalışmalar yükleme kılavuzunda [lisans yöneticisini yükleme ve etkinleştirme](https://help.solidworks.com/2019/English/Installation/install_guide/t_installing_snl_lic_mgr.htm) .  Bu ayarı yaparken, sonraki adımlarda gereksinim duyadıklarından bu yana kullanılan **bağlantı noktası numarasını** ve [**seri numarasını**](https://help.solidworks.com/2019/english/installation/install_guide/r_hid_state_serial_number.htm) unutmayın.

Lisans sunucunuz kurulduktan sonra, [sanal ağı (VNet)](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network) [Laboratuvar hesabınıza](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account)eşleyebilmeniz gerekecektir.  Laboratuvar sanal makinelerinin lisans sunucusuna erişebilmesi ve tam tersi şekilde laboratuvar oluşturmadan önce ağ eşlemesinin tamamlanması gerekir.

> [!NOTE]
> Laboratuvar sanal makineleri ve lisans sunucusu arasında iletişime izin vermek için güvenlik duvarınızda uygun bağlantı noktalarının açıldığını doğrulamanız gerekir.  Örneğin, lisans sunucusunun güvenlik duvarına gelen ve giden kuralların nasıl ekleneceğini gösteren [Windows Güvenlik Duvarı Için Lisans Yöneticisi bilgisayar bağlantı noktalarını değiştirme](http://help.solidworks.com/2019/english/installation/install_guide/t_mod_ports_on_lic_mgr_for_firewall.htm) yönergelerine bakın.  Ayrıca, laboratuvar sanal makinelerinin bağlantı noktalarını açmanız gerekebilir.  Laboratuvarın genel IP adresini alma da dahil olmak üzere, bu konuda daha fazla bilgi edinmek için [Labs için güvenlik duvarı ayarları](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-firewall-settings) makalesindeki adımları izleyin.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu Laboratuvarı ayarlamak için, başlamak üzere bir Azure aboneliğine ve laboratuvar hesabına sahip olmanız gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni bir laboratuar hesabı oluşturma hakkında daha fazla bilgi için [Laboratuvar hesabı ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account)hakkında öğreticiye bakın. Ayrıca var olan bir laboratuvar hesabı da kullanabilirsiniz.

### <a name="lab-account-settings"></a>Laboratuvar hesabı ayarları

Laboratuvar hesabı için aşağıdaki tabloda açıklanan ayarları etkinleştirin. Market görüntülerinin nasıl etkinleştirileceği hakkında daha fazla bilgi için, [Laboratuvar oluşturucuları için kullanılabilen Market görüntülerinin nasıl belirtilme](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)hakkındaki makaleye bakın.

| Laboratuvar hesabı ayarı | Yönergeler |
| ------------------- | ------------ |
|Market görüntüsü| Laboratuvar hesabınızda kullanılmak üzere Windows 10 Pro görüntüsünü etkinleştirin.|

> [!NOTE]
> Windows 10 ' a ek olarak, Solidçalışmalar Windows 'un diğer sürümlerini destekler.  Ayrıntılar için bkz. [Solidçalışmalar sistem gereksinimleri](https://www.solidworks.com/sw/support/SystemRequirements.html) .

### <a name="lab-settings"></a>Laboratuvar ayarları

Bir sınıf Laboratuvarı ayarlarken aşağıdaki tablodaki ayarları kullanın. Sınıf Laboratuvarı oluşturma hakkında daha fazla bilgi için bkz. sınıf Laboratuvarı ayarlama öğreticisi.

| Laboratuvar ayarları | Değer/yönergeler |
| ------------ | ------------------ |
|Sanal makine boyutu| **Küçük GPU (görselleştirme)**.  Bu VM, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir.|  
|Sanal makine görüntüsü| Windows 10 Pro|

> [!NOTE]
> **Küçük GPU (görselleştirme)** sanal makine boyutu, yüksek performanslı bir grafik deneyimini etkinleştirecek şekilde yapılandırılmıştır.  Bu sanal makine boyutu hakkında daha fazla bilgi için [GPU 'lar ile laboratuvar ayarlama konusundaki](./how-to-setup-lab-gpu.md)makaleye bakın.

> [!WARNING]
> Laboratuvar oluşturmadan **önce** laboratuvar hesabının sanal ağını, lisans sunucusu için sanal ağa [eşdüzey](https://www.mathworks.com/support/requirements/matlab-system-requirements.html) olarak kaçırmayın.

## <a name="template-virtual-machine-configuration"></a>Şablon sanal makine yapılandırması

Bu bölümdeki adımlarda, Solidçalışmalar yükleme dosyalarını indirerek ve istemci yazılımını yükleyerek şablon sanal makinenizin nasıl ayarlanacağı gösterilmektedir:

1. Şablon sanal makinesini başlatın ve RDP kullanarak makineye bağlanın.

1. Solidçalışmalar istemci yazılımı için yükleme dosyalarını indirin. İndirmek için iki seçeneğiniz vardır:
   - [Solidçalışmalar müşteri portalından](https://login.solidworks.com/nidp/idff/sso?id=cpenglish&sid=1&option=credential&sid=1&target=https%3A%2F%2Fcustomerportal.solidworks.com%2F)indirin.
   - Sunucudaki bir dizinden indirin.  Bu seçeneği kullandıysanız, sunucuya şablon sanal makineden erişilebildiğinden emin olmanız gerekir.  Örneğin, bu sunucu, laboratuvar hesabınızla eşlenen sanal ağda bulunabilir.
  
    Ayrıntılar için bkz. solidçalışmalarda [tek tek bilgisayarlara yükleme](http://help.solidworks.com/2019/english/Installation/install_guide/c_installing_on_individual_computers.htm?id=fc149e8a968a422a89e2a943265758d3#Pg0) , kesintisdçalışmalar Yükleme Kılavuzu.

1. Yükleme dosyaları indirildikten sonra, Kesintisdçalışmalar Yükleme Yöneticisi 'Ni kullanarak istemci yazılımını yükleme. Bkz. [bir lisans istemcisini](http://help.solidworks.com/2019/english/installation/install_guide/t_installing_snl_license_client.htm) , Solidçalışmalar yükleme kılavuzuna yükleme.

    > [!NOTE]
    > **Sunucu Ekle** iletişim kutusunda, lisans sunucunuz için kullanılan **bağlantı noktası numarası** ve LISANS sunucusunun adı veya IP adresi sorulur.

## <a name="cost"></a>Maliyet

Bu sınıf için olası bir maliyet tahminini ele alalım. Bu tahmine lisans sunucusunu çalıştırmanın maliyeti dahil değildir. 25 öğrencilerden oluşan bir sınıf kullanacağız. 20 saatlik zamanlanan sınıf zamanı vardır. Ayrıca, her öğrenci, zamanlanan sınıf zamanı dışında ev ödevleri veya atamalar için 10 saatlik kota alır. Seçtiğimiz sanal makine boyutu, 160 laboratuvar birimi olan **küçük GPU (görselleştirme)** idi.

25 öğrenci \* (20 zamanlanan saat + 10 kota saati) \* 160 laboratuvar birimi * 0,01 saat başına usd = 1200,00 ABD Doları

>[!IMPORTANT]
> Maliyet tahmini yalnızca örnek amaçlıdır.  Fiyatlandırma hakkındaki güncel Ayrıntılar için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).  

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar, herhangi bir laboratuvarı ayarlamak için ortaktır.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
