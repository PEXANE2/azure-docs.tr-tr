---
title: Azure dosyaları için paylaşılan anlık görüntülere genel bakış | Microsoft Docs
description: Paylaşma anlık görüntüsü, bir Azure dosya paylaşımının, bir zamanda, paylaşımın yedeklenme yöntemi olarak bir noktada alınmış bir salt okuma sürümüdür.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d415ef165da18312a458d7d14fba18acd1bf44cf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84235615"
---
# <a name="overview-of-share-snapshots-for-azure-files"></a>Azure Dosyaları için paylaşım anlık görüntülerine genel bakış

Azure Dosyalar, dosya paylaşımlarının anlık görüntüsünü alma imkanı sunar. Paylaşım anlık görüntüleri, paylaşımın belirli bir noktadaki durumunu yakalar. Bu makalede paylaşım anlık görüntülerinin sağladığı imkanlar ve bu kullanım örneğinde bu imkanlardan nasıl faydalanabileceğiniz anlatılmıştır.

## <a name="when-to-use-share-snapshots"></a>Paylaşım anlık görüntülerinin kullanılması gereken durumlar

### <a name="protection-against-application-error-and-data-corruption"></a>Uygulama hatasına ve veri bozulmasına karşı koruma sağlama

Dosya paylaşımlarını kullanan uygulamalar yazma, okuma, depolama, iletme ve işleme gibi işlemler gerçekleştirir. Hatalı yapılandırılmış olan veya içinde tespit edilememiş bir hata bulunan uygulamalar yanlışlıkla verilerin üzerine yazabilir veya birkaç bloğa zarar verebilir. Bu senaryolara karşı koruma sağlamak için yeni uygulama kodunu dağıtmadan önce paylaşımın anlık görüntüsünü alabilirsiniz. Yeni dağıtımda bir hata veya uygulama kusuru olması halinde dosya paylaşımındaki verilerinizin önceki sürümüne geri dönebilirsiniz. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>Yanlışlıkla silme veya istenmeyen değişikliklere karşı koruma

Bir dosya paylaşımında bulunan bir metin dosyası üzerinde çalıştığınızı düşünün. Metin dosyası kapatıldıktan sonra değişikliklerinizi geri alma imkanını kaybedersiniz. Bu gibi durumlarda, dosyanın önceki bir sürümünü kurtarmanız gerekir. Yanlışlıkla yeniden adlandırılması veya silinmesi halinde dosyanın önceki sürümlerini kurtarmak için paylaşım anlık görüntülerini kullanabilirsiniz.

### <a name="general-backup-purposes"></a>Genel yedekleme amacıyla

Bir dosya paylaşımı oluşturduktan sonra bu dosya paylaşımından düzenli olarak paylaşım anlık görüntüsü oluşturarak veri yedeği olarak kullanabilirsiniz. Düzenli olarak alınan paylaşım anlık görüntüleri, verilerin önceki sürümlerinin korunmasına ve ileride yapılacak denetimler veya olağanüstü durum kurtarma çalışmaları için kullanılmasına yardımcı olur. Anlık görüntü almak ve yönetmek için [Azure dosya paylaşma yedeklemesini](../../backup/azure-file-share-backup-overview.md) bir yedekleme çözümü olarak kullanmanızı öneririz. Ayrıca, CLı veya PowerShell kullanarak anlık görüntüleri kendiniz de alabilir ve yönetebilirsiniz.

## <a name="capabilities"></a>Özellikler

Paylaşılan anlık görüntü, verilerinizin zaman içindeki bir noktadaki kopyasıdır. REST API kullanarak anlık görüntü oluşturabilir, silebilir ve yönetebilirsiniz. Aynı yetenekler istemci kitaplığı, Azure CLı ve Azure portal de mevcuttur. 

