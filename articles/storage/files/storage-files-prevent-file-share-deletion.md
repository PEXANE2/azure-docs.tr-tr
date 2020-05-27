---
title: Yanlışlıkla silmeyi engelle-Azure dosya paylaşımları
description: Azure dosya paylaşımları için geçici silme ve verileri kurtarma için nasıl kullanabileceğinizi ve yanlışlıkla silmeyi engellemeyi öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: rogarana
ms.subservice: files
services: storage
ms.openlocfilehash: 96e3d5001d11455337ae092776a1a4c5c3738012
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83883075"
---
# <a name="prevent-accidental-deletion-of-azure-file-shares"></a>Azure dosya paylaşımlarının yanlışlıkla silinmesini engelle

Azure depolama, artık dosya paylaşımları için geçici silme olanağı sunmaktadır. Geçici silme, bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla silindiğinde verilerinizi kurtarmanıza olanak sağlar.

## <a name="how-soft-delete-works"></a>Geçici silmenin çalışması

Etkinleştirildiğinde, geçici silme, dosya paylaşımlarınızı silindiklerinde kaydetmenizi ve kurtarmanızı sağlar. Veriler silindiğinde, kalıcı olarak silinmesi yerine geçici olarak silinen bir duruma geçer. Geçici olarak silinen verilerin kalıcı olarak silinmeden önce kurtarılabilir olduğu süreyi yapılandırabilirsiniz.

Geçici silme, yeni veya mevcut dosya paylaşımlarında etkinleştirilebilir. Geçici silme Ayrıca geriye dönük olarak uyumludur; Bu nedenle, geçici silme korumalarının avantajlarından yararlanmak için uygulamalarınızda herhangi bir değişiklik yapmanız gerekmez. 

Geçici olarak silinen Premium dosya paylaşımları için dosya paylaşım kotası (bir dosya paylaşımının sağlanan boyutu), paylaşım tamamen silindiğinde, geçici olarak silinen paylaşımın sona erme tarihine kadar toplam depolama hesabı kota hesaplamasında kullanılır.

### <a name="availability"></a>Kullanılabilirlik

Azure dosya paylaşımları için geçici silme, tüm depolama katmanlarında, tüm depolama hesabı türlerinde ve Azure dosyalarının kullanılabildiği her bölgede kullanılabilir.

## <a name="configuration-settings"></a>Yapılandırma ayarları

Dosya paylaşımları için geçici silme, depolama hesabı düzeyinde etkinleştirilir, geçici silme ayarları bir depolama hesabındaki tüm dosya paylaşımları için geçerlidir. Yeni bir depolama hesabı oluşturduğunuzda, geçici silme varsayılan olarak kapalıdır. Geçici silme, var olan depolama hesapları için de varsayılan olarak kapalıdır. Dilediğiniz zaman geçici silme özelliğini açıp kapatabilirsiniz.

Dosya paylaşımları için geçici silmeyi etkinleştirirseniz, bazı dosya paylaşımlarını silin ve ardından geçici silme devre dışı bırakın. paylaşımlar bu döneme kaydedildiyse, bu dosya paylaşımlarını erişebilmeye ve kurtarmaya devam edebilirsiniz. Geçici silme özelliğini etkinleştirdiğinizde, saklama süresini de yapılandırmanız gerekir.

Saklama süresi, geçici olarak silinen dosya paylaşımlarının kurtarılmasına ve kurtarma için kullanılabilir olduğu süreyi gösterir. Açıkça silinen dosya paylaşımları için, veri silindiğinde Bekletme dönemi saati başlar. Şu anda, 1 ila 365 gün arasında geçici olarak silinen paylaşımlar tutabilirsiniz.

Geçici silme bekletme süresini istediğiniz zaman değiştirebilirsiniz. Güncelleştirilmiş bir bekletme dönemi yalnızca, saklama dönemi güncelleştirildikten sonra silinen paylaşımlar için geçerlidir. Saklama süresi güncelleştirmesi öncesinde silinen paylaşımlar, verilerin silindiği zaman yapılandırılan bekletme dönemine göre sona erer.

Geçici silme durumundaki bir dosya paylaşımının süre sonu zamanından önce kalıcı olarak silinmesi için, paylaşımdan silmeyi geri almanız, geçici silme işlemini devre dışı bırakmanız ve ardından yeniden silmeniz gerekir. Daha sonra, bu depolama hesabındaki diğer tüm dosya paylaşımları, geçici silme kapalıyken yanlışlıkla silinmeye karşı savunmasız olduğundan, geçici silme işlemini yeniden etkinleştirmeniz gerekir.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

Hem standart hem de Premium dosya paylaşımları, sağlanan kapasite yerine geçici silme sırasında kullanılan kapasiteye göre faturalandırılır. Ayrıca, Premium dosya paylaşımları, geçici silme durumundayken anlık görüntü hızında faturalandırılır. Standart dosya paylaşımları, geçici silme durumundayken normal fiyat üzerinden faturalandırılır. Yapılandırılan saklama süresinden sonra kalıcı olarak silinen veriler için ücretlendirilmezsiniz.

Azure dosya depolama için genel olarak fiyatlar hakkında daha fazla bilgi için bkz. [Azure Dosya Depolama fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

Geçici silme özelliğini ilk kez etkinleştirdiğinizde, özelliğin faturanızı nasıl etkilediğini daha iyi anlamak için küçük bir bekletme süresi kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Geçici silme özelliğini etkinleştirme ve kullanma hakkında bilgi edinmek için, [geçici silme özelliğini etkinleştirmeye](storage-files-enable-soft-delete.md) devam edin