---
title: Sorun Giderme Azure Kalesi | Microsoft Dokümanlar
description: Bu makalede, Azure Kalesi'nin nasıl sorun giderilmeye başvurduğunu öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: charwen
ms.openlocfilehash: 749d7125c013f419197ef8243d2475e612dc81b5
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619179"
---
# <a name="troubleshoot-azure-bastion"></a>Azure Bastion sorunlarını giderme

Bu makalede, Azure Kalesi'nin nasıl sorun giderilen nasıl giderilen gösterilmektedir.

## <a name="unable-to-create-an-nsg-on-azurebastionsubnet"></a><a name="nsg"></a>AzureBastionSubnet'te NSG oluşturulamıyor

**S:** Azure Bastion alt ağında bir NSG oluşturmaya çalıştığımda şu hatayı alıyorum: *'Ağ güvenlik <NSG name> grubunun Azure Bastion Subnet AzureBastionSubnet için gerekli kuralları yoktur'*.

**A:** *AzureBastionSubnet'e*bir NSG oluşturup uygularsanız, NSG'nizde aşağıdaki kuralları eklediğinizden emin olun. Bu kuralları eklemezseniz, NSG oluşturma/güncelleştirmebaşarısız olur.

1. Düzlem bağlantısını kontrol edin – GatewayManager'dan gelen 443'te
2. Tanılama günlüğü ve diğerleri – 443'ten AzureCloud'a gidenler (Bu hizmet etiketindeki bölgesel etiketler henüz desteklenmez.)
3. Hedef VM - 3389 ve 22 için Giden VirtualNetwork için

NSG kurallarının bir örneği [hızlı başlangıç şablonunda](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azure-bastion)başvuru için kullanılabilir.
Daha fazla bilgi [için Azure Kalesi için NSG kılavuzuna](bastion-nsg.md)bakın.

## <a name="unable-to-use-my-ssh-key-with-azure-bastion"></a><a name="sshkey"></a>Azure Bastion ile SSH anahtarımı kullanamıyor

**S:** SSH anahtar dosyama göz atmaya çalıştığımda şu hatayı alıyorum: *'SSH Private tuşu -----BEGIN RSA PRIVATE KEY----- ile başlamalı ve -----END RSA PRIVATE KEY-----' ile bitmelidir.*

**A:** Azure Bastion, şu anda yalnızca RSA SSH tuşlarını destekler. Hedef VM'de ortak anahtar sağlanmış ssh için RSA özel anahtarı olan bir anahtar dosyaya göz attığınızdan emin olun. 

Örnek olarak, yeni bir RSA SSH anahtarı oluşturmak için aşağıdaki komutu kullanabilirsiniz:

**ssh-keygen -t rsa -b 4096 -C "email@domain.com**

Çıktı:

```
ashishj@dreamcatcher:~$ ssh-keygen -t rsa -b 4096 -C "email@domain.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ashishj/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ashishj/.ssh/id_rsa.
Your public key has been saved in /home/ashishj/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:c+SBciKXnwceaNQ8Ms8C4h46BsNosYx+9d+AUxdazuE email@domain.com
The key's randomart image is:
+---[RSA 4096]----+
|      .o         |
| .. ..oo+. +     |
|=.o...B==.O o    |
|==o  =.*oO E     |
|++ .. ..S =      |
|oo..   + =       |
|...     o o      |
|         . .     |
|                 |
+----[SHA256]-----+
```

## <a name="unable-to-sign-in-to-my-windows-domain-joined-virtual-machine"></a><a name="domain"></a>Windows etki alanı yla birleştirilmiş sanal makinemde oturum açamıyor

**S:** Etki alanına katılan Windows sanal makineme bağlanamam.

**A:** Azure Bastion, yalnızca kullanıcı adı parolası tabanlı etki alanı oturum açma için etki alanına birleştirilmiş VM oturum açma işlemini destekler. Azure portalında etki alanı kimlik bilgilerini belirtirken,username@domainoturum açmak için *etki alanı\kullanıcı adı* biçimi yerine UPN ( ) biçimini kullanın. Bu, etki alanına katılan veya karma birleştirilmiş (her ikisi de etki alanı yla birleştirilmiş ve Azure AD'ye katılan) sanal makineler için desteklenir. Azure AD'ye özel sanal makineler için desteklenmez.

## <a name="file-transfer-issues"></a><a name="filetransfer"></a>Dosya aktarım sorunları

**S:** Dosya aktarımı Azure Bastion ile desteklenir mi?

**A:** Dosya aktarım şu anda desteklenmez. Destek eklemek için çalışıyoruz.

## <a name="black-screen-in-the-azure-portal"></a><a name="blackscreen"></a>Azure portalında siyah ekran

**S:** Azure Bastion kullanarak bağlanmaya çalıştığımda, Azure portalında siyah bir ekran alırım.

**A:** Bu durum, web tarayıcınız ile Azure Bastion (istemci Internet güvenlik duvarınız WebSockets trafiğini veya benzeri engellemeleri olabilir) veya Azure Kalesi ile hedef VM arasında bir ağ bağlantısı sorunu olduğunda gerçekleşir. Çoğu durumda AzureBastionSubnet'e veya sanal ağınızdaki RDP/SSH trafiğini engelleyen hedef VM alt ağına uygulanan bir NSG içerir. İstemci internet güvenlik duvarınızda WebSockets trafiğine izin verin ve hedef VM alt netinizdeki NSG'leri kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Bastion SSS'ye](bastion-faq.md)bakın.