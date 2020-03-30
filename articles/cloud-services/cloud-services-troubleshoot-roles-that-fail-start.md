---
title: Başlatılamayan sorun giderme rolleri | Microsoft Dokümanlar
description: Bulut Hizmeti rolünün başlatılamamasının bazı yaygın nedenleri aşağıda veda edinilebilir. Bu sorunlara çözümler de sağlanmaktadır.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 869453d92f536a62aacc2be52598223158566ae0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71122739"
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Başlatılamayan Bulut Hizmeti rolleri sorun giderme
Azure Bulut Hizmetleri rolleri ile ilgili başlatılamayan bazı sık karşılaşılan sorunlar ve çözümler aşağıda verilmiştir.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Eksik DL'ler veya bağımlılıklar
**Başlatma,** **Meşgul**ve **Durdurma** durumları arasında döngü olan yanıt vermeyen roller ve roller eksik DL'lerden veya derlemelerden kaynaklanabilir.

Eksik DLs veya derlemeler belirtileri olabilir:

* Rol örneğiniz, **Başlatma,** **Meşgul**ve **Durdurma** durumları üzerinden bisiklete binmektir.
* Rol örneğiniz **Hazır'a** taşındı, ancak web uygulamanıza gidin, sayfa görünmüyor.

Bu sorunları araştırmak için önerilen birkaç yöntem vardır.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Bir web rolünde eksik DLL sorunlarını tanılama
Bir web rolünde dağıtılan bir web sitesine gidince ve tarayıcı aşağıdakine benzer bir sunucu hatası görüntülediğinde, bir DLL'nin eksik olduğunu gösterebilir.

