---
title: 'Öğretici: & Alma verileri kabul etme-Azure veri paylaşımının önizlemesi'
description: Öğretici-Azure veri paylaşma önizlemesi kullanarak verileri kabul edin ve alın
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 235ef25b2d655c4388dee5bdcf88d179f3373697
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327390"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share-preview"></a>Öğretici: Azure veri paylaşma önizlemesi 'ni kullanarak verileri kabul etme ve alma

Bu öğreticide, Azure veri paylaşma önizlemesi kullanarak bir veri paylaşımının davetini kabul etme hakkında bilgi edineceksiniz. Sizinle paylaşılan verilerin nasıl alınacağını ve her zaman sizinle paylaşılan verilerin en son anlık görüntüsüne sahip olduğunuzdan emin olmak için düzenli yenileme aralığını nasıl etkinleştireceğinizi öğreneceksiniz. 

> [!div class="checklist"]
> * Azure veri paylaşımının önizleme davetini kabul etme
> * Azure veri paylaşma önizleme hesabı oluşturma
> * Verileriniz için bir hedef belirtin
> * Zamanlanmış yenileme için veri paylaşımınıza abonelik oluşturma

## <a name="prerequisites"></a>Önkoşullar
Bir veri paylaşma davetini kabul etmeden önce, aşağıda listelenen birkaç Azure kaynağı sağlamalısınız. 

Bir veri paylaşma davetini kabul etmeden önce tüm önkoşulların tümünün tamamlandığından emin olun. 

* Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure depolama hesabı: Henüz bir tane yoksa, bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)oluşturabilirsiniz. 
* Veri paylaşma daveti: " **@No__t-1**' den Azure Data Share davetiyesi" başlıklı konuyla Microsoft Azure bir davetiye.
* *Microsoft. Authorization/role atamalar/Write* izninde bulunan depolama hesabına rol ataması ekleme izni. Bu izin sahip rolünde bulunur. 
* Microsoft. DataShare için kaynak sağlayıcısı kaydı. Bunun nasıl yapılacağı hakkında bilgi için bkz. [Azure kaynak sağlayıcıları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) belgeleri. 

> [!IMPORTANT]
> Bir Azure veri paylaşımının kabul edileceği ve alınacağı, önce Microsoft. DataShare kaynak sağlayıcısını kaydetmeniz ve verileri kabul ettiğiniz depolama hesabının sahibi olmanız gerekir. Veri paylaşımının kaynak sağlayıcısını kaydetmek için [Azure veri paylaşımının önizlemesine sorun giderme](data-share-troubleshoot.md) ve depolama hesabının sahibi olarak kendiniz ekleme konularında belgelenen yönergeleri izleyin. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="open-invitation"></a>Daveti aç

Veri sağlayıcınızdan bir davet için gelen kutunuzu denetleyin. Davet, **<yourdataprovider@domain.com> ' den Azure veri paylaşma daveti**başlıklı Microsoft Azure. Birden çok davetiye varsa doğru paylaşımın kabul edildiğinizden emin olmak için, paylaşılan adı ' nı bir yere göz atın. 

Davetinizi Azure 'da görmek için **daveti görüntüle** ' yi seçin. Bu sizi aldığınız paylaşımlar görünümüne götürür.

![Davetiye davetlerinin](./media/invitations.png "listesi") 

Görüntülemek istediğiniz paylaşma seçeneğini belirleyin. 

## <a name="accept-invitation"></a>Daveti kabul et
**Kullanım koşulları**dahil olmak üzere tüm alanların incelendiğini doğrulayın. Kullanım koşullarını kabul ediyorsanız, kabul ettiğinizi belirtmek için kutuyu denetlemeniz gerekir. 

![Kullanım koşulları](./media/terms-of-use.png "kullanım koşulları") 

*Hedef veri paylaşma hesabı*altında, veri paylaşımınızı dağıtmak istediğiniz aboneliği ve kaynak grubunu seçin. 

