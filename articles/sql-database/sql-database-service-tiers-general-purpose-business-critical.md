---
title: Azure SQL-Datenbank – „Universell“ und „Unternehmenskritisch“ | Microsoft-Dokumentation
description: In diesem Artikel werden die Dienstebenen „Universell“ und „Unternehmenskritisch“ im vCore-basierten Kaufmodell erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 02/23/2019
ms.openlocfilehash: ccbecd7aec3980d5adf91340ff0e230fb410f4f3
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70918853"
---
# <a name="azure-sql-database-service-tiers"></a>Dienstebenen für Azure SQL-Datenbank

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um selbst bei Infrastrukturausfällen eine Verfügbarkeit von 99,99 Prozent sicherzustellen. In Azure SQL-Datenbank werden drei Dienstebenen verwendet, die jeweils unterschiedliche Architekturmodelle aufweisen. Diese Dienstebenen lauten:

- [Universell](sql-database-service-tier-general-purpose.md) ist für die meisten generischen Workloads konzipiert.
- [Unternehmenskritisch](sql-database-service-tier-business-critical.md) ist für Workloads mit geringer Latenz und einem lesbaren Replikat konzipiert.
- [Hyperscale](sql-database-service-tier-hyperscale.md) ist für sehr große Datenbanken (bis zu 100 TB) mit mehreren lesbaren Replikaten konzipiert.

In diesem Artikel werden die Unterschiede zwischen den Dienstebenen, die Speicher- und Sicherungsaspekte für die Dienstebenen „Universell“ und „Unternehmenskritisch“ im vCore-basierten Kaufmodell beschrieben.

## <a name="service-tier-comparison"></a>Vergleich der Dienstebenen

In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen den Dienstebenen für die neueste Generation (Gen5) beschrieben. Beachten Sie, dass sich die Merkmale der Dienstebenen bei Einzeldatenbanken und verwalteten Instanzen unterscheiden können.

