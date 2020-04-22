---
title: Temel Genel'den Standart Genele yükseltme - Azure Yük Dengeleyicisi
description: Bu makalede, Azure Genel Yük Bakiyeleyicisini Temel SKU'dan Standart SKU'ya nasıl yükseltilen gösterilmektedir
services: load-balancer
author: irenehua
ms.service: load-balancer
ms.topic: article
ms.date: 01/23/2020
ms.author: irenehua
ms.openlocfilehash: a2d6f41756d87e43ac7db9e6a8670c453920c834
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770363"
---
# <a name="upgrade-azure-public-load-balancer"></a>Azure Genel Yük Dengeleyicisi'ni yükseltin
[Azure Standart Yük Dengeleyici,](load-balancer-overview.md) bölge artıklığı sayesinde zengin bir işlevsellik kümesi ve yüksek kullanılabilirlik sunar. Yük Dengeleyici SKU hakkında daha fazla bilgi edinmek için [karşılaştırma tablosuna](https://docs.microsoft.com/azure/load-balancer/concepts-limitations#skus)bakın.

Yükseltmenin üç aşaması vardır:

1. Yapılandırmayı geçirin
2. Standart Yük Dengeleyicisinin arka uç havuzlarına VM ekleme

Bu makalede, yapılandırma geçişi kapsar. Arka uç havuzlarına VM eklemek, özel ortamınıza bağlı olarak değişebilir. Ancak, bazı üst düzey, genel öneriler [sağlanır.](#add-vms-to-backend-pools-of-standard-load-balancer)

## <a name="upgrade-overview"></a>Yükseltmeye genel bakış

Aşağıdakileri yapan bir Azure PowerShell komut dosyası kullanılabilir:

* Kaynak grubunda ve belirttiğiniz konumda standart bir SKU Yük Dengeleyicisi oluşturur.
* Temel SKU Yük Dengeleyicisi'nin yapılandırmalarını yeni oluşturulan Standart Yük Dengeleyicisine sorunsuz bir şekilde kopyalar.
* Giden bağlantı sağlayan varsayılan giden bir kural oluşturur.

### <a name="caveatslimitations"></a>Uyarılar\Sınırlamalar

* Komut dosyası yalnızca Genel Yük Dengeleyici yükseltmesi destekler. İç Temel Yük Dengeleyici yükseltmesi için talimatlar için [bu sayfaya](https://docs.microsoft.com/azure/load-balancer/upgrade-basicinternal-standard) bakın.
* Standart Yük Dengeleyici'nin yeni bir genel adresi vardır. Farklı SNU'lara sahip oldukları için, mevcut Temel Yük Dengeleyicisi ile ilişkili IP adreslerini sorunsuz bir şekilde Standart Yük Dengeleyicisine taşımak mümkün değildir.
* Standart yük dengeleyicisi farklı bir bölgede oluşturulursa, eski bölgede bulunan VM'leri yeni oluşturulan Standart Yük Dengeleyicisi ile ilişkilendiremezsinuz. Bu sınırlamayı aşmak için yeni bölgede yeni bir VM oluşturduğunuzdan emin olun.
* Yük Dengeleyicinizde ön uç IP yapılandırması veya arka uç havuzu yoksa, komut dosyasını çalıştıran bir hataya çarpma olasılığınız yüksektir. Lütfen boş olmadıklarından emin olun.

## <a name="download-the-script"></a>Komut dosyasını indirin

[PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzurePublicLBUpgrade/2.0)geçiş komut dosyasını indirin.
## <a name="use-the-script"></a>Komut dosyasını kullanma

Yerel PowerShell ortamı kurulumunuza ve tercihlerinize bağlı olarak sizin için iki seçenek vardır:

* Azure Az modülleri yüklü değilse veya Azure Az modüllerini kaldırmakta sakınca görmüyorsanız, en iyi seçenek `Install-Script` komut dosyasını çalıştırma seçeneğini kullanmaktır.
* Azure Az modüllerini saklamanız gerekiyorsa, en iyi seçenek komut dosyasını indirip doğrudan çalıştırmaktır.

Azure Az modüllerinin yüklü olup olmadığını belirlemek `Get-InstalledModule -Name az`için çalıştırın. Yüklü Az modülleri görmüyorsanız, `Install-Script` yöntemi kullanabilirsiniz.

### <a name="install-using-the-install-script-method"></a>Install-Script yöntemini kullanarak yükleme

Bu seçeneği kullanmak için bilgisayarınızda Azure Az modüllerinin yüklü olmaması gerekir. Yüklüyse, aşağıdaki komut bir hata görüntüler. Azure Az modüllerini kaldırabilir veya komut dosyasını el ile indirip çalıştırmak için diğer seçeneği kullanabilirsiniz.
  
Komut dosyasını aşağıdaki komutla çalıştırın:

`Install-Script -Name AzurePublicLBUpgrade`

Bu komut aynı zamanda gerekli Az modüllerini de yükler.  

### <a name="install-using-the-script-directly"></a>Komut dosyasını kullanarak doğrudan yükleme

Bazı Azure Az modülleriniz yüklüyse ve bunları kaldıramıyorsanız (veya kaldırmak istemiyorsanız), komut dosyası indirme bağlantısındaki **Manuel İndir** sekmesini kullanarak komut dosyasını el ile indirebilirsiniz. Komut dosyası ham nupkg dosyası olarak indirilir. Bu nupkg dosyasından komut dosyasını yüklemek için Bkz. [Manuel Paket İndir.](/powershell/scripting/gallery/how-to/working-with-packages/manual-download)

Betiği çalıştırmak için:

1. Azure'a bağlanmak için kullanın. `Connect-AzAccount`

1. Az `Import-Module Az` modüllerini almak için kullanın.

1. Gerekli parametreleri inceleyin:

   * **oldRgName: [String]: Gerekli** – Bu yükseltmek istediğiniz mevcut Temel Yük Dengeleyici için kaynak grubudur. Bu dize değerini bulmak için Azure portalına gidin, Temel Yük Bakiyesi kaynağınızı seçin ve yük bakiyesi için **Genel Bakış'ı** tıklatın. Kaynak Grubu bu sayfada yer alır.
   * **oldLBName: [String]: Gerekli** – Bu yükseltmek istediğiniz mevcut Basic Balancer adıdır. 
   * **newrgName: [String]: Gerekli** – Bu, Standart Yük Dengeleyicisinin oluşturulacağı kaynak grubudur. Yeni bir kaynak grubu veya varolan bir grup olabilir. Varolan bir kaynak grubu seçerseniz, Yük Dengeleyicisi adının kaynak grubunda benzersiz olması gerektiğini unutmayın. 
   * **newlocation: [String]: Gerekli** – Bu, Standart Yük Dengeleyicisinin oluşturulacağı yerdir. Diğer mevcut kaynaklarla daha iyi ilişkilendirmek için seçilen Temel Yük Dengeleyicisinin aynı konumunu Standart Yük Dengeleyicisine devretmek önerilir.
   * **newLBName: [String]: Gerekli** – Bu oluşturulacak Standart Yük Dengeleyicisi'nin adıdır.
1. Uygun parametreleri kullanarak komut dosyasını çalıştırın. Bitirmesi beş ila yedi dakika sürebilir.

    **Örnek**

   ```azurepowershell
   AzurePublicLBUpgrade.ps1 -oldRgName "test_publicUpgrade_rg" -oldLBName "LBForPublic" -newrgName "test_userInput3_rg" -newlocation "centralus" -newLbName "LBForUpgrade"
   ```

### <a name="add-vms-to-backend-pools-of-standard-load-balancer"></a>Standart Yük Dengeleyicisinin arka uç havuzlarına VM ekleme

İlk olarak, komut dosyasının Temel Genel Yük Dengeleyicinizden tam yapılandırmaile birlikte yeni bir Standart Genel Yük Dengeleyicisi'ni başarıyla oluşturacağından çift kontrol edin. Bunu Azure portalından doğrulayabilirsiniz.

El ile test olarak Standart Yük Dengeleyicisi aracılığıyla az miktarda trafik gönderdiğinden emin olun.
  
Yeni oluşturulan Standart Genel Yük Dengeleyicisinin arka uç havuzlarına VM'leri nasıl eklediğinize ilişkin birkaç senaryo ve her biri için önerilerimiz aşağıda verilmiştir:

* **Mevcut VM'leri eski Temel Genel Yük Dengeleyicisinin arka uç havuzlarından yeni oluşturulan Standart Genel Yük Dengeleyicisinin arka uç havuzlarına taşıma.**
    1. Bu hızlı başlangıçta görevleri yapmak için [Azure portalında](https://portal.azure.com)oturum açın.
 
    1. Sol menüdeki **Tüm kaynakları** seçin ve ardından kaynak listesinden yeni oluşturulan Standart **Yük Dengeleyicisini** seçin.
   
    1. **Ayarlar**’ın altında **Arka Uç Havuzları**’nı seçin.
   
    1. Temel Yük Dengeleyicisi'nin arka uç havuzuyla eşleşen arka uç havuzunu seçin ve aşağıdaki değeri seçin: 
      - **Sanal Makine**: Temel Yük Dengeleyicisi'nin eşleşen arka uç havuzundan VM'leri aşağı indirin ve seçin.
    1. **Kaydet**’i seçin.
    >[!NOTE]
    >Genel IP'leri olan VM'ler için, önce aynı IP adresinin garanti edilemediğinizde Standart IP adresleri oluşturmanız gerekir. VM'leri Temel IP'lerden ayırın ve bunları yeni oluşturulan Standart IP adresleriyle ilişkilendirin. Daha sonra, Standart Yük Dengeleyici'nin arka uç havuzuna VM eklemek için yönergeleri izleyebilirsiniz. 

* **Yeni oluşturulan Standart Genel Yük Dengeleyicisinin arka uç havuzlarına eklemek için yeni VM'ler oluşturma.**
    * VM oluşturmak ve Standart Yük Dengeleyici ile ilişkilendirmek için nasıl daha fazla talimat [burada](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal#create-virtual-machines)bulunabilir.

### <a name="create-an-outbound-rule-for-outbound-connection"></a>Giden bağlantı için giden kural oluşturma

Giden bir kural oluşturmak için [yönergeleri](https://docs.microsoft.com/azure/load-balancer/configure-load-balancer-outbound-portal#create-outbound-rule-configuration) izleyin, böylece
* Giden NAT'yi sıfırdan tanımlayın.
* Varolan giden NAT'nin davranışını ölçeklendirin ve ayarlayın.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Azure PowerShell komut dosyasında yapılandırmayı v1'den v2'ye geçirmek için herhangi bir sınırlama var mı?

Evet. Bkz. [Uyarılar/Sınırlamalar.](#caveatslimitations)

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-basic-load-balancer-to-the-newly-created-standard-load-balancer"></a>Azure PowerShell komut dosyası, temel yük bakiyemden yeni oluşturulan Standart Yük Dengeleyicisi'ne de trafik geçişyapar mı?

Hayır. Azure PowerShell komut dosyası yalnızca yapılandırmayı geçirtir. Gerçek trafik geçişi sizin sorumluluğunuzda dır ve sizin kontrolünüzdedir.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Ben bu komut dosyası kullanarak bazı sorunlar la karşılaştı. Nasıl yardım alabilirim?
  
Azure Desteği ile slbupgradesupport@microsoft.combir destek servis talebi açmak için e-posta gönderebilir veya her ikisini de yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Standart Yük Dengeleyicisi hakkında bilgi edinin](load-balancer-overview.md)