!['/' Uygulamasında Sunucu Hatası.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Özel hataları kapatarak sorunları tanılama
Özel hata modunu Kapalı olarak ayarlamak ve hizmeti yeniden dağıtmak için web rolü için web.config'i yapılandırarak daha eksiksiz hata bilgileri görüntülenebilir.

Uzak Masaüstü'nü kullanmadan daha eksiksiz hataları görüntülemek için:

1. Çözümü Microsoft Visual Studio'da açın.
2. Çözüm **Gezgini'nde,** web.config dosyasını bulun ve açın.
3. web.config dosyasında, system.web bölümünü bulun ve aşağıdaki satırı ekleyin:

    ```xml
    <customErrors mode="Off" />
    ```
4. Dosyayı kaydedin.
5. Hizmeti yeniden paketleyip yeniden dağıtın.

Hizmet yeniden dağıtıldıktan sonra, eksik derleme veya DLL adında bir hata iletisi görürsünüz.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Hatayı uzaktan görüntüleyerek sorunları tanılama
Role erişmek ve daha eksiksiz hata bilgilerini uzaktan görüntülemek için Uzak Masaüstü'nü kullanabilirsiniz. Uzak Masaüstü'nü kullanarak hataları görüntülemek için aşağıdaki adımları kullanın:

1. Azure SDK 1.3 veya sonraki lerinin yüklü olduğundan emin olun.
2. Visual Studio kullanarak çözümün dağıtımı sırasında Uzak Masaüstü'nü etkinleştirin. Daha fazla bilgi için Visual [Studio'u kullanarak Azure Bulut Hizmetlerinde Rol Için Uzak Masaüstü Bağlantısını Etkinleştir'e](cloud-services-role-enable-remote-desktop-visual-studio.md)bakın.
3. Microsoft Azure portalında, örnek **hazır**bir durum gösterdiğinde, örneğin içine uzak. Bulut Hizmetleri ile uzak masaüstü kullanma hakkında daha fazla bilgi için, [rol örnekleri içine Remote](cloud-services-role-enable-remote-desktop-new-portal.md#remote-into-role-instances)bakın.
5. Uzak Masaüstü yapılandırması sırasında belirtilen kimlik bilgilerini kullanarak sanal makinede oturum açın.
6. Komut penceresi açın.
7. `IPconfig` yazın.
8. IPV4 Adres değerini not edin.
9. Internet Explorer'ı açın.
10. Web uygulamasının adresini ve adını yazın. Örneğin, `http://<IPV4 Address>/default.aspx`.

Web sitesinde gezinmek artık daha açık hata iletilerini döndürecek:

* '/' Uygulamasında Sunucu Hatası.
* Açıklama: Geçerli web isteğinin yürütülmesi sırasında işlenmemiş bir özel durum oluştu. Hata ve kodda nereden kaynaklandığı hakkında daha fazla bilgi için lütfen yığın izini gözden geçirin.
* Özel Durum Ayrıntıları: System.IO.FIleNotFoundException: Dosya veya derleme 'Microsoft.WindowsAzure.StorageClient, Sürüm=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35' veya bağımlılıklarından birini yükleyemedi. Sistem belirtilen dosyayı bulamıyor.

Örnek:

!['/' Uygulamasında Açık Sunucu Hatası](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Hesaplama emülatörü kullanarak sorunları tanılama
Eksik bağımlılıklar ve web.config hataları sorunlarını tanılamak ve sorun gidermek için Microsoft Azure bilgi işlem emülatörü'ni kullanabilirsiniz.

Bu tanı yöntemini kullanarak en iyi sonuçları elde etmek için, temiz bir Windows yüklemesi olan bir bilgisayar veya sanal makine kullanmanız gerekir. Azure ortamını en iyi şekilde simüle etmek için Windows Server 2008 R2 x64'u kullanın.

1. [Azure SDK'nın](https://azure.microsoft.com/downloads/)bağımsız sürümünü yükleyin.
2. Geliştirme makinesinde bulut hizmeti projesini oluşturun.
3. Windows Gezgini'nde, bulut hizmeti projesinin bin\debug klasörüne gidin.
4. .csx klasörünü ve .cscfg dosyasını, sorunları ayıklamak için kullandığınız bilgisayara kopyalayın.
5. Temiz makinede, bir Azure SDK Komut `csrun.exe /devstore:start`İstemi penceresi açın ve yazın.
6. Komut isteminde, `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`yazın.
7. Rol başladığında, Internet Explorer'da ayrıntılı hata bilgilerini görürsünüz. Sorunu daha fazla tanılamak için standart Windows sorun giderme araçlarını da kullanabilirsiniz.

## <a name="diagnose-issues-by-using-intellitrace"></a>IntelliTrace kullanarak sorunları tanıla
.NET Framework 4'ü kullanan çalışan ve web rolleri için Microsoft Visual Studio Enterprise'da bulunan [IntelliTrace'i](/visualstudio/debugger/intellitrace)kullanabilirsiniz.

Hizmeti IntelliTrace etkinleştirilmiş olarak dağıtmak için aşağıdaki adımları izleyin:

1. Azure SDK 1.3 veya sonraki lerinin yüklü olduğunu doğrulayın.
2. Visual Studio'yu kullanarak çözümü dağıtın. Dağıtım sırasında ,NET 4 rolleri onay kutusunun **IntelliTrace'ini etkinleştir'i** işaretleyin.
3. Örnek başladıktan sonra **Sunucu Gezgini'ni**açın.
4. **Azure\\Bulut Hizmetleri** düğümini genişletin ve dağıtımı bulun.
5. Rol örneklerini görene kadar dağıtımı genişletin. Örneklerden birine sağ tıklayın.
6. **IntelliTrace günlüklerini görüntüle'yi**seçin. **IntelliTrace Özeti** açılacaktır.
7. Özetin özel durumlar bölümünü bulun. Özel durumlar varsa, bölüm **Özel Durum Verileri**olarak etiketlenir.
8. Özel **Durum Verilerini** Genişletin ve **System.IO.FileNotFoundException** hatalarını aşağıdakilere benzer şekilde arayın:

![Özel durum verileri, eksik dosya veya derleme](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adres eksik DL'ler ve derlemeler
Eksik DLL ve derleme hatalarını gidermek için aşağıdaki adımları izleyin:

1. Çözümü Visual Studio'da açın.
2. **Çözüm Gezgini'nde,** **Başvurular** klasörünü açın.
3. Hatada tanımlanan derlemeyi tıklatın.
4. **Özellikler** bölmesinde, **Yerel Kopyala özelliğini** bulun ve değeri **True**olarak ayarlayın.
5. Bulut hizmetini yeniden dağıtın.

Tüm hataların düzeltildiğini doğruladıktan sonra, .NET 4 rolleri onay kutusunu n için **IntelliTrace'i etkinleştir'i** denetlemeden hizmeti dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bulut hizmetleri için daha fazla [sorun giderme makalelerini](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) görüntüleyin.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rol sorunlarını nasıl gidereceklerini öğrenmek için [Kevin Williamson'ın blog serisine](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)bakın.
