---
title: Azure Veri Gezgini kümeniz için doğru VM SKU'u seçin
description: Bu makalede, Azure Veri Gezgini kümesi için en uygun SKU boyutunun nasıl seçilen olduğu açıklanmaktadır.
author: avneraa
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 2d078f9715a0cfa171f0c88776a4ab78c15215a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77561859"
---
# <a name="select-the-correct-vm-sku-for-your-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümeniz için doğru VM SKU'yu seçin 

Yeni bir küme oluşturduğunuzda veya değişen iş yükü için bir kümeyi en iyi duruma getirdiğinizde, Azure Veri Gezgini aralarından seçim yapabileceğiniz birden çok sanal makine (VM) STU sunar. VM'ler, iş yükleri için size en uygun maliyeti sağlamak için özenle seçilmiştir. 

Veri yönetimi kümesinin boyutu ve VM SKU'su Azure Veri Gezgini hizmeti tarafından tamamen yönetilir. Bunlar, motorun VM boyutu ve yutma iş yükü gibi faktörlertarafından belirlenir. 

[Kümeyi ölçekleyerek](manage-cluster-vertical-scaling.md)motor kümesi için VM SKU'yu istediğiniz zaman değiştirebilirsiniz. İlk senaryoya uyan en küçük SKU boyutuyla başlamak en iyisidir. Kümenin ölçekletilmesinin, küme yeni VM SKU ile yeniden oluşturulurken 30 dakikaya kadar bir kapalı kalma süresine neden olduğunu unutmayın.

