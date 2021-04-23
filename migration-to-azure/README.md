---
description: Impact on interfaces when migrating to Azure
---

# Azure migration

When the Ultimo environment is migrated to the Azure cloud, this will have an impact on existing intefaces. What the impact is on each interface depends on how the interface technically works. If communication with another system is done through webservices, the only change will be a different endpoint when system connect to Ultimo. If there is no direct communication between systems, the interface will most likely exchange data through files. These so called file based interfaces are no longer possible on Azure. Ultimo environments running on Azure do not have direct access to any FTP server or local network share. These file based interfaces therefor have to be reconfigured.

Modern applications offer webservices to transfer data between systems. Migrating to webservices is the recommended option for existing file based interfaces. There is no general guide for this action, each interface has to be reconfigured specifically. Note that acctivities to reconfigure the Ultimo part of the interface will require additional consultancy. These activities are not part of the migration project. Activities will also be required to change the other side of the interface. This can possibly be handled by your IT department, but third parties may also be involved. A business scan can help you with creating an inventory of current interfaces and wether it is possible to migrate to webservices.

If migration to direct communication with webservices is not possible, some alternatives can be considered, as described on the following pages. Using one of these alternatives requires action from both the Ultimo migration team and your IT department, as described on these pages.

