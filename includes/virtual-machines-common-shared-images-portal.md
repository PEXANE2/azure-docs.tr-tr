---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/06/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 729e757c69887bbdce324e2d8383c970995dc94a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73903662"
---
## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

> [!NOTE]
> Önizleme sırasında Paylaşılan Resim Galerilerini kullanmak için kaydolduysanız, sağlayıcıyı `Microsoft.Compute` yeniden kaydetmeniz gerekebilir. [Bulut Kabuğu'nu](https://shell.azure.com/bash) açın ve yazın:`az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Resim galerisi oluşturma

Resim galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harfler, basamaklar, noktalar ve dönemlerdir. Galeri adı tire içeremez.  Galeri adları aboneliğinizde benzersiz olmalıdır. 

Aşağıdaki örnek, *myGalleryRG* kaynak grubunda *myGallery* adlı bir galeri oluşturur.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Arama kutusunda **paylaşılan resim galerisi** türünü kullanın ve sonuçlarda Paylaşılan resim **galerisini** seçin.
1. Paylaşılan **resim galerisi** sayfasında **Oluştur'u**tıklatın.
1. Doğru aboneliği seçin.
1. **Kaynak grubunda,** **yeni oluştur'u** seçin ve ad için *myGalleryRG* yazın.
1. **Ad,** galeri adı için *myGallery* yazın.
1. **Bölge**için varsayılan bırakın.
1. Test *için resim galerisim* gibi galerinin kısa bir açıklamasını yazabilirsiniz. ve sonra **Gözden Geçir + oluştur'u**tıklatın.
1. Doğrulama geçtikten sonra **Oluştur'u**seçin.
1. Dağıtım tamamlandığında **kaynağa git'i**seçin.
   
## <a name="create-an-image-definition"></a>Görüntü tanımı oluşturma 

Görüntü tanımları görüntüler için mantıksal bir gruplandırma oluşturur. Bunlar, içlerinde oluşturulan görüntü sürümleri hakkındaki bilgileri yönetmek için kullanılır. Resim tanım adları büyük veya küçük harflerden, basamaklardan, noktalardan, tirelerden ve dönemlerden oluşur. Görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için [Resim tanımlarına](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions)bakın.

Galerinizin içinde galeri resim tanımıoluşturun. Bu örnekte, galeri resmi *myImageDefinition*olarak adlandırılır.

1. Yeni resim gasinizin sayfasında, sayfanın üst kısmından **yeni bir resim tanımı ekle'yi** seçin. 
1. **Resim tanım adı için** *myImageDefinition*yazın.
1. **İşletim sistemi**için, kaynağıvm dayalı doğru seçeneği seçin.
1. **VM üretimi için,** kaynağıvm'unuza göre seçeneği seçin. Çoğu durumda, bu *Gen 1*olacaktır. Daha fazla bilgi [için, nesil 2 VM'ler için Destek'e](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2)bakın.
1. **İşletim sistemi durumu**için, kaynağıvm'unuzu temel alan seçeneği seçin. Daha fazla bilgi için [Genelleştirilmiş ve özel](../articles/virtual-machines/linux/shared-image-galleries.md#generalized-and-specialized-images)olarak bkz.
1. **Publisher**için *myPublisher*yazın. 
1. **Teklif**için , *myOffer*yazın.
1. **SKU**için, *mySKU*yazın.
1. Tamamlandığında Gözden **Geçir + oluştur'u**seçin.
1. Görüntü tanımı doğrulamayı geçtikten sonra **Oluştur'u**seçin.
1. Dağıtım tamamlandığında **kaynağa git'i**seçin.


## <a name="create-an-image-version"></a>Resim sürümü oluşturma

Yönetilen bir görüntüden görüntü sürümü oluşturun. Bu örnekte, görüntü sürümü *1.0.0'dır* ve hem *Batı Orta ABD* hem de Güney Orta *ABD* veri merkezlerine çoğaltılır. Çoğaltma için hedef bölgeleri seçerken, *kaynak* bölgeyi çoğaltma hedefi olarak da eklemeniz gerektiğini unutmayın.

Görüntü sürümü için izin verilen karakterler sayılar ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Yama*.

Görüntü sürümü oluşturma adımları, kaynağın genelleştirilmiş bir görüntü mü yoksa özel leştirilmiş bir VM'nin anlık görüntüsü mü olduğuna bağlı olarak biraz farklıdır. 

### <a name="option-generalized"></a>Seçenek: Genelleştirilmiş

1. Resim tanımınız için sayfada, sayfanın üst kısmından **sürüm ekle'yi** seçin.
1. **Bölge'de,** yönetilen görüntünüzün depolandığı bölgeyi seçin. Görüntü sürümleri, oluşturuldukları yönetilen görüntüyle aynı bölgede oluşturulmalıdır.
1. **Ad**için , yazın *1.0.0*. Resim sürüm adı *büyük*takip etmelidir. *küçük*. yama formatında, *tümseler* kullanılarak. 
1. **Kaynak görüntüde,** açılan görüntüden kaynak yönetilen görüntünüzseçin.
1. **En son dan Dışla,** *No*varsayılan değerini bırakın .
1. **Yaşam sonu tarihi**için, gelecekte birkaç ay olan takvimden bir tarih seçin.
1. Çoğaltma'da, **Varsayılan yineleme sayısını** 1 olarak bırakın. **Replication** Kaynak bölgeye çoğaltmanız gerekir, bu nedenle ilk yinelemeyi varsayılan olarak bırakın ve ardından *Doğu ABD*olmak üzere ikinci bir yineleme bölgesi seçin.
1. İşi bittiğinde **Gözden Geçir + oluştur'u**seçin. Azure yapılandırmayı doğrular.
1. Görüntü sürümü doğrulamayı geçtiğinde **Oluştur'u**seçin.
1. Dağıtım tamamlandığında **kaynağa git'i**seçin.

Görüntüyü tüm hedef bölgelere kopyalamak biraz zaman alabilir.

### <a name="option-specialized"></a>Seçenek: Özel

1. Resim tanımınız için sayfada, sayfanın üst kısmından **sürüm ekle'yi** seçin.
1. **Bölge'de**anlık görüntünün depolandığı bölgeyi seçin. Görüntü sürümleri, oluşturuldukları kaynakla aynı bölgede oluşturulmalıdır.
1. **Ad**için , yazın *1.0.0*. Resim sürüm adı *büyük*takip etmelidir. *küçük*. yama formatında, *tümseler* kullanılarak. 
1. **OS disk anlık görüntüsünde,** açılan dosyadan kaynağınızdaki VM anlık görüntüsünü seçin. Kaynağınızda VM'nin eklemek istediğiniz bir veri diski varsa, açılan dosyadan doğru **LUN** numarasını seçin ve ardından **Veri diski anlık görüntüsü**için veri diskinin anlık görüntüsünü seçin. 
1. **En son dan Dışla,** *No*varsayılan değerini bırakın .
1. **Yaşam sonu tarihi**için, gelecekte birkaç ay olan takvimden bir tarih seçin.
1. Çoğaltma'da, **Varsayılan yineleme sayısını** 1 olarak bırakın. **Replication** Kaynak bölgeye çoğaltmanız gerekir, bu nedenle ilk yinelemeyi varsayılan olarak bırakın ve ardından *Doğu ABD*olmak üzere ikinci bir yineleme bölgesi seçin.
1. İşi bittiğinde **Gözden Geçir + oluştur'u**seçin. Azure yapılandırmayı doğrular.
1. Görüntü sürümü doğrulamayı geçtiğinde **Oluştur'u**seçin.
1. Dağıtım tamamlandığında **kaynağa git'i**seçin.

## <a name="share-the-gallery"></a>Galeriyi paylaşın

Görüntü galerisi düzeyinde erişimi paylaşmanızı öneririz. Aşağıdaki size yeni oluşturduğunuz galeriyi paylaşmada size yol alabilirsiniz.

1. Azure [portalını](https://portal.azure.com)açın.
1. Soldaki menüde Kaynak **gruplarını**seçin. 
1. Kaynak grupları listesinde **myGalleryRG'yi**seçin. Kaynak grubunuzun bıçağı açılacak.
1. **myGalleryRG** sayfasının sol menüsündeki menüde **Access denetimi (IAM)** seçeneğini belirleyin. 
1. **Rol atamaekle**altında **Ekle'yi**seçin. **Rol atama ekle** bölmesi açılır. 
1. **Rolü**altında, **Reader**seçin.
1. **Erişim atama**altında, Azure **AD kullanıcı, grup veya hizmet sorumlusu**varsayılan bırakın.
1. **Select**, davet etmek istediğiniz kişinin e-posta adresini yazın.
1. Kullanıcı kuruluşunuzun dışındaysa, **Bu kullanıcıya Microsoft ile işbirliği yapmalarını sağlayan bir e-posta gönderilecek** iletiyi görürsünüz. E-posta adresini olan kullanıcıyı seçin ve ardından **Kaydet'i**tıklatın.

Kullanıcı kuruluşunuzun dışındaysa, kuruluşa katılmak için bir e-posta daveti alır. Kullanıcının daveti kabul etmesi gerekir, o zaman galeriyi ve tüm resim tanımlarını ve sürümlerini kaynak listesinde görebilir.

