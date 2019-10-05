---
title: Hızlı başlangıç-Azure CLı kullanarak Azure DNS bir bölge ve kayıt oluşturma
description: Hızlı başlangıç-Azure DNS bir DNS bölgesi ve kaydı oluşturma hakkında bilgi edinin. Bu, Azure CLı kullanarak ilk DNS bölgenizi ve kaydınızı oluşturup yönetmek için adım adım kılavuzudur.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: b5d842c2d6ff84a0f17c4e8be0bfade018edc48b
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71959984"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure DNS bir bölge ve kayıt oluşturma

Bu makale, Windows, Mac ve Linux 'ta kullanılabilen Azure CLı kullanarak ilk DNS bölgenizi ve kaydınızı oluşturma adımlarında size kılavuzluk eder. [Azure Portal](dns-getstarted-portal.md) veya [Azure PowerShell](dns-getstarted-powershell.md)kullanarak da bu adımları uygulayabilirsiniz.

DNS bölgesi, belirli bir etki alanı için DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS barındırılmaya başlamak için, bu etki alanı adı için bir DNS bölgesi oluşturmanız gerekir. Etki alanınız için her bir DNS kaydı daha sonra bu DNS bölgesi içinde oluşturulur. Son olarak, DNS bölgenizi Internet 'Te yayımlamak için etki alanı için ad sunucularını yapılandırmanız gerekir. Bu adımların her biri aşağıda açıklanmıştır.

Ayrıca, özel DNS bölgelerini de destekler Azure DNS. Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [özel etki alanları için Azure DNS kullanma](private-dns-overview.md). Özel bir DNS bölgesi oluşturma hakkında bir örnek için bkz. [CLI kullanarak Azure DNS özel bölgelerini kullanmaya başlama](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-the-resource-group"></a>Kaynak grubu oluşturma

DNS bölgesini oluşturmadan önce, DNS bölgesini içerecek bir kaynak grubu oluşturun:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>DNS bölgesi oluşturma

@No__t-0 komutu kullanılarak bir DNS bölgesi oluşturulur. Bu komuta ilişkin yardımı görmek için `az network dns zone create -h` yazın.

Aşağıdaki örnek, *Myresourcegroup*kaynak grubunda *contoso. xyz* adlı bir DNS bölgesi oluşturur. Değerleri kendi değerlerinizle değiştirerek bir DNS bölgesi oluşturmak için örneği kullanın.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.xyz
```

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

DNS kaydı oluşturmak için `az network dns record-set [record type] add-record` komutunu kullanın. Kayıtlar hakkında yardım için bkz. `azure network dns record-set A add-record -h`.

Aşağıdaki örnek, "MyResourceGroup" kaynak grubundaki "contoso. xyz" DNS bölgesinde "www" göreli adına sahip bir kayıt oluşturur. Kayıt kümesinin tam adı "www. contoso. xyz" dir. Kayıt türü "A", IP adresi "10.10.10.10" ve varsayılan TTL 3600 saniyedir (1 saat).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.xyz -n www -a 10.10.10.10
```

## <a name="view-records"></a>Kayıtları görüntüle

Bölgenizdeki DNS kayıtlarını listelemek için şunu çalıştırın:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.xyz
```

## <a name="test-the-name-resolution"></a>Ad çözünürlüğünü test etme

Test ' A ' kaydıyla bir test DNS bölgesine sahip olduğunuza göre, ad çözümlemesini *nslookup*adlı bir araçla test edebilirsiniz. 

**DNS ad çözümlemesini test etmek için:**

1. Bölgenizin ad sunucularının listesini almak için aşağıdaki cmdlet 'i çalıştırın:

   ```azurecli
   az network dns record-set ns show --resource-group MyResourceGroup --zone-name contoso.xyz --name @
   ```

1. Ad sunucusu adlarından birini, önceki adımın çıktısından kopyalayın.

1. Bir komut istemi açın ve aşağıdaki komutu çalıştırın:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Örneğin:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Aşağıdaki ekrana benzer bir şey görmeniz gerekir:

   ![nslookup](media/dns-getstarted-portal/nslookup.PNG)

**Www\.contoso.xyz** ana bilgisayar adı, yalnızca yapılandırdığınız gibi **10.10.10.10**olarak çözümlenir. Bu sonuç, ad çözümlemenin doğru çalıştığını doğrular.

## <a name="delete-all-resources"></a>Tüm kaynakları Sil

Artık gerekli olmadığında, kaynak grubunu silerek bu hızlı başlangıçta oluşturulan tüm kaynakları silebilirsiniz:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak ilk DNS bölgenizi ve kaydınızı oluşturduğunuza göre, özel bir etki alanında bir Web uygulaması için kayıtlar oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Özel bir etki alanında bir Web uygulaması için DNS kayıtları oluşturma](./dns-web-sites-custom-domain.md)