> [!TIP]
> Azure Veri Gezgini kümesi için Bilgi [İşlem Ayrılmış Örnekler (RI)](https://docs.microsoft.com/azure/virtual-machines/windows/prepay-reserved-vm-instances) geçerlidir.  

Bu makalede, çeşitli VM SKU seçenekleri açıklanır ve en iyi seçimi yapmak yardımcı olabilecek teknik ayrıntıları sağlar.

## <a name="select-a-cluster-type"></a>Küme türünü seçme

Azure Veri Gezgini iki tür küme sunar:

* **Üretim**: Üretim kümeleri, motor ve veri yönetimi kümeleri için iki düğüm içerir ve Azure Veri Gezgini [SLA](https://azure.microsoft.com/support/legal/sla/data-explorer/v1_0/)altında çalıştırılır.

* **Dev/Test (SLA yok)**: Dev/Test kümelerinde motor kümesi için tek bir D11 v2 düğümü ve veri yönetimi kümesi için tek bir D1 düğümü vardır. Bu küme türü, düşük örnek sayısı ve motor biçimlendirme ücreti olmaması nedeniyle en düşük maliyet yapılandırmasıdır. Artıklık yoksun olduğundan, bu küme yapılandırması için SLA yok.

## <a name="sku-types"></a>SKU türleri

Bir Azure Veri Gezgini kümesi oluşturduğunuzda, planlanan iş yükü için *en uygun* VM SKU'yu seçin. Aşağıdaki iki Azure Veri Gezgini SKU ailesi arasından seçim yapabilirsiniz:

* **D v2**: D SKU bilgisayar için optimize edilir ve iki çeşittir:
    * VM'nin kendisi
    * VM, birinci sınıf depolama diskleriyle birlikte

* **LS**: L SKU depolama için optimize edin. Benzer fiyatlı D SKU çok daha büyük bir SSD boyutuna sahiptir.

Kullanılabilir SKU türleri arasındaki temel farklar aşağıdaki tabloda açıklanmıştır:
 
| Öznitelik | D SKU | L SKU |
|---|---|---
|**Küçük SKUs**|Minimal boyutu iki çekirdekli D11|Minimum boyutu dört çekirdekli L4 |
|**Kullanılabilir -lik**|Tüm bölgelerde mevcuttur (DS+PS sürümü daha sınırlı kullanılabilirlik durumuna sahiptir)|Birkaç bölgede mevcuttur |
|**Çekirdek&nbsp;başına GB önbelleği başına maliyet**|D SKU ile yüksek, DS+PS sürümü ile düşük|You-You-Go Seçeneği ile en düşük |
|**Ayrılmış Örnekler (RI) fiyatlandırması**|Yüksek indirim (üç&nbsp;yıllık taahhüt için yüzde 55'in üzerinde)|Daha düşük indirim&nbsp;(üç yıllık taahhüt için yüzde 20) |  

## <a name="select-your-cluster-vm"></a>Kümevm'inizi seçin 

Küme VM'nizi seçmek için [dikey ölçekleme yapılandırın.](manage-cluster-vertical-scaling.md#configure-vertical-scaling) 

Aralarından seçim yapabileceğiniz çeşitli VM SKU seçenekleriyle, senaryonuz için performans ve sıcak önbellek gereksinimleri için maliyetleri optimize edebilirsiniz. 
* Yüksek sorgu hacmi için en uygun performansa ihtiyacınız varsa, ideal SKU bilgisayar için optimize edilmelidir. 
* Büyük hacimli verileri nispeten daha düşük sorgu yüküyle sorgulamanız gerekiyorsa, depolama için optimize edilmiş SKU maliyetleri azaltmaya ve yine de mükemmel performans sağlamaya yardımcı olabilir.

Küçük SNU'lar için küme başına örnek sayısı sınırlı olduğundan, daha büyük RAM'lere sahip daha büyük VM'ler kullanmak tercih edilir. Ram kaynağına daha fazla talep koyan bazı sorgu türleri için daha `joins`fazla RAM gerekir, örneğin. Bu nedenle, ölçeklendirme seçeneklerini düşünürken, daha fazla örnek ekleyerek ölçeklendirmek yerine daha büyük bir SKU'ya ölçeklendirmenizi öneririz.

## <a name="vm-options"></a>VM seçenekleri

Azure Veri Gezgini kümesi VM'lerinin teknik özellikleri aşağıdaki tabloda açıklanmıştır:

|**Adı**| **Kategori** | **SSD boyutu** | **Çekirdekler** | **Ram** | **Premium depolama diskleri&nbsp;(1 TB)**| **Küme başına minimum örnek sayısı** | **Küme başına maksimum örnek sayısı**
|---|---|---|---|---|---|---|---
|D11 v2| işlem için optimize edilmiş | 75&nbsp;GB    | 2 | 14&nbsp;GB | 0 | 1 | 8 (1 olan dev/test SKU hariç)
|D12 v2| işlem için optimize edilmiş | 150&nbsp;GB   | 4 | 28&nbsp;GB | 0 | 2 | 16
|D13 v2| işlem için optimize edilmiş | 307&nbsp;GB   | 8 | 56&nbsp;GB | 0 | 2 | 1000
|D14 v2| işlem için optimize edilmiş | 614&nbsp;GB   | 16| 112&nbsp;GB | 0 | 2 | 1000
|DS13 v2 + 1&nbsp;TB&nbsp;PS| depolama için optimize edilmiş | 1&nbsp;TB | 8 | 56&nbsp;GB | 1 | 2 | 1000
|DS13 v2 + 2&nbsp;TB&nbsp;PS| depolama için optimize edilmiş | 2&nbsp;TB | 8 | 56&nbsp;GB | 2 | 2 | 1000
|DS14 v2 + 3&nbsp;TB&nbsp;PS| depolama için optimize edilmiş | 3&nbsp;TB | 16 | 112&nbsp;GB | 2 | 2 | 1000
|DS14 v2 + 4&nbsp;TB&nbsp;PS| depolama için optimize edilmiş | 4&nbsp;Vest | 16 | 112&nbsp;GB | 4 | 2 | 1000
|L4s v1| depolama için optimize edilmiş | 650&nbsp;GB | 4 | 32&nbsp;GB | 0 | 2 | 16
|L8s v1| depolama için optimize edilmiş | 1.3&nbsp;TB | 8 | 64&nbsp;GB | 0 | 2 | 1000
|L16s_1| depolama için optimize edilmiş | 2.6&nbsp;TB | 16| 128&nbsp;GB | 0 | 2 | 1000

* Azure Veri Gezgini [ListesiSkus API'yi](/dotnet/api/microsoft.azure.management.kusto.clustersoperationsextensions.listskus?view=azure-dotnet)kullanarak bölge başına güncelleştirilmiş VM SKU listesini görüntüleyebilirsiniz. 
* [Çeşitli SNU'lar](/azure/virtual-machines/windows/sizes)hakkında daha fazla bilgi edinin. 

## <a name="next-steps"></a>Sonraki adımlar

* Farklı ihtiyaçlara bağlı olarak VM SKU'yu değiştirerek istediğiniz zaman motor kümesini [büyütebilir veya küçültebilirsiniz.](manage-cluster-vertical-scaling.md) 

* Değişen taleplere bağlı olarak kapasiteyi değiştirmek için motor kümesinin boyutunu ölçeklendirebilir [veya ölçeklendirebilirsiniz.](manage-cluster-horizontal-scaling.md)

