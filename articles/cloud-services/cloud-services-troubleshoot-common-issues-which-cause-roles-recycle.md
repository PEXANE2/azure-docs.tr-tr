---
title: Bulut Hizmeti rollerinin geri dönüştürülmesinin yaygın nedenleri | Microsoft Dokümanlar
description: Aniden geri dönüşüm yapan bir bulut hizmeti rolü önemli kapalı kalma sürelerine neden olabilir. Rollerin geri dönüştürülmesine neden olan ve kapalı kalma süresini azaltmanıza yardımcı olabilecek bazı sık karşılaşılan sorunlar aşağıda verebilirsiniz.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: a644e211cc933ca686f0bd6a13b0d2ba8ae20162
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114105"
---
# <a name="common-issues-that-cause-roles-to-recycle"></a>Rollerin geri dönüştürülmesine neden olan yaygın sorunlar
Bu makalede, dağıtım sorunlarının bazı yaygın nedenleri tartışılır ve bu sorunları çözmenize yardımcı olacak sorun giderme ipuçları sağlar. Bir uygulamada bir sorunun var olduğunun bir göstergesi, rol örneğinin başlatılamaması veya başlatma, meşgul ve durdurma durumları arasında geçiş yaptığı dır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Eksik çalışma zamanı bağımlılıkları
Uygulamanızdaki bir rol .NET Framework veya Azure yönetilen kitaplığın parçası olmayan herhangi bir derlemeye dayanıyorsa, bu derlemeyi uygulama paketine açıkça eklemeniz gerekir. Diğer Microsoft çerçevelerinin varsayılan olarak Azure'da kullanılmadığını unutmayın. Rolünüz böyle bir çerçeveye dayanıyorsa, bu derlemeleri uygulama paketine eklemeniz gerekir.

Uygulamanızı oluşturmadan ve paketlemeden önce aşağıdakileri doğrulayın:

* Visual studio kullanıyorsanız, Projenizde Azure SDK veya .NET Framework'ün parçası olmayan her başvurulan derleme için **Yerel Kopyala** özelliğinin **True** olarak ayarlandığından emin olun.
* web.config dosyasının derleme öğesindeki kullanılmayan derlemelere başvurmadığından emin olun.
* Her .cshtml dosyasının **Yapı Eylemi** **İçerik**olarak ayarlanır. Bu, dosyaların pakette doğru görünmesini sağlar ve başvurulan diğer dosyaların pakette görünmesini sağlar.

## <a name="assembly-targets-wrong-platform"></a>Montaj hedefleri yanlış platform
Azure, 64 bit'lik bir ortamdır. Bu nedenle, 32 bit lik bir hedef için derlenen .NET derlemeleri Azure'da çalışmaz.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rol, başlatma veya durdurma sırasında işlenmemiş özel durumlar atar
[OnStart], [OnStop]ve [Çalıştır] yöntemlerini içeren [RoleEntryPoint] sınıfının yöntemleri tarafından atılan tüm özel durumlar işlenmemiştir. Bu yöntemlerden birinde işlenmemiş bir özel durum oluşursa, rol geri dönüştürür. Rol tekrar tekrar geri dönüşüm oluyorsa, her başlatmaya çalıştığında işlenmemiş bir özel durum atıyor olabilir.

## <a name="role-returns-from-run-method"></a>Çalıştır yönteminden rol döndürür
[Çalıştır] yöntemi süresiz olarak çalıştırmak için tasarlanmıştır. Kodunuz [Çalıştır] yöntemini geçersiz kılıyorsa, süresiz olarak uyumalıdır. [Çalıştır] yöntemi dönerse, rol geri dönüşüme olur.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Yanlış TanılamaBağlantıString ayarı
Uygulama Azure Diagnostics kullanıyorsa, hizmet `DiagnosticsConnectionString` yapılandırma dosyanızın yapılandırma ayarını belirtmesi gerekir. Bu ayar, Azure'daki depolama hesabınıza bir HTTPS bağlantısı belirtmelidir.

Uygulama paketinizi `DiagnosticsConnectionString` Azure'a dağıtmadan önce ayarınızın doğru olduğundan emin olmak için aşağıdakileri doğrulayın:  

* Ayar, `DiagnosticsConnectionString` Azure'daki geçerli bir depolama hesabını işaret ediyor.  
  Varsayılan olarak, bu ayar taklit edilmiş depolama hesabını işaret eder, bu nedenle uygulama paketinizi dağıtmadan önce bu ayarı açıkça değiştirmeniz gerekir. Bu ayarı değiştirmezseniz, rol örneği tanılama izlemeyi başlatmaya çalıştığında bir özel durum atılır. Bu, rol örneğinin süresiz olarak geri dönüştürülmesine neden olabilir.
* Bağlantı dizesi aşağıdaki [biçimde](../storage/common/storage-configure-connection-string.md)belirtilir. (Protokol HTTPS olarak belirtilmelidir.) *MyAccountName'yi* depolama hesabınızın adı ile değiştirin ve *MyAccountKey'i* erişim anahtarınızla değiştirin:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Microsoft Visual Studio için Azure Araçları'nı kullanarak uygulamanızı geliştiriyorsanız, bu değeri ayarlamak için özellik sayfalarını kullanabilirsiniz.

## <a name="exported-certificate-does-not-include-private-key"></a>Dışa aktarılan sertifikaözel anahtar içermez
TLS altında bir web rolü çalıştırmak için, dışa aktarılan yönetim sertifikanızın özel anahtarı içerdiğinden emin olmalısınız. Sertifikayı dışa aktarmak için *Windows Sertifika Yöneticisi'ni* kullanıyorsanız, özel anahtar seçeneğini **Dışa Aktar'a** **Evet'i** seçtiğinizden emin olun. Sertifika, şu anda desteklenen tek biçim olan PFX biçiminde dışa aktarılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Bulut hizmetleri için daha fazla [sorun giderme makalelerini](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) görüntüleyin.

[Kevin Williamson'ın blog serisinde](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)daha fazla rol geri dönüşüm senaryoları görüntüleyin.

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[Onstart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[Onstop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Çalıştırmak]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
