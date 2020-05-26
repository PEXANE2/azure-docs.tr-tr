---
title: Azure hızlı başlangıç-Istenen durum yapılandırması ile VM yapılandırma | Microsoft Docs
description: Bu makale, Istenen durum yapılandırması ile VM yapılandırmaya başlamanıza yardımcı olur.
services: automation
ms.subservice: dsc
keywords: dsc, yapılandırma, otomasyon
ms.date: 11/06/2018
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 28e8f921fa7620d1fec7dec1788ed769026624d7
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836728"
---
# <a name="configure-a-vm-with-desired-state-configuration"></a>Istenen durum yapılandırması ile VM yapılandırma

Azure Otomasyonu durum yapılandırmasını etkinleştirerek, Istenen durum yapılandırması 'nı (DSC) kullanarak Windows ve Linux sunucularınızın yapılandırmalarını yönetebilir ve izleyebilirsiniz. İstenen yapılandırmadan alınan yapılandırmalar tanımlanabilir veya otomatik olarak düzeltilebilir. Bu hızlı başlangıçta, bir Linux sanal makinesini etkinleştirme ve Azure Otomasyonu durum yapılandırması kullanılarak bir lamba yığını dağıtma işlemleri.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
* Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
* Red Hat Enterprise Linux, CentOS veya Oracle Linux çalıştıran bir Azure Resource Manager VM (klasik değil). VM oluşturma yönergeleri için bkz. [Azure portalında ilk Linux sanal makinenizi oluşturma](../virtual-machines/linux/quick-create-portal.md)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Adresinden Azure 'da oturum açın https://portal.azure.com .

## <a name="enable-a-virtual-machine"></a>Sanal makineyi etkinleştir

Bir makineyi durum yapılandırma özelliği için etkinleştirmek üzere birçok farklı yöntem vardır. Bu hızlı başlangıçta bir Otomasyon hesabı kullanarak bir VM 'nin özelliğinin nasıl etkinleştirileceği açıklanır. [Azure Otomasyonu durum yapılandırması tarafından yönetim için makineleri etkinleştir](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)' i okuyarak makinelerinizi durum yapılandırması için etkinleştirmek üzere farklı yöntemler hakkında daha fazla bilgi edinebilirsiniz.

1. Azure portalının sol tarafındaki bölmede **Otomasyon hesapları**'nı seçin. Sol bölmede görünür değilse, **tüm hizmetler** ' e tıklayın ve sonuçta elde edilen görünümde arama yapın.
1. Listeden bir Otomasyon hesabı seçin.
1. Otomasyon hesabının sol bölmesinde, **Durum yapılandırması (DSC)** öğesini seçin.
2. VM seçme sayfasını açmak için **Ekle**'ye tıklayın.
3. DSC 'nin etkinleştirileceği sanal makineyi bulun. Aradığınız sanal makineyi bulmak için arama alanını ve filtre seçeneklerini kullanabilirsiniz.
4. Sanal makineye tıklayın ve ardından **Bağlan** ' a tıklayın.
5. Sanal makineye uygun DSC ayarlarını seçin. Zaten bir yapılandırma hazırladıysanız, bunu olarak belirtebilirsiniz `Node Configuration Name` . Makinenin yapılandırma davranışını denetlemek için [yapılandırma modunu](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaConfig) ayarlayabilirsiniz.
6. **Tamam**'a tıklayın. DSC Uzantısı sanal makineye dağıtıldığında, durum olarak gösterilir `Connecting` .