**Veri** paylaşma hesabı alanı için, mevcut bir veri paylaşma hesabınız yoksa **Yeni oluştur** ' u seçin. Aksi takdirde, veri paylaşımınızı kabul etmek istediğiniz mevcut bir veri paylaşma hesabı seçin. 

*Alınan paylaşma adı* alanı Için, veriler tarafından belirtilen varsayılanı bırakabilir veya alınan paylaşıma yönelik yeni bir ad belirtebilirsiniz. 

![Hedef veri paylaşma hesabı](./media/target-data-share.png "hedef veri paylaşma hesabı") 

Kullanım koşullarını kabul ettikten ve paylaşımınız için bir konum belirttikten sonra *kabul et ve Yapılandır*' ı seçin. Bu seçeneği belirlerseniz, bir paylaşılan abonelik oluşturulur ve bir sonraki ekranda, verilerinize kopyalanması için bir hedef depolama hesabı seçmeniz istenir. 

![Kabul etme seçenekleri](./media/accept-options.png "kabul seçenekleri") 

Şimdi daveti kabul etmek, ancak depolama alanınızı daha sonra yapılandırmak isterseniz, *kabul et ' i ve daha sonra Yapılandır*' ı seçin. Bu seçenek, hedef depolama hesabınızı daha sonra yapılandırmanıza olanak tanır. Depolama alanınızı daha sonra yapılandırmaya devam etmek için, bkz. [depolama hesabınızı yapılandırma](how-to-configure-mapping.md) sayfası, veri paylaşma yapılandırmanızın nasıl sürdürüleceği hakkında ayrıntılı adımlar için. 

Daveti kabul etmek istemiyorsanız *Reddet*' i seçin. 

## <a name="configure-storage"></a>Depolama alanını yapılandırma
*Hedef depolama ayarları*altında, verilerinizi almak istediğiniz aboneliği, kaynak grubunu ve depolama hesabını seçin. 

![Hedef depolama ayarları](./media/target-storage-settings.png "hedef depolama alanı") 

Verilerinizin düzenli olarak yenilenmesini almak için, anlık görüntü ayarlarını etkinleştirdiğinizden emin olun. Veri sağlayıcınız onu veri paylaşımında içerdiğine yalnızca bir anlık görüntü ayarı görürsünüz. 

![Anlık görüntü ayarları](./media/snapshot-settings.png "anlık görüntü ayarları") 

*Kaydet*’i seçin. 

## <a name="trigger-a-snapshot"></a>Anlık görüntü tetikleyin

Alınan paylaşımlar-> Ayrıntılar sekmesinde bir anlık görüntüyü **tetikleyici anlık görüntüsünü**seçerek tetikleyebilirsiniz. Burada, verilerinizin tam veya artımlı anlık görüntüsünü tetikleyebilirsiniz. Veri sağlayıcınızdan ilk kez veri alıyorsanız tam kopya ' ı seçin. 

![Anlık görüntü](./media/trigger-snapshot.png "tetikleyici anlık görüntüsünü") tetikleme 

Son çalıştırma durumu *başarılı*olduğunda, alınan verileri görüntülemek için depolama hesabını açın. 

Kullandığınız depolama hesabını denetlemek için **veri kümeleri**' ni seçin. 

![Tüketici veri kümeleri](./media/consumer-datasets.png "Tüketici veri kümesi eşleme") 

## <a name="view-history"></a>Geçmişi görüntüleme
Anlık görüntülerinizin geçmişini görüntülemek için, alınan paylaşımlar-> geçmişi ' ne gidin. Burada, son 60 gün için oluşturulan tüm anlık görüntülerin geçmişini bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir Azure veri paylaşımının nasıl kabul edileceği ve alınacağı ile karşılaşacağınızı öğrenirsiniz. Azure veri paylaşma kavramları hakkında daha fazla bilgi edinmek için [Concepts ' a geçin: Azure veri paylaşma terminolojisi @ no__t-0.