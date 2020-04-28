---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: b2dec95e0258933b50d4437f1cb317639b62883d
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188414"
---
### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>SharePoint 2010 ' i SharePoint 2013 ' ye yükseltin ve ardından SharePoint için StorSomple bağdaştırıcısını yükledikten sonra
> [!IMPORTANT]
> Daha önce KÇY aracılığıyla dış depolamaya taşınan dosyalar, yükseltme tamamlanana ve KÇY özelliği yeniden etkinleştirilene kadar kullanılamaz. Kullanıcı etkisini sınırlandırmak için, planlanan bir bakım penceresi sırasında herhangi bir yükseltme veya yeniden yükleme gerçekleştirin.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>SharePoint 2010 ' i SharePoint 2013 ' e yükseltmek ve sonra bağdaştırıcıyı yüklemek için
1. SharePoint 2010 grubunda, externalized Blobların ve KÇY 'nin etkinleştirildiği içerik veritabanlarının BLOB depolama yolunu aklınızda bulunur. 
2. Yeni SharePoint 2013 grubunu yükleyip yapılandırın. 
3. Veritabanlarını, uygulamaları ve site koleksiyonlarını SharePoint 2010 grubundan yeni SharePoint 2013 grubuna taşıyın. Yönergeler için [SharePoint 2013 yükseltme Işlemine genel bakış](https://technet.microsoft.com/library/cc262483.aspx)bölümüne gidin.
4. Yeni gruba SharePoint için StorSimple bağdaştırıcısını yükler. Yordamlara yönelik [SharePoint Için StorSimple bağdaştırıcısını yüklemeye](#install-the-storsimple-adapter-for-sharepoint) gidin.
5. Adım 1 ' de not ettiğiniz bilgileri kullanarak, aynı içerik veritabanları kümesi için KÇY 'yi etkinleştirin ve SharePoint 2010 yüklemesinde kullanılan BLOB depolama yolunu sağlayın. Yordamlar için [KÇY 'Yi yapılandırma](#configure-rbs) bölümüne gidin. Bu adımı tamamladıktan sonra, daha önce externalized dosyaları yeni gruptan erişilebilir olmalıdır. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükseltme
> [!IMPORTANT]
> Aşağıdaki nedenlerden dolayı bu yükseltmenin planlı bir bakım penceresi sırasında gerçekleşmesini zamanlamanız gerekir:
> 
> * Daha önce externalized içerik, bağdaştırıcı yeniden yükleninceye kadar kullanılamayacak.
> * SharePoint için StorSimple bağdaştırıcısının önceki sürümünü kaldırdıktan sonra siteye yüklenen içerikler, ancak yeni sürümü yüklemeden önce içerik veritabanında depolanır. Yeni bağdaştırıcıyı yükledikten sonra bu içeriği StorSimple cihazına taşımanız gerekir. İçeriği geçirmek için SharePoint 'e `RBS Migrate()` dahil edilen Microsoft PowerShell cmdlet 'ini kullanabilirsiniz. Daha fazla bilgi için bkz. [KÇY içine veya dışına Içerik geçirme](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükseltmek için
1. SharePoint için StorSimple bağdaştırıcısının önceki sürümünü kaldırın.
   
   > [!NOTE]
   > Bu, içerik veritabanlarında KÇY 'yi otomatik olarak devre dışı bırakır. Ancak, mevcut Bloblar StorSimple cihazında kalır. KÇY devre dışı bırakıldığından ve Bloblar içerik veritabanlarına geri geçirilmediğinden, bu bloblara yönelik istekler başarısız olur. 
   > 
   > 
2. SharePoint için yeni StorSimple bağdaştırıcısını yükler. Yeni bağdaştırıcı, önceden etkinleştirilen veya KÇY için devre dışı bırakılan içerik veritabanlarını otomatik olarak tanır ve önceki ayarları kullanacaktır.

