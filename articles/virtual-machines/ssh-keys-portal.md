---
title: Azure portal SSH anahtarları oluşturma
description: Linux VM 'lerini bağlamak için Azure portal SSH anahtarları oluşturma ve depolama hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/31/2020
ms.author: cynthn
ms.openlocfilehash: 39dbf7d9ad933dd47f0a566f02b5e276e4b615a3
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2020
ms.locfileid: "87514474"
---
# <a name="generate-and-store-ssh-keys-in-the-azure-portal"></a>Azure portal SSH anahtarları oluşturma ve depolama

Portalı Linux VM 'Leri dağıtmak için sık sık kullanıyorsanız, doğrudan portalda oluşturarak veya bilgisayarınızdan karşıya yükleyerek SSH anahtarlarını daha basit bir şekilde kullanabilirsiniz.

İlk olarak bir VM oluşturduğunuzda bir SSH anahtarı oluşturabilir ve bunları diğer VM 'Ler için yeniden kullanabilirsiniz. Ya da, kuruluşunuzun ihtiyaçlarına uyacak şekilde Azure 'da depolanan bir anahtarlar kümesine sahip olmanız için SSH anahtarlarını ayrı olarak da oluşturabilirsiniz. 

Mevcut anahtarlarınız varsa ve bunları portalda kullanmayı basitleştirmek istiyorsanız, bunları karşıya yükleyebilir ve yeniden kullanmak üzere Azure 'da saklayabilirsiniz.

Linux VM 'Leri ile SSH anahtarları oluşturma ve kullanma hakkında daha ayrıntılı bilgi için bkz. (Linux VM 'lerine bağlanmak için SSH anahtarlarını kullanma) [./Linux/SSH-From-Windows.MD].

## <a name="generate-new-keys"></a>Yeni anahtarlar oluştur

1. [Azure portal] ( https://portal.azure.com .

1. Sayfanın üst kısmında aramak için *SSH* yazın. **Market*altında **SSH anahtarları**' nı seçin.

1. **SSH anahtarı** sayfasında **Oluştur**' u seçin.

:::image type="content" source="./media/ssh-keys/portal-sshkey.png" alt-text="Yeni bir kaynak grubu oluştur ve bir SSH anahtar çifti oluştur":::

1. Anahtarlarınızın depolandığı yeni bir kaynak grubu oluşturmak için **kaynak grubu** ' nda **Yeni oluştur** ' u seçin. Kaynak grubunuz için bir ad yazın ve **Tamam**' ı seçin.

1. **Bölgede** , anahtarlarınızı depolamak için bir bölge seçin. Anahtarları herhangi bir bölgede kullanabilirsiniz, bu yalnızca depolanacağı bölgedir.

1. Anahtar **çifti adında**anahtarınız için bir ad yazın.

1. **SSH ortak anahtar kaynağı**' nda **ortak anahtar kaynağı oluştur**' u seçin. 

1. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin.

1. Doğrulama başarılı olduktan sonra **Oluştur**' u seçin.

1. Ardından, için bir açılır pencere alacaksınız, **özel anahtarı indir ve kaynak oluştur**' u seçin. Bu işlem SSH anahtarını bir. pek dosyası olarak indirir.

:::image type="content" source="./media/ssh-keys/download-key.png" alt-text="Özel anahtarı bir. pek dosyası olarak indirin":::

1. . Pek dosyası indirildikten sonra, bilgisayarınıza SSH istemcinizden kolayca işaret etmek için daha kolay bir yere taşımak isteyebilirsiniz.


## <a name="connect-to-the-vm"></a>VM’ye bağlanma

Yerel bilgisayarınızda bir PowerShell istemi açın ve şunu yazın:

```powershell
ssh -i <path to the .pem file> username@<ipaddress of the VM>
```

Örneğin şunu yazın:`ssh -i /Downloads/mySSHKey.pem azureuser@123.45.67.890`


## <a name="upload-an-ssh-key"></a>SSH anahtarını karşıya yükle

Ayrıca, Azure 'da depolanacak ortak bir SSH anahtarını da yükleyebilirsiniz. SSH anahtar çifti oluşturma hakkında daha fazla bilgi için bkz. [Linux VM 'lerine bağlanmak IÇIN SSH anahtarlarını kullanma ](./linux/ssh-from-windows.md).

1. [Azure portal] ( https://portal.azure.com .

1. Sayfanın üst kısmında aramak için *SSH* yazın. **Market*altında **SSH anahtarları**' nı seçin.

1. **SSH anahtarı** sayfasında **Oluştur**' u seçin.

:::image type="content" source="./media/ssh-keys/upload.png" alt-text="Azure 'da depolanacak SSH ortak anahtarını karşıya yükleme":::

1. Anahtarlarınızın depolandığı yeni bir kaynak grubu oluşturmak için **kaynak grubu** ' nda **Yeni oluştur** ' u seçin. Kaynak grubunuz için bir ad yazın ve **Tamam**' ı seçin.

1. **Bölgede** , anahtarlarınızı depolamak için bir bölge seçin. Anahtarları herhangi bir bölgede kullanabilirsiniz, bu yalnızca depolanacağı bölgedir.

1. Anahtar **çifti adında**anahtarınız için bir ad yazın.

1. **SSH ortak anahtar kaynağı**' nda **mevcut ortak anahtarı karşıya yükle**' yi seçin. 

1. Ortak anahtarın tüm içeriğini **karşıya yükleme anahtarına** yapıştırın ve sonra **gözden geçir + oluştur**' u seçin.

1. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. 

Anahtar karşıya yüklendikten sonra, bir VM oluştururken kullanmayı seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM 'Leri ile SSH anahtarları kullanma hakkında daha fazla bilgi için bkz. (Linux VM 'lerine bağlanmak için SSH anahtarlarını kullanma) [./Linux/SSH-From-Windows.MD].