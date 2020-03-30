---
title: Azure Dosyaları için paylaşım anlık görüntülerine genel bakış | Microsoft Dokümanlar
description: Paylaşım anlık görüntüsü, paylaşımı yedeklemenin bir yolu olarak, bir noktada alınan Azure Dosyaları paylaşımının salt okunur sürümüdür.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: c05b79d2f1da8076b507ca9ee7a06504de21d5ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72333172"
---
# <a name="overview-of-share-snapshots-for-azure-files"></a>Azure Dosyaları için paylaşım anlık görüntülerine genel bakış 
Azure Files, dosya paylaşımlarının paylaşım anlık görüntülerini alma olanağı sağlar. Anlık görüntüleri paylaş, bu noktada paylaşım durumunu yakalar. Bu makalede, anlık görüntülerdeki özelliklerin neler sağladığını ve özel kullanım örneğinizde bunlardan nasıl yararlanabileceğinizi açıklıyoruz.

## <a name="when-to-use-share-snapshots"></a>Paylaşım anlık görüntülerini ne zaman kullanılır?

### <a name="protection-against-application-error-and-data-corruption"></a>Uygulama hatası ve veri bozulmasına karşı koruma
Dosya paylaşımlarını kullanan uygulamalar yazma, okuma, depolama, iletim ve işleme gibi işlemleri gerçekleştirir. Bir uygulama yanlış yapılandırılırsa veya kasıtsız bir hata tanıtılırsa, yanlışlıkla üzerine yazı yazmak veya birkaç blokta hasar meydana gelebilir. Bu senaryolara karşı korumaya yardımcı olmak için, yeni uygulama kodu dağıtmadan önce bir paylaşım anlık görüntüsü alabilirsiniz. Yeni dağıtımla birlikte bir hata veya uygulama hatası getirilirse, bu dosya paylaşımında verilerinizin önceki bir sürümüne geri dönebilirsiniz. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>Yanlışlıkla silmelere veya istenmeyen değişikliklere karşı koruma
Dosya paylaşımında bir metin dosyası üzerinde çalıştığınızı düşünün. Metin dosyası kapatıldıktan sonra, değişikliklerinizi geri alabilme yeteneğini kaybedersiniz. Bu gibi durumlarda, daha sonra dosyanın önceki bir sürümünü kurtarmak gerekir. Yanlışlıkla yeniden adlandırıldıveya silinmişse, dosyanın önceki sürümlerini kurtarmak için anlık görüntüleri paylaş'ı kullanabilirsiniz.

### <a name="general-backup-purposes"></a>Genel yedekleme amaçları
Bir dosya paylaşımı oluşturduktan sonra, veri yedekleme için kullanmak için dosya paylaşımının bir paylaşım anlık görüntüsünü düzenli aralıklarla oluşturabilirsiniz. Bir paylaşım anlık görüntüsü, düzenli olarak alındığında, gelecekteki denetim gereksinimleri veya olağanüstü durum kurtarma için kullanılabilecek verilerin önceki sürümlerini korumaya yardımcı olur.

## <a name="capabilities"></a>Özellikler
Paylaşım anlık görüntüsü, verilerinizin yalnızca okunan bir kopyasıdır. REST API'sini kullanarak anlık görüntüleri oluşturabilir, silebilir ve yönetebilirsiniz. Aynı özellikler istemci kitaplığında, Azure CLI'de ve Azure portalında da kullanılabilir. 

