---
title: Upgrade requirements
intro: 'Before upgrading {% data variables.product.prodname_ghe_server %}, review these recommendations and requirements to plan your upgrade strategy.'
redirect_from:
  - /enterprise/admin/installation/upgrade-requirements
  - /enterprise/admin/guides/installation/finding-the-current-github-enterprise-release
  - /enterprise/admin/enterprise-management/upgrade-requirements
  - /admin/enterprise-management/upgrade-requirements
  - /enterprise/admin/guides/installation/about-upgrade-requirements
  - /admin/enterprise-management/updating-the-virtual-machine-and-physical-resources/upgrade-requirements
  - /admin/monitoring-managing-and-updating-your-instance/updating-the-virtual-machine-and-physical-resources/upgrade-requirements
  - /admin/monitoring-and-managing-your-instance/updating-the-virtual-machine-and-physical-resources/upgrade-requirements
versions:
  ghes: '*'
type: reference
topics:
  - Enterprise
  - Upgrades
---

> [!NOTE]
> * Upgrade packages are available at [enterprise.github.com](https://enterprise.github.com/releases) for supported versions. Verify the availability of the upgrade packages you will need to complete the upgrade. If a package is not available, visit {% data variables.contact.contact_ent_support %} and contact us for assistance.
> * If you're using {% data variables.product.prodname_ghe_server %} Clustering, see [AUTOTITLE](/admin/enterprise-management/configuring-clustering/upgrading-a-cluster) in the {% data variables.product.prodname_ghe_server %} Clustering Guide for specific instructions unique to clustering.
> * The release notes for {% data variables.product.prodname_ghe_server %} provide a comprehensive list of new features for every version of {% data variables.product.prodname_ghe_server %}. For more information, see the [releases page](https://enterprise.github.com/releases).

## Recommendations

* Include as few upgrades as possible in your upgrade process. For example, instead of upgrading from {% data variables.product.prodname_enterprise %} {{ enterpriseServerReleases.supported[2] }} to {{ enterpriseServerReleases.supported[1] }} to {{ enterpriseServerReleases.latest }}, you could upgrade from {% data variables.product.prodname_enterprise %} {{ enterpriseServerReleases.supported[2] }} to {{ enterpriseServerReleases.latest }}. Use the [{% data variables.enterprise.upgrade_assistant %}](https://support.github.com/enterprise/server-upgrade) to find the upgrade path from your current release version.
* If you’re several versions behind, upgrade {% data variables.location.product_location %} as far forward as possible with each step of your upgrade process. Using the latest version possible on each upgrade allows you to take advantage of performance improvements and bug fixes. For example, you could upgrade from {% data variables.product.prodname_enterprise %} 2.7 to 2.8 to 2.10, but upgrading from {% data variables.product.prodname_enterprise %} 2.7 to 2.9 to 2.10 uses a later version in the second step.
* Use the latest patch release when upgrading. {% data reusables.enterprise_installation.enterprise-download-upgrade-pkg %}
* Use a staging instance to test the upgrade steps. For more information, see [AUTOTITLE](/admin/installation/setting-up-a-github-enterprise-server-instance/setting-up-a-staging-instance).
* When running multiple upgrades, ensure data migrations and upgrade tasks running in the background are fully complete before proceeding to the next feature upgrade. To check the status of these processes, you can use the `ghe-migrations` and `ghe-check-background-upgrade-jobs` command-line utilities. For more information, see [AUTOTITLE](/admin/configuration/configuring-your-enterprise/command-line-utilities#upgrading-github-enterprise-server).
* Take a snapshot before upgrading your virtual machine. For more information, see [AUTOTITLE](/admin/upgrading-your-instance/preparing-to-upgrade/taking-a-snapshot). After the snapshot is taken, turn off automatic snapshots to avoid possible performance impacts during upgrade.
* Ensure you have a recent, successful backup of your instance. For more information, see the [{% data variables.product.prodname_enterprise_backup_utilities %} README.md file](https://github.com/github/backup-utils#readme).

## Requirements

* You must upgrade from a feature release that's **at most** two releases behind. For example, to upgrade to {% data variables.product.prodname_enterprise %} {{ enterpriseServerReleases.latest }}, you must be on {% data variables.product.prodname_enterprise %} {{ enterpriseServerReleases.supported[1] }} or {{ enterpriseServerReleases.supported[2] }}.
* When upgrading using an upgrade package, schedule a maintenance window for {% data variables.product.prodname_ghe_server %} end users.
* {% data reusables.enterprise_installation.hotpatching-explanation %}
* A hotpatch may require downtime if the affected services (like kernel, MySQL, or Elasticsearch) require a VM reboot or a service restart. You'll be notified when a reboot or restart is required. You can complete the reboot or restart at a later time.
* Additional root storage must be available when upgrading through hotpatching, as it installs multiple versions of certain services until the upgrade is complete. Pre-flight checks will notify you if you don't have enough root disk storage.
* When upgrading through hotpatching, your instance cannot be too heavily loaded, as it may impact the hotpatching process.
* Upgrading to {% data variables.product.prodname_ghe_server %} 2.17 migrates your audit logs from Elasticsearch to MySQL. This migration also increases the amount of time and disk space it takes to restore a snapshot. Before migrating, check the number of bytes in your Elasticsearch audit log indices with this command:

``` shell
curl -s http://localhost:9201/audit_log/_stats/store | jq ._all.primaries.store.size_in_bytes
```

Use the number to estimate the amount of disk space the MySQL audit logs will need. The script also monitors your free disk space while the import is in progress. Monitoring this number is especially useful if your free disk space is close to the amount of disk space necessary for migration.

{% data reusables.enterprise_installation.preflight-checks %}

## Next steps

After reviewing these recommendations and requirements, you can upgrade {% data variables.product.prodname_ghe_server %}. For more information, see [AUTOTITLE](/admin/upgrading-your-instance/preparing-to-upgrade/overview-of-the-upgrade-process).
