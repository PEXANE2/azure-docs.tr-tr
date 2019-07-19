---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/28/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 9832ae6010f262322328ec22876a35c89e1bedca
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850333"
---
VM 'Lerin tek bir bölgeye yerleştirilmesi, örnekler arasındaki fiziksel mesafeyi azaltır. Bunları tek bir kullanılabilirlik alanına koymak, bunları fiziksel olarak bir araya getirir. Ancak, Azure ayak izi büyüdükçe, tek bir kullanılabilirlik alanı birden fazla fiziksel veri merkezine yayılabilir ve bu da uygulamanızı etkileyen bir ağ gecikme süresi oluşmasına neden olabilir. 

Olası en düşük gecikme süresini elde etmek üzere VM 'Leri olabildiğince yakın bir şekilde almak için, bunları bir yakınlık yerleşimi grubu içinde dağıtmanız gerekir.

Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimi grupları, düşük gecikme süresinin gereksinim olduğu iş yükleri için faydalıdır.


- Tek başına VM 'Ler arasında düşük gecikme süresi.
- Tek bir kullanılabilirlik kümesindeki veya bir sanal makine ölçek kümesindeki VM 'Ler arasında düşük gecikme süresi. 
- Tek başına VM 'Ler, birden çok kullanılabilirlik kümesindeki VM 'Ler veya birden çok ölçek kümesi arasında düşük gecikme süresi. Çok katmanlı bir uygulamayı bir araya getirmek için tek bir yerleştirme grubunda birden çok işlem kaynağınız olabilir. 
- Farklı donanım türleri kullanan birden çok uygulama katmanı arasında düşük gecikme süresi. Örneğin, arka ucu bir kullanılabilirlik kümesinde M serisi ile ve bir bir D serisi örneğinde, bir ölçek kümesinde, tek bir yakınlık yerleşimi grubunda ön uç ile çalışır.

> [!IMPORTANT]
> Yakınlık yerleşimi grupları Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Önizleme sırasında yakınlık yerleşimi grupları bu bölgelerde kullanılamaz: **Japonya Doğu**, **Avustralya Doğu** ve **Hindistan Orta**.


## <a name="best-practices"></a>En iyi uygulamalar 
- En düşük gecikme süresi için, hızlandırılmış ağlarla birlikte yakınlık yerleştirme gruplarını kullanın. Daha fazla bilgi için bkz. [hızlandırılmış ağ Ile Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [hızlandırılmış ağ ile Windows sanal makinesi oluşturma](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Tüm VM boyutlarını tek bir Resource Manager şablonunda dağıtın. İhtiyaç duyduğunuz tüm VM SKU 'Larını ve boyutlarını desteklemeyen donanımlarda sahanın önüne geçmek için, tüm uygulama katmanlarını tek bir şablona dahil edin, böylece bunların hepsi aynı anda dağıtılacaktır.
- Dağıtımınızı PowerShell, CLı veya SDK kullanarak yaparsanız, bir ayırma hatası `OverconstrainedAllocationRequest`alabilirsiniz. Bu durumda, mevcut tüm VM 'Leri durdurup serbest bırakabilir ve dağıtım betiğindeki sırayı, başarısız olan VM SKU 'SU/boyutlarına başlayacak şekilde değiştirmelisiniz. 
- VM 'lerden silinen mevcut bir yerleştirme grubunu yeniden kullandığınızda, VM 'Leri eklemeden önce silme işleminin tam tamamlanmasını bekleyin.
- Gecikme süresi ilk önceliğiniz ise, VM 'Leri bir yakınlık yerleşimi grubuna ve çözümün tamamına bir kullanılabilirlik alanına yerleştirin. Ancak dayanıklılık en üst önceliğiniz ise, örneklerinizi birden çok kullanılabilirlik alanına (tek bir yakınlık yerleştirme grubu bölgelere yayılamaz) yayın.