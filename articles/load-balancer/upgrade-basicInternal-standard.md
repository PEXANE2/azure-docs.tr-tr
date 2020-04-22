---
title: Temel Dahili'den Standart Dahili'ye Yükseltme - Azure Yük Dengeleyicisi
description: Bu makalede, Azure Dahili Yük Dengeleyicisi'ni Temel SKU'dan Standart SKU'ya nasıl yükseltebilirsiniz
services: load-balancer
author: irenehua
ms.service: load-balancer
ms.topic: article
ms.date: 02/23/2020
ms.author: irenehua
ms.openlocfilehash: 239dc0f3133a5adf59a23d333131c91d3a655597
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770379"
---
# <a name="upgrade-azure-internal-load-balancer--no-outbound-connection-required"></a>Azure Dahili Yük Dengeleyicisi'ni Yükseltin- Giden Bağlantı Gerekmez
[Azure Standart Yük Dengeleyici,](load-balancer-overview.md) bölge artıklığı sayesinde zengin bir işlevsellik kümesi ve yüksek kullanılabilirlik sunar. Yük Dengeleyici SKU hakkında daha fazla bilgi edinmek için [karşılaştırma tablosuna](https://docs.microsoft.com/azure/load-balancer/concepts-limitations#skus)bakın.

Bu makalede, Temel Yük Dengeleyicisi ile aynı yapılandırmaya sahip bir Standart Yük Dengeleyicisi ve Temel Yük Dengeleyicisi'nden Standart Yük Dengeleyicisine geçen bir PowerShell komut dosyası tanıtıştır.

## <a name="upgrade-overview"></a>Yükseltmeye genel bakış

Aşağıdakileri yapan bir Azure PowerShell komut dosyası kullanılabilir:

* Belirttiğiniz konumda standart bir SKU Yük Dengeleyicisi oluşturur. Standart İç Yük Dengeleyicisi tarafından [giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) sağlanmayacağını unutmayın.
* Temel SKU Yük Dengeleyicisi'nin yapılandırmalarını yeni oluşturulan Standart Yük Dengeleyicisine sorunsuz bir şekilde kopyalar.
* Özel IP'leri Temel Yük Dengeleyici'nden yeni oluşturulan Standart Yük Dengeleyicisine sorunsuz bir şekilde taşıyın.
* VM'leri Temel Yük Dengeleyicisinin arka uç havuzundan Standart Yük Dengeleyicisinin arka uç havuzuna sorunsuz bir şekilde taşıyın

### <a name="caveatslimitations"></a>Uyarılar\Sınırlamalar

* Komut dosyası yalnızca giden bağlantı gerektirmediği durumlarda İç Yük Dengeleyici yükseltmesi destekler. Bazı VM'leriniz için [giden bağlantıya](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) ihtiyacınız varsa, talimatlar için lütfen bu [sayfaya](upgrade-InternalBasic-To-PublicStandard.md) bakın. 
* Standart yük dengeleyicisi farklı bir bölgede oluşturulursa, eski bölgede bulunan VM'leri yeni oluşturulan Standart Yük Dengeleyicisi ile ilişkilendiremezsinuz. Bu sınırlamayı aşmak için yeni bölgede yeni bir VM oluşturduğunuzdan emin olun.
* Yük Dengeleyicinizde ön uç IP yapılandırması veya arka uç havuzu yoksa, komut dosyasını çalıştıran bir hataya çarpma olasılığınız yüksektir. Boş olmadıklarından emin olun.

## <a name="download-the-script"></a>Komut dosyasını indirin

[PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureILBUpgrade/2.0)geçiş komut dosyasını indirin.
## <a name="use-the-script"></a>Komut dosyasını kullanma

Yerel PowerShell ortamı kurulumunuza ve tercihlerinize bağlı olarak sizin için iki seçenek vardır:

* Azure Az modülleri yüklü değilse veya Azure Az modüllerini kaldırmakta sakınca görmüyorsanız, en iyi seçenek `Install-Script` komut dosyasını çalıştırma seçeneğini kullanmaktır.
* Azure Az modüllerini saklamanız gerekiyorsa, en iyi seçenek komut dosyasını indirip doğrudan çalıştırmaktır.

Azure Az modüllerinin yüklü olup olmadığını belirlemek `Get-InstalledModule -Name az`için çalıştırın. Yüklü Az modülleri görmüyorsanız, `Install-Script` yöntemi kullanabilirsiniz.

### <a name="install-using-the-install-script-method"></a>Install-Script yöntemini kullanarak yükleme

Bu seçeneği kullanmak için bilgisayarınızda Azure Az modüllerinin yüklü olmaması gerekir. Yüklüyse, aşağıdaki komut bir hata görüntüler. Azure Az modüllerini kaldırabilir veya komut dosyasını el ile indirip çalıştırmak için diğer seçeneği kullanabilirsiniz.
  
Komut dosyasını aşağıdaki komutla çalıştırın:

`Install-Script -Name AzureILBUpgrade`

Bu komut aynı zamanda gerekli Az modüllerini de yükler.  

### <a name="install-using-the-script-directly"></a>Komut dosyasını kullanarak doğrudan yükleme

Bazı Azure Az modülleriniz yüklüyse ve bunları kaldıramıyorsanız (veya kaldırmak istemiyorsanız), komut dosyası indirme bağlantısındaki **Manuel İndir** sekmesini kullanarak komut dosyasını el ile indirebilirsiniz. Komut dosyası ham nupkg dosyası olarak indirilir. Bu nupkg dosyasından komut dosyasını yüklemek için Bkz. [Manuel Paket İndir.](/powershell/scripting/gallery/how-to/working-with-packages/manual-download)

Betiği çalıştırmak için:

1. Azure'a bağlanmak için kullanın. `Connect-AzAccount`

1. Az `Import-Module Az` modüllerini almak için kullanın.

1. Gerekli parametreleri inceleyin:

   * **rgName: [String]: Gerekli** – Bu, mevcut Temel Yük Dengeleyiciniz ve yeni Standart Yük Dengeleyiciniz için kaynak grubudur. Bu dize değerini bulmak için Azure portalına gidin, Temel Yük Bakiyesi kaynağınızı seçin ve yük bakiyesi için **Genel Bakış'ı** tıklatın. Kaynak Grubu bu sayfada yer alır.
   * **oldLBName: [String]: Gerekli** – Bu yükseltmek istediğiniz mevcut Basic Balancer adıdır. 
   * **newlocation: [String]: Gerekli** – Bu, Standart Yük Dengeleyicisinin oluşturulacağı yerdir. Diğer mevcut kaynaklarla daha iyi ilişkilendirmek için seçilen Temel Yük Dengeleyicisinin aynı konumunu Standart Yük Dengeleyicisine devretmek önerilir.
   * **newLBName: [String]: Gerekli** – Bu oluşturulacak Standart Yük Dengeleyicisi'nin adıdır.
1. Uygun parametreleri kullanarak komut dosyasını çalıştırın. Bitirmesi beş ila yedi dakika sürebilir.

    **Örnek**

   ```azurepowershell
   AzureILBUpgrade.ps1 -rgName "test_InternalUpgrade_rg" -oldLBName "LBForInternal" -newlocation "centralus" -newLbName "LBForUpgrade"
   ```

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Azure PowerShell komut dosyasında yapılandırmayı v1'den v2'ye geçirmek için herhangi bir sınırlama var mı?

Evet. Bkz. [Uyarılar/Sınırlamalar.](#caveatslimitations)

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-basic-load-balancer-to-the-newly-created-standard-load-balancer"></a>Azure PowerShell komut dosyası, temel yük bakiyemden yeni oluşturulan Standart Yük Dengeleyicisi'ne de trafik geçişyapar mı?

Evet trafik göç eder. Trafiği kişisel olarak geçirmek istiyorsanız, VM'leri sizin için taşımayan [bu komut dosyasını](https://www.powershellgallery.com/packages/AzureILBUpgrade/1.0) kullanın.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Ben bu komut dosyası kullanarak bazı sorunlar la karşılaştı. Nasıl yardım alabilirim?
  
Azure Desteği ile slbupgradesupport@microsoft.combir destek servis talebi açmak için e-posta gönderebilir veya her ikisini de yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Standart Yük Dengeleyicisi hakkında bilgi edinin](load-balancer-overview.md)
