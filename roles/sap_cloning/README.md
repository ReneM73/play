# Execute SAP Clone Role

This Ansible role is designed to manage SAP volume cleanup and cloning tasks.

The `execute_sap_cloning` role is designed to automate the volume cleanup and cloning in a NetApp ONTAP environment. This role is part of the `netapp-ansible-sap-refresh` playbook and handles the core logic for identifying cleanup and clone SAP volumes.

## Role Workflow

### main.yml

- **Execute Task `cleanup.yml`**
- **Execute Task `clone_and_mount.yml` if `clean_only is false`**

### cleanup.yml
  1. **Get all LUNs from volume**:
    - Execute `sgs.ontap.rest_info` role to get all LUNs from volume `06_clone`

  2. **Unmap all LUNs from volumes**:
    - Execute `sgs.ontap.lun` role with task from `map.yml` to unmap all LUNs from volume `06_clone`

  3. **Delete volume if `delete_volume is true`**:
    - Execute `sgs.ontap.volume` role to delete the volume `06_clone`

  4. **Rename volume if `delete_volume is false`**:
    - Execute `sgs.ontap.volume` role to rename the volume `06_clone` to `06_clone_tobedeleted_"DateToDay + 30 days"

### clone_and_mount.yml
  1. **Get all snapshot starting with `SP_` from volume**
    - Execute `sgs.ontap.rest_info` role to get all snapshots from the volume
  2. **Clone volume**
    - Execute `sgs.ontap.volume` role with task from `clone.yml` to clone the volume from the latest snapshot
  3.  **Get all LUNs from volume**:
    - Execute `sgs.ontap.rest_info` role to get all LUNs from volume
  4. **Mount all LUNs in volume**
    - Execute `sgs.ontap.lun` role with task from `map.yml` to mount the volumes


## Requirements

- NetApp ONTAP API credentials with sufficient permissions to manage volumes, LUNs, and initiator groups.
- Ansible collection `sgs.ontap` installed.

## Role Variables

The following variables are used by the role:

| Variable              | Type    | Description                                                   |
|-----------------------|---------|---------------------------------------------------------------|
| `volume_sap_clone`    | dict    | SAP volumes from this playbook `inventories/all.yml           |
| `vserver_sap_fc`         | string  | SAP vserver name from project inventory `inventores/sap.yml   |