Bir paylaşımın anlık görüntülerini hem REST API hem de SMB kullanarak görüntüleyebilirsiniz. Dizin veya dosya sürümlerinin listesini alabilir ve belirli bir sürümü doğrudan sürücü olarak bağlayabilirsiniz (yalnızca Windows-bkz. [sınırlara](#limits)ulaşılabilir). 

Bir paylaşma anlık görüntüsü oluşturulduktan sonra okunabilir, kopyalanabilir veya silinebilir, ancak değiştirilmez. Bir tam paylaşma anlık görüntüsünü başka bir depolama hesabına kopyalayamazsınız. Bu dosyayı AzCopy veya diğer kopyalama mekanizmalarını kullanarak yapmanız gerekir.

Paylaşılan anlık görüntü özelliği dosya paylaşma düzeyinde sağlanır. Tek tek dosyaları geri yüklemeye izin vermek için tek tek dosya düzeyinde alma sağlanır. SMB, REST API, Portal, istemci kitaplığı veya PowerShell/CLı araçları kullanarak bir dosya paylaşımının tamamını geri yükleyebilirsiniz.

Bir dosya paylaşımının paylaşma anlık görüntüsü, temel dosya paylaşımıyla aynıdır. Tek fark, paylaşma anlık görüntüsünün alındığı saati göstermek için Share URI 'sine bir **DateTime** değeri eklenmeiydi. Örneğin, bir dosya paylaşımının URI 'SI http: \/ /storagesample.Core.File.Windows.net/myshare ise, paylaşılan anlık görüntü URI 'si şuna benzerdir:
```
http://storagesample.core.file.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z
```

Ortak olarak silinene kadar paylaşma anlık görüntüleri korunur. Paylaşılan anlık görüntü, temel dosya paylaşımının içeriğini aşamaz. Geçerli anlık görüntülerinizi izlemek için taban dosya paylaşımıyla ilişkili anlık görüntüleri numaralandırabilirsiniz. 

Bir dosya paylaşımının paylaşma anlık görüntüsünü oluşturduğunuzda, paylaşımın sistem özelliklerindeki dosyaların aynı değerlerle paylaşma anlık görüntüsüne kopyalanması gerekir. Temel dosyalar ve dosya paylaşımının meta verileri, oluşturduğunuz zaman paylaşma anlık görüntüsü için ayrı meta veriler belirtmediğiniz takdirde, paylaşma anlık görüntüsüne de kopyalanır.

Önce tüm paylaşma anlık görüntülerini silmediğiniz takdirde, paylaşılan anlık görüntülere sahip bir paylaşıma silemezsiniz.

## <a name="space-usage"></a>Alan kullanımı

Paylaşılan anlık görüntü, doğası halinde artımsal. Yalnızca en son paylaşma anlık görüntülerinizin ardından değiştirilen veriler kaydedilir. Bu, paylaşma anlık görüntüsünü oluşturmak için gereken süreyi en aza indirir ve depolama maliyetlerine kaydeder. Nesne veya özellik veya meta veri güncelleştirme işlemi için herhangi bir yazma işlemi, "değiştirilen içerik" öğesine doğru sayılır ve paylaşma anlık görüntüsünde saklanır. 

Alanı korumak için, karmaşıklığın en yüksek olduğu süre için paylaşma anlık görüntüsünü silebilirsiniz.

Paylaşma anlık görüntüleri artımlı olarak kaydedilse bile, paylaşımın geri yüklenmesi için yalnızca en son paylaşma anlık görüntüsünü saklamanız gerekir. Bir paylaşma anlık görüntüsünü sildiğinizde, yalnızca bu paylaşılan anlık görüntüye özgü veriler kaldırılır. Etkin anlık görüntüler, verilerinizi (paylaşma anlık görüntüsünün alındığı zamandan) orijinal konuma veya alternatif bir konuma gözatmanıza ve geri yüklemenize gerek duyduğunuz tüm bilgileri içerir. Öğe düzeyinde geri yükleme yapabilirsiniz.

Anlık görüntüler 5 TB 'lik paylaşma sınırınıza doğru sayılmaz. Toplamda ne kadar alan paylaşma anlık görüntüsü kaplayacağı bir sınır yoktur. Depolama hesabı sınırları hala geçerlidir.

## <a name="limits"></a>Sınırlar

Azure dosyalarının bugün izin verdiği en fazla paylaşılan anlık görüntü sayısı 200 ' dir. 200 anlık görüntüleri paylaştıktan sonra, yenilerini oluşturmak için eski paylaşılan anlık görüntüleri silmeniz gerekir. 

Paylaşılan anlık görüntü oluşturmaya yönelik eşzamanlı çağrılara yönelik bir sınır yoktur. Belirli bir dosya paylaşımının anlık görüntülerini paylaşan alan miktarına yönelik bir sınır yoktur. 

Bugün, Linux üzerinde paylaşma anlık görüntülerini bağlamak mümkün değildir. Bunun nedeni, Linux SMB istemcisinin Windows gibi anlık görüntüleri bağlamayı desteklemeleridir.

## <a name="copying-data-back-to-a-share-from-share-snapshot"></a>Paylaşım anlık görüntüsünden paylaşıma veri kopyalama

Dosyalar ve paylaşım anlık görüntüleri ile yapılan kopyalama işlemleri şu kurallara tabidir:

Bir dosya paylaşımı anlık görüntüsündeki dosyalardan herhangi birini temel paylaşıma veya başka bir konuma kopyalayabilirsiniz. Paylaşım anlık görüntüsündeki dosyaları tek tek kopyalayarak bir dosyanın eski bir sürümünü geri yükleyebilir veya dosya paylaşımının tamamını geri yükleyebilirsiniz. Paylaşım anlık görüntüsü, temel paylaşım olacak şekilde yükseltilmez. 

Kopyalama işlemi sonrasında paylaşım anlık görüntüsü aynı şekilde kalır ancak paylaşım anlık görüntüsündeki verilerin bir kopyası temel dosya paylaşımının üzerine yazılır. Geri yüklenen tüm dosyalar "değiştirilen içerik" olarak kabul edilir.

Bir paylaşım anlık görüntüsündeki dosyalardan birini farklı bir adla farklı bir konuma kopyalayabilirsiniz. Sonuçta elde edilen hedef dosya, paylaşım anlık görüntüsü değil yazılabilir bir dosya olur. Bu durumda temel dosya paylaşımınız etkilenmez.

Kopyalanan dosya ile hedef dosyanın üzerine yazıldığında özgün hedef dosyayla ilişkilendirilmiş olan paylaşım anlık görüntüleri etkilenmez.

## <a name="general-best-practices"></a>Genel en iyi uygulamalar

[Azure dosya paylaşma yedeklemesini](../../backup/azure-file-share-backup-overview.md) , anlık görüntüleri almayı otomatikleştirmek ve anlık görüntüleri yönetmek için bir yedekleme çözümü olarak kullanmanızı öneririz. Azure üzerinde altyapı çalıştırırken, mümkün olduğunda veri kurtarma yedeklemelerini otomatik hale getirin. Otomatik eylemler, el ile gerçekleştirilen işlemlerden daha güvenilirdir, veri korumayı ve kurtarılabilirliğe yardımcı olur. Azure dosya paylaşma yedeklemesini, REST API, Istemci SDK 'sını veya Otomasyon için komut dosyasını kullanabilirsiniz.

Paylaşma anlık görüntüsü Zamanlayıcı 'yı dağıtmadan önce, paylaşılan anlık görüntü sıklığınızı ve bekletme ayarlarını dikkatle gözden geçirin ve gereksiz ücretleri kullanmaktan kaçının.

Paylaşılan anlık görüntüler yalnızca dosya düzeyinde koruma sağlar. Paylaşılan anlık görüntüler bir dosya paylaşımında veya depolama hesabında FAT-parmak silme işlemlerini engellemez. Bir depolama hesabının yanlışlıkla silinmekten korunmasını sağlamak için, [geçici silme özelliğini etkinleştirebilir](storage-files-prevent-file-share-deletion.md)veya depolama hesabını ve/veya kaynak grubunu kilitleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Paylaşma anlık görüntüleriyle çalışma:
    - [Azure dosya paylaşma yedeklemesi](../../backup/azure-file-share-backup-overview.md)
    - [PowerShell](storage-how-to-use-files-powershell.md)
    - [CLI](storage-how-to-use-files-cli.md)
    - [Windows](storage-how-to-use-files-windows.md#accessing-share-snapshots-from-windows)
    - [Anlık görüntü paylaşma SSS](storage-files-faq.md#share-snapshots)