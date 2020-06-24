---
title: Azure Lab Services ile MATLAB öğretmek için laboratuvar ayarlama | Microsoft Docs
description: Azure Lab Services ile MATLAB öğretmek için laboratuvar ayarlamayı öğrenin.
services: lab-services
author: emaher
ms.service: lab-services
ms.topic: article
ms.date: 05/27/2020
ms.author: enewman
ms.openlocfilehash: 81ee3e50d9e13fec45ca86c56aed7547315ac2a1
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84899028"
---
# <a name="setup-a-lab-to-teach-matlab"></a>MATLAB öğretmek için laboratuvar kurma

Matris Laboratuvarı temsil eden [MATLAB](https://www.mathworks.com/products/matlab.html), [MathWorks](https://www.mathworks.com/)'den programlama platformudur.  İşlem gücünü ve görselleştirmeyi, Math, mühendislik, fizik ve Chemistry alanlarında popüler bir araç haline getirir.

[Kampüs genelinde bir lisans](https://www.mathworks.com/academia/tah-support-program/administrators.html)kullanıyorsanız, şablon makinesinde MATLAB yükleyici dosyalarını ındırmek için [MATLAB yükleme dosyalarını indirme](https://www.mathworks.com/matlabcentral/answers/259632-how-can-i-get-matlab-installation-files-for-use-on-an-offline-machine) adresindeki yönergeler bölümüne bakın.  

Bu makalede, bir lisans sunucusu ile MATLAB istemci yazılımını kullanan bir sınıfın nasıl ayarlanacağı gösterilmektedir.

## <a name="license-server"></a>Lisans sunucusu

Laboratuvarınızın şablon makinesini değiştirmeden önce, sunucuyu [ağ Lisans Yöneticisi](https://www.mathworks.com/help/install/administer-network-licenses.html) yazılımını çalıştıracak şekilde ayarlamanız gerekir.  Bu yönergeler yalnızca, kullanıcıların bir lisans anahtarları havuzunu paylaşmasına izin veren MATLAB için ağ lisanslama seçeneğini belirleyen kurumlar için geçerlidir.  Ayrıca, daha sonra için lisans dosyasını ve dosya yükleme anahtarını kaydetmeniz gerekir.  Bir lisans dosyasının nasıl indirileceği hakkında ayrıntılı yönergeler için, [Internet bağlantısı Ile ağ Lisansı Yöneticisi 'ni yükleme](https://www.mathworks.com/help/install/ug/install-network-license-manager-with-internet-connection.html) makalesindeki ilk adıma bakın.

Bir lisans sunucusunun nasıl yükleneceğini kapsayan ayrıntılı yönergeler, [ağ Lisansı Yöneticisi 'Ni Internet bağlantısı ile yükler](https://www.mathworks.com/help/install/ug/install-network-license-manager-with-internet-connection.html)' de bulunabilir.  Ödünç almak için bkz. ödünç sağlama [Lisans](https://www.mathworks.com/help/install/license/borrow-licenses.html) makalesi.

Lisans sunucusunun şirket içi bir ağda veya Azure 'daki özel bir ağda bulunduğu varsayılırsa, [sanal ağın](how-to-connect-peer-virtual-network.md) [Laboratuvar hesabınıza](tutorial-setup-lab-account.md)yönelik olarak eşdüzey olduğunu unutmayın.  Laboratuvar sanal makinelerinin lisans sunucusuna erişebilmesi için laboratuvar oluşturmadan önce ağ eşlemesinin tamamlanması gerekir.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu Laboratuvarı kurmak için başlamak üzere bir Azure aboneliğine sahip olmanız gerekir.  Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilir veya mevcut bir hesabı kullanabilirsiniz.  Yeni bir laboratuar hesabı oluşturmak için bkz. [Laboratuvar hesabı ayarlama öğreticisi](tutorial-setup-lab-account.md).

Yeni bir laboratuvar oluşturmak için [sınıf Laboratuvarı Oluşturma öğreticisini](tutorial-setup-classroom-lab.md)izleyin.  Aşağıdaki ayarları uygulayın:

| Sanal makine boyutu | Görüntü |
| -------------------- | ----- |
| Orta | Windows 10 |

MATLAB, daha fazla işletim sisteminde desteklenir.  Ayrıntılar için bkz. [MATLAB sistem gereksinimleri](https://www.mathworks.com/support/requirements/matlab-system-requirements.html) .

> [!WARNING]
> Laboratuvar oluşturmadan önce laboratuvar hesabının sanal ağını, lisans sunucusu için sanal ağa [eşdüzey](https://www.mathworks.com/support/requirements/matlab-system-requirements.html) olarak kaçırmayın.

## <a name="template-machine"></a>Şablon makinesi

Şablon makinesi oluşturulduktan sonra, aşağıdaki önemli görevleri gerçekleştirmek için makineyi başlatın ve bu sunucuya bağlanın.

1. MATLAB istemci yazılımının yükleme dosyalarını indirin.
2. Dosya yükleme anahtarını kullanarak MATLAB 'yi yükleme.

MATLAB yüklemek çok parçalı bir işlem olacaktır.  İlk bölüm, MATLAB dosyalarını ve yüklenmesini istediğiniz diğer ürünleri indirir.  Bir dosya yükleme anahtarı kullanmak, ürünlerin yüklenmesi için tüm yükleme dosyalarının önceden indirilmesini gerektirir.  İkinci bölüm, şablon VM 'ye MATLAB yazılımını yüklemek ve yazılımı etkinleştirmek olacaktır.  Şablon VM, lisans sunucusu kullanılarak etkinleştirilecek şekilde yapılandırıldıysa, öğrenci VM 'Leri aynı olur.

### <a name="download-installation-files"></a>Yükleme dosyalarını indir

Yükleme dosyalarını indirmek ve lisans dosyasını ve dosya yükleme anahtarını almak için bir lisans yöneticisi olmanız gerekir.  Yükleme dosyalarının indirileceği adımlar aşağıda verilmiştir.

1. Hesabınızla oturum açın [https://www.mathworks.com](https://www.mathworks.com) .
2. **Hesabım**' ı seçin.
3. Hesap sayfasının **yazılımım** bölümünde, laboratuvar Için ağ Lisans Yöneticisi kurulumuna bağlı olan lisansa tıklayın.
4. Lisans ayrıntısı sayfasında, **ürünleri indir**' e tıklayın.
5. Yükleyicinin kendiliğinden ayıklanmasına bekleyin.
6. Yükleyiciyi başlatın.  
7. **MathWorks hesabınızda oturum açın** sayfasında, MathWorks hesabınızı girin.
8. **MathWorks lisans sözleşmesi** sayfasında, terimi kabul edin ve **İleri** düğmesine tıklayın.
9. **Gelişmiş Seçenekler** açılan düğmesine tıklayın ve **yükleme yapmadan indirmek istiyorum**' u seçin.
10. **Hedef Seç klasörü**üzerinde **İleri**' ye tıklayın.
11. MATLAB 'yi yüklemekte olduğunuz bilgisayarın platformu olarak **Windows** ' u seçin.
12. **Ürün Seç** SAYFASıNDA, MATLAB 'in yüklemek istediğiniz diğer tüm MathWorks ürünleriyle birlikte seçildiğinden emin olun.
13. **Seçimleri ve Indirmeyi Onayla** sayfasında, **indirmeyi Başlat**' ı tıklatın.  
14. Seçilen ürünlerin indirilmesini bekleyin.  **Son**'a tıklayın.

ISO görüntüsünü MathWorks Web sitesinden de indirebilirsiniz.

1. Hesabınızla oturum açın [https://www.mathworks.com](https://www.mathworks.com) .
2. Adresine gidin [https://www.mathworks.com/downloads](https://www.mathworks.com/downloads) .
3. Yüklemek istediğiniz MATLAB sürümünü seçin.
4. {Version}, R2020a gibi bir şeydir Ilgili bağlantıların altındaki "{Version}. iso görüntüsünü al" bağlantısına tıklayın.
5. Windows için mavi **karşıdan yükleme sürümü** bağlantısına tıklayın.

### <a name="run-installer"></a>Yükleyiciyi Çalıştır

Dosyalar indirildikten sonra ikinci adım yükleyiciyi çalıştırmak için kullanılır. Bu adımı gerçekleştirmek için bir kez daha lisans yöneticisi olmanız gerekir.  Yalnızca lisans yöneticileri, bir dosya yükleme anahtarıyla MATLAB yükleyebilir.

1. İndirilen lisans dosyasını denetleyin ve sunucu satırının lisans sunucusunu doğru şekilde listelediğinden emin olun.  Lisans dosyasının nasıl biçimlendirilmesi gerektiği hakkında bilgi için bkz. [güncelleştirme ağ lisansı](https://www.mathworks.com/help/install/ug/network-license-files.html), [Lisans ödünç](https://www.mathworks.com/help/install/license/borrow-licenses.html)alma ve [ana bilgisayar kimliği makaleleri bulma](https://www.mathworks.com/matlabcentral/answers/101892-what-is-a-host-id-how-do-i-find-my-host-id-in-order-to-activate-my-license) .
2. MATLAB yükleyicisini başlatın.
3. **MathWorks hesabınızda oturum açın** sayfasında, MathWorks hesabınızı girin.
4. **MathWorks lisans sözleşmesi** sayfasında, terimi kabul edin ve **İleri** düğmesine tıklayın.
5. **Gelişmiş Seçenekler** açılan düğmesine tıklayın ve **bir dosya yükleme anahtarım var**' ı seçin.
6. **Dosya yükleme anahtarını kullanarak yükleme** sayfasında, lisans sunucusu için dosya yükleme anahtarını girin.   **İleri**’ye tıklayın.
7. **Lisans dosyası seç** sayfasında, daha önce yükleme dosyalarını indirirken kaydedilen lisans dosyasına gidin.
8. **Hedef klasörü seç** sayfasında, **İleri**' ye tıklayın.
9. **Ürünleri Seç** sayfasında **İleri**' ye tıklayın.
10. **Seçenekleri seçin** sayfasında **İleri**' ye tıklayın.
11. **Seçimleri ve yüklemeyi Onayla** sayfasında, **yüklemeye başla**' yı tıklatın.
12. **Yükleme Tamam** SAYFASıNDA, **MATLAB 'yi etkinleştir** ' in işaretli olduğundan emin olun.  **Son**'a tıklayın.

## <a name="cost-estimate"></a>Maliyet tahmini

Bu sınıf için olası bir maliyet tahminini ele alalım.  Bu tahmine lisans sunucusunu çalıştırmanın maliyeti dahil değildir.  25 öğrencilerden oluşan bir sınıf kullanacağız.  20 saatlik zamanlanan sınıf zamanı vardır.  Ayrıca, her öğrenci, zamanlanan sınıf zamanı dışında ev ödevleri veya atamalar için 10 saatlik kota alır.  Seçtiğiniz sanal makine boyutu, 55 laboratuvar birimi olan orta idi.

Bu sınıf için olası bir maliyet tahmini örneği aşağıda verilmiştir:

25 öğrenci \* (20 zamanlanan saat + 10 kota saati) \* 55 laboratuvar birimi \* 0,01 saat başına abd doları = 412,50 ABD Doları

>[!IMPORTANT]
> Maliyet tahmini yalnızca örnek amaçlıdır.  Fiyatlandırma hakkındaki güncel Ayrıntılar için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).  

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar, herhangi bir laboratuvarı ayarlamak için ortaktır.

- [Şablon oluşturma, yönetme ve yayımlama](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
