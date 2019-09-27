---
title: 'Öğretici: Kuruluşunuz dışında paylaşma-Azure veri paylaşımının önizlemesi'
description: Öğretici-Azure veri paylaşma önizlemesi kullanarak müşteriler ve iş ortakları ile veri paylaşma
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: f7df46a6a6f149ef0228fda8c967469a25dc3d50
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327410"
---
# <a name="tutorial-share-your-data-using-azure-data-share-preview"></a>Öğretici: Azure veri paylaşımının önizlemesini kullanarak verilerinizi paylaşma

Bu öğreticide, yeni bir Azure veri paylaşımı ayarlamayı ve verilerinizi Azure kuruluşunuzun dışındaki müşteriler ve iş ortaklarıyla paylaşmaya nasıl başlayacağınızı öğreneceksiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Veri paylaşma oluşturun.
> * Veri paylaşımınıza veri kümeleri ekleyin.
> * Veri paylaşımınız için bir eşitleme zamanlaması etkinleştirin. 
> * Veri paylaşımınıza alıcı ekleyin. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure depolama hesabı: Henüz bir tane yoksa, bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) oluşturabilirsiniz
* *Microsoft. Authorization/role atamalar/Write* izninde bulunan depolama hesabına rol ataması ekleme izni. Bu izin sahip rolünde bulunur. 
* Alıcılarınızın Azure oturum açma e-posta adresi (e-posta diğer adlarını kullanarak çalışmaz).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-data-share-account"></a>Veri paylaşma hesabı oluşturma

Azure Kaynak grubunda bir Azure veri paylaşma kaynağı oluşturun.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

1. *Veri paylaşımında*arama yapın.

1. Veri paylaşma (Önizleme) öğesini seçin ve **Oluştur**' u seçin.

1. Aşağıdaki bilgilerle Azure veri paylaşma kaynağınızın temel ayrıntılarını doldurun. 

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Name | *datashareacount* | Veri paylaşma hesabınız için bir ad belirtin. |
    | Subscription | Aboneliğiniz | Veri paylaşma hesabınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Resource group | *test-resource-group* | Mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Location | *Doğu ABD 2* | Veri paylaşma hesabınız için bir bölge seçin.
    | | |

1. Veri paylaşma hesabınızı sağlamak için **Oluştur** ' u seçin. Yeni bir veri paylaşma hesabının sağlanması genellikle yaklaşık 2 dakika veya daha kısa sürer. 

1. Dağıtım tamamlandığında **Kaynağa Git**' i seçin.

## <a name="create-a-data-share"></a>Veri paylaşma oluşturma

1. Veri paylaşımında genel bakış sayfasına gidin.

    ![](./media/share-receive-data.png "Verilerinizi") paylaşma 

1. **Verilerinizi paylaşmayı Başlat**' ı seçin.

1. **Oluştur**’u seçin.   

1. Veri paylaşımınızın ayrıntılarını doldurun. Bir ad, içerik paylaşma açıklaması ve kullanım koşulları (isteğe bağlı) belirtin. 

    ![Entersharedetails](./media/enter-share-details.png "Paylaşım ayrıntılarını girin") 

1. **Devam et** 'i seçin

1. Veri paylaşımınıza veri kümeleri eklemek için veri **kümesi Ekle**' yi seçin. 

    ![Veri kümeleri](./media/datasets.png "veri kümeleri")

1. Eklemek istediğiniz veri kümesi türünü seçin. 

    ![Adddataset](./media/add-datasets.png "veri kümesi Ekle")    

1. Paylaşmak istediğiniz nesneye gidin ve ' veri kümesi Ekle ' seçeneğini belirleyin. 

    ![Selectdataset](./media/select-datasets.png "veri kümelerini seçme")    

1. Alıcılar sekmesinde, ' + Alıcı Ekle ' seçeneğini belirleyerek veri tüketicinizin e-posta adreslerini girin. 

    ![Addrecipients](./media/add-recipient.png "Alıcı Ekle") 

1. **Devam et** 'i seçin

1. Veri tüketicinizin verilerinizin artımlı güncelleştirmelerini almasını istiyorsanız, anlık görüntü zamanlamasını etkinleştirin. 

    ![Enablesnapshots](./media/enable-snapshots.png "anlık görüntüleri etkinleştir") 

1. Bir başlangıç saati ve yinelenme aralığı seçin. 

1. **Devam et** 'i seçin

1. Gözden geçir + Oluştur sekmesinde paket Içeriklerinizi, ayarlarınızı, alıcılarını ve eşitleme ayarlarınızı gözden geçirin. **Oluştur**’u seçin

Azure veri paylaşımınız artık oluşturulmuştur ve veri paylaşımınızın alıcısı artık davetinizi kabul etmeye hazırdır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure veri paylaşımının nasıl oluşturulacağını ve alıcıların nasıl davet alınacağını öğrenirsiniz. Bir veri tüketicisinin bir veri paylaşımının kabul edip alabileceği hakkında bilgi edinmek için, [verileri kabul etme ve alma](subscribe-to-data-share.md) öğreticisine geçin. 
