---
title: Temel Iç istemciden standart Iç Azure Load Balancer yükseltme
description: Bu makalede, Azure Iç Load Balancer temel SKU 'dan standart SKU 'ya nasıl yükselteceğiniz gösterilmektedir
services: load-balancer
author: irenehua
ms.service: load-balancer
ms.topic: article
ms.date: 02/23/2020
ms.author: irenehua
ms.openlocfilehash: 543227ac9c07207112177dfaccbd00723b61a314
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77566406"
---
# <a name="upgrade-azure-internal-load-balancer-from-basic-sku-to-standard-sku"></a>Temel SKU 'dan standart SKU 'ya Azure Iç Load Balancer yükseltme
[Azure Standart Load Balancer](load-balancer-overview.md) , bölge artıklığı aracılığıyla zengin bir işlev kümesi ve yüksek kullanılabilirlik sağlar. Load Balancer SKU 'SU hakkında daha fazla bilgi için bkz. [karşılaştırma tablosu](https://docs.microsoft.com/azure/load-balancer/concepts-limitations#skus).

Bir yükseltmede iki aşama vardır:

1. Yapılandırmayı geçirme
2. Standart Load Balancer arka uç havuzlarına VM ekleme

Bu makalede yapılandırma geçişi ele alınmaktadır. Arka uç havuzlara sanal makine eklemek, belirli ortamınıza bağlı olarak değişebilir. Ancak, bazı üst düzey genel öneriler [sağlanır](#add-vms-to-backend-pools-of-standard-load-balancer).

## <a name="upgrade-overview"></a>Yükseltmeye genel bakış

Aşağıdakileri gerçekleştiren bir Azure PowerShell betiği vardır:

* Kaynak grubunda ve belirttiğiniz konumda standart bir Iç SKU Load Balancer oluşturur.
* Temel SKU Iç Load Balancer yapılandırmalarının yeni oluşturma standart Iç Load Balancer sorunsuz bir şekilde kopyasını oluşturur.

### <a name="caveatslimitations"></a>Caveats\Limitations

* Betik yalnızca dahili Load Balancer yükseltmesini destekler. Iç temel Load Balancer yükseltme için, giden bağlantı istenmiyorsa standart bir iç Load Balancer oluşturun ve giden bağlantı gerekliyse standart bir Iç Load Balancer ve standart Iç Load Balancer oluşturun.
* Standart Load Balancer yeni ortak adreslere sahiptir. Farklı SKU 'Lara sahip olduklarından, var olan temel Load Balancer ilişkili IP adreslerini Standart Load Balancer sorunsuzca taşımak olanaksızdır.
* Standart yük dengeleyici farklı bir bölgede oluşturulduysa, eski bölgede var olan VM 'Leri yeni oluşturulan Standart Load Balancer ilişkilendiremeyeceksiniz. Bu kısıtlamayı geçici olarak çözmek için yeni bölgede yeni bir VM oluşturun.
* Load Balancer herhangi bir ön uç IP yapılandırması veya arka uç havuzu yoksa, betiği çalıştırırken bir hatayla karşılaşamayacaksınız. Lütfen boş olmadıklarından emin olun.

## <a name="download-the-script"></a>Betiği indir

[PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureILBUpgrade/1.0)geçiş betiğini indirin.
## <a name="use-the-script"></a>Betiği kullan

Yerel PowerShell ortamınız kuruluma ve tercihlerinize bağlı olarak sizin için iki seçenek vardır:

* Azure az modüller yüklü değilse veya Azure az modüllerini kaldırmayı bilmiyorsanız en iyi seçenek, komut dosyasını çalıştırmak için `Install-Script` seçeneğini kullanmaktır.
* Azure az modules tutmanız gerekiyorsa, en iyi sonuç, betiği indirmek ve doğrudan çalıştırmak olacaktır.

Azure az modules yüklü olup olmadığınızı öğrenmek için `Get-InstalledModule -Name az`çalıştırın. Yüklü az modül görmüyorsanız, `Install-Script` yöntemini kullanabilirsiniz.

### <a name="install-using-the-install-script-method"></a>Install-Script metodunu kullanarak install

Bu seçeneği kullanmak için, bilgisayarınızda Azure az modules yüklü olmamalıdır. Yüklüyse, aşağıdaki komut bir hata görüntüler. Azure az modüller ' i kaldırabilir veya betiği el ile indirmek ve çalıştırmak için diğer seçeneği kullanabilirsiniz.
  
Betiği aşağıdaki komutla çalıştırın:

`Install-Script -Name AzureILBUpgrade`

Bu komut ayrıca gerekli az modülleri de yüklüyor.  

### <a name="install-using-the-script-directly"></a>Betiği kullanarak doğrudan yüklemeyi

Bazı Azure az modülleriniz varsa ve bunları kaldıramıyorsanız (veya kaldırmak istemiyorsanız), betik indirme bağlantısındaki **El Ile indir** sekmesini kullanarak betiği el ile indirebilirsiniz. Betik, ham nupkg dosyası olarak indirilir. Betiği bu nupkg dosyasından yüklemek için bkz. [El Ile paket indirme](/powershell/scripting/gallery/how-to/working-with-packages/manual-download).

Betiği çalıştırmak için:

1. Azure 'a bağlanmak için `Connect-AzAccount` kullanın.

1. Az modülleri içeri aktarmak için `Import-Module Az` kullanın.

1. Gerekli parametreleri inceleyin:

   * **RgName: [dize]: gerekli** – bu, var olan temel Load Balancer ve yeni standart Load Balancer için kaynak grubudur. Bu dize değerini bulmak için Azure portal gidin, temel Load Balancer kaynağınızı seçin ve yük dengeleyiciye **Genel Bakış ' a** tıklayın. Kaynak grubu bu sayfada bulunur.
   * **Oldlbname: [dize]: gerekli** – bu, yükseltmek Istediğiniz mevcut temel dengeleyicinizin adıdır. 
   * **newLocation: [dize]: gerekli** – standart Load Balancer oluşturulacağı konumdur. Diğer mevcut kaynaklarla daha iyi ilişki sağlamak için, seçilen temel Load Balancer aynı konumun Standart Load Balancer aynı konuma devralması önerilir.
   * **Newlbname: [dize]: gerekli** – bu, oluşturulacak standart Load Balancer adıdır.
1. Uygun parametreleri kullanarak betiği çalıştırın. Tamamlanması beş ila yedi dakika sürebilir.

    **Örnek**

   ```azurepowershell
   ./AzureILBUpgrade.ps1 -rgName "test_InternalUpgrade_rg" -oldLBName "LBForInternal" -newlocation "centralus" -newLbName "LBForUpgrade"
   ```

### <a name="add-vms-to-backend-pools-of-standard-load-balancer"></a>Standart Load Balancer arka uç havuzlarına VM ekleme

İlk olarak, betiğin, temel Iç Load Balancer üzerinden geçirilen tam yapılandırmayla yeni bir standart dahili Load Balancer başarıyla oluşturulduğunu bir kez daha kontrol edin. Bunu Azure portal doğrulayabilirsiniz.

El ile test olarak Standart Load Balancer aracılığıyla az miktarda trafik gönderdiğinizden emin olun.
  
Aşağıda, yeni oluşturulan standart Iç Load Balancer için arka uç havuzlarına sanal makineler eklemenin ve her biri için önerdiğimiz bazı senaryolar verilmiştir:

* **Mevcut VM 'leri eski temel iç Load Balancer arka uç havuzlarından yeni oluşturulan Standart iç Load Balancer arka uç havuzlarından taşıma**.
    1. Bu hızlı başlangıçta görevleri yapmak için [Azure Portal](https://portal.azure.com)oturum açın.
 
    1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **Yeni oluşturulan standart Load Balancer** seçin.
   
    1. **Ayarlar**’ın altında **Arka Uç Havuzları**’nı seçin.
   
    1. Temel Load Balancer arka uç havuzuyla eşleşen arka uç havuzunu seçin, aşağıdaki değeri seçin: 
      - **Sanal makine**: açılır ve temel Load Balancer eşleşen arka uç havuzundan VM 'leri seçin.
    1. **Kaydet**’i seçin.
    >[!NOTE]
    >Ortak IP 'leri olan VM 'Ler için, ilk olarak aynı IP adresinin garantili olmadığı standart IP adresleri oluşturmanız gerekecektir. VM 'Lerin temel IP 'lerden ilişkisini kaldırın ve yeni oluşturulan standart IP adresleriyle ilişkilendirin. Ardından, Standart Load Balancer arka uç havuzuna VM 'Ler eklemek için yönergeleri takip edebilirsiniz. 

* **Yeni oluşturulan Standart iç Load Balancer arka uç havuzlarına eklemek için yeni VM 'Ler oluşturma**.
    * VM oluşturma ve Standart Load Balancer ile ilişkilendirme hakkında daha fazla yönerge [burada](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-portal#create-virtual-machines)bulunabilir.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Yapılandırmayı v1 'den v2 'ye geçirmek için Azure PowerShell betiğiyle ilgili herhangi bir sınırlama var mı?

Evet. Bkz. [Uyarılar/sınırlamalar](#caveatslimitations).

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-basic-load-balancer-to-the-newly-created-standard-load-balancer"></a>Azure PowerShell betiği, temel Load Balancer trafik üzerinde yeni oluşturulan Standart Load Balancer da geçiş yapar mi?

Hayır. Azure PowerShell betiği yalnızca yapılandırmayı geçirir. Gerçek trafik geçişi, sizin ve denetiminizin sorumluluğundadır.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Bu betiği kullanmayla ilgili bazı sorunlarla karşılaştım. Nasıl yardım alabilirim?
  
slbupgradesupport@microsoft.combir e-posta gönderebilir, Azure desteğiyle bir destek talebi açabilir veya her ikisini de yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Standart Load Balancer hakkında bilgi edinin](load-balancer-overview.md)