| | Ressourcentyp | Allgemeiner Zweck |  Hyperscale | Unternehmenskritisch |
|:---:|:---:|:---:|:---:|:---:|
| **Am besten geeignet für** | |  Die meisten geschäftlichen Workloads. Bietet budgetorientierte ausgewogene Compute- und Speicheroptionen. | Datenanwendungen mit hohem Datenkapazitätsbedarf und der Möglichkeit, Speicher automatisch zu skalieren und Computeressourcen nahtlos zu skalieren. | OLTP-Anwendungen mit hoher Transaktionsrate und den geringsten Latenzen bei E/A-Vorgängen. Bietet höchste Resilienz gegenüber Ausfällen durch mehrere isolierte Replikate.|
|  **Verfügbar in Ressourcentyp:** ||Einzeldatenbank/Pool für elastische Datenbanken/verwaltete Instanz | Einzeldatenbank | Einzeldatenbank/Pool für elastische Datenbanken/verwaltete Instanz |
| **Computegröße**|Einzeldatenbank/Pool für elastische Datenbanken | 1 bis 80 virtuelle Kerne | 1 bis 80 virtuelle Kerne | 1 bis 80 virtuelle Kerne |
| | Verwaltete Instanz | 4, 8, 16, 24, 32, 40, 64, 80 virtuelle Kerne | – | 4, 8, 16, 24, 32, 40, 64, 80 virtuelle Kerne |
| | Verwaltete Instanzpools | 2, 4, 8, 16, 24, 32, 40, 64, 80 virtuelle Kerne | – | – |
| **Speichertyp** | Alle | Storage Premium (remote, pro Instanz) | Entkoppelter Speicher mit lokalem SSD-Cache (pro Instanz) | Äußerst schneller lokaler SSD-Speicher (pro Instanz) |
| **Datenbankgröße** | Einzeldatenbank/Pool für elastische Datenbanken | 5 GB – 4 TB | Bis zu 100 TB | 5 GB – 4 TB |
| | Verwaltete Instanz  | 32 GB – 8 TB | – | 32 GB – 4 TB |
| **Speichergröße** | Einzeldatenbank/Pool für elastische Datenbanken | 5 GB – 4 TB | Bis zu 100 TB | 5 GB – 4 TB |
| | Verwaltete Instanz  | 32 GB – 8 TB | – | 32 GB – 4 TB |
| **TempDB-Größe** | Einzeldatenbank/Pool für elastische Datenbanken | [32 GB pro virtuellem Kern](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-for-provisioned-compute) | [32 GB pro virtuellem Kern](sql-database-vcore-resource-limits-single-databases.md#hyperscale-service-tier-for-provisioned-compute) | [32 GB pro virtuellem Kern](sql-database-vcore-resource-limits-single-databases.md#business-critical-service-tier-for-provisioned-compute) |
| | Verwaltete Instanz  | [24 GB pro virtuellem Kern](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | – | Bis zu 4 TB – [begrenzt durch Speichergröße](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
| **E/A-Durchsatz** | Einzeldatenbank | [500 IOPS pro virtuellem Kern](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-for-provisioned-compute) | Die tatsächlichen IOPs hängen von der Workload ab. | [4000 IOPS pro virtuellem Kern](sql-database-vcore-resource-limits-single-databases.md#business-critical-service-tier-for-provisioned-compute)|
| | Verwaltete Instanz | [100–250 MB/s und 500–7.500 IOPS pro Datei](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | – | [1375 IOPS pro virtuellem Kern](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
| **Schreibdurchsatz** | Einzeldatenbank | [1.875 MB/Sek. pro virtuellem Kern (max. 30 MB/s)](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-for-provisioned-compute) | Hyperscale ist eine mehrstufige Architektur mit Caching auf mehreren Ebenen. Die tatsächlichen IOPs hängen von der Workload ab. | [6 MB/Sek. pro virtuellem Kern (max. 96 MB/s)](sql-database-vcore-resource-limits-single-databases.md#business-critical-service-tier-for-provisioned-compute) |
| | Verwaltete Instanz | [3 MB/Sek. pro virtuellem Kern (max. 22 MB/s)](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) | – | [4 MB/Sek. pro virtuellem Kern (max. 48 MB/s)](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) |
|**Verfügbarkeit**|Alle| 99,99 % |  [99,95 % mit einem sekundären Replikat, 99,99 % mit weiteren Replikaten](sql-database-service-tier-hyperscale-faq.md#what-slas-are-provided-for-a-hyperscale-database) | 99,99 % <br/> [99,995 % mit zonenredundantem Singleton](https://azure.microsoft.com/blog/understanding-and-leveraging-azure-sql-database-sla/) |
|**Sicherungen**|Alle|RA-GRS, 7 - 35 Tage (standardmäßig 7 Tage)| RA-GRS, 7 Tage, konstante Zeitpunktwiederherstellung (Point-in-Time Recovery, PITR) | RA-GRS, 7 - 35 Tage (standardmäßig 7 Tage) |
|**In-Memory-OLTP** | | – | Verfügbar | – |
|**Schreibgeschützte Replikate**| | 0 | 1 (integriert, im Preis inbegriffen) | 0–4 |
|**Preise/Abrechnung** | Einzeldatenbank | [Virtueller Kern, reservierter Speicher und Sicherungsspeicher](https://azure.microsoft.com/pricing/details/sql-database/single/) werden in Rechnung gestellt. <br/>IOPS werden nicht in Rechnung gestellt. | [Virtueller Kern für jedes Replikat, verwendeter Speicher, Sicherungsspeicher und IOPS](https://azure.microsoft.com/pricing/details/sql-database/single/) werden in Rechnung gestellt. <br/>Sicherungsspeicher wird noch nicht abgerechnet. | [Virtueller Kern, reservierter Speicher und Sicherungsspeicher](https://azure.microsoft.com/pricing/details/sql-database/single/) werden in Rechnung gestellt. <br/>IOPS werden nicht in Rechnung gestellt. |
|| Verwaltete Instanz | [V-Kern und reservierter Speicher](https://azure.microsoft.com/pricing/details/sql-database/managed/) werden in Rechnung gestellt. <br/>IOPS werden nicht in Rechnung gestellt.<br/>Sicherungsspeicher wird noch nicht abgerechnet. | – | [V-Kern und reservierter Speicher](https://azure.microsoft.com/pricing/details/sql-database/managed/) werden in Rechnung gestellt. <br/>IOPS werden nicht in Rechnung gestellt.<br/>Sicherungsspeicher wird noch nicht abgerechnet. | 
|**Rabattmodelle**| | [Reservierte Instanzen](sql-database-reserved-capacity.md)<br/>[Azure-Hybridvorteil](sql-database-service-tiers-vcore.md#azure-hybrid-benefit) (nicht verfügbar für Dev/Test-Abonnements)<br/>[Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/)- und [Pay-as-you-Go](https://azure.microsoft.com/offers/ms-azr-0023p/)-Dev/Test-Abonnements| [Reservierte Instanzen](sql-database-reserved-capacity.md)<br/>[Azure-Hybridvorteil](sql-database-service-tiers-vcore.md#azure-hybrid-benefit) (nicht verfügbar für Dev/Test-Abonnements)<br/>[Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/)- und [Pay-as-you-Go](https://azure.microsoft.com/offers/ms-azr-0023p/)-Dev/Test-Abonnements| [Reservierte Instanzen](sql-database-reserved-capacity.md)<br/>[Azure-Hybridvorteil](sql-database-service-tiers-vcore.md#azure-hybrid-benefit) (nicht verfügbar für Dev/Test-Abonnements)<br/>[Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/)- und [Pay-as-you-Go](https://azure.microsoft.com/offers/ms-azr-0023p/)-Dev/Test-Abonnements|

> [!NOTE]
> Informationen zur Dienstebene „Hyperscale“ im vCore-basierten Kaufmodell finden Sie unter [Dienstebene „Hyperscale“](sql-database-service-tier-hyperscale.md). Einen Vergleich zwischen vCore-basiertem Kaufmodell und DTU-basiertem Kaufmodell finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Daten- und Protokollspeicher

Die folgenden Faktoren haben Einfluss darauf, wie viel Speicher für Daten-und Protokolldateien verwendet wird:

- Der zugeordnete Speicher wird für Datendateien (MDF) und Protokolldateien (LDF) verwendet.
- Jede Einzeldatenbank-Computegröße unterstützt eine maximale Datenbankgröße, die standardmäßig bei 32 GB liegt.
- Wenn Sie die erforderliche Einzeldatenbankgröße (die Größe der MDF-Datei) konfigurieren, werden automatisch 30 Prozent zusätzlicher Speicher hinzugefügt, um LDF-Dateien zu unterstützen.
- Die Speichergröße für eine verwaltete SQL-Datenbank-Instanz muss als Vielfaches von 32 GB angegeben werden.
- Sie können eine beliebige Einzeldatenbankgröße zwischen 10 GB und dem unterstützten Maximum auswählen.
  - In den Dienstebenen „Standard“ oder „Universell“ erhöhen oder verringern Sie die Speichergröße in Schritten von 10 GB.
  - In den Dienstebenen „Premium“ oder „Unternehmenskritisch“ erhöhen oder verringern Sie die Speichergröße in Schritten von 250 GB.
- In der Dienstebene „Universell“ wird für `tempdb` eine angefügte SSD verwendet, und diese Speicherkosten sind im V-Kern-Preis enthalten.
- In der Dienstebene „Unternehmenskritisch“ wird für `tempdb` die angefügte SSD für MDF- und LDF-Dateien gemeinsam genutzt, und die `tempdb`-Speicherkosten sind im V-Kern-Preis enthalten.

> [!IMPORTANT]
> Ihnen wird der gesamte Speicher berechnet, der für MDF- und LDF-Dateien zugeordnet ist.

Verwenden Sie [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql) zum Überwachen der aktuellen Gesamtgröße Ihrer MDF- und LDF-Dateien. Verwenden Sie [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql), um die aktuelle Größe der einzelnen MDF- und LDF-Dateien zu überwachen.

> [!IMPORTANT]
> Unter bestimmten Umständen müssen Sie ggf. eine Datenbank verkleinern, um ungenutzten Speicherplatz freizugeben. Weitere Informationen finden Sie unter [Verwalten von Dateispeicherplatz in Azure SQL-Datenbank](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Sicherungen und Speicher

Der Speicher für Datenbanksicherungen wird zugeordnet, um die Funktionen „Point-in-Time-Wiederherstellung“ (Point in Time Restore, PITR) und [Langzeitaufbewahrung](sql-database-long-term-retention.md) (Long Term Retention, LTR) von SQL-Datenbank zu unterstützen. Dieser Speicher wird für jede Datenbank separat zugeordnet und als zwei Arten von getrennten Datenbankgebühren berechnet.

- **PITR**: Einzelne Datenbanksicherungen werden automatisch in den [georedundanten Speicher mit Lesezugriff](../storage/common/storage-designing-ha-apps-with-ragrs.md) (Read-Access Geo-Redundant, RA-GRS) kopiert. Die Speichergröße wird dynamisch erhöht, wenn neue Sicherungen erstellt werden. Der Speicher wird für wöchentliche vollständige Sicherungen, tägliche differenzielle Sicherungen und im 5-Minuten-Takt kopierte Sicherungen von Transaktionsprotokollen verwendet. Der Speicherverbrauch richtet sich nach der Änderungsrate der Datenbank und nach dem Aufbewahrungszeitraum für Sicherungen. Sie können für jede Datenbank eine separate Aufbewahrungsdauer konfigurieren, die zwischen 7 und 35 Tagen liegt. Eine Mindestspeichermenge, die 100 Prozent (1x) der Datenbankgröße entspricht, wird kostenlos zur Verfügung gestellt. Für die meisten Datenbanken reicht diese Menge aus, um Sicherungen für sieben Tage aufzubewahren.
- **LTR**: SQL-Datenbank verfügt über eine Option zum Konfigurieren der Langzeitaufbewahrung von vollständigen Sicherungen für eine Dauer von bis zu zehn Jahren. Wenn Sie eine LTR-Richtlinie einrichten, werden diese Sicherungen automatisch im RA-GRS-Speicher gespeichert. Sie können jedoch steuern, wie häufig die Sicherungen kopiert werden. Zur Einhaltung unterschiedlicher Konformitätsanforderungen können Sie verschiedene Aufbewahrungszeiträume für wöchentliche, monatliche und/oder jährliche Sicherungen auswählen. Wie viel Speicher für LTR-Sicherungen verwendet wird, richtet sich nach der ausgewählten Konfiguration. Sie können den LTR-Preisrechner verwenden, um die Kosten für LTR-Speicher zu schätzen. Weitere Informationen finden Sie unter dem Thema zur [Langzeitaufbewahrung von SQL-Datenbank](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Nächste Schritte

- Ausführliche Informationen zu bestimmten Computegrößen und Speichergrößen für Einzeldatenbanken in den Dienstebenen „Universell“ und „Unternehmenskritisch“ finden Sie unter [Ressourcenlimits für Einzeldatenbanken, die das auf virtuellen Kernen (V-Kernen) basierende (vCore-basierte) Kaufmodell verwenden](sql-database-vcore-resource-limits-single-databases.md).
- Ausführliche Informationen zu bestimmten Computegrößen und Speichergrößen für Pools für elastische Datenbanken in den Dienstebenen „Universell“ und „Unternehmenskritisch“ finden Sie unter [Ressourcenlimits für Pools für elastische Datenbanken, die das vCore-basierte Kaufmodell verwenden](sql-database-vcore-resource-limits-elastic-pools.md).