![DSC için bir Azure VM 'yi etkinleştirme](./media/automation-quickstart-dsc-configuration/dsc-onboard-azure-vm.png)

## <a name="import-modules"></a>Modülleri içeri aktarma

Modüller DSC kaynakları içerir ve [PowerShell Galerisi](https://www.powershellgallery.com)birçok bulunabilir. Yapılandırlarınızda kullanılan kaynakların, derlenmeden önce Otomasyon hesabına aktarılması gerekir. Bu öğretici için **nx** adlı modülün kullanılması gerekir.

1. Otomasyon hesabının sol bölmesinde, **paylaşılan kaynaklar**altında **modüller Galerisi** ' ni seçin.
1. Adının bir bölümünü yazarak içeri aktarılacak modülü arayın: `nx` .
1. İçeri aktarılacak modüle tıklayın.
1. **İçeri Aktar**’a tıklayın.

![DSC Modülünü içeri aktarma](./media/automation-quickstart-dsc-configuration/dsc-import-module-nx.png)

## <a name="import-the-configuration"></a>Yapılandırmayı içeri aktarma

Bu hızlı başlangıçta makinede Apache HTTP Server, MySQL ve PHP yapılandırması gerçekleştiren bir DSC yapılandırması kullanılmaktadır. Bkz. [DSC yapılandırması](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations).

Bir metin düzenleyicisinde aşağıdakini yazın ve yerel olarak **Ampserver. ps1**olarak kaydedin.

```powershell-interactive
configuration LAMPServer {
   Import-DSCResource -module "nx"

   Node localhost {

        $requiredPackages = @("httpd","mod_ssl","php","php-mysql","mariadb","mariadb-server")
        $enabledServices = @("httpd","mariadb")

        #Ensure packages are installed
        ForEach ($package in $requiredPackages){
            nxPackage $Package{
                Ensure = "Present"
                Name = $Package
                PackageManager = "yum"
            }
        }

        #Ensure daemons are enabled
        ForEach ($service in $enabledServices){
            nxService $service{
                Enabled = $true
                Name = $service
                Controller = "SystemD"
                State = "running"
            }
        }
   }
}
```

Yapılandırmayı içeri aktarmak için:

1. Otomasyon hesabının sol bölmesinde **Durum yapılandırması (DSC)** öğesini seçin ve ardından **Yapılandırmalar** sekmesine tıklayın.
2. **+ Ekle**'ye tıklayın.
3. Önceki adımda kaydettiğiniz yapılandırma dosyasını seçin.
4. **Tamam**'a tıklayın.

## <a name="compile-a-configuration"></a>Yapılandırma derleme

Bir düğüme atanabilmesi için önce bir düğüm yapılandırmasına (MOF belgesi) bir DSC yapılandırması derlemeniz gerekir. Derleme yapılandırmayı doğrular ve parametre değerlerinin girilmesini sağlar. Yapılandırma derleme hakkında daha fazla bilgi edinmek için bkz. [yapılandırmaları durum yapılandırmasında derleme](automation-dsc-compile.md).

1. Otomasyon hesabının sol bölmesinde **Durum Yapılandırması (DSC)** öğesini seçin ve ardından **yapılandırmalar** sekmesine tıklayın.
1. Yapılandırmayı seçin `LAMPServer` .
1. Menü seçeneklerinde **Derle** ' yi ve ardından **Evet**' i seçin.
1. Yapılandırma görünümünde, sıraya alınmış yeni bir derleme işi görürsünüz. İş başarıyla tamamlandıktan sonra bir sonraki adıma geçebilirsiniz. Herhangi bir başarısızlık varsa, Ayrıntılar için derleme işine tıklayabilirsiniz.

## <a name="assign-a-node-configuration"></a>Düğüm yapılandırması atama

Bir DSC düğümüne derlenmiş bir düğüm yapılandırması atayabilirsiniz. Atama yapılandırmayı makineye uygular ve bu yapılandırmadan her türlü Drın için otomatik olarak düzeltir.

1. Otomasyon hesabının sol bölmesinde **Durum Yapılandırması (DSC)** öğesini seçin ve ardından **düğümler** sekmesine tıklayın.
1. Yapılandırmanın atanacağı düğümü seçin.
1. **Düğüm Yapılandırması Ata**'ya tıklayın
1. Düğüm yapılandırmasını seçin `LAMPServer.localhost` ve **Tamam**' a tıklayın. Durum Yapılandırması artık derlenen yapılandırmayı düğüme atar ve düğüm durumu olarak değişir `Pending` . Sonraki düzenli denetim üzerinde, düğüm yapılandırmayı alır, uygular ve rapor durumunu raporlar. Düğüm ayarlarına bağlı olarak, düğümün yapılandırmayı alması 30 dakika kadar sürebilir. 
1. Düğümü hemen denetim gerçekleştirmeye zorlamak için şu komutu Linux sanal makinesinde yerel olarak çalıştırabilirsiniz: `sudo /opt/microsoft/dsc/Scripts/PerformRequiredConfigurationChecks.py`

![Düğüm Yapılandırması Atama](./media/automation-quickstart-dsc-configuration/dsc-assign-node-configuration.png)

## <a name="view-node-status"></a>Düğüm durumunu görüntüle

Otomasyon hesabınızdaki tüm durum yapılandırması tarafından yönetilen düğümlerin durumunu görüntüleyebilirsiniz. Bilgiler **Durum Yapılandırması (DSC)** seçilerek ve **düğümler** sekmesine tıklanarak görüntülenir. Görüntülenecek durum, düğüm yapılandırması veya ad aramasına göre filtre uygulayabilirsiniz.

![DSC Düğümü Durumu](./media/automation-quickstart-dsc-configuration/dsc-node-status.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Linux sanal makinesini durum yapılandırması için etkinleştirdiniz, bir lamba yığını için yapılandırma oluşturdunuz ve yapılandırmayı VM 'ye dağıttınız. Azure Otomasyonu durum yapılandırması 'nı sürekli dağıtımı etkinleştirmek üzere nasıl kullanabileceğinizi öğrenmek için, makaleye devam edin:

> [!div class="nextstepaction"]
> [Chocolatey ile sürekli dağıtım ayarlama](./automation-dsc-cd-chocolatey.md)