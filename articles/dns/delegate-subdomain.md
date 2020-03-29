---
title: Bir alt etki alanını temsilciolarak - Azure DNS
description: Bu öğrenme yolu ile bir Azure DNS alt etki alanını atamaya başlayın.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: rohink
ms.openlocfilehash: cd1443a9ca8ccf7172072078734f21d789d36194
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937426"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Azure DNS alt etki alanını temsilci olarak

Bir DNS alt etki alanını devretmek için Azure portalını kullanabilirsiniz. Örneğin, contoso.com etki alanına sahipseniz, *mühendislik* adı verilen bir alt etki alanını, contoso.com bölgesinden ayrı olarak yönetebileceğiniz başka bir ayrı bölgeye devredebilirsiniz.

İsterseniz, [Azure PowerShell'i](delegate-subdomain-ps.md)kullanarak bir alt etki alanı devralabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bir Azure DNS alt etki alanını devretmek için öncelikle ortak etki alanınızı Azure DNS'ye devretmeniz gerekir. Ad sunucularınızı temsilci olarak nasıl yapılandıracağınıza ilişkin talimatlar için [azure DNS'ye bir etki alanı devretme](./dns-delegate-domain-azure-dns.md) bilgisini görün. Etki alanınız Azure DNS bölgenize devredildikten sonra alt etki alanınızı devredebilirsiniz.

> [!NOTE]
> Contoso.com bu makale boyunca örnek olarak kullanılır. contoso.com yerine kendi etki alanı adınızı yazın.

## <a name="create-a-zone-for-your-subdomain"></a>Alt etki alanınız için bir bölge oluşturma

İlk olarak, **mühendislik** alt etki alanı için bölge oluşturun.

1. Azure portalından **kaynak oluştur'u**seçin.
2. Arama kutusunda, **DNS**yazın ve **DNS bölgesini**seçin.
3. **Oluştur'u**seçin.
4. **DNS bölge oluşturma** bölmesinde, **Ad** metin kutusuna **engineering.contoso.com** yazın.
5. Bölgenizin kaynak grubunu seçin. Benzer kaynakları bir arada tutmak için üst bölgeyle aynı kaynak grubunu kullanmak isteyebilirsiniz.
6. **Oluştur'u**tıklatın.
7. Dağıtım başarılı olduktan sonra yeni bölgeye gidin.

## <a name="note-the-name-servers"></a>Ad sunucularını not edin

Ardından, mühendislik alt etki alanı için dört ad sunucularını not edin.

**Mühendislik** bölgesi bölmesine, bölgenin dört ad sunucularını not edin. Bu ad sunucularını daha sonra kullanırsınız.

## <a name="create-a-test-record"></a>Test kaydı oluşturma

Sınama için kullanılacak bir **A** kaydı oluşturun. Örneğin, bir **www** A kaydı oluşturun ve **10.10.10.10** IP adresiyle yapılandırın.

## <a name="create-an-ns-record"></a>NS kaydı oluşturma

Ardından, **mühendislik** bölgesi için bir ad sunucusu (NS) kaydı oluşturun.

1. Üst etki alanı için bölgeye gidin.
2. **+ Kayıt kümesi**’ni seçin.
3. Kayıt **ekle** bölmesinde, **Ad** metin kutusuna **mühendislik** yazın.
4. **Türü**için, **NS**seçin.
5. **Ad sunucusu**altında, **mühendislik** bölgesinden daha önce kaydettiğiniz dört ad sunucularını girin.
6. **Tamam**'a tıklayın.

## <a name="test-the-delegation"></a>Delegasyonu test edin

Delegasyonu test etmek için nslookup'u kullanın.

1. PowerShell penceresini açın.
2. Komut isteminde, yazın`nslookup www.engineering.contoso.com.`
3. Adresi gösteren **10.10.10.10'u**gösteren yetkili olmayan bir yanıt almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

[Azure'da barındırılan hizmetler için ters DNS'yi nasıl yapılandırabileceğinizi](dns-reverse-dns-for-azure-services.md)öğrenin.