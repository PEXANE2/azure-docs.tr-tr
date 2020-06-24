---
title: Azure dosya paylaşımından AD DS katılmış bir VM 'ye bağlama
description: Şirket içi Active Directory Domain Services katılmış makinelerinize bir dosya paylaşma bağlama hakkında bilgi edinin.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: b061b86226df38c402de3d3d65385ebf583f024b
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85214426"
---
# <a name="part-four-mount-a-file-share-from-a-domain-joined-vm"></a>Dördüncü kısım: etki alanına katılmış bir VM 'den bir dosya paylaşma bağlama

Bu makaleye başlamadan önce, önceki makaleyi tamamladığınızdan emin olun, [Dizin ve dosya düzeyi IZINLERINI SMB üzerinden yapılandırın](storage-files-identity-ad-ds-configure-permissions.md).

Bu makalede açıklanan işlem, dosya paylaşımınızın ve erişim izninizin doğru ayarlandığını ve bir Azure dosya paylaşımının etki alanına katılmış bir VM 'den erişebileceğini doğrular. Share-Level RBAC rol atamasının etkili olması biraz zaman alabilir. 

Aşağıdaki görüntüde gösterildiği gibi, izinleri verilen kimlik bilgilerini kullanarak istemcide oturum açın.

![Kullanıcı kimlik doğrulaması için Azure AD oturum açma ekranını gösteren ekran görüntüsü](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

## <a name="mounting-prerequisites"></a>Önkoşulları bağlama

Dosya payını bağlayabilmeniz için önce aşağıdaki önkoşulların yerine geldiğinden emin olun:

- Depolama hesabı anahtarınızı kullanarak daha önce dosya paylaşımının bağlandığından bir istemciden dosya paylaşımından bağlama yapıyorsanız, paylaşımın bağlantısını kesdiğinizden, depolama hesabı anahtarının kalıcı kimlik bilgilerini kaldırmış olduğunuzdan ve şu anda kimlik doğrulaması için AD DS kimlik bilgileri kullandığınızdan emin olun.
- İstemciniz AD DS bir görüş satırı içermelidir. Makinenizin veya sanal makinenizin AD DS tarafından yönetilen ağ tükeniyorsa, kimlik doğrulaması için AD DS ulaşmak üzere VPN 'yi etkinleştirmeniz gerekir.

Yer tutucu değerlerini kendi değerlerinizle değiştirin, ardından Azure dosya paylaşımından bağlama için aşağıdaki komutu kullanın:

```cli
# Always mount your share using.file.core.windows.net, even if you setup a private endpoint for your share.
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

AD DS kimlik bilgileriyle bağlantı sorunlarıyla karşılaşırsanız, kılavuz için bkz. [Azure dosyaları ad kimlik bilgileriyle bağlama](storage-troubleshoot-windows-file-connection-problems.md#unable-to-mount-azure-files-with-ad-credentials) .

Dosya paylaşımınızı bağlama başarılı olduysa, Azure dosya paylaşımlarınız için şirket içi AD DS kimlik doğrulamasını başarıyla etkinleştirmiş ve yapılandırmış olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Depolama hesabını temsil eden AD DS ' de oluşturduğunuz kimlik, parola döndürmeyi zorlayan bir etki alanında veya OU 'da yer alıyorsa, parolanızı güncelleştirme yönergeleri için sonraki makaleye geçin:

[AD DS 'de depolama hesabı Kimliğinizin parolasını güncelleştirin](storage-files-identity-ad-ds-update-password.md)