Hem REST API'sini hem de SMB'yi kullanarak bir paylaşımın anlık görüntülerini görüntüleyebilirsiniz. Dizinin veya dosyanın sürümlerinin listesini alabilir ve belirli bir sürümü doğrudan sürücü olarak monte edebilirsiniz (yalnızca Windows'da kullanılabilir - [Bkz. Sınırlar).](#limits) 

Paylaşım anlık görüntüsü oluşturulduktan sonra okunabilir, kopyalanabilir veya silinebilir, ancak değiştirilemez. Bir paylaşım anlık görüntüsünün tamamını başka bir depolama hesabına kopyalayamazsınız. AzCopy veya diğer kopyalama mekanizmaları kullanarak, dosya ile bu dosya yapmak zorunda.

Paylaşım anlık görüntüsü özelliği dosya paylaşımı düzeyinde sağlanır. Alma, tek tek dosyaların geri verilmesine olanak sağlamak için tek tek dosya düzeyinde sağlanır. SMB, REST API, portal, istemci kitaplığı veya PowerShell/CLI aracını kullanarak tam bir dosya paylaşımını geri yükleyebilirsiniz.

Bir dosya paylaşımının paylaşım anlık görüntüsü, temel dosya paylaşımıyla aynıdır. Tek fark, bir **DateTime** değerinin, hisse anlık görüntüsünün alındığı zamanı belirtmek için uri paylaşımına eklenmesidir. Örneğin, bir dosya paylaşımı http://storagesample.core.file.windows.net/myshareURI ise, hisse anlık URI benzer:
```
http://storagesample.core.file.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z
```

Anlık görüntüleri açıkça silinene kadar paylaşma anlık görüntüleri devam eder. Bir paylaşım anlık görüntüsü temel dosya paylaşımından daha uzun süre yaşayamaz. Geçerli anlık görüntülerinizi izlemek için temel dosya paylaşımıyla ilişkili anlık görüntüleri sayısala aktarabilirsiniz. 

Bir dosya paylaşımının paylaşım anlık görüntüsünü oluşturduğunuzda, paylaşımın sistem özelliklerindeki dosyalar aynı değerlerle paylaşım anlık görüntüsüne kopyalanır. Oluşturduğunuzda paylaşım anlık görüntüsü için ayrı meta veriler belirtmediğiniz sürece, temel dosyalar ve dosya paylaşımının meta verileri de paylaşım anlık görüntüsüne kopyalanır.

Önce tüm paylaşım anlık görüntülerini silmediğiniz sürece, paylaşım anlık görüntüleri olan bir paylaşımı silemezsiniz.

## <a name="space-usage"></a>Alan kullanımı 
Paylaşım anlık görüntüleri doğada artımlı. Yalnızca en son paylaşım anlık görüntünüzden sonra değişen veriler kaydedilir. Bu, hisse anlık görüntüsünü oluşturmak için gereken süreyi en aza indirir ve depolama maliyetlerinden tasarruf sağlar. Nesneye veya özellik veya meta veri güncelleştirme işlemine yapılan herhangi bir yazma işlemi "değiştirilen içerik" olarak sayılır ve paylaşım görüntüsünde depolanır. 

Alanı korumak için, karmaşanın en yüksek olduğu dönem için paylaşım anlık görüntüsünü silebilirsiniz.

Paylaşım anlık görüntüleri artımlı olarak kaydedilen olsa da, paylaşımı geri yüklemek için yalnızca en son paylaşım anlık görüntüsünü saklamanız gerekir. Bir paylaşım anlık görüntüsünü sildiğinizde, yalnızca bu paylaşım anlık görüntüsüne özgü veriler kaldırılır. Etkin anlık görüntüler, verilerinize göz atmanız ve geri yüklemeniz gereken tüm bilgileri (paylaşım anlık görüntüsünün alındığı andan itibaren) orijinal konuma veya alternatif bir konuma içerir. Öğe düzeyinde geri yükleyebilirsiniz.

Anlık görüntüler 5-TB paylaşım sınırınıza dahil değildir. Toplam da ne kadar alan payı anlık görüntüsünün yer kapsadığının sınırı yoktur. Depolama hesabı sınırları hala geçerlidir.

## <a name="limits"></a>Sınırlar
Azure Dosyaları'nın bugün izin verdiği maksimum paylaşım anlık görüntüsü sayısı 200'dür. 200 paylaşım anlık görüntüsünden sonra, yenilerini oluşturmak için eski paylaşım anlık görüntülerini silmeniz gerekir. 

Paylaşım anlık görüntüleri oluşturmak için eşzamanlı çağrıların sınırı yoktur. Belirli bir dosya paylaşımının anlık görüntülerini paylaşabileceği alan miktarı için bir sınır yoktur. 

Bugün, Linux üzerinde paylaşım anlık montaj mümkün değildir. Bunun nedeni, Linux SMB istemcisinin Windows gibi anlık montajı desteklememesidir.

## <a name="copying-data-back-to-a-share-from-share-snapshot"></a>Verileri paylaşım anlık görüntüsünden paylaşıma kopyalama
Dosyaları içeren kopyalama işlemleri ve anlık görüntüleri paylaşma aşağıdaki kurallara uyar:

Dosya paylaşımı anlık görüntüsündeki tek tek dosyaları temel paylaşımına veya başka bir konuma kopyalayabilirsiniz. Dosyanın önceki bir sürümünü geri yükleyebilir veya dosyayı dosyayla paylaşan anlık görüntüden kopyalayarak dosya paylaşımının tamamını geri yükleyebilirsiniz. Hisse anlık görüntüsü temel paylaşıma yükseltilmez. 

Paylaşım anlık görüntüsü kopyalandıktan sonra bozulmadan kalır, ancak temel dosya paylaşımı, paylaşım anlık görüntüsünde bulunan verilerin bir kopyasıyla üzerine yazılır. Geri yüklenen tüm dosyalar "değiştirilen içeriğe" doğru sayılır.

Bir dosyayı paylaşım anlık görüntüsünde farklı bir ada sahip farklı bir hedefe kopyalayabilirsiniz. Elde edilen hedef dosya, paylaşım anlık görüntüsü değil, yazılabilir bir dosyadır. Bu durumda, temel dosya paylaşımınız bozulmadan kalır.

Bir hedef dosya bir kopyayla üzerine yazıldığında, özgün hedef dosyasıyla ilişkili tüm paylaşım anlık görüntüleri bozulmadan kalır.

## <a name="general-best-practices"></a>Genel en iyi uygulamalar 
Azure'da altyapı çalıştırırken, mümkün olduğunca veri kurtarma için yedeklemeleri otomatikleştirin. Otomatikleştirilmiş eylemler manuel işlemlerden daha güvenilirdir ve veri koruma ve kurtarılabilirliği geliştirmeye yardımcı olur. REST API'yi, İstemci SDK'sını veya komut dosyasını otomasyon için kullanabilirsiniz.

Anlık görüntü tarifesini dağıtmadan önce, gereksiz ücretlere maruz kalmamak için paylaşım anlık görüntü sıklığınızı ve bekletme ayarlarınızı dikkatlice düşünün.

Anlık görüntüleri paylaşın yalnızca dosya düzeyinde koruma sağlar. Anlık görüntüleri paylaşma, dosya paylaşımı veya depolama hesabındayağ parmak silmelerini engellemez. Bir depolama hesabının yanlışlıkla silmelere karşı korunmasına yardımcı olmak için depolama hesabını veya kaynak grubunu kilitleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- İçindeki hisse anlık görüntüleriyle çalışma:
    - [Powershell](storage-how-to-use-files-powershell.md)
    - [CLI](storage-how-to-use-files-cli.md)
    - [Windows](storage-how-to-use-files-windows.md#accessing-share-snapshots-from-windows)
    - [Anlık görüntü SSS'yi paylaşın](storage-files-faq.md#share-snapshots)
