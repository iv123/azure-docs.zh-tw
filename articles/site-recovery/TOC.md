# 概觀
## [什麼是 Site Recovery？](site-recovery-overview.md)
## [Site Recovery 如何運作？](site-recovery-azure-to-azure-architecture.md)
## [將 Hyper-V 複寫至 Azure 如何運作？](site-recovery-hyper-v-azure-architecture.md)
## [哪些工作負載可以受到保護？](site-recovery-workload.md)
## [Site Recovery 支援矩陣](site-recovery-support-matrix-azure-to-azure.md)
## [常見問題集](site-recovery-faq.md)
## [觀看簡介](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# 開始使用
## [複寫 Azure 虛擬機器 (預覽)](site-recovery-azure-to-azure.md)
## [將 VMware VM 複寫到 Azure](site-recovery-vmware-to-azure.md)
## [將實體伺服器複寫到 Azure](site-recovery-physical-servers-to-azure.md)
## [將 Hyper-V VM 複寫至 Azure (使用 VMM)](site-recovery-vmm-to-azure.md)
## [將 Hyper-V VM 複寫至 Azure](site-recovery-hyper-v-site-to-azure.md)
## [將 Hyper-V VM 複寫至次要站台 (使用 VMM)](site-recovery-vmm-to-vmm.md)
## [將 VMware VM 和實體伺服器複寫至次要網站](site-recovery-vmware-to-vmware.md)
## [在多租用戶部署 (CSP) 中將 VMware VM 複寫至 Azure](site-recovery-multi-tenant-support-vmware-using-csp.md)

# 作法
## 規劃
### [Azure 複寫的必要條件](site-recovery-azure-to-azure-prereq.md)
### [為 Azure VM (預覽) 規劃網路輸出連線能力](site-recovery-azure-to-azure-networking-guidance.md)
### [為內部部署機器規劃網路基礎結構](site-recovery-network-design.md)
### [規劃網路對應](site-recovery-network-mapping-azure-to-azure.md)
### [規劃容量並調整 Azure 中的 VMware 複寫](site-recovery-plan-capacity-vmware.md)
### [將 VMware 複寫至 Azure 的部署規劃工具](site-recovery-deployment-planner.md)
### [適用於 Hyper-V 複寫的 Capacity Planner](site-recovery-capacity-planner.md)
### [使用角色型存取控制 VM 複寫](site-recovery-role-based-linked-access-control.md)

## 設定
### [設定來源環境](site-recovery-set-up-vmware-to-azure.md)
### [設定目標環境](site-recovery-prepare-target-vmware-to-azure.md)
### [設定複寫設定](site-recovery-setup-replication-settings-vmware.md)
### [部署 VMware 複寫的行動服務](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [使用 System Center Configuration Manager 部署行動服務](site-recovery-install-mobility-service-using-sccm.md)
#### [使用 Azure 自動化 DSC 部署行動服務](site-recovery-automate-mobility-service-install.md)
### [啟用複寫](site-recovery-replicate-azure-to-azure.md)
## 容錯移轉和容錯回復
### [設定復原方案](site-recovery-create-recovery-plans.md)
#### [將 Runbook 新增至復原方案](site-recovery-runbook-automation.md)
### [執行測試容錯移轉](site-recovery-test-failover-to-azure.md)
### [針對受保護的機器進行容錯移轉](site-recovery-failover.md)
### [容錯移轉之後重新保護機器](site-recovery-how-to-reprotect-azure-to-azure.md)
### [從 Azure 容錯移轉](site-recovery-failback-azure-to-vmware.md)

## 移轉
### [移轉至 Azure](site-recovery-migrate-to-azure.md)
### [在不同的 Azure 地區之間移轉](site-recovery-migrate-azure-to-azure.md)
### [將 AWS Windows 執行個體移轉到 Azure](site-recovery-migrate-aws-to-azure.md)
### [將已移轉的機器複寫到另一個 Azure 區域](site-recovery-azure-to-azure-after-migration.md)

## 工作負載
### [Active Directory 和 DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-sharepoint.md)
### [Dynamics AX](site-recovery-dynamicsax.md)
### [RDS](site-recovery-workload.md#protect-rds)
### [Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [IIS 型 Web 應用程式](site-recovery-iis.md)
### [Citrix XenApp 和 XenDesktop](site-recovery-citrix-xenapp-and-xendesktop.md)
### [其他工作負載](site-recovery-workload.md#workload-summary)
## 將複寫自動化
### [自動將 Hyper-V 複寫至 Azure (不使用 VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [自動將 Hyper-V 複寫至 Azure (透過 VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [自動將 Hyper-V 複寫至 Azure (透過 VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## 管理
### [編輯複寫設定](site-recovery-setup-replication-settings-vmware.md#edit-replication-policy.md)
### [管理 Azure 中的處理序伺服器](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [管理組態伺服器](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [管理相應放大處理序伺服器](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [管理 vCenter 伺服器](site-recovery-vmware-to-azure-manage-vCenter.md)
### [移除伺服器並停用保護](site-recovery-manage-registration-and-protection.md)
## 疑難排解
### [收集記錄](site-recovery-monitoring-and-troubleshooting.md)
### [Azure VM 複寫問題](site-recovery-azure-to-azure-troubleshoot-errors.md)
### [內部部署至 Azure 複寫問題](site-recovery-vmware-to-azure-protection-troubleshoot.md)

# 參考
## [PowerShell](/powershell/module/azurerm.siterecovery)
## [PowerShell 傳統](/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# 相關參考
## [Azure 自動化](/azure/automation/)

# 資源
## [學習路徑](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [論壇](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [部落格](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [價格](https://azure.microsoft.com/pricing/details/site-recovery/)
## [服務更新](https://azure.microsoft.com/updates/?product=site-recovery)
