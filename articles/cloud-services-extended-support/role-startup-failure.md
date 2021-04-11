---
title: Azure Cloud Services için rol örneği başlatma hatası (genişletilmiş destek)
description: Azure Cloud Services için rol örneği başlatma hatası giderme (genişletilmiş destek)
ms.topic: article
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachadw
ms.date: 04/01/2021
ms.custom: ''
ms.openlocfilehash: ced7675350c0e0deadf77837cf0f13ba54fffab9
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2021
ms.locfileid: "106382364"
---
# <a name="troubleshoot-azure-cloud-service-extended-support-roles-that-fail-to-start"></a>Başlayamayabilir Azure bulut hizmeti (genişletilmiş destek) rollerinin sorunlarını giderme
İşte başlayamamasının başarısız olduğu Cloud Services (genişletilmiş destek) rolleriyle ilgili bazı yaygın sorunlar ve çözümleri aşağıda verilmiştir.

## <a name="cloud-service-operation-failed-with-roleinstancestartuptimeouterror"></a>Bulut hizmeti Işlemi Roleınstancestartuptimeouterror ile başarısız oldu
Hizmet sunucunuzun bir veya daha fazla rol örneği (genişletilmiş destek), belirlenen sürede başlatılamayabilir. Bu rol örneklerinin başlaması zaman alabilir veya geri dönüşüm olabilir ve rol örneği bir Roleınstancestartuptimeouterror ile başarısız olabilir bu bir rol uygulama hatasıdır. Rol uygulaması iki ana bölüm içerir: ' Başlangıç görevleri ' ve ' rol kodu (RoleEntryPoint uygulaması) ', her ikisi de rol geri dönüşüme neden olabilir. Rol kilitlenirse PaaS Aracısı her zaman yeniden başlatılır. 

Hata durumunda rol örneklerinin geçerli durumunu ve ayrıntılarını almak için şunu kullanın:

