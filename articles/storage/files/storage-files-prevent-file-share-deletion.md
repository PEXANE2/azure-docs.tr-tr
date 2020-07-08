---
title: Yanlışlıkla silmeyi engelle-Azure dosya paylaşımları
description: Azure dosya paylaşımları için geçici silme ve verileri kurtarma için nasıl kullanabileceğinizi ve yanlışlıkla silmeyi engellemeyi öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: 6ee38dd6f9a2e254c57d6f79c09eee7bccfcd0aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84204693"
---
# <a name="prevent-accidental-deletion-of-azure-file-shares"></a>Azure dosya paylaşımlarının yanlışlıkla silinmesini engelle

Azure depolama, artık dosya paylaşımları (Önizleme) için geçici silme olanağı sunuyor. Geçici silme, bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla silindiğinde verilerinizi kurtarmanıza olanak sağlar.

## <a name="how-soft-delete-preview-works"></a>Geçici silme (Önizleme) nasıl işe yarar?

Azure dosya paylaşımları için geçici silme etkinleştirildiğinde, bir dosya paylaşımı silinirse, kalıcı olarak silinmeyeceği yerine geçici olarak silinen bir duruma geçer. Geçici olarak silinen verilerin kalıcı olarak silinmeden önce kurtarılabilir olduğu süreyi yapılandırabilirsiniz.

Geçici silme, yeni ya da varolan dosya paylaşımlarında etkinleştirilebilir. Geçici silme Ayrıca geriye dönük olarak uyumludur; Bu nedenle, geçici silme korumalarının avantajlarından yararlanmak için uygulamalarınızda herhangi bir değişiklik yapmanız gerekmez. 

Geçici silme durumundaki bir dosya paylaşımının bitiş zamanından önce kalıcı olarak silinmesi için, paylaşımdan silmeyi geri almanız, geçici silme işlemini devre dışı bırakmanız ve ardından yeniden silmeniz gerekir. Daha sonra, bu depolama hesabındaki diğer tüm dosya paylaşımları, geçici silme kapalıyken yanlışlıkla silinmeye karşı savunmasız olduğundan, geçici silme işlemini yeniden etkinleştirmeniz gerekir.

Geçici olarak silinen Premium dosya paylaşımları için dosya paylaşım kotası (bir dosya paylaşımının sağlanan boyutu), paylaşım tamamen silindiğinde, geçici olarak silinen paylaşımın sona erme tarihine kadar toplam depolama hesabı kota hesaplamasında kullanılır.

## <a name="availability"></a>Kullanılabilirlik

Azure dosya paylaşımları için geçici silme (Önizleme), tüm depolama katmanlarında, tüm depolama hesabı türlerinde ve Azure dosyalarının kullanılabildiği her bölgede kullanılabilir.

## <a name="configuration-settings"></a>Yapılandırma ayarları

### <a name="enabling-or-disabling-soft-delete"></a>Geçici silme etkinleştiriliyor veya devre dışı bırakılıyor

Dosya paylaşımları için geçici silme, depolama hesabı düzeyinde etkinleştirilir, bu nedenle, geçici silme ayarları bir depolama hesabındaki tüm dosya paylaşımları için geçerlidir. Dilediğiniz zaman geçici silme özelliğini etkinleştirebilir veya devre dışı bırakabilirsiniz. Yeni bir depolama hesabı oluşturduğunuzda, dosya paylaşımları için geçici silme varsayılan olarak devre dışıdır. Geçici silme Ayrıca, mevcut depolama hesapları için varsayılan olarak devre dışıdır. Azure dosya paylaşımı için Azure dosya [paylaşımı yedeklemesini](../../backup/azure-file-share-backup-overview.md) yapılandırdıysanız, bu paylaşımın depolama hesabında Azure dosya paylaşımları için geçici silme özelliği otomatik olarak etkinleştirilir.

Dosya paylaşımları için geçici silmeyi etkinleştirirseniz, bazı dosya paylaşımlarını silin ve ardından geçici silme devre dışı bırakın. paylaşımlar bu döneme kaydedildiyse, bu dosya paylaşımlarını erişebilmeye ve kurtarmaya devam edebilirsiniz. Geçici silme özelliğini etkinleştirdiğinizde, saklama süresini de yapılandırmanız gerekir.

### <a name="retention-period"></a>Bekletme süresi

Saklama süresi, geçici olarak silinen dosya paylaşımlarının saklanacağı ve kurtarma için kullanılabildiği süredir. Açıkça silinen dosya paylaşımları için, veri silindiğinde Bekletme dönemi saati başlar. Şu anda 1 ila 365 gün arasında bir saklama süresi belirtebilirsiniz. Geçici silme bekletme süresini istediğiniz zaman değiştirebilirsiniz. Güncelleştirilmiş bir bekletme dönemi yalnızca, saklama dönemi güncelleştirildikten sonra silinen paylaşımlar için geçerlidir. Saklama süresi güncelleştirmesi öncesinde silinen paylaşımlar, verilerin silindiği zaman yapılandırılan bekletme dönemine göre sona erer.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Hem standart hem de Premium dosya paylaşımları, sağlanan kapasite yerine geçici silme sırasında kullanılan kapasiteye göre faturalandırılır. Ayrıca, Premium dosya paylaşımları, geçici silme durumundayken anlık görüntü hızında faturalandırılır. Standart dosya paylaşımları, geçici silme durumundayken normal fiyat üzerinden faturalandırılır. Yapılandırılan saklama süresinden sonra kalıcı olarak silinen veriler için ücretlendirilmezsiniz.

Azure dosya depolama için genel olarak fiyatlar hakkında daha fazla bilgi için bkz. [Azure Dosya Depolama fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

Geçici silme özelliğini ilk kez etkinleştirdiğinizde, özelliğin faturanızı nasıl etkilediğini daha iyi anlamak için küçük bir bekletme süresi kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Geçici silme özelliğini etkinleştirme ve kullanma hakkında bilgi edinmek için, [geçici silme özelliğini etkinleştirmeye](storage-files-enable-soft-delete.md)devam edin.
