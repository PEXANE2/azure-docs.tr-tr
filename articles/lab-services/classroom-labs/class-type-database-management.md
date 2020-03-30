---
title: İlişkisel veritabanları için veritabanı yönetimini öğretmek için bir laboratuvar ayarlama | Microsoft Dokümanlar
description: İlişkisel veritabanlarının yönetimini öğretmek için nasıl bir laboratuvar kurup kurmayı öğrenin.
services: lab-services
documentationcenter: na
author: emaher
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2019
ms.author: enewman
ms.openlocfilehash: 4c375487b30595251753021033c98cf0ca1e8dd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77469927"
---
# <a name="set-up-a-lab-to-teach-database-management-for-relational-databases"></a>İlişkisel veritabanları için veritabanı yönetimini öğretmek için bir laboratuvar ayarlama

Bu makalede, Azure Lab Hizmetleri'nde temel veritabanları yönetimi sınıfı için bir laboratuvar nasıl ayarlanır. Veritabanları kavramları, üniversitedeki Bilgisayar Bilimleri bölümlerinin çoğunda öğretilen giriş derslerinden biridir. Yapılandırılmış Sorgu Dili (SQL) uluslararası bir standarttır. SQL, veritabanındaki içeriği ekleme, bunlara erişim ve yönetme dahil olmak üzere ilişki veritabanı yönetimi için standart dildir.  En hızlı işleme, kanıtlanmış güvenilirlik, kolaylık ve kullanım esnekliği ile ünlüdür.

Bu makalede, hem MySQL Database Server hem de SQL Server 2019 sunucusuna sahip bir laboratuvarda sanal makine şablonu nasıl ayarlanın caiz olduğunu göstereceğiz.  [MySQL](https://www.mysql.com/) serbestçe kullanılabilir açık kaynak İlişkisel Veritabanı Yönetim Sistemi (RDBMS) olduğunu.  [SQL Server 2019,](https://www.microsoft.com/sql-server/sql-server-2019) Microsoft'un RDBMS'sinin en son sürümüdür.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ve laboratuvar hesabına ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliği ni aldıktan sonra Azure Lab Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni bir laboratuvar hesabı oluşturma hakkında daha fazla bilgi için, [Laboratuvar Hesabı Kurma Öğretici'sine](tutorial-setup-lab-account.md)bakın.  Varolan bir laboratuvar hesabını da kullanabilirsiniz.

### <a name="lab-account-settings"></a>Laboratuvar hesap ayarları

Laboratuvar hesabı için aşağıdaki tabloda açıklanan ayarları etkinleştirin. Pazar yeri görüntülerini etkinleştirme hakkında daha fazla bilgi için laboratuvar [oluşturucularının kullanabileceği Market görüntülerini belirt'e](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)bakın.

| Laboratuvar hesap ayarı | Yönergeler |
| ------------------- | ------------ |
|Pazar yeri görüntüsü| Laboratuvar hesabınızda kullanmak için 'Windows Server 2019'da SQL Server 2019 Standardı' görüntüsünü etkinleştirin.|

### <a name="lab-settings"></a>Laboratuvar ayarları

Sınıf laboratuarı kurarken aşağıdaki tablodaki ayarları kullanın.  Sınıf laboratuarı oluşturmak için daha fazla bilgi için [bkz.](tutorial-setup-classroom-lab.md)

| Laboratuvar ayarları | Değer/talimatlar |
| ------------ | ------------------ |
|Sanal Makine Boyutu| Orta. Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için en uygun uyrdu.|
|Sanal Makine Görüntüsü| Windows Server 2019'da SQL Server 2019 Standardı|

## <a name="template-machine-configuration"></a>Şablon makine yapılandırması

MySQL'i Windows Server 2019'a yüklemek [için, MySQL Community Server'ı Sanal Makine'de Yükle ve Çalıştır'da](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/mysql-2008r2?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json#install-and-run-mysql-community-server-on-the-virtual-machine)belirtilen adımları takip edebilirsiniz.

SQL Server 2019, yeni laboratuarı oluştururken seçtiğimiz sanal makine görüntüsüne önceden yüklenmiş durumda.

## <a name="cost-estimate"></a>Maliyet tahmini

Bu sınıf için olası bir maliyet tahminini karşılayalım.  25 öğrencilik bir sınıf kullanacağız.  20 saatlik ders saati var.  Ayrıca, her öğrenci planlanan ders saati dışında ödev veya ödevler için 10 saat kontenjan alır.  Seçtiğimiz sanal makine boyutu 42 laboratuvar birimi olan orta büyüklükteydi.

Aşağıda, bu sınıf için olası bir maliyet tahmini örneği verilmiştir:

25 \* öğrenci (20 planlanan saat + \* 10 kontenjan saati) 0,42 USD /saat = 315,00 USD

Fiyatlandırma hakkında daha fazla ayrıntı için Azure [Lab Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/lab-services/)bakın.

## <a name="conclusion"></a>Sonuç

Bu makalede, mysql ve SQL Server kullanarak temel veritabanı yönetimi kavramları için bir laboratuvar oluşturmak için gerekli adımları size yol açtı. Diğer veritabanları sınıfları için benzer bir kurulum kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar herhangi bir laboratuvar kurmak için ortak.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