* PowerShell: bir bulut hizmetindeki rol örneğinin çalışma zamanı durumu hakkında bilgi almak için [Get-Azcloudserviceroleınstanceview](https://docs.microsoft.com/powershell/module/az.cloudservice/get-azcloudserviceroleinstanceview) cmdlet 'ini kullanın.
```powershell
Get-AzCloudServiceRoleInstanceView -ResourceGroupName "ContosOrg" -CloudServiceName "ContosoCS" -RoleInstanceName "WebRole1_IN_0"
 
Statuses           PlatformFaultDomain PlatformUpdateDomain
--------           ------------------- --------------------
{RoleStateStarting} 0                   0
```

* Azure portal: bulut hizmetinize gidin ve roller ve örnekler sekmesini seçin. Rol örneğine tıklayarak durum ayrıntılarını alın 
 
   :::image type="content" source="media/role-startup-failure-1.png" alt-text="Görüntüde portalda rol başlatma hatası gösterilir.":::
   
Başlatma, meşgul ve durdurma durumları arasında geçiş yapmak için Azure Cloud Services (genişletilmiş destek) rolleriyle ilgili bazı yaygın sorunlar ve çözümleri aşağıda bulabilirsiniz.

## <a name="missing-dlls-or-dependencies"></a>Eksik DLL 'Ler veya bağımlılıklar
Başlatma, meşgul ve durdurma durumları arasında geçiş yapan yanıt vermeyen roller ve roller, eksik DLL 'Ler veya derlemeler nedeniyle oluşabilir.
Eksik DLL 'Ler veya derlemelerin belirtileri şu olabilir:

* Rol örneğiniz **başlatma**, **meşgul** ve **durdurma** durumlarıyla geçiş yapıyor.
* Rol örneğiniz **başlamaya** taşındı, ancak Web uygulamanıza gittiğinizde sayfa görünmez.

Bu sorunları araştırmak için önerilen birkaç yöntem vardır.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Eksik DLL sorunlarını bir Web rolünde tanılayın
Bir Web rolünde dağıtılan bir Web sitesine gittiğinizde ve tarayıcı aşağıdakine benzer bir sunucu hatası görüntülerse, bir DLL 'nin eksik olduğunu gösterebilir.

:::image type="content" source="media/role-startup-failure-2.png" alt-text="Görüntü, rol başlatma hatasında çalışma zamanı hatasını gösterir":::

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Özel hataları kapatarak sorunları tanılayın
Web rolü web.config, hizmetin kapalı ve yeniden dağıtılması için özel hata modunu ayarlamak üzere yapılandırılarak daha fazla hata bilgisi görüntülenebilir.
Uzak Masaüstü kullanmadan daha fazla Tamam hata görüntülemek için:
1.  Çözümü Microsoft Visual Studio açın.
2.  Çözüm Gezgini, web.config dosyasını bulun ve açın.
3.  web.config dosyasında, System. Web bölümünü bulun ve aşağıdaki satırı ekleyin:
 ```xml
<customErrors mode="Off" />
```
4.  Dosyayı kaydedin.
5.  Hizmeti yeniden paketleyin ve dağıtın.
Hizmet yeniden dağıtıldıktan sonra, eksik derleme veya DLL adına sahip bir hata iletisi görürsünüz.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Hatayı uzaktan görüntüleyerek sorunları tanılayın
Role erişmek ve daha fazla hata bilgisini uzaktan görüntülemek için Uzak Masaüstü 'Nü kullanabilirsiniz. Uzak Masaüstü kullanarak hataları görüntülemek için aşağıdaki adımları kullanın:
1.  Bulut hizmeti için Uzak Masaüstü uzantısını etkinleştirin (genişletilmiş destek). Daha fazla bilgi için bkz. [Azure Portal kullanarak Uzak Masaüstü uzantısı Cloud Services (genişletilmiş destek) uygulama](enable-rdp.md)
2.  Azure portal, örnek, örneğine uzak bir durumu gösterir. Cloud Services ile uzak masaüstü 'nü kullanma hakkında daha fazla bilgi için bkz. [Uzak Masaüstü ile rol örneklerine bağlanma](https://docs.microsoft.com/azure/cloud-services-extended-support/enable-rdp#connect-to-role-instances-with-remote-desktop-enabled)
3.  Uzak Masaüstü yapılandırması sırasında belirtilen kimlik bilgilerini kullanarak sanal makinede oturum açın.
4.  Komut penceresi açın.
5.  IPconfig yazın.
6.  IPv4 adresi değerini aklınızda edin.
7.  Internet Explorer'ı açın.
8.  Web uygulamasının adresini ve adını yazın. Örneğin, http:// <IPV4 Address> /default. aspx.
Web sitesine gidildiğinde artık daha açık hata iletileri geri alınacaktır:
* '/' Uygulamasında sunucu hatası.
* Açıklama: geçerli Web isteğinin yürütülmesi sırasında işlenmeyen bir özel durum oluştu. Hata ve kodun kaynaklandığı yer hakkında daha fazla bilgi için lütfen yığın izlemesini gözden geçirin.
* Özel durum ayrıntıları: System. ıO. FIleNotFoundException: dosya veya derleme ' Microsoft. WindowsAzure. StorageClient, Version = 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35 ' veya bağımlılıklarından biri yüklenemedi. Sistem belirtilen dosyayı bulamıyor.
Örnek:

  :::image type="content" source="media/role-startup-failure-3.png" alt-text="Görüntü, rol başlatma hatasında özel durumu gösterir":::
  
## <a name="diagnose-issues-by-using-the-compute-emulator"></a>İşlem öykünücüsünü kullanarak sorunları tanılama
Eksik bağımlılıkların ve web.config hatalarının sorunlarını tanılamak ve gidermek için Azure Işlem öykünücüsünü kullanabilirsiniz.
Bu tanılama yöntemini kullanmanın en iyi sonucu için, temiz bir Windows yüklemesi olan bir bilgisayar veya sanal makine kullanmanız gerekir. 
1.  [Azure SDK 'sını](https://azure.microsoft.com/downloads/) yükler 
2.  Geliştirme makinesinde, bulut hizmeti projesini derleyin.
3.  Windows Gezgini 'nde, bulut hizmeti projesinin bin\debug klasörüne gidin.
4.  . CSX klasörünü ve. cscfg dosyasını, sorunları ayıklamak için kullandığınız bilgisayara kopyalayın.
5.  Temiz makinede, bir Azure SDK komut Istemi penceresi açın ve csrun.exe/devstore: start yazın.
6.  Komut isteminde,. cscfg dosyasının yolunu> <. CSX klasörüne <CSV Çalıştır ' ı>/Launchbrowseryazın.
7.  Rol başlatıldığında, Internet Explorer 'da ayrıntılı hata bilgileri görürsünüz. Ayrıca, sorunu tanılamak için standart Windows sorun giderme araçları 'nı kullanabilirsiniz.

## <a name="diagnose-issues-by-using-intellitrace"></a>IntelliTrace kullanarak sorunları tanılama
.NET Framework 4 kullanan çalışan ve Web rolleri için, Microsoft Visual Studio Enterprise kullanılabilen [IntelliTrace](https://docs.microsoft.com/visualstudio/debugger/intellitrace)kullanabilirsiniz.
Hizmeti IntelliTrace etkin olarak dağıtmak için şu adımları izleyin:
1.  Azure SDK 1,3 veya sonraki bir sürümünün yüklü olduğunu doğrulayın.
2.  Visual Studio 'Yu kullanarak çözümü dağıtın. Dağıtım sırasında, .NET 4 rolleri için IntelliTrace 'i etkinleştir onay kutusunu işaretleyin.
3.  Örnek başladıktan sonra Sunucu Gezgini açın.
4.  Azure\Cloud Services düğümünü genişletin ve dağıtımı bulun.
5.  Rol örneklerini görene kadar dağıtımı genişletin. Örneklerden birine sağ tıklayın.
6.  IntelliTrace günlüklerini görüntüle ' yi seçin. IntelliTrace Özeti açılır.
7.  Özetin özel durumlar bölümünü bulun. Özel durumlar varsa, Bölüm özel durum verileri olarak etiketlenir.
8.  Özel durum verilerini genişletin ve aşağıdakilere benzer şekilde System. ıO. FileNotFoundException hatalarını arayın:

    :::image type="content" source="media/role-startup-failure-4.png" alt-text="Görüntü, rol başlatma hatasında özel durum verilerini gösterir" lightbox="media/role-startup-failure-4.png":::

## <a name="address-missing-dlls-and-assemblies"></a>Eksik DLL 'Ler ve derlemeler için adres
Eksik DLL ve derleme hatalarını gidermek için aşağıdaki adımları izleyin:
1.  Visual Studio 'da çözümü açın.
2.  Çözüm Gezgini, başvurular klasörünü açın.
3.  Hatada tanımlanan derlemeye tıklayın.
4.  Özellikler bölmesinde, yerel özelliği Kopyala ' yı bulun ve değeri true olarak ayarlayın.
5.  Bulut hizmetini yeniden dağıtın.
Tüm hataların düzeltilmediğini doğruladıktan sonra, .NET 4 rolleri için IntelliTrace 'i etkinleştir onay kutusunu işaretleyerek Hizmeti dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar 
- Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme hakkında bilgi edinmek için bkz. [Kevin Williamson 'ın blog serisi](https://docs.microsoft.com/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data).
