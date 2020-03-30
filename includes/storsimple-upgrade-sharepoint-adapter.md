---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: b2dec95e0258933b50d4437f1cb317639b62883d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188414"
---
### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>SharePoint 2010'u SharePoint 2013'e yükseltin ve ardından SharePoint için StorSomple Adaptörü'ne yükleyin
> [!IMPORTANT]
> Daha önce RBS aracılığıyla harici depolama ya taşınmış olan dosyalar, yükseltme tamamlanana ve RbS özelliği yeniden etkinleştirilene kadar kullanılamaz. Kullanıcı etkisini sınırlamak için, planlanan bakım penceresi sırasında herhangi bir yükseltme veya yeniden yükleme gerçekleştirin.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>SharePoint 2010'u SharePoint 2013'e yükseltmek ve ardından bağdaştırıcıyı yüklemek için
1. SharePoint 2010 çiftliğinde, dışa uygun BLOB'lar ve RBS'nin etkin olduğu içerik veritabanları için BLOB mağaza yoluna dikkat edin. 
2. Yeni SharePoint 2013 çiftliğini yükleyin ve yapılandırın. 
3. Veritabanlarını, uygulamaları ve site koleksiyonlarını SharePoint 2010 çiftliğinden yeni SharePoint 2013 çiftliğine taşıyın. Talimatlar [için SharePoint 2013 yükseltme işlemine genel bakış bölümüne](https://technet.microsoft.com/library/cc262483.aspx)gidin.
4. Yeni çiftliğe SharePoint için StorSimple Adaptör'üne yükleyin. Yordamlar [için SharePoint için StorSimple Adaptör'ünü yükleyin.](#install-the-storsimple-adapter-for-sharepoint)
5. Adım 1'de belirttiğiniz bilgileri kullanarak, aynı içerik veritabanları kümesi için KÇY'yi etkinleştirin ve SharePoint 2010 yüklemesinde kullanılan blob mağaza yolunu sağlayın. Yordamlar için [RbS'yi Yapılandır'a](#configure-rbs) gidin. Bu adımı tamamladıktan sonra, daha önce dışa dışlanmış dosyalara yeni çiftlikten erişilebilir olmalıdır. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Adaptörü yükseltin
> [!IMPORTANT]
> Bu yükseltmeyi, aşağıdaki nedenlerle planlanan bir bakım penceresinde oluşacak şekilde zamanlamanız gerekir:
> 
> * Daha önce dışa uyaran yeniden yüklenene kadar haricileştirilmiş içerik kullanılamaz.
> * SharePoint için StorSimple Adaptörü'nün önceki sürümünü yükledikten sonra siteye yüklenen ancak yeni sürümü yüklemeden önce siteye yüklenen tüm içerikler içerik veritabanında depolanır. Yeni bağdaştırıcıyı yükledikten sonra bu içeriği StorSimple aygıtına taşımanız gerekir. İçeriği geçirmek için `RBS Migrate()` SharePoint ile birlikte verilen Microsoft PowerShell cmdlet'ini kullanabilirsiniz. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/ff628255.aspx) 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Adaptör'u yükseltmek için
1. SharePoint için StorSimple Adaptörü'nün önceki sürümünü kaldırın.
   
   > [!NOTE]
   > Bu, içerik veritabanlarındaki KÇY'yi otomatik olarak devre dışı katır. Ancak, varolan BLOB'lar StorSimple aygıtında kalır. RbS devre dışı bırakıldığından ve BLOB'lar içerik veritabanlarına geri geçirilemedığından, bu BLOB'lar için yapılan tüm istekler başarısız olur. 
   > 
   > 
2. SharePoint için yeni StorSimple Adaptör'üne yükleyin. Yeni bağdaştırıcı, daha önce Etkinleştirilen veya Devre dışı bırakılan içerik veritabanlarını otomatik olarak tanır ve önceki ayarları kullanır.

