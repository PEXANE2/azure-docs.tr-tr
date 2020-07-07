---
title: DOD etkisi düzey 5 şema örneği
description: '\ Print yapıt parametresi ayrıntıları dahil DOD etkisi düzeyi 5 şema örneği için adımları dağıtın.'
ms.date: 06/30/2020
ms.topic: sample
ms.openlocfilehash: 9267ed0de35107b8f6582d852ab925b4dbbbdba4
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802685"
---
# <a name="deploy-the-dod-impact-level-5-blueprint-sample"></a>DOD etkisi düzeyi 5 şema örneğini dağıtma

Savunma etki düzeyi 5 (DOD IL5) şema örnek Azure şemaları departmanı dağıtmak için aşağıdaki adımlar gerçekleştirilmelidir:

> [!div class="checklist"]
> - Örnekten yeni bir şema oluştur
> - Örnek kopyanızı **yayımlandı** olarak işaretleyin
> - Şema kopyanızı mevcut bir aboneliğe atama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, bir başlangıç olarak örneği kullanarak ortamınızda yeni bir şema oluşturarak şema örneğini uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Soldaki **Başlarken** sayfasında, şema _Oluştur_altında **Oluştur** düğmesini seçin.

1. _Diğer örnekler_ altında **DOD etkisi düzeyi 5** şema örneğini bulun ve **Bu örneği kullan**' ı seçin.

1. Şema örneği _hakkında temel bilgileri_ girin:

   - **Şema adı**: DOD etkisi düzeyi 5 şema örneği kopyanız için bir ad sağlayın.
   - **Tanım konumu**: üç noktayı kullanın ve örneğin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _yapıtlar_ sekmesini veya sonraki: sayfanın en altındaki **yapıtları** seçin.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunda, daha sonra tanımlayacağımız parametreler vardır. Şema örneğini gözden geçirmeyi bitirdiğinizde **Taslağı kaydet** ' i seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı Yayımla

Şema örneğinin kopyası artık ortamınızda oluşturulmuştur. **Taslak** modunda oluşturulur ve atanmadan ve dağıtılmadan önce **yayımlanmaları** gerekir. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik, DOD etkisi düzey 5 denetimleriyle hizalamadan uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Sayfanın üst kısmındaki şemayı **Yayımla** ' yı seçin. Sağ taraftaki yeni sayfada, şema örneğinin kopyası için bir **Sürüm** belirtin. Daha sonra bir değişiklik yaparsanız, bu özellik için faydalıdır. "DOD IL5 şema örneğinden yayınlanan ilk sürüm" gibi **değişiklik notları** sağlayın. Ardından sayfanın alt kısmında **Yayımla** ' yı seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adım, her bir şema örneğinin kopyasının her dağıtımını yapmak için parametrelerin sağlandığı yerdir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Şema tanım sayfasının en üstünde şema **ata** ' yı seçin.

1. Şema atamasının parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: şema örneğinin kopyasını kaydettiğiniz yönetim grubundaki bir veya daha fazla abonelik seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: ad, BLUEPRINT adına göre önceden doldurulur.
       Gerektiğinde değiştirin veya olduğu gibi bırakın.
     - **Konum**: yönetilen kimliğin oluşturulacağı bölgeyi seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - Şema **tanımı sürümü**: şema örneğinin kopyasının **yayınlanmış** bir sürümünü seçin.

   - Kilit ataması

     Ortamınız için BLUEPRINT Lock ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistem tarafından _atanmış_ yönetilen kimlik seçeneğini bırakın.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametreler, tanımlanan yapıt için geçerlidir. Bu parametreler, Blueprint atama sırasında tanımlandıklarından [dinamik parametrelerdir](../../concepts/parameters.md#dynamic-parameters) . Tam liste veya yapıt parametreleri ve açıklamaları için bkz. [yapıt parametreleri tablosu](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra sayfanın alt kısmındaki **ata** ' yı seçin. Şema ataması oluşturulur ve yapıt dağıtımı başlar. Dağıtım kabaca bir saat sürer. Dağıtımın durumunu denetlemek için, BLUEPRINT atamasını açın.

> [!WARNING]
> Azure şemaları hizmeti ve yerleşik şema örnekleri **ücretsiz olarak ücretsizdir**. Azure kaynakları [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Bu şema örneği tarafından dağıtılan çalışan kaynakların maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri tablosu

Aşağıdaki tabloda, şema yapıt parametrelerinin bir listesi verilmiştir:

|Yapıt adı|Yapıt türü|Parametre adı|Description|
|-|-|-|-|
|DoD etkisi düzeyi 5|İlke Ataması|Windows VM yöneticileri grubuna dahil olması gereken kullanıcıların listesi|Administrators yerel grubuna dahil olması gereken kullanıcıların noktalı virgülle ayrılmış listesi; Ex: yönetici; myUser1; myUser2|
|DoD etkisi düzeyi 5|İlke Ataması|Windows VM Yöneticileri grubundan dışlanan kullanıcıların listesi|Yöneticiler yerel grubunda dışlanması gereken kullanıcıların noktalı virgülle ayrılmış listesi; Ex: yönetici; myUser1; myUser2|
|DoD etkisi düzeyi 5|İlke Ataması|Tanılama günlükleri etkinleştirilmiş olması gereken kaynak türlerinin listesi||
|DoD etkisi düzeyi 5|İlke Ataması|VM Aracısı raporlama için Log Analytics çalışma alanı KIMLIĞI|VM aracılarının rapor etmesi gereken Log Analytics çalışma alanının KIMLIĞI (GUID)|
|DoD etkisi düzeyi 5|İlke Ataması|Ağ Izleyicisi 'nin etkinleştirilmesi gereken bölgelerin listesi|Bölgelerin tam listesini almak için Get-AzLocation kullanın,|
|DoD etkisi düzeyi 5|İlke Ataması|Windows Web sunucuları için en düşük TLS sürümü|Windows Web sunucularında etkinleştirilmesi gereken en düşük TLS protokolü sürümü|
|DoD etkisi düzeyi 5|İlke Ataması|En son PHP sürümü|Uygulama Hizmetleri için desteklenen en son PHP sürümü|
|DoD etkisi düzeyi 5|İlke Ataması|En son Java sürümü|Uygulama Hizmetleri için desteklenen en son Java sürümü|
|DoD etkisi düzeyi 5|İlke Ataması|En son Windows Python sürümü|Uygulama Hizmetleri için desteklenen en son Python sürümü|
|DoD etkisi düzeyi 5|İlke Ataması|En son Linux Python sürümü|Uygulama Hizmetleri için desteklenen en son Python sürümü|
|DoD etkisi düzeyi 5|İlke Ataması|İsteğe bağlı: denetim kapsamına eklemek için Log Analytics aracısını destekleyen Windows sanal makine görüntülerinin listesi|Noktalı virgülle ayrılmış görüntülerin listesi; Ex:/Subscriptions/ <subscriptionId> /resourceGroups/YourResourceGroup/Providers/Microsoft.COMPUTE/images/ContosoStdImage|
|DoD etkisi düzeyi 5|İlke Ataması|İsteğe bağlı: denetim kapsamına eklemek için Log Analytics aracısını destekleyen Linux VM görüntülerinin listesi|Noktalı virgülle ayrılmış görüntülerin listesi; Ex:/Subscriptions/ <subscriptionId> /resourceGroups/YourResourceGroup/Providers/Microsoft.COMPUTE/images/ContosoStdImage|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: aboneliğinize birden fazla sahip atanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: disk şifrelemesi sanal makinelere uygulanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: yüksek önem derecesi uyarıları için abonelik sahibine e-posta bildirimi etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Işlev uygulamaları için uzaktan hata ayıklama kapatılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: İşlev Uygulaması bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: API uygulamasının bir parçası olarak kullanılıyorsa ' PHP Version ' öğesinin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: yalnızca Redis Cache güvenli bağlantı etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Endpoint Protection çözümünün sanal makine ölçek kümelerine yüklenmesi gerekir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL yönetilen örneği için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: depolama hesaplarına Güvenli aktarım etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke etkisi: sanal makinelerde Uyarlamalı uygulama denetimleri etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke etkisi: PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedekleme etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Web uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü ' nin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: aboneliğiniz için en fazla 3 sahip belirtilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: aboneliğiniz için bir güvenlik iletişim e-posta adresi belirtilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: CORS her kaynağın Web uygulamalarınıza erişmesine izin vermemelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: yazma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Web uygulamasının bir parçası olarak kullanılıyorsa, ' Python sürümü ' öğesinin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Işlev uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü ' öğesinin en son olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: WEB uygulamasının bir parçası olarak kullanılıyorsa, ' PHP Version ' öğesinin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: API uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü ' nin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke etkisi: MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Web uygulamasının bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: API uygulamasının bir parçası olarak kullanılıyorsa ' Java Version ' öğesinin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Web uygulamasını çalıştırmak için kullanılmışsa ' HTTP Version ' nin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: API uygulamanızda en son TLS sürümü kullanılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: MFA 'nın aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL Server Gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: API uygulamasını çalıştırmak için kullanılmışsa ' HTTP Version ' nin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Microsoft ıaasantimalware uzantısı, Windows Server 'lar üzerinde dağıtılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Işlev uygulamasının bir parçası olarak kullanılıyorsa ' Java Version ' öğesinin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Internet 'e yönelik uç nokta ile erişim kısıtlı olmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Güvenlik Merkezi Standart fiyatlandırma katmanı seçilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: özel RBAC kurallarının kullanımını denetleme|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL Server 'da denetim etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Log Analytics Aracısı sanal makinelere yüklenmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: DDoS koruma standardı etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: MFA 'nın aboneliğinizdeki sahip izinlerine sahip hesaplarda etkinleştirilmesi gerekir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Işlev uygulamasının bir parçası olarak kullanılıyorsa, ' PHP Version ' öğesinin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Gelişmiş veri güvenliği SQL sunucularınızda etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Gelişmiş veri güvenliği, SQL yönetilen örneklerinizin üzerinde etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL yönetilen örnek gelişmiş veri güvenliği ayarları 'nda yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Azure Güvenlik Merkezi 'nde eksik Endpoint Protection Izleyicisi|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: sanal makinelere tam zamanında ağ erişim denetimi uygulanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: aboneliğiniz için bir güvenlik ilgili kişisi telefon numarası belirtilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Gelişmiş tehdit koruması türleri SQL yönetilen örneği gelişmiş veri güvenliği ayarları içinde ' tümü ' olarak ayarlanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: depolama hesapları için coğrafi olarak yedekli depolama etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: API uygulamasının bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL Server Gelişmiş veri güvenliği ayarlarında yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke etkisi: Web uygulamaları için uzaktan hata ayıklama kapatılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: uzun vadeli coğrafi olarak yedekli yedekleme, Azure SQL veritabanları için etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Işlev uygulamasını çalıştırmak için kullanılmışsa ' HTTP Version ' nin en son sürümü olduğundan emin olun|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: MFA 'nın aboneliğinizdeki okuma izinlerine sahip hesaplarda etkinleştirilmesi gerekir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için efekt: uzaktan hata ayıklama API Apps için kapalı olmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke etkisi: sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: güvenlik açığı değerlendirmesi SQL sunucularınızda etkinleştirilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Log Analytics Aracısı sanal makine ölçek kümelerine yüklenmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: Web uygulamanızda en son TLS sürümü kullanılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: İşlev Uygulaması en son TLS sürümü kullanılmalıdır|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: [Önizleme]: Kubernetes Hizmetleri, güvenlik açığı olmayan bir Kubernetes sürümüne yükseltilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|
|DoD etkisi düzeyi 5|İlke Ataması|İlke için etki: SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir|Bu ilke için Azure Ilkesi etkisi; efektler hakkında daha fazla bilgi için bkz.https://aka.ms/policyeffects|

## <a name="next-steps"></a>Sonraki adımlar

DOD etkisi düzeyi 5 şema örneğini dağıtma adımlarını gözden geçirdiğinize göre, şema ve denetim eşlemesi hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [DOD etkisi düzey 5 şema-genel bakış](./index.md) 
>  [DOD etki düzeyi 5 şema-Control eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